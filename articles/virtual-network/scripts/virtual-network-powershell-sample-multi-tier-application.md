---
title: Criar um VNet para aplicações multi-camadas - amostra de script Azure PowerShell
description: Crie uma rede virtual para aplicações multi-camadas - amostra de script Azure PowerShell.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: twooley
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: powershell
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 12/13/2018
ms.author: kumud
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 51cafb0a344c5b6226b3b033d8c60327eaf2c05f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "91267963"
---
# <a name="create-a-network-for-multi-tier-applications-script-sample"></a>Criar uma rede para o exemplo de script de aplicações de várias camadas

Este script de exemplo cria uma rede virtual com as sub-redes de front-end e back-end. O tráfego para a sub-rede do front-end está limitado a HTTP e SSH, enquanto o tráfego para a sub-rede de back-end está limitado a MySQL, porta 3306. Depois de executar o script, tem duas máquinas virtuais, uma em cada sub-rede nas quais pode implementar software MySQL e o servidor Web.

Pode executar o script a partir do [Azure Cloud Shell](https://shell.azure.com/powershell) ou a partir de uma instalação local do PowerShell. Se utilizar o PowerShell localmente, este script requer a versão 1.0 ou mais tarde do módulo Azure PowerShell. Para localizar a versão instalada, execute `Get-Module -ListAvailable Az`. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-az-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Connect-AzAccount` para criar uma ligação com o Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

<!-- gitHub issue https://github.com/MicrosoftDocs/azure-docs/issues/17748 -->
Um ID de sub-rede é atribuído depois de ter criado uma rede virtual; especificamente, utilizando o New-AzVirtualNetwork cmdlet com a opção Sub-rede. Se configurar a sub-rede utilizando a New-AzVirtualNetworkSubnetConfig cmdlet antes da chamada para a New-AzVirtualNetwork, só verá o ID da sub-rede depois de ligar para a New-AzVirtualNetwork.

[!code-azurepowershell-interactive[main](../../../powershell_scripts/virtual-network/virtual-network-multi-tier-application/virtual-network-multi-tier-application.ps1  "Virtual network for multi-tier application")]

## <a name="clean-up-deployment"></a>Limpar a implementação

Execute o seguinte comando para remover o grupo de recursos, a VM e todos os recursos relacionados:

```powershell
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos para criar um grupo de recursos, uma rede virtual e grupos de segurança de rede. Cada comando na tabela seguinte liga à documentação específica do comando:

| Comando | Notas |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) | Cria uma rede e sub-rede virtual de front-end do Azure. |
| [New-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) | Cria uma sub-rede de back-end. |
| [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) | Cria um endereço IP público para aceder à VM a partir da Internet. |
| [Novo AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface) | Cria interfaces de rede virtual e anexa-as a sub-redes de front-end e back-end da rede virtual. |
| [Grupo deSegurança New-AzNetwork](/powershell/module/az.network/new-aznetworksecuritygroup) | Cria grupos de segurança (NSG) que estão associados às sub-redes de front-end e back-end. |
| [New-AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig) |Cria regras do NSG que permitem ou bloquear portas específicas para sub-redes específicas. |
| [New-AzVM](/powershell/module/az.compute/new-azvm) | Cria máquinas virtuais e anexa um NIC para cada VM. Este comando também especifica a imagem da máquina virtual a utilizar e as credenciais administrativas. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Elimina um grupo de recursos e todos os recursos contidos no mesmo. |

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre o Azure PowerShell, veja [Documentação do Azure PowerShell](/powershell/azure/).

Pode ver exemplos adicionais de scripts do PowerShell de rede virtual em [Exemplos do PowerShell de rede virtual](../powershell-samples.md).
