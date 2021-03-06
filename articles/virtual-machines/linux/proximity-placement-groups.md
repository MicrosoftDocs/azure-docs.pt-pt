---
title: Criar um grupo de colocação de proximidade usando o Azure CLI
description: Saiba como criar e utilizar grupos de colocação de proximidade para máquinas virtuais em Azure.
author: cynthn
ms.service: virtual-machines
ms.subservice: proximity-placement-groups
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 3/8/2021
ms.author: cynthn
ms.openlocfilehash: e4f91afa86a0d99b4ce42e96295bf2ae1f9fcd9f
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/20/2021
ms.locfileid: "107771448"
---
# <a name="deploy-vms-to-proximity-placement-groups-using-azure-cli"></a>Implementar VMs em grupos de colocação por proximidade com a CLI do Azure

Para obter VMs o mais próximo possível, alcançando a menor latência possível, deve implantá-los dentro de um [grupo de colocação de proximidade](../co-location.md#proximity-placement-groups).

Um grupo de colocação de proximidade é um agrupamento lógico usado para garantir que os recursos de computação Azure estão fisicamente localizados perto uns dos outros. Os grupos de colocação de proximidade são úteis para cargas de trabalho onde a baixa latência é um requisito.


## <a name="create-the-proximity-placement-group"></a>Criar o grupo de colocação de proximidade
Crie um grupo de colocação de proximidade utilizando [`az ppg create`](/cli/azure/ppg#az_ppg_create) . 

```azurecli-interactive
az group create --name myPPGGroup --location westus
az ppg create \
   -n myPPG \
   -g myPPGGroup \
   -l westus \
   -t standard 
```

## <a name="list-proximity-placement-groups"></a>Listar grupos de colocação de proximidade

Pode listar todos os seus grupos de colocação de proximidade usando [a lista az ppg](/cli/azure/ppg#az_ppg_list).

```azurecli-interactive
az ppg list -o table
```

## <a name="create-a-vm"></a>Criar uma VM

Crie um VM dentro do grupo de colocação de proximidade utilizando [o novo az vm](/cli/azure/vm#az_vm_create).

```azurecli-interactive
az vm create \
   -n myVM \
   -g myPPGGroup \
   --image UbuntuLTS \
   --ppg myPPG  \
   --generate-ssh-keys \
   --size Standard_D1_v2  \
   -l westus
```

Pode ver o VM no grupo de colocação de proximidade usando [o show az ppg](/cli/azure/ppg#az_ppg_show).

```azurecli-interactive
az ppg show --name myppg --resource-group myppggroup --query "virtualMachines"
```

## <a name="availability-sets"></a>Conjuntos de Disponibilidade
Também pode criar um conjunto de disponibilidade no seu grupo de colocação de proximidade. Utilize o mesmo `--ppg` parâmetro com [a az vm disponibilidade-set criar](/cli/azure/vm/availability-set#az_vm_availability_set_create) para criar um conjunto de disponibilidade e todos os VMs no conjunto de disponibilidade também serão criados no mesmo grupo de colocação de proximidade.

## <a name="scale-sets"></a>Conjuntos de dimensionamento

Também pode criar um conjunto de escala no seu grupo de colocação de proximidade. Utilize o mesmo `--ppg` parâmetro com [az vmss criar](/cli/azure/vmss#az_vmss_create) para criar um conjunto de escala e todas as instâncias serão criadas no mesmo grupo de colocação de proximidade.

## <a name="next-steps"></a>Passos seguintes

Saiba mais sobre os comandos [Azure CLI](/cli/azure/ppg) para grupos de colocação de proximidade.