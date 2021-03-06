---
title: Configure a monitorização da GPU com insights de contentores | Microsoft Docs
description: Este artigo descreve como pode configurar a monitorização de clusters kubernetes com GPU NVIDIA e AMD, ativando nós com insights de contentores.
ms.topic: conceptual
ms.date: 03/27/2020
ms.openlocfilehash: 2958b000ac0dabcd7fddf75a58f553b705a95e9a
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/20/2021
ms.locfileid: "101731872"
---
# <a name="configure-gpu-monitoring-with-container-insights"></a>Configure a monitorização da GPU com insights de contentores

Começando pela versão do agente *ciprod03022019,* o agente integrado de informações de contentores suporta agora a monitorização da utilização da GPU (unidades de processamento gráfico) nos nós de cluster Kubernetes conscientes da GPU e monitoriza as cápsulas/contentores que solicitam e utilizam recursos da GPU.

## <a name="supported-gpu-vendors"></a>Fornecedores de GPU apoiados

Os conhecimentos dos contentores suportam a monitorização dos clusters gpu dos seguintes fornecedores de GPU:

- [NVIDIA](https://developer.nvidia.com/kubernetes-gpu)

- [AMD](https://github.com/RadeonOpenCompute/k8s-device-plugin)

Os insights do contentor iniciam automaticamente a monitorização da utilização da GPU nos nós, e a GPU solicitando cápsulas e cargas de trabalho, recolhendo as seguintes métricas em intervalos de 60sec e armazenando-as na tabela **InsightMetrics.**

>[!NOTE]
>Após o provisionamento do cluster com nós GPU, certifique-se de que [o controlador gpu](../../aks/gpu-cluster.md) é instalado conforme exigido pela AKS para executar cargas de trabalho gpu. Os insights dos contentores recolhem métricas de GPU através de cápsulas de condutor da GPU que correm no nó. 

|Nome da métrica |Dimensão métrica (etiquetas) |Description |
|------------|------------------------|------------|
|containerGpuDutyCycle |container.azm.ms/clusterId, container.azm.ms/clusterName, containerName, gpuId, gpuModel, gpuVendor|Percentagem de tempo durante o período de amostragem anterior (60 segundos) durante o qual a GPU estava ocupada/ativamente a processar para um recipiente. O ciclo de serviços é um número entre 1 e 100. |
|containerGpuLimits |container.azm.ms/clusterId, container.azm.ms/clusterName, nome de contentor |Cada recipiente pode especificar limites como uma ou mais GPUs. Não é possível solicitar ou limitar uma fração de uma GPU. |
|containerGpuRequests |container.azm.ms/clusterId, container.azm.ms/clusterName, nome de contentor |Cada recipiente pode solicitar uma ou mais GPUs. Não é possível solicitar ou limitar uma fração de uma GPU.|
|containerGpumemoryTotalBytes |container.azm.ms/clusterId, container.azm.ms/clusterName, containerName, gpuId, gpuModel, gpuVendor |Quantidade de memória GPU em bytes disponíveis para um recipiente específico. |
|containerGpumemoryUsedBytes |container.azm.ms/clusterId, container.azm.ms/clusterName, containerName, gpuId, gpuModel, gpuVendor |Quantidade de memória GPU em bytes utilizados por um recipiente específico. |
|nodeGpuAllocatable |container.azm.ms/clusterId, container.azm.ms/clusterName, gpuVendor |Número de GPUs num nó que pode ser usado por Kubernetes. |
|nodeGpuCapacity |container.azm.ms/clusterId, container.azm.ms/clusterName, gpuVendor |Número total de GPUs num nó. |

## <a name="gpu-performance-charts"></a>Gráficos de desempenho da GPU 

Os insights do contentor incluem gráficos pré-configurados para as métricas listadas anteriormente na tabela como um livro de GPU para cada cluster. Consulte [os livros de trabalho em informações sobre o recipiente](../insights/container-insights-reports.md) para obter uma descrição dos livros disponíveis para informações sobre o contentor.

## <a name="next-steps"></a>Passos seguintes

- Consulte [as GPUs de utilização para cargas de trabalho intensivas em Azure Kubernetes Service](../../aks/gpu-cluster.md) (AKS) para aprender a implementar um cluster AKS que inclui nós ativados por GPU.

- Saiba mais sobre [GPU Optimized VM SKUs no Microsoft Azure](../../virtual-machines/sizes-gpu.md).

- Reveja [o suporte da GPU em Kubernetes](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/) para saber mais sobre o apoio experimental de Kubernetes para gerir GPUs em um ou mais nós em um cluster.
