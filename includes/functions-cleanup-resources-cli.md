---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 10/18/2020
ms.author: glenga
ms.openlocfilehash: 0f560e292ecba8a1186a400e4a40b0552c394298
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "93425152"
---
## <a name="clean-up-resources"></a>Limpar recursos

Se continuar até ao [próximo passo](#next-steps) e adicionar uma bagagem de saída da fila Azure Storage, mantenha todos os seus recursos no lugar, pois irá basear-se no que já fez.

Caso contrário, utilize o seguinte comando para eliminar o grupo de recursos e todos os seus recursos contidos para evitar custos adicionais.

 # <a name="azure-cli"></a>[CLI do Azure](#tab/azure-cli)

```azurecli
az group delete --name AzureFunctionsQuickstart-rg
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell
Remove-AzResourceGroup -Name AzureFunctionsQuickstart-rg
```

---