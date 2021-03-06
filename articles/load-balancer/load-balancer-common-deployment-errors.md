---
title: Resolver erros de implementação comuns
titleSuffix: Azure Load Balancer
description: Descreve como resolver erros comuns quando implementa balanceadores de carga Azure
services: load-balancer
documentationcenter: na
tags: top-support-issue
author: anavinahar
ms.service: load-balancer
ms.topic: troubleshooting
ms.date: 04/27/2020
ms.author: anavin
ms.openlocfilehash: ae3899a61e942695fed1e3da5fc543b298a42e24
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "94695559"
---
# <a name="troubleshoot-common-azure-deployment-errors-with-azure-load-balancer"></a>Resolução de problemas erros comuns de implantação do Azure com o Equilibrador de Carga Azure

Este artigo descreve alguns erros comuns de implementação do Balançador de Carga Azure e fornece informações para resolver os erros. Se procura informações sobre um código de erro e essa informação não é fornecida neste artigo, avise-nos. Na parte inferior desta página, pode deixar feedback. O feedback é rastreado com problemas do GitHub.

## <a name="error-codes"></a>Códigos de erro

| Código de erro | Detalhes e mitigação |
| ------- | ---------- |
|DifferentSkuLoadBalancersAndPublicIPAddressNotAllowed| Tanto o SKU DE IP público como o Balancer de Carga SKU devem coincidir. Certifique-se de que o Azure Load Balancer e o Public IP SKUs correspondem. O SKU standard é recomendado para cargas de trabalho de produção. Saiba mais sobre as [diferenças em SKUs](./skus.md)  |
|DifferentSkuLoadBalancerAndPublicIPAddressNotAllowedInVMSS | A balança de máquinas virtuais define o padrão dos Balanceadores de Carga Básicos quando o SKU não é especificado ou implementado sem IPs públicos padrão. Re-implementar a balança de máquinas virtuais definida com IPs públicos padrão nas instâncias individuais para garantir que o Balancer de Carga Padrão é selecionado ou simplesmente selecionar um LB standard ao implementar a balança de máquina virtual definida a partir do portal Azure. |
|MaxAvailabilitySetsInLoadBalancerReached | O pool de backend de um Balancer de Carga pode conter um máximo de 150 conjuntos de disponibilidade. Se não tiver conjuntos de disponibilidade explicitamente definidos para os seus VMs na piscina de backend, cada VM vai para o seu próprio Conjunto de Disponibilidade. Assim, a implantação de 150 VMs autónomos implicaria que teria 150 conjuntos de disponibilidade, atingindo assim o limite. Pode implementar um conjunto de disponibilidade e adicionar-lhe VMs adicionais como uma solução alternativa. |
|NetworkInterfaceAndLoadBalancerAreInDifferentAvailabilitySets | Para o balanceador de carga Basic Sku, a interface de rede e o balançador de carga têm de estar no mesmo conjunto de disponibilidade. |
|RegrasOfSameLoadBalancerTypeUseSameBackendPortProtocolAndIPConfig| Não é possível ter mais do que uma regra sobre um determinado tipo de balançador de carga (interno, público) com a mesma porta de backend e protocolo referenciado pelo mesmo conjunto de balanças de máquinas virtuais. Atualize a sua regra para alterar esta criação de regras duplicadas. |
|RegrasOfSameLoadBalancerTypeUseSameBackendPortProtocolAndVmssIPConfig| Não é possível ter mais do que uma regra sobre um determinado tipo de balançador de carga (interno, público) com a mesma porta de backend e protocolo referenciado pelo mesmo conjunto de balanças de máquinas virtuais. Atualize os parâmetros da sua regra para alterar esta criação de regras duplicadas. |
|OutroInternalLoadBalancerExists| Pode ter apenas um Balanceador de Carga de referência interna do tipo o mesmo conjunto de VMs/interfaces de rede no backend do Balanceador de Carga. Atualize a sua implementação para garantir que está a criar apenas um Balanceador de Carga do mesmo tipo. |
|Não-InuseInactiveHealthProbe| Não é possível ter uma sonda que não seja utilizada por nenhuma regra configurada para a saúde da balança de máquinas virtual. Certifique-se de que a sonda que está configurada está a ser ativamente utilizada. |
|VMScaleSetCannotUseMultipleLoadBalancersOfSameType| Não é possível ter vários Balanceadores de Carga do mesmo tipo (internos, públicos). Pode ter um máximo de um balançador de carga interno e público. |
|VMScaleSetCannotReferenceLoadbalancerWhenLargeScaleOrCrossAZ | O Balancer de Carga Básica não é suportado para conjuntos de escala de máquina virtual de grupo de colocação múltipla ou conjunto de escala de máquina virtual de zona de disponibilidade cruzada. Utilize o Balanceador de Carga Padrão. |
|MarketplacePurchaseEelegibilidadeFailed | Mude para a conta administrativa correta para permitir compras devido à subscrição ser uma Subscrição EA. Pode ler mais [aqui](../marketplace/marketplace-faq-publisher-guide.md#what-could-block-a-customer-from-completing-a-purchase). |
|RecursosDeploymentFailure| Se o balanceador de carga estiver num estado de falha, siga estes passos para corrigir o estado de falha:<ol><li>Vá a https://resources.azure.com , e inscreva-se com as suas credenciais do portal Azure.</li><li>Selecione **Leitura/Escrita**.</li><li>À esquerda, expanda **Subscrições** e, em seguida, expanda a Subscrição com o Balanceador de Carga a atualizar.</li><li>Expanda **ResourceGroups** e, em seguida, expanda o grupo de recursos com o Balanceador de Carga a atualizar.</li><li>Selecione **Microsoft.Network**  >  **LoadBalancers** e, em seguida, selecione o Balanceador de Carga para atualizar, **LoadBalancer_1**.</li><li>Na página de exibição para **LoadBalancer_1**, selecione   >  **GET Edit**.</li><li>Atualize o valor **do ProvisioningState** de **Failed** to **Succeeded**.</li><li>Selecione **PUT**.</li></ol>|
|  |  |

## <a name="next-steps"></a>Passos seguintes

* Veja através da tabela de [comparação SKU](./skus.md) do balanceador de carga Azure
* Saiba mais sobre [os limites do balançador de carga Azure](../azure-resource-manager/management/azure-subscription-service-limits.md#load-balancer)