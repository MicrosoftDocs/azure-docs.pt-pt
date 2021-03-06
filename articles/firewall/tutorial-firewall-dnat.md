---
title: 'Tutorial: Filtrar o tráfego de internet de entrada com ADN da Azure Firewall usando o portal'
description: Neste tutorial, vai aprender a implementar e configurar a DNAT do Azure Firewall com o portal do Azure.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: tutorial
ms.date: 03/01/2021
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: a1d3bdae1e870b094472a63d4b808d9df95c129d
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/20/2021
ms.locfileid: "101741910"
---
# <a name="tutorial-filter-inbound-internet-traffic-with-azure-firewall-dnat-using-the-azure-portal"></a>Tutorial: Filtrar o tráfego de internet de entrada com ADN da Azure Firewall usando o portal Azure

Pode configurar a Tradução de Endereços de Rede de Destino (DNAT) do Azure Firewall para traduzir e filtrar o tráfego de Internet de entrada para as sub-redes. Ao configurar o DNAT, a ação de recolha de regras NAT está definida para **Dnat**. Cada regra na coleção de regras NAT pode então ser usada para traduzir o seu endereço IP público de firewall e porta para um endereço IP privado e porta. As regras DNAT adicionam implicitamente uma regra de rede correspondente para permitir o tráfego traduzido. Por razões de segurança, a abordagem recomendada é adicionar uma fonte de Internet específica para permitir o acesso do DNAT à rede e evitar a utilização de wildcards. Para saber mais sobre a lógica de processamento de regras do Azure Firewall, veja [Lógica de processamento de regras do Azure Firewall](rule-processing.md).

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Configurar um ambiente de rede de teste
> * Implementar uma firewall
> * Criar uma rota predefinida
> * Configurar uma regra DNAT
> * Testar a firewall

## <a name="prerequisites"></a>Pré-requisitos

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.



## <a name="create-a-resource-group"></a>Criar um grupo de recursos

1. Inicie sessão no Portal do Azure em [https://portal.azure.com](https://portal.azure.com).
2. Na página inicial do portal Azure, selecione **grupos de recursos** e, em seguida, selecione **Adicionar**.
4. Em **Subscrição**, selecione a sua subscrição.
1. Em **Nome do grupo de recursos**, escreva **RG-DNAT-Test**.
5. Para **a Região,** selecione uma região. Todos os outros recursos que cria devem estar na mesma região.
6. Selecione **Rever + criar**.
1. Selecione **Criar**.

## <a name="set-up-the-network-environment"></a>Configurar o ambiente de rede

Neste tutorial, vai criar duas VNets em modo peering:

- **VNet-Hub** - a firewall está nesta VNet.
- **VN-Spoke** - o servidor de carga de trabalho está nesta VNet.

Em primeiro lugar, crie as VNets e, em seguida, configure o peering entre elas.

### <a name="create-the-hub-vnet"></a>Criar a VNet Hub

1. A partir da página inicial do portal Azure, selecione **Todos os serviços**.
2. Em **Rede,** selecione **redes Virtuais.**
3. Selecione **Adicionar**.
7. Para **o grupo de recursos**, selecione **RG-DNAT-Test**.
1. Em **Nome**, escreva **VN-Hub**.
1. Para **a Região,** selecione a mesma região que usou antes.
1. Selecione **Seguinte: Endereços IP**.
1. Para **o espaço IPv4 Address,** aceite o **padrão 10.0.0.0/16**.
1. No **nome da sub-rede,** selecione predefinição.
1. Editar o **nome sub-rede** e escrever **AzureFirewallSubnet**.

     A firewall estará nesta sub-rede, e o nome da sub-rede **tem** de ser AzureFirewallSubnet.
     > [!NOTE]
     > O tamanho da sub-rede AzureFirewallSubnet é /26. Para obter mais informações sobre o tamanho da sub-rede, consulte [a Azure Firewall FAQ](firewall-faq.yml#why-does-azure-firewall-need-a--26-subnet-size).

10. Para **a gama de endereços da sub-rede**, tipo **10.0.1.0/26**.
11. Selecione **Guardar**.
1. Selecione **Rever + criar**.
1. Selecione **Criar**.

### <a name="create-a-spoke-vnet"></a>Criar uma VNet spoke

1. A partir da página inicial do portal Azure, selecione **Todos os serviços**.
2. Em **Rede,** selecione **redes Virtuais.**
3. Selecione **Adicionar**.
1. Para **o grupo de recursos**, selecione **RG-DNAT-Test**.
1. Em **Nome**, escreva **VN-Spoke**.
1. Para **a Região,** selecione a mesma região que usou antes.
1. Selecione **Seguinte: Endereços IP**.
1. Para **o espaço IPv4 Address,** edite o padrão e **escreva 192.168.0.0/16**.
1. **Selecione Adicionar sub-rede**.
1. Para o **nome sub-rede** tipo **SN-Workload**.
10. Para **a gama de endereços sub-rede**, tipo **192.168.1.0/24**.
11. Selecione **Adicionar**.
1. Selecione **Rever + criar**.
1. Selecione **Criar**.

### <a name="peer-the-vnets"></a>Configurar o peering entre as VNets

Agora, configure o peering entre as duas VNets.

1. Selecione a rede virtual **VN-Hub.**
2. Em **Definições**, **selecione Peerings**.
3. Selecione **Adicionar**.
4. Nesta **rede virtual**, para o nome de link **peering**, **digite Peer-HubSpoke**.
5. Em **rede virtual remota**, para nome de link **peering**, **digite Peer-SpokeHub**. 
1. Selecione **VN-Spoke** para a rede virtual.
1. Aceite todos os outros incumprimentos e, em seguida, **selecione Adicionar**.

## <a name="create-a-virtual-machine"></a>Criar uma máquina virtual

Crie uma máquina virtual de carga de trabalho e coloque-a na sub-rede **SN-Workload**.

1. No menu do portal do Azure, selecione **Criar um recurso**.
2. Em **Popular**, selecione **Windows Server 2016 Datacenter**.

**Noções básicas**

1. Em **Subscrição**, selecione a sua subscrição.
1. Para **o grupo de recursos**, selecione **RG-DNAT-Test**.
1. Para **o nome da máquina virtual,** **escreva Srv-Workload**.
1. Para **a Região**, selecione o mesmo local que usou anteriormente.
1. Escreva um nome de utilizador e uma palavra-passe.
1. Selecione **Seguinte: Discos**.

**Discos**
1. Selecione **Seguinte: Rede**.

**Rede**

1. Para **rede virtual**, selecione **VN-Spoke**.
2. Em **Sub-rede**, selecione **SN-Workload**.
3. Para **IP público**, selecione **Nenhum**.
4. Para **portas de entrada pública**, selecione **Nenhum**. 
2. Deixe as outras definições predefinidos e selecione **Seguinte: Gestão**.

**Gestão**

1. Para **diagnósticos de arranque,** selecione **Disable**.
1. Selecione **Rever + Criar**.

**Comentário + Criar**

Reveja o resumo e, em seguida, **selecione Criar**. Este processo vai demorar alguns minutos a concluir.

Após a conclusão da implementação, tome nota do endereço IP privado para a máquina virtual. Será utilizado mais tarde quando configurar a firewall. Selecione o nome da máquina virtual e em **Definições**, **selecione Networking** para encontrar o endereço IP privado.

## <a name="deploy-the-firewall"></a>Implementar a firewall

1. A partir da página inicial do portal, **selecione Criar um recurso**.
1. Procure por **Firewall** e, em seguida, selecione **Firewall**.
1. Selecione **Criar**. 
1. Na página **Criar uma firewall**, utilize a seguinte tabela para configurar a firewall:

   |Definição  |Valor  |
   |---------|---------|
   |Subscrição     |\<your subscription\>|
   |Grupo de recursos     |Selecione **RG-DNAT-Test** |
   |Name     |**FW-DNAT-test**|
   |Region     |Selecionar a mesma localização que utilizou anteriormente|
   |Gestão de firewall|**Use as regras firewall (clássica) para gerir esta firewall**|
   |Escolher uma rede virtual     |**Utilizar existente**: VN-Hub|
   |Endereço IP público     |**Adicione novo**, Nome: **fw-pip**.|

5. Aceite os outros incumprimentos e, em seguida, selecione **Review + create**.
6. Reveja o resumo e, em seguida, **selecione Criar** para criar a firewall.

   Este processo irá demorar alguns minutos a implementar.
7. Após a implementação concluída, vá ao grupo de recursos **RG-DNAT-Test** e selecione a firewall **de teste FW-DNAT.**
8. Observe os endereços IP privados e públicos da firewall. Irá usá-los mais tarde quando criar a rota padrão e a regra NAT.

## <a name="create-a-default-route"></a>Criar uma rota predefinida

Na sub-rede **SN-Workload**, vai configurar a rota de saída predefinida para passar pela firewall.

1. A partir da página inicial do portal Azure, selecione **Todos os serviços**.
2. Em **Rede,** selecione **tabelas de rota**.
3. Selecione **Adicionar**.
5. Em **Subscrição**, selecione a sua subscrição.
1. Para **o grupo de recursos**, selecione **RG-DNAT-Test**.
1. Para **a Região**, selecione a mesma região que usou anteriormente.
1. Em **Nome**, escreva **RT-FWroute**.
1. Selecione **Rever + criar**.
1. Selecione **Criar**.
1. Selecione **Ir para recurso**.
1. Selecione **sub-redes** e, em seguida, **selecione Associate**.
1. Para **rede virtual**, selecione **VN-Spoke**.
1. Em **Sub-rede**, selecione **SN-Workload**.
1. Selecione **OK**.
1. Selecione **Rotas** e, em seguida, selecione **Adicionar**.
1. Em **Nome da rota**, escreva **FW-DG**.
1. Em **Prefixo de endereço**, escreva **0.0.0.0/0**.
1. Em **Tipo de salto seguinte**, selecione **Aplicação virtual**.

    O Azure Firewall é, de facto, um serviço gerido, mas a aplicação virtual funciona nesta situação.
18. Em **Endereço do próximo salto**, escreva o endereço IP privado para a firewall, que anotou anteriormente.
19. Selecione **OK**.

## <a name="configure-a-nat-rule"></a>Configure uma regra NAT

1. Abra o grupo de recursos **RG-DNAT-Test** e selecione a firewall **de teste FW-DNAT.** 
2. Na página **de teste FW-DNAT,** em **Definições,** selecione **Regras (clássicas)**. 
3. Selecione **Adicionar a coleção de regras NAT**. 
4. Em **Nome**, escreva **RC-DNAT-01**. 
5. Em **Prioridade**, escreva **200**. 
6. Em **Regras**, em **Nome**, escreva **RL-01**.
7. Em **Protocolo**, selecione **TCP**.
1. Para **o tipo de fonte**, selecione endereço **IP**.
1. Para **fonte,** escreva *. 
1. Para **endereços de destino,** digite o endereço IP público da firewall. 
1. Em **Portas de Destino**, escreva **3389**. 
1. Em **Endereço Traduzido**, escreva o endereço IP privado da máquina virtual Srv-Workload. 
1. Em **Porta traduzida**, escreva **3389**. 
1. Selecione **Adicionar**. Este processo vai demorar alguns minutos a concluir.

## <a name="test-the-firewall"></a>Testar a firewall

1. Ligue uma área de trabalho remota ao endereço IP público da firewall. Deverá estar ligado à máquina virtual **Srv-Workload**.
2. Feche o ambiente de trabalho remoto.

## <a name="clean-up-resources"></a>Limpar os recursos

Pode manter os recursos da firewall para o próximo tutorial. Se já não precisar dos mesmos, elimine o grupo de recursos **RG-DNAT-Test** para eliminar todos os recursos relacionados com a firewall.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Configurar um ambiente de rede de teste
> * Implementar uma firewall
> * Criar uma rota predefinida
> * Configurar uma regra DNAT
> * Testar a firewall

Em seguida, pode monitorizar os registos do Azure Firewall.

> [!div class="nextstepaction"]
> [Tutorial: monitorizar registos do Azure Firewall](./firewall-diagnostics.md)
