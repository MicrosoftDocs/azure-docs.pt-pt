---
title: Iniciar e Parar um Serviço Azure Kubernetes (AKS)
description: Saiba como parar ou iniciar um cluster Azure Kubernetes Service (AKS).
services: container-service
ms.topic: article
ms.date: 09/24/2020
author: palma21
ms.openlocfilehash: 2d3c946bc2f98b0c06fe33dcaaa77a5399f6d56b
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/20/2021
ms.locfileid: "107782734"
---
# <a name="stop-and-start-an-azure-kubernetes-service-aks-cluster"></a>Parar e Iniciar um cluster de serviço Azure Kubernetes (AKS)

As suas cargas de trabalho AKS podem não precisar de funcionar continuamente, por exemplo, um cluster de desenvolvimento que é utilizado apenas durante o horário comercial. Isto leva a tempos em que o seu cluster Azure Kubernetes Service (AKS) pode estar inativo, funcionando não mais do que os componentes do sistema. Você pode reduzir a pegada do cluster [escalando todas as `User` piscinas de nós para 0](scale-cluster.md#scale-user-node-pools-to-0), mas a sua [ `System` piscina](use-system-pools.md) ainda é necessária para executar os componentes do sistema enquanto o cluster está em execução. Para otimizar ainda mais os seus custos durante estes períodos, pode desligar completamente (parar) o seu cluster. Esta ação irá parar completamente o seu plano de controlo e os nós do agente, permitindo-lhe economizar em todos os custos de computação, mantendo todos os seus objetos e estado de cluster armazenados para quando o recomeçar. Em seguida, pode retomar o lugar que deixou depois de um fim de semana ou ter o seu grupo a funcionar apenas enquanto dirige os seus trabalhos de lote.

## <a name="before-you-begin"></a>Antes de começar

Este artigo pressupõe que você tem um cluster AKS existente. Se precisar de um cluster AKS, consulte o quickstart AKS [utilizando o Azure CLI][aks-quickstart-cli] ou [utilizando o portal Azure][aks-quickstart-portal].

### <a name="limitations"></a>Limitações

Ao utilizar a função de arranque/paragem do cluster, aplicam-se as seguintes restrições:

- Esta funcionalidade é suportada apenas para conjuntos de escala de máquina virtual.
- O estado de aglomerado de um aglomerado AKS parado é preservado por até 12 meses. Se o seu aglomerado for parado por mais de 12 meses, o estado de agrupamento não pode ser recuperado. Para mais informações, consulte as [Políticas de Apoio À AKS.](support-policies.md)
- Só pode iniciar ou eliminar um cluster AKS parado. Para realizar qualquer operação como escala ou upgrade, inicie primeiro o seu cluster.

## <a name="stop-an-aks-cluster"></a>Pare um cluster AKS

Pode usar o `az aks stop` comando para parar os nós e o plano de controlo de um cluster AKS em funcionamento. O exemplo a seguir para um cluster chamado *myAKSCluster:*

```azurecli-interactive
az aks stop --name myAKSCluster --resource-group myResourceGroup
```

Pode verificar quando o seu cluster é interrompido utilizando o comando [az aks e][az-aks-show] confirmando os `powerState` espetáculos como `Stopped` na saída abaixo:

```json
{
[...]
  "nodeResourceGroup": "MC_myResourceGroup_myAKSCluster_westus2",
  "powerState":{
    "code":"Stopped"
  },
  "privateFqdn": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
[...]
}
```

Se os `provisioningState` espetáculos `Stopping` significarem que o teu grupo ainda não parou completamente.

> [!IMPORTANT]
> Se estiver a utilizar [os Orçamentos de Interrupção](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/) do Pod, a operação stop pode demorar mais tempo, uma vez que o processo de drenagem levará mais tempo a ser concluído.

## <a name="start-an-aks-cluster"></a>Inicie um cluster AKS

Pode usar o `az aks start` comando para iniciar um aglomerado AKS parado e um plano de controlo. O cluster é reiniciado com o estado do plano de controlo anterior e o número de nós de agente.  
O exemplo a seguir inicia um cluster chamado *myAKSCluster:*

```azurecli-interactive
az aks start --name myAKSCluster --resource-group myResourceGroup
```

Pode verificar quando o seu cluster começou usando o comando [az aks show][az-aks-show] e confirmando os `powerState` programas como na saída `Running` abaixo:

```json
{
[...]
  "nodeResourceGroup": "MC_myResourceGroup_myAKSCluster_westus2",
  "powerState":{
    "code":"Running"
  },
  "privateFqdn": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
[...]
}
```

Se os `provisioningState` espetáculos `Starting` significa que o seu grupo ainda não começou completamente.

## <a name="next-steps"></a>Passos seguintes

- Para aprender a escalar `User` piscinas para 0, consulte [ `User` piscinas scale a 0](scale-cluster.md#scale-user-node-pools-to-0).
- Para aprender a economizar custos usando instâncias spot, consulte Adicionar uma piscina de nó de [ponto à AKS](spot-node-pool.md).
- Para saber mais sobre as políticas de apoio da AKS, consulte as [políticas de apoio da AKS.](support-policies.md)

<!-- LINKS - external -->

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[az-extension-add]: /cli/azure/extension#az_extension_add
[az-extension-update]: /cli/azure/extension#az_extension_update
[az-feature-register]: /cli/azure/feature#az_feature_register
[az-feature-list]: /cli/azure/feature#az_feature_list
[az-provider-register]: /cli/azure/provider#az_provider_register
[az-aks-show]: /cli/azure/aks#az_aks_show
