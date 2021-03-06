---
title: Verifique a utilização do recurso Azure contra os limites | Microsoft Docs
description: Saiba como verificar o uso do seu recurso Azure contra os limites de subscrição do Azure.
services: networking
documentationcenter: na
author: KumudD
ms.author: kumud
tags: azure-resource-manager
ms.service: azure
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/05/2018
ms.openlocfilehash: d629e65106145a4af364cd9dd489250c8910c25d
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/20/2021
ms.locfileid: "107778576"
---
# <a name="check-resource-usage-against-limits"></a>Verificar a utilização de recursos em relação aos limites

Neste artigo, aprende a ver o número de cada tipo de recurso de rede que implementou na sua subscrição e quais são os [seus limites de subscrição.](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fnetworking%2ftoc.json#networking-limits) A capacidade de visualizar o uso de recursos contra limites é útil para acompanhar o uso atual e planear a utilização futura. Pode utilizar o [Portal Azure,](#azure-portal) [o PowerShell](#powershell)ou o [Azure CLI](#azure-cli) para acompanhar a utilização.

## <a name="azure-portal"></a>Portal do Azure

1. Inicie sessão no [portal](https://portal.azure.com)Azure .
2. No topo, canto esquerdo do portal Azure, selecione **Todos os serviços**.
3. Introduza *subscrições* na caixa **filtro.** Quando a opção **Subscrições** aparecer nos resultados da pesquisa, selecione-a.
4. Selecione o nome da subscrição para a quais pretende visualizar as informações de utilização.
5. Em **DEFINIÇÕES,** selecione **Utilização + quota**.
6. Pode selecionar as seguintes opções:
   - **Tipos de recursos**: Pode selecionar todos os tipos de recursos ou selecionar os tipos específicos de recursos que pretende visualizar.
   - **Fornecedores**: Pode selecionar todos os fornecedores de recursos ou selecionar **Computação,** **Rede** ou **Armazenamento.**
   - **Localizações**: Pode selecionar todas as localizações do Azure ou selecionar localizações específicas.
   - Pode selecionar para mostrar todos os recursos, ou apenas os recursos onde pelo menos um é implantado.

     O exemplo na seguinte imagem mostra todos os recursos da rede com pelo menos um recurso implantado nos EUA Orientais:

       ![Ver dados de utilização](./media/check-usage-against-limits/view-usage.png)

     Pode separar as colunas selecionando o título da coluna. Os limites apresentados são os limites para a sua subscrição. Se precisar de aumentar um limite por defeito, selecione **Request Increase**, em seguida, complete e envie o pedido de apoio. Todos os recursos têm um limite máximo listado nos [limites](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fnetworking%2ftoc.json#networking-limits)de Azure. Se o seu limite atual já estiver no número máximo, o limite não pode ser aumentado.

## <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Pode executar os comandos que seguem na [Azure Cloud Shell,](https://shell.azure.com/powershell)ou executando o PowerShell a partir do seu computador. O Azure Cloud Shell é uma concha interativa gratuita. Tem as ferramentas comuns do Azure pré-instaladas e configuradas para utilização com a sua conta. Se executar o PowerShell a partir do computador, precisa do módulo Azure PowerShell, versão 1.0.0 ou posterior. Corra `Get-Module -ListAvailable Az` no seu computador, para encontrar a versão instalada. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-az-ps). Se estiver a executar o PowerShell localmente, também precisa de correr `Login-AzAccount` para iniciar sessão no Azure.

Veja a sua utilização contra limites com [a Get-AzNetworkUsage](/powershell/module/az.network/get-aznetworkusage). O exemplo a seguir obtém a utilização de recursos onde pelo menos um recurso é implantado na localização dos EUA Orientais:

```azurepowershell-interactive
Get-AzNetworkUsage `
  -Location eastus `
  | Where-Object {$_.CurrentValue -gt 0} `
  | Format-Table ResourceType, CurrentValue, Limit
```

Recebe uma saída formatada da mesma forma que a seguinte saída de exemplo:

```output
ResourceType            CurrentValue Limit
------------            ------------ -----
Virtual Networks                   1    50
Network Security Groups            2   100
Public IP Addresses                1    60
Network Interfaces                 1 24000
Network Watchers                   1     1
```

## <a name="azure-cli"></a>CLI do Azure

Se utilizar os comandos da interface da linha de comando do Azure (CLI) para completar as tarefas neste artigo, ou executar os comandos na [Azure Cloud Shell](https://shell.azure.com/bash), ou executando o CLI a partir do seu computador. Este artigo requer a versão Azure CLI 2.0.32 ou posterior. Execute `az --version` para localizar a versão instalada. Se precisar de instalar ou atualizar, veja [Instalar a CLI do Azure](/cli/azure/install-azure-cli). Se estiver a executar o Azure CLI localmente, também precisa correr `az login` para entrar em Azure.

Ver a sua utilização contra limites com [as listas de az.](/cli/azure/network#az_network_list_usages) O exemplo a seguir obtém a utilização de recursos na localização dos EUA Orientais:

```azurecli-interactive
az network list-usages \
  --location eastus \
  --out table
```

Recebe uma saída formatada da mesma forma que a seguinte saída de exemplo:

```output
Name                    CurrentValue Limit
------------            ------------ -----
Virtual Networks                   1    50
Network Security Groups            2   100
Public IP Addresses                1    60
Network Interfaces                 1 24000
Network Watchers                   1     1
Load Balancers                     0   100
Application Gateways               0    50
```