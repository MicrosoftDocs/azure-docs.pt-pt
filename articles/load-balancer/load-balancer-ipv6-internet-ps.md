---
title: Criar um equilibrador de carga virado para a Internet com iPv6 - Azure PowerShell
titleSuffix: Azure Load Balancer
description: Saiba como criar um equilibrador de carga virado para a Internet com o IPv6 utilizando o PowerShell para Gestor de Recursos
services: load-balancer
documentationcenter: na
author: asudbring
keywords: ipv6, balançador de carga azul, pilha dupla, ip público, ipv6 nativo, móvel, iot
ms.service: load-balancer
ms.custom: seodec18
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: allensu
ms.openlocfilehash: 43203a756bcb42c7d00de9c11e9223f1d8b9e2a8
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "87001592"
---
# <a name="get-started-creating-an-internet-facing-load-balancer-with-ipv6-using-powershell-for-resource-manager"></a>Começar a criar um equilibrador de carga virado para a Internet com o IPv6 usando o PowerShell para Gestor de Recursos

> [!div class="op_single_selector"]
> * [PowerShell](load-balancer-ipv6-internet-ps.md)
> * [CLI do Azure](load-balancer-ipv6-internet-cli.md)
> * [Modelo](load-balancer-ipv6-internet-template.md)

>[!NOTE] 
>Este artigo descreve uma funcionalidade introdutória do IPv6 para permitir que os Balanceadores de Carga Básica forneçam conectividade IPv4 e IPv6. A conectividade abrangente do IPv6 está agora disponível com [o IPv6 para VNETs Azure,](../virtual-network/ipv6-overview.md) que integra a conectividade IPv6 com as suas Redes Virtuais e inclui funcionalidades-chave como as regras do Grupo de Segurança da Rede IPv6, encaminhamento definido pelo utilizador IPv6, equilíbrio de carga básica e padrão IPv6, e muito mais.  IPv6 para Azure VNETs é a norma recomendada para aplicações IPv6 em Azure. Ver [IPv6 para implementação de Powershell Azure VNET](../virtual-network/virtual-network-ipv4-ipv6-dual-stack-standard-load-balancer-powershell.md) 

Um balanceador de carga do Azure é um balanceador de carga de Camada 4 (TCP, UDP). O balanceador de carga fornece elevada disponibilidade, ao distribuir o tráfego de entrada entre instâncias de serviço com bom estado de funcionamento nos serviços cloud ou máquinas virtuais num conjunto de balanceador de carga. O Balanceador de Carga do Azure pode também apresentar esses serviços em várias portas, vários endereços IP ou ambos.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="example-deployment-scenario"></a>Cenário de implantação de exemplo

O diagrama seguinte ilustra a solução de equilíbrio de carga que está a ser implantada neste artigo.

![Cenário do Balanceador de carga](./media/load-balancer-ipv6-internet-ps/lb-ipv6-scenario.png)

Neste cenário, criará os seguintes recursos Azure:

* um Balanceador de Carga virado para a Internet com um IPv4 e um endereço IP público IPv6
* duas regras de equilíbrio de carga para mapear os VIPs públicos para os pontos finais privados
* um Conjunto de Disponibilidade para que contenha os dois VMs
* duas máquinas virtuais (VMs)
* uma interface de rede virtual para cada VM com endereços IPv4 e IPv6 atribuídos

## <a name="deploying-the-solution-using-the-azure-powershell"></a>Implantação da solução utilizando o Azure PowerShell

Os passos seguintes mostram como criar um equilibrador de carga virado para a Internet utilizando o Azure Resource Manager com o PowerShell. Com o Azure Resource Manager, cada recurso é criado e configurado individualmente, em seguida, juntos para criar um recurso.

Para implantar um equilibrador de carga, cria e configura os seguintes objetos:

* Configuração IP frontend - contém endereços IP públicos para o tráfego de rede de entrada.
* Backend address pool - contém interfaces de rede (NICs) para que as máquinas virtuais recebam tráfego de rede do equilibrador de carga.
* Regras de balanceamento de carga - contém as regras que mapeiam uma porta pública no balanceador de carga para a porta no conjunto de endereços de back-end.
* Regras NAT de entrada - contém as regras que mapeiam uma porta pública no balanceador de carga para uma porta de uma máquina virtual específica no conjunto de endereços de back-end.
* Sondas - contém sondas utilizadas para verificar a disponibilidade de instâncias das máquinas virtuais no conjunto de endereços de back-end.

Para obter mais informações, consulte [os componentes do Balançador de Carga Azure](./components.md).

## <a name="set-up-powershell-to-use-resource-manager"></a>Configurar o PowerShell para utilizar o Resource Manager

Certifique-se de que tem a mais recente versão de produção do módulo Azure Resource Manager para PowerShell.

