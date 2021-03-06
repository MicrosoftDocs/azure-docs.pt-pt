---
title: Implementar VMs num conjunto de disponibilidade usando Azure PowerShell
description: Saiba como utilizar o Azure PowerShell para implementar máquinas virtuais altamente disponíveis em Conjuntos de Disponibilidade
services: virtual-machines
author: mimckitt
ms.service: virtual-machines
ms.topic: how-to
ms.date: 3/8/2021
ms.author: mimckitt
ms.reviewer: cynthn
ms.custom: mvc
ms.openlocfilehash: 178a29ea37195ddd2013ca5220663a75132beb24
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2021
ms.locfileid: "102555912"
---
# <a name="create-and-deploy-virtual-machines-in-an-availability-set-using-azure-powershell"></a>Criar e implementar máquinas virtuais num conjunto de disponibilidade usando a Azure PowerShell

Neste tutorial, aprende-se a aumentar a disponibilidade e fiabilidade das suas Máquinas Virtuais (VMs) utilizando conjuntos de disponibilidade. Disponibilidade Os conjuntos certificam-se de que os VMs que implementa no Azure são distribuídos por múltiplos nós de hardware isolados, num cluster. 

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar um conjunto de disponibilidade
> * Criar uma VM num conjunto de disponibilidade
> * Verificar os tamanhos de VM disponíveis
> * Verificar o Assistente do Azure


## <a name="launch-azure-cloud-shell"></a>Iniciar o Azure Cloud Shell

O Azure Cloud Shell é um shell interativo gratuito que pode utilizar para executar os passos neste artigo. Tem as ferramentas comuns do Azure pré-instaladas e configuradas para utilização com a sua conta. 

Para abrir o Cloud Shell, basta selecionar **Experimente** no canto superior direito de um bloco de código. Também pode lançar cloud Shell num separador de navegador indo para [https://shell.azure.com/powershell](https://shell.azure.com/powershell) . Selecione **Copiar** para copiar os blocos de código, cole-o no Cloud Shell e prima Enter para executá-lo.

## <a name="create-an-availability-set"></a>Criar um conjunto de disponibilidade

O hardware numa localização está dividido em vários domínios de atualização e domínios de falhas. Um **domínio de atualização** é um grupo de VMs e hardware físico subjacente que pode ser reiniciado ao mesmo tempo. As VMs no mesmo **domínio de falha** partilham o mesmo armazenamento, bem como a fonte de energia e o comutador de rede.  

Pode criar um conjunto de disponibilidade utilizando [o New-AzAvailabilitySet](/powershell/module/az.compute/new-azavailabilityset). Neste exemplo, o número de domínios de atualização e avaria é *2* e o conjunto de disponibilidade é nomeado *myAvailabilitySet*.

Crie um grupo de recursos.

```azurepowershell-interactive
New-AzResourceGroup `
   -Name myResourceGroupAvailability `
   -Location EastUS
```

Crie um conjunto de disponibilidade gerido utilizando [o New-AzAvailabilitySet](/powershell/module/az.compute/new-azavailabilityset) com o `-sku aligned` parâmetro.

```azurepowershell-interactive
New-AzAvailabilitySet `
   -Location "EastUS" `
   -Name "myAvailabilitySet" `
   -ResourceGroupName "myResourceGroupAvailability" `
   -Sku aligned `
   -PlatformFaultDomainCount 2 `
   -PlatformUpdateDomainCount 2
```

## <a name="create-vms-inside-an-availability-set"></a>Criar VMs num conjunto de disponibilidade
Os VMs devem ser criados dentro do conjunto de disponibilidade para se certificar de que estão corretamente distribuídos pelo hardware. Não é possível adicionar um VM existente a um conjunto de disponibilidade depois de criado. 


Quando criar um VM com [New-AzVM,](/powershell/module/az.compute/new-azvm)utilize o `-AvailabilitySetName` parâmetro para especificar o nome do conjunto de disponibilidade.

Primeiro, defina um nome de utilizador e palavra-passe para a VM com [Get-Credential](/powershell/module/microsoft.powershell.security/get-credential):

```azurepowershell-interactive
$cred = Get-Credential
```

Agora crie dois VMs com [New-AzVM](/powershell/module/az.compute/new-azvm) no conjunto de disponibilidade.

```azurepowershell-interactive
for ($i=1; $i -le 2; $i++)
{
    New-AzVm `
        -ResourceGroupName "myResourceGroupAvailability" `
        -Name "myVM$i" `
        -Location "East US" `
        -VirtualNetworkName "myVnet" `
        -SubnetName "mySubnet" `
        -SecurityGroupName "myNetworkSecurityGroup" `
        -PublicIpAddressName "myPublicIpAddress$i" `
        -AvailabilitySetName "myAvailabilitySet" `
        -Credential $cred
}
```

Demora alguns minutos para criar e configurar ambas as VMs. Quando terminar, tem duas máquinas virtuais distribuídas no hardware subjacente. 

Se olhar para a disponibilidade definida no portal indo para **Grupos de Recursos**  >  **myResourceGroupAvailability**  >  **myAvailabilitySet,** deve ver como os VMs são distribuídos pelos dois domínios de falha e atualização.

![Conjunto de disponibilidade no portal](./media/tutorial-availability-sets/fd-ud.png)

## <a name="check-for-available-vm-sizes"></a>Verificar os tamanhos de VM disponíveis 

Quando cria um VM dentro de um conjunto de disponibilidade, precisa de saber quais os tamanhos VM disponíveis no hardware. Utilize o comando [Get-AzVMSize](/powershell/module/az.compute/get-azvmsize) para obter todos os tamanhos disponíveis para máquinas virtuais que pode implementar no conjunto de disponibilidade.

```azurepowershell-interactive
Get-AzVMSize `
   -ResourceGroupName "myResourceGroupAvailability" `
   -AvailabilitySetName "myAvailabilitySet"
```

## <a name="check-azure-advisor"></a>Verificar o Assistente do Azure 

Também pode utilizar o Azure Advisor para obter mais informações sobre como melhorar a disponibilidade dos seus VMs. O Azure Advisor analisa a sua configuração e telemetria de utilização, recomendando soluções que o ajudem a melhorar a eficácia de custos, desempenho, disponibilidade e segurança dos seus recursos Azure.

Inicie sessão no [portal do Azure](https://portal.azure.com), selecione **Todos os serviços** e escreva **Assistente**. O painel De aconselhamento apresenta recomendações personalizadas para a subscrição selecionada. Para obter mais informações, veja [Introdução ao Assistente do Azure](../../advisor/advisor-get-started.md).


## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Criar um conjunto de disponibilidade
> * Criar uma VM num conjunto de disponibilidade
> * Verificar os tamanhos de VM disponíveis
> * Verificar o Assistente do Azure

Avance para o tutorial seguinte para saber mais sobre os conjuntos de dimensionamento de máquinas virtuais.

> [!div class="nextstepaction"]
> [Criar um conjunto de dimensionamento de VM](tutorial-create-vmss.md)
