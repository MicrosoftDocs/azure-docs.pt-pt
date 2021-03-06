---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 04/21/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: f8095fc7c9be6ea23a573de8a5e871965d6c2a66
ms.sourcegitcommit: 2aeb2c41fd22a02552ff871479124b567fa4463c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/22/2021
ms.locfileid: "107866039"
---
|Nome |Descrição |Políticas |Versão |
|---|---|---|---|
|[\[Pré-visualização \] : Implementar - Configurar pré-requisitos para permitir agentes de segurança do Azure Monitor e da Azure em máquinas virtuais](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Monitoring/AzureMonitoring_Prerequisites.json) |Configure as máquinas para instalar automaticamente os agentes Azure Monitor e Azure Security. O Security Center recolhe eventos dos agentes e utiliza-os para fornecer alertas de segurança e tarefas de endurecimento personalizadas (recomendações). Crie um grupo de recursos e log Analytics espaço de trabalho na mesma região que a máquina para armazenar registos de auditoria. Esta política aplica-se apenas aos VM em algumas regiões. |5 |1.0.0-pré-visualização |
|[Ativar monitor de Azure para conjuntos de balanças de máquinas virtuais](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Monitoring/AzureMonitor_VMSS.json) |Ativar o Monitor Azure para os conjuntos de balanças de máquinas virtuais no âmbito especificado (Grupo de gestão, subscrição ou grupo de recursos). Toma o espaço de trabalho log Analytics como parâmetro. Nota: se a sua atualização de escala definir A política está definida como Manual, tem de aplicar a extensão a todos os VMs do conjunto, chamando-lhes a atualização. No CLI, esta seria az vmss atualização-instâncias. |6 |1.0.1 |
|[Ativar monitor Azure para VMs](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Monitoring/AzureMonitor_VM.json) |Ativar o Azure Monitor para as máquinas virtuais (VMs) no âmbito especificado (grupo de gestão, subscrição ou grupo de recursos). Toma o espaço de trabalho log Analytics como parâmetro. |10 |2.0.0 |