1. Iniciar sessão no Azure

    ```azurepowershell-interactive
    Connect-AzAccount
    ```

    Introduza as credenciais quando lhe for pedido.

2. Verifique as subscrições da conta

   ```azurepowershell-interactive
    Get-AzSubscription
    ```

3. Escolha qual das subscrições do Azure utilizar.

    ```azurepowershell-interactive
    Select-AzSubscription -SubscriptionId 'GUID of subscription'
    ```

4. Criar um grupo de recursos (ignore este passo se utilizar um grupo de recursos existente)

    ```azurepowershell-interactive
    New-AzResourceGroup -Name NRP-RG -location "West US"
    ```

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Criar uma rede virtual e um endereço IP público para o conjunto IP de front-end

1. Criar uma rede virtual com uma sub-rede.

    ```azurepowershell-interactive
    $backendSubnet = New-AzVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
    $vnet = New-AzvirtualNetwork -Name VNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet
    ```

2. Crie recursos de endereço IP público (PIP) para o conjunto de endereços IP front-end. Certifique-se de que altera o valor `-DomainNameLabel` antes de executar os seguintes comandos. O valor deve ser único na região de Azure.

    ```azurepowershell-interactive
    $publicIPv4 = New-AzPublicIpAddress -Name 'pub-ipv4' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Static -IpAddressVersion IPv4 -DomainNameLabel lbnrpipv4
    $publicIPv6 = New-AzPublicIpAddress -Name 'pub-ipv6' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Dynamic -IpAddressVersion IPv6 -DomainNameLabel lbnrpipv6
    ```

    > [!IMPORTANT]
    > O equilibrador de carga utiliza o rótulo de domínio do IP público como prefixo para o seu FQDN. Neste exemplo, as FQDNs são *lbnrpipv4.westus.cloudapp.azure.com* e *lbnrpipv6.westus.cloudapp.azure.com.*

## <a name="create-a-front-end-ip-configurations-and-a-back-end-address-pool"></a>Crie configurações IP Front-End e um Pool de Endereços Back-End

1. Crie uma configuração de endereço frontal que utilize os endereços IP públicos criados.

    ```azurepowershell-interactive
    $FEIPConfigv4 = New-AzLoadBalancerFrontendIpConfig -Name "LB-Frontendv4" -PublicIpAddress $publicIPv4
    $FEIPConfigv6 = New-AzLoadBalancerFrontendIpConfig -Name "LB-Frontendv6" -PublicIpAddress $publicIPv6
    ```

2. Crie piscinas de endereços back-end.

    ```azurepowershell-interactive
    $backendpoolipv4 = New-AzLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv4"
    $backendpoolipv6 = New-AzLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv6"
    ```

## <a name="create-lb-rules-nat-rules-a-probe-and-a-load-balancer"></a>Crie regras LB, regras NAT, uma sonda e um equilibrador de carga

Este exemplo cria os seguintes itens:

* uma regra NAT para traduzir todo o tráfego de entrada na porta 443 para o porto 4443
* Uma regra de balanceador de carga para balancear todo o tráfego de entrada na porta 80 à porta 80 nos endereços do conjunto de back-end.
* uma regra do balançador de carga para permitir a ligação rdp aos VMs na porta 3389.
* uma regra da sonda para verificar o estado de saúde numa página chamada *HealthProbe.aspx* ou um serviço na porta 8080
* um equilibrador de carga que usa todos estes objetos

1. Crie as regras NAT.

    ```azurepowershell-interactive
    $inboundNATRule1v4 = New-AzLoadBalancerInboundNatRuleConfig -Name "NicNatRulev4" -FrontendIpConfiguration $FEIPConfigv4 -Protocol TCP -FrontendPort 443 -BackendPort 4443
    $inboundNATRule1v6 = New-AzLoadBalancerInboundNatRuleConfig -Name "NicNatRulev6" -FrontendIpConfiguration $FEIPConfigv6 -Protocol TCP -FrontendPort 443 -BackendPort 4443
    ```

2. Crie uma sonda de estado de funcionamento. Existem duas formas de configurar uma pesquisa:

    Sonda HTTP

    ```azurepowershell-interactive
    $healthProbe = New-AzLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
    ```

    ou sonda TCP

    ```azurepowershell-interactive
    $healthProbe = New-AzLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -Protocol Tcp -Port 8080 -IntervalInSeconds 15 -ProbeCount 2
    $RDPprobe = New-AzLoadBalancerProbeConfig -Name 'RDPprobe' -Protocol Tcp -Port 3389 -IntervalInSeconds 15 -ProbeCount 2
    ```

    Para este exemplo, vamos usar as sondas TCP.

3. Crie uma regra de balanceador de carga.

    ```azurepowershell-interactive
    $lbrule1v4 = New-AzLoadBalancerRuleConfig -Name "HTTPv4" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
    $lbrule1v6 = New-AzLoadBalancerRuleConfig -Name "HTTPv6" -FrontendIpConfiguration $FEIPConfigv6 -BackendAddressPool $backendpoolipv6 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
    $RDPrule = New-AzLoadBalancerRuleConfig -Name "RDPrule" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $RDPprobe -Protocol Tcp -FrontendPort 3389 -BackendPort 3389
    ```

4. Crie o equilibrador de carga utilizando os objetos previamente criados.

    ```azurepowershell-interactive
    $NRPLB = New-AzLoadBalancer -ResourceGroupName NRP-RG -Name 'myNrpIPv6LB' -Location 'West US' -FrontendIpConfiguration $FEIPConfigv4,$FEIPConfigv6 -InboundNatRule $inboundNATRule1v6,$inboundNATRule1v4 -BackendAddressPool $backendpoolipv4,$backendpoolipv6 -Probe $healthProbe,$RDPprobe -LoadBalancingRule $lbrule1v4,$lbrule1v6,$RDPrule
    ```

## <a name="create-nics-for-the-back-end-vms"></a>Criar NICs para os VMs de back-end

1. Obtenha a Rede Virtual e a Sub-rede de Rede Virtual, onde os NICs precisam de ser criados.

    ```azurepowershell-interactive
    $vnet = Get-AzVirtualNetwork -Name VNet -ResourceGroupName NRP-RG
    $backendSubnet = Get-AzVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet
    ```

2. Crie configurações IP e NICs para os VMs.

    ```azurepowershell-interactive
    $nic1IPv4 = New-AzNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4 -LoadBalancerInboundNatRule $inboundNATRule1v4
    $nic1IPv6 = New-AzNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6 -LoadBalancerInboundNatRule $inboundNATRule1v6
    $nic1 = New-AzNetworkInterface -Name 'myNrpIPv6Nic0' -IpConfiguration $nic1IPv4,$nic1IPv6 -ResourceGroupName NRP-RG -Location 'West US'

    $nic2IPv4 = New-AzNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4
    $nic2IPv6 = New-AzNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6
    $nic2 = New-AzNetworkInterface -Name 'myNrpIPv6Nic1' -IpConfiguration $nic2IPv4,$nic2IPv6 -ResourceGroupName NRP-RG -Location 'West US'
    ```

## <a name="create-virtual-machines-and-assign-the-newly-created-nics"></a>Crie máquinas virtuais e atribua os NICs recém-criados

Para obter mais informações sobre a criação de um VM, consulte [Criar e pré-configurar uma Máquina Virtual do Windows com o Gestor de Recursos e a Azure PowerShell](../virtual-machines/windows/quick-create-powershell.md?toc=%2fazure%2fload-balancer%2ftoc.json)

1. Criar uma conta de Conjunto de Disponibilidade e Armazenamento

    ```azurepowershell-interactive
    New-AzAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG -location 'West US'
    $availabilitySet = Get-AzAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG
    New-AzStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct' -Location 'West US' -SkuName "Standard_LRS"
    $CreatedStorageAccount = Get-AzStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct'
    ```

2. Crie cada VM e atribua os NICs criados anteriormente

    ```azurepowershell-interactive
    $mySecureCredentials= Get-Credential -Message "Type the username and password of the local administrator account."

    $vm1 = New-AzVMConfig -VMName 'myNrpIPv6VM0' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
    $vm1 = Set-AzVMOperatingSystem -VM $vm1 -Windows -ComputerName 'myNrpIPv6VM0' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
    $vm1 = Set-AzVMSourceImage -VM $vm1 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm1 = Add-AzVMNetworkInterface -VM $vm1 -Id $nic1.Id -Primary
    $osDisk1Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM0osdisk.vhd"
    $vm1 = Set-AzVMOSDisk -VM $vm1 -Name 'myNrpIPv6VM0osdisk' -VhdUri $osDisk1Uri -CreateOption FromImage
    New-AzVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm1

    $vm2 = New-AzVMConfig -VMName 'myNrpIPv6VM1' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
    $vm2 = Set-AzVMOperatingSystem -VM $vm2 -Windows -ComputerName 'myNrpIPv6VM1' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
    $vm2 = Set-AzVMSourceImage -VM $vm2 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm2 = Add-AzVMNetworkInterface -VM $vm2 -Id $nic2.Id -Primary
    $osDisk2Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM1osdisk.vhd"
    $vm2 = Set-AzVMOSDisk -VM $vm2 -Name 'myNrpIPv6VM1osdisk' -VhdUri $osDisk2Uri -CreateOption FromImage
    New-AzVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm2
    ```


