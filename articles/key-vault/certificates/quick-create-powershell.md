---
title: Quickstart - Definir & ver certificados Azure Key Vault com Azure PowerShell
description: Quickstart mostrando como definir e recuperar um certificado do Azure Key Vault usando Azure PowerShell
services: key-vault
author: msmbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: certificates
ms.topic: quickstart
ms.custom: mvc, seo-javascript-september2019, seo-javascript-october2019, devx-track-azurepowershell
ms.date: 01/27/2021
ms.author: mbaldwin
ms.openlocfilehash: 32150fb4cce76d5a2537c5ec969f265e0d8aae20
ms.sourcegitcommit: 260a2541e5e0e7327a445e1ee1be3ad20122b37e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/21/2021
ms.locfileid: "107816193"
---
# <a name="quickstart-set-and-retrieve-a-certificate-from-azure-key-vault-using-azure-powershell"></a>Quickstart: set and recuperar um certificado do Azure Key Vault usando Azure PowerShell

Neste arranque rápido, cria-se um cofre chave no Cofre da Chave Azure com o Azure PowerShell. O Azure Key Vault é um serviço cloud que funciona como um arquivo de segredos seguro. Pode armazenar chaves, palavras-passe, certificados e outros segredos em segurança. Para mais informações sobre o Key Vault, poderá rever a [Visão Geral.](../general/overview.md) O Azure PowerShell é utilizado para criar e gerir recursos Azure usando comandos ou scripts. Uma vez que tenha concluído isso, irá guardar um certificado.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar o PowerShell localmente, este tutorial requer a versão 1.0.0 ou posterior do módulo Azure PowerShell. Escreva `$PSVersionTable.PSVersion` para encontrar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-az-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Login-AzAccount` para criar uma ligação com o Azure.

```azurepowershell-interactive
Login-AzAccount
```

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

[!INCLUDE [Create a resource group](../../../includes/key-vault-powershell-rg-creation.md)]

## <a name="create-a-key-vault"></a>Criar um cofre de chaves

[!INCLUDE [Create a key vault](../../../includes/key-vault-powershell-kv-creation.md)]

## <a name="add-a-certificate-to-key-vault"></a>Adicione um certificado ao Cofre de Chaves

Para adicionar um certificado ao cofre, só precisa dar alguns passos adicionais. Este certificado pode ser utilizado por um pedido. 

Digite os comandos abaixo para criar um certificado auto-assinado com uma política chamada **ExemploCertificate** :

```azurepowershell-interactive
$Policy = New-AzKeyVaultCertificatePolicy -SecretContentType "application/x-pkcs12" -SubjectName "CN=contoso.com" -IssuerName "Self" -ValidityInMonths 6 -ReuseKeyOnRenewal

Add-AzKeyVaultCertificate -VaultName "<your-unique-keyvault-name>" -Name "ExampleCertificate" -CertificatePolicy $Policy
```

Pode agora fazer referência a este certificado que adicionou ao Azure Key Vault utilizando o seu URI. Utilize **"https://<o seu nome de keyvault>.vault.azure.net/certificates/ExampleCertificate"** para obter a versão atual. 

Para ver o certificado previamente armazenado:

```azurepowershell-interactive
Get-AzKeyVaultCertificate -VaultName "<your-unique-keyvault-name>" -Name "ExampleCertificate"
```

Criaste um Cofre-Chave, armazenaste um certificado e recuperaste-o.

**Resolução de problemas:**

Operação devolveu um código de estado inválido 'Proibido'

Se receber este erro, a conta que acede ao Cofre da Chave Azure não tem as permissões adequadas para criar certificados.

Executar o seguinte comando Azure PowerShell para atribuir as permissões adequadas:

```azurepowershell-interactive
Set-AzKeyVaultAccessPolicy -VaultName <KeyVaultName> -ObjectId <AzureObjectID> -PermissionsToCertificates get,list,update,create
```

## <a name="clean-up-resources"></a>Limpar os recursos

[!INCLUDE [Create a key vault](../../../includes/key-vault-powershell-delete-resources.md)]

## <a name="next-steps"></a>Passos seguintes

Neste quickstart criou um Cofre-Chave e guardou um certificado nele. Para saber mais sobre o Key Vault e como integrá-lo com as suas aplicações, continue para os artigos abaixo.

- Leia uma [visão geral do cofre da chave Azure](../general/overview.md)
- Consulte a referência para os [cmdlets Azure PowerShell Key Vault](/powershell/module/az.keyvault/)
- Reveja a visão geral da [segurança do Cofre-Chave](../general/security-features.md)
