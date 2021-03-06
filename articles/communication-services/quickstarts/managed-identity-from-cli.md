---
title: Criar um Azure Ative Directory gerido aplicação de identidade a partir do Azure CLI
titleSuffix: An Azure Communication Services quickstart
description: Identidades geridas permitem autorizar o acesso dos Serviços de Comunicação Azure a partir de aplicações em execução em VMs Azure, aplicações de funções e outros recursos. Este quickstart está focado na gestão da identidade utilizando o Azure CLI.
services: azure-communication-services
author: jbeauregardb
ms.service: azure-communication-services
ms.topic: how-to
ms.date: 03/10/2021
ms.author: jbeauregardb
ms.reviewer: mikben
ms.openlocfilehash: a5ee7e8de85a1a53359f651a74e2f9f5e51edb70
ms.sourcegitcommit: d40ffda6ef9463bb75835754cabe84e3da24aab5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/07/2021
ms.locfileid: "107030785"
---
# <a name="authorize-access-with-managed-identity-to-your-communication-resource-in-your-development-environment"></a>Autorizar o acesso com identidade gerida ao seu recurso de comunicação no seu ambiente de desenvolvimento

A Azure Identity SDK fornece suporte de autenticação simbólica Azure Ative (Azure AD) para o Azure SDK. As versões mais recentes dos SDKs de Serviços de Comunicação Azure para .NET, Java, Python e JavaScript integram-se na biblioteca de Identidade Azure para fornecer um meio simples e seguro para adquirir um token OAuth 2.0 para autorização dos pedidos de Serviços de Comunicação Azure.

Uma vantagem do Azure Identity SDK é que permite-lhe usar o mesmo código para autenticar em vários serviços, quer a sua aplicação esteja a decorrer no ambiente de desenvolvimento ou no Azure. A Azure Identity SDK autentica um diretor de segurança. Quando o seu código está em execução em Azure, o diretor de segurança é uma identidade gerida para os recursos da Azure. No ambiente de desenvolvimento, a identidade gerida não existe, pelo que o SDK autentica o utilizador ou uma aplicação registada para efeitos de teste.

## <a name="prerequisites"></a>Pré-requisitos

 - CLI do Azure. [Guia de instalação](/cli/azure/install-azure-cli)
 - Uma conta Azure com uma subscrição ativa. [Criar uma conta gratuita](https://azure.microsoft.com/free)

## <a name="setting-up"></a>Configuração

As identidades geridas devem ser ativadas sobre os recursos do Azure que está a autorizar. Para aprender como permitir identidades geridas para recursos Azure, consulte um destes artigos:

- [Portal do Azure](../../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md)
- [Azure PowerShell](../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md)
- [CLI do Azure](../../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md)
- [Modelo Azure Resource Manager](../../active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm.md)
- [SDKs gestor de recursos Azure](../../active-directory/managed-identities-azure-resources/qs-configure-sdk-windows-vm.md)
- [Serviços aplicacionais](../../app-service/overview-managed-identity.md)

## <a name="authenticate-a-registered-application-in-the-development-environment"></a>Autenticar uma aplicação registada no ambiente de desenvolvimento

Se o seu ambiente de desenvolvimento não suporta um único login ou login através de um navegador web, então pode usar uma aplicação registada para autenticar a partir do ambiente de desenvolvimento.

### <a name="creating-an-azure-active-directory-registered-application"></a>Criação de um Diretório Ativo Azure

Para criar uma aplicação registada a partir do CLI Azure, tem de ser acedido à conta Azure onde pretende que as operações ocorram. Para isso, pode utilizar o `az login` comando e inserir as suas credenciais no browser. Assim que iniciar sessão na sua conta Azure a partir do CLI, podemos ligar para o `az ad sp create-for-rbac` comando para criar a aplicação registada.

Os exemplos a seguir usam o Azure CLI para criar uma nova aplicação registada

```azurecli
az ad sp create-for-rbac --name <application-name> 
```

O `az ad sp create-for-rbac` comando devolverá uma lista de propriedades principais de serviço no formato JSON. Copie estes valores para que possa usá-los para criar as variáveis ambientais necessárias no passo seguinte.

```json
{
    "appId": "generated-app-ID",
    "displayName": "service-principal-name",
    "name": "http://service-principal-uri",
    "password": "generated-password",
    "tenant": "tenant-ID"
}
```
> [!IMPORTANT]
> As atribuições de funções azure podem demorar alguns minutos a propagar-se.

#### <a name="set-environment-variables"></a>Definir variáveis de ambiente

O Azure Identity SDK lê valores de três variáveis ambientais em tempo de execução para autenticar a aplicação. A tabela seguinte descreve o valor a definir para cada variável ambiente.

|Variável de ambiente|Valor
|-|-
|`AZURE_CLIENT_ID`|`appId` valor do JSON gerado 
|`AZURE_TENANT_ID`|`tenant` valor do JSON gerado
|`AZURE_CLIENT_SECRET`|`password` valor do JSON gerado

> [!IMPORTANT]
> Depois de definir as variáveis ambientais, feche e reabriu a janela da consola. Se estiver a utilizar o Visual Studio ou outro ambiente de desenvolvimento, poderá ter de reiniciá-lo para que registe as novas variáveis ambientais.

Uma vez definidas estas variáveis, deverá poder utilizar o objeto DefaultAzureCredential no seu código para autenticar ao cliente de serviço à sua escolha.


## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Saiba mais sobre a autenticação](../concepts/authentication.md)

Também pode querer:

- [Saiba mais sobre a Biblioteca de Identidade Azure](/dotnet/api/overview/azure/identity-readme)