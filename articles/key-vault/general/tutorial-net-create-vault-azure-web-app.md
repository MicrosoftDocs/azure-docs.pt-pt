---
title: Tutorial - Use Azure Key Vault com uma aplicação web Azure em .NET
description: Neste tutorial, você vai configurar uma aplicação web Azure em uma aplicação ASP.NET Core para ler um segredo a partir do seu cofre chave.
services: key-vault
author: msmbaldwin
manager: rajvijan
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 05/06/2020
ms.author: mbaldwin
ms.custom: devx-track-csharp, devx-track-azurecli
ms.openlocfilehash: 91467d1de3a1543736115ef7a25281733c8eb85d
ms.sourcegitcommit: 260a2541e5e0e7327a445e1ee1be3ad20122b37e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/21/2021
ms.locfileid: "107819219"
---
# <a name="tutorial-use-a-managed-identity-to-connect-key-vault-to-an-azure-web-app-in-net"></a>Tutorial: Use uma identidade gerida para ligar o Key Vault a uma aplicação web Azure em .NET

[Azure Key Vault](./overview.md) fornece uma forma de armazenar credenciais e outros segredos com segurança aumentada. Mas o seu código precisa de autenticar no Cofre de Chaves para os recuperar. [Identidades geridas para recursos Azure](../../active-directory/managed-identities-azure-resources/overview.md) ajudam a resolver este problema, dando aos serviços Azure uma identidade gerida automaticamente no Azure Ative Directory (Azure AD). Pode utilizar esta identidade para autenticar qualquer serviço que suporte a autenticação AZure AD, incluindo o Key Vault, sem ter de apresentar credenciais no seu código.

Neste tutorial, irá criar e implementar a aplicação web Azure para o [Azure App Service.](../../app-service/overview.md) Você usará uma identidade gerida para autenticar a sua aplicação web Azure com um cofre de chaves Azure usando [a biblioteca de clientes secretos Azure Key Vault para .NET](/dotnet/api/overview/azure/key-vault) e o [Azure CLI](/cli/azure/get-started-with-azure-cli). Os mesmos princípios básicos aplicam-se quando utiliza a linguagem de desenvolvimento à sua escolha, Azure PowerShell, e/ou o portal Azure.

Para obter mais informações sobre aplicações web de serviço da Azure App e implementação apresentadas neste tutorial, consulte:
- [Descrição geral do Serviço de Aplicações](../../app-service/overview.md)
- [Crie uma aplicação web core ASP.NET no Azure App Service](../../app-service/quickstart-dotnetcore.md)
- [Implantação local de Git para o Serviço de Aplicações Azure](../../app-service/deploy-local-git.md)

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, precisa de:

* Uma subscrição do Azure. [Crie um de graça.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* O [Núcleo .NET 3.1 SDK (ou mais tarde)](https://dotnet.microsoft.com/download/dotnet-core/3.1).
* Uma instalação [Git](https://www.git-scm.com/downloads) da versão 2.28.0 ou superior.
* [A Azure CLI](/cli/azure/install-azure-cli) ou [Azure PowerShell](/powershell/azure/).
* [Cofre de Chaves Azure.](./overview.md) Pode criar um cofre-chave utilizando o [portal Azure,](quick-create-portal.md)o [Azure CLI](quick-create-cli.md)ou [a Azure PowerShell](quick-create-powershell.md).
* Um segredo do Cofre [chave.](../secrets/about-secrets.md) Pode criar um segredo utilizando o [portal Azure,](../secrets/quick-create-portal.md) [PowerShell](../secrets/quick-create-powershell.md)ou [o Azure CLI](../secrets/quick-create-cli.md).

Se já tiver a sua aplicação web implantada no Azure App Service, pode saltar para [configurar o acesso da aplicação web a um cofre de chaves](#create-and-assign-a-managed-identity) e modificar secções de código de [aplicação web.](#modify-the-app-to-access-your-key-vault)

## <a name="create-a-net-core-app"></a>Criar uma aplicação .NET Core
Neste passo, irá configurar o projeto local .NET Core.

Numa janela terminal da sua máquina, crie um diretório nomeado `akvwebapp` e faça dele o diretório atual:

```bash
mkdir akvwebapp
cd akvwebapp
```

Crie uma aplicação .NET Core utilizando o novo comando [web do dotnet:](/dotnet/core/tools/dotnet-new)

```bash
dotnet new web
```

Execute a aplicação localmente para que saiba como deve ser quando a implementar para Azure:

```bash
dotnet run
```

Num navegador web, vá à aplicação em `http://localhost:5000` .

Será apresentada a mensagem "Olá, Mundo!" mensagem da aplicação da amostra exibida na página.

Para obter mais informações sobre a criação de aplicações web para o Azure, consulte [Criar uma aplicação web Core ASP.NET no Azure App Service](../../app-service/quickstart-dotnetcore.md)

## <a name="deploy-the-app-to-azure"></a>Implementar a aplicação no Azure

Neste passo, irá implementar a sua aplicação .NET Core para o Azure App Service utilizando o Git local. Para obter mais informações sobre como criar e implementar aplicações, consulte [Criar uma aplicação web Core ASP.NET em Azure](../../app-service/quickstart-dotnetcore.md).

### <a name="configure-the-local-git-deployment"></a>Configure a implantação local de Git

Na janela do terminal, selecione **Ctrl+C** para fechar o servidor web.  Inicializar um repositório git para o projeto .NET Core:

```bash
git init --initial-branch=main
git add .
git commit -m "first commit"
```

Pode utilizar a FTP e o Git local para implementar uma aplicação web Azure utilizando um *utilizador de implementação.* Depois de configurar o utilizador de implementação, pode usá-lo para todas as suas implementações Azure. O nome de utilizador e palavra-passe de implementação ao nível da sua conta são diferentes das suas credenciais de subscrição Azure. 

Para configurar o utilizador de implementação, executar o comando [de conjunto de utilizadores de implementação az webapp.](/cli/azure/webapp/deployment/user?#az_webapp_deployment_user_set) Escolha um nome de utilizador e uma palavra-passe que adere a estas diretrizes: 

- O nome do utilizador tem de ser exclusivo no Azure. Para os pushes de Git locais, não pode conter o símbolo do sinal ao sinal (@). 
- A palavra-passe deve ter pelo menos oito caracteres de comprimento e conter dois dos seguintes três elementos: letras, números e símbolos. 

```azurecli-interactive
az webapp deployment user set --user-name "<username>" --password "<password>"
```

A saída JSON mostra a palavra-passe como `null` . Se tiver um `'Conflict'. Details: 409` erro, altere o nome de utilizador. Se obtiver o `'Bad Request'. Details: 400` erro, utilize uma palavra-passe mais forte. 

Grave o seu nome de utilizador e palavra-passe para que possa usá-lo para implementar as suas aplicações web.

### <a name="create-a-resource-group"></a>Criar um grupo de recursos

Um grupo de recursos é um recipiente lógico no qual você implanta recursos Azure e os gere. Crie um grupo de recursos para conter tanto o seu cofre-chave como a sua aplicação web utilizando o comando [de criação do grupo AZ:](/cli/azure/group?#az_group_create)

```azurecli-interactive
az group create --name "myResourceGroup" -l "EastUS"
```

### <a name="create-an-app-service-plan"></a>Crie um plano do Serviço de Aplicações

Crie um [plano de Serviço de Aplicações](../../app-service/overview-hosting-plans.md) utilizando o plano de serviço de aplicações Azure CLI [az criar](/cli/azure/appservice/plan) comando. Este exemplo a seguir cria um plano de Serviço de Aplicações nomeado `myAppServicePlan` no `FREE` nível de preços:

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE
```

Quando o plano de Serviço de Aplicações é criado, o Azure CLI exibe informações semelhantes às que vê aqui:

<pre>
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "West Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "app",
  "location": "West Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  &lt; JSON data removed for brevity. &gt;
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
} 
</pre>

Para obter mais informações, veja [Gerir um plano do Serviço de Aplicações no Azure](../../app-service/app-service-plan-manage.md).

### <a name="create-a-web-app"></a>Criar uma aplicação Web

Crie uma [aplicação web Azure](../../app-service/overview.md) no `myAppServicePlan` plano de Serviço de Aplicações. 

> [!Important]
> Como um cofre chave, uma aplicação web Azure deve ter um nome único. `<your-webapp-name>`Substitua-a pelo nome da sua aplicação web nos seguintes exemplos.


```azurecli-interactive
az webapp create --resource-group "myResourceGroup" --plan "myAppServicePlan" --name "<your-webapp-name>" --deployment-local-git
```

Quando a aplicação web é criada, o Azure CLI mostra uma saída semelhante à que vê aqui:

<pre>
Local git is configured with url of 'https://&lt;username&gt;@&lt;your-webapp-name&gt;.scm.azurewebsites.net/&lt;ayour-webapp-name&gt;.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "clientCertExclusionPaths": null,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "&lt;your-webapp-name&gt;.azurewebsites.net",
  "deploymentLocalGitUrl": "https://&lt;username&gt;@&lt;your-webapp-name&gt;.scm.azurewebsites.net/&lt;your-webapp-name&gt;.git",
  "enabled": true,
  &lt; JSON data removed for brevity. &gt;
}
</pre>

O URL do comando Git é mostrado na `deploymentLocalGitUrl` propriedade, no formato `https://<username>@<your-webapp-name>.scm.azurewebsites.net/<your-webapp-name>.git` . Salve esta URL. Precisará dela mais tarde.

Agora configure a sua aplicação web para implementar a partir da `main` sucursal:

```azurecli-interactive
 az webapp config appsettings set -g MyResourceGroup -name "<your-webapp-name>"--settings deployment_branch=main
```

Vá à sua nova aplicação utilizando o seguinte comando. `<your-webapp-name>`Substitua-o pelo nome da sua aplicação.

```bash
https://<your-webapp-name>.azurewebsites.net
```

Verá a página inicial padrão de uma nova aplicação web Azure.

### <a name="deploy-your-local-app"></a>Implemente a sua aplicação local

Regresse à janela de terminal local e adicione um remoto do Azure ao seu repositório Git local. No comando seguinte, `<deploymentLocalGitUrl-from-create-step>` substitua-o pelo URL do comando Git que guardou na secção [Criar uma aplicação web.](#create-a-web-app)

```bash
git remote add azure <deploymentLocalGitUrl-from-create-step>
```

Utilize o seguinte comando para empurrar para o comando Azure para implementar a sua aplicação. Quando git Credencial Manager lhe pedir credenciais, use as credenciais que criou na secção [de configuração do Git local.](#configure-the-local-git-deployment)

```bash
git push azure main
```

Este comando pode levar alguns minutos a correr. Enquanto funciona, apresenta informações semelhantes às que vê aqui:
<pre>
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 285 bytes | 95.00 KiB/s, done.
Total 3 (delta 2), reused 0 (delta 0), pack-reused 0
remote: Deploy Async
remote: Updating branch 'main'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'd6b54472f7'.
remote: Repository path is /home/site/repository
remote: Running oryx build...
remote: Build orchestrated by Microsoft Oryx, https://github.com/Microsoft/Oryx
remote: You can report issues at https://github.com/Microsoft/Oryx/issues
remote:
remote: Oryx Version      : 0.2.20200114.13, Commit: 204922f30f8e8d41f5241b8c218425ef89106d1d, ReleaseTagName: 20200114.13
remote: Build Operation ID: |imoMY2y77/s=.40ca2a87_
remote: Repository Commit : d6b54472f7e8e9fd885ffafaa64522e74cf370e1
.
.
.
remote: Deployment successful.
remote: Deployment Logs : 'https://&lt;your-webapp-name&gt;.scm.azurewebsites.net/newui/jsonviewer?view_url=/api/deployments/d6b54472f7e8e9fd885ffafaa64522e74cf370e1/log'
To https://&lt;your-webapp-name&gt;.scm.azurewebsites.net:443/&lt;your-webapp-name&gt;.git
   d87e6ca..d6b5447  main -> main
</pre>

Aceda (ou atualize) a aplicação implementada utilizando o seu navegador web:

```bash
http://<your-webapp-name>.azurewebsites.net
```

Será apresentada a mensagem "Olá, Mundo!" mensagem que viu antes quando `http://localhost:5000` visitou.

Para obter mais informações sobre a implementação de aplicações web usando o Git, consulte [a implementação do Git Local para o Azure App Service](../../app-service/deploy-local-git.md)
 
## <a name="configure-the-web-app-to-connect-to-key-vault"></a>Configure a aplicação web para ligar ao Key Vault

Nesta secção, irá configurar o acesso à Web para Key Vault e atualizar o seu código de aplicação para recuperar um segredo do Key Vault.

### <a name="create-and-assign-a-managed-identity"></a>Criar e atribuir uma identidade gerida

Neste tutorial, usaremos [identidade gerida](../../active-directory/managed-identities-azure-resources/overview.md) para autenticar o Key Vault. A identidade gerida gere automaticamente as credenciais de aplicação.

No CLI Azure, para criar a identidade para a aplicação, executar o comando [de atribuição de identidade az webapp:](/cli/azure/webapp/identity?#az_webapp_identity_assign)

```azurecli-interactive
az webapp identity assign --name "<your-webapp-name>" --resource-group "myResourceGroup"
```

O comando devolverá este corte JSON:

```json
{
  "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "type": "SystemAssigned"
}
```

Para dar permissão à sua aplicação web para **fazer** e **listar** operações no seu cofre chave, passe o `principalId` comando de definição de chave Azure CLI [az:](/cli/azure/keyvault?#az_keyvault_set_policy)

```azurecli-interactive
az keyvault set-policy --name "<your-keyvault-name>" --object-id "<principalId>" --secret-permissions get list
```

Também pode atribuir políticas de acesso utilizando o [portal Azure](./assign-access-policy-portal.md) ou [PowerShell](./assign-access-policy-powershell.md).

### <a name="modify-the-app-to-access-your-key-vault"></a>Modifique a app para aceder ao cofre da chave

Neste tutorial, você usará a biblioteca secreta do [Azure Key Vault](/dotnet/api/overview/azure/security.keyvault.secrets-readme) para fins de demonstração. Também pode utilizar [a biblioteca de clientes de certificados Azure Key Vault,](/dotnet/api/overview/azure/security.keyvault.certificates-readme)ou [biblioteca de clientes chave Azure Key Vault](/dotnet/api/overview/azure/security.keyvault.keys-readme).

#### <a name="install-the-packages"></a>Instalar as embalagens

A partir da janela do terminal, instale a biblioteca secreta do Azure Key Vault para pacotes de biblioteca de clientes .NET e Azure Identity:

```console
dotnet add package Azure.Identity
dotnet add package Azure.Security.KeyVault.Secrets
```

#### <a name="update-the-code"></a>Atualizar o código

Encontre e abra o ficheiro .cs Startup no seu projeto akvwebapp. 

Adicione estas linhas ao cabeçalho:

```csharp
using Azure.Identity;
using Azure.Security.KeyVault.Secrets;
using Azure.Core;
```

Adicione as seguintes linhas antes da `app.UseEndpoints` chamada, atualizando o URI para refletir o `vaultUri` cofre da chave. Este código utiliza  [o DefaultAzureCredential()](/dotnet/api/azure.identity.defaultazurecredential) para autenticar o Key Vault, que utiliza um símbolo da identidade gerida para autenticar. Para obter mais informações sobre a autenticação no Cofre de Chaves, consulte o [Guia do Desenvolvedor.](./developers-guide.md#authenticate-to-key-vault-in-code) O código também usa recuo exponencial para retretes no caso de o Cofre de Chaves estar a ser estrangulado. Para obter mais informações sobre os limites de transação do Key Vault, consulte a orientação de [estrangulamento do Azure Key Vault](./overview-throttling.md).

```csharp
SecretClientOptions options = new SecretClientOptions()
    {
        Retry =
        {
            Delay= TimeSpan.FromSeconds(2),
            MaxDelay = TimeSpan.FromSeconds(16),
            MaxRetries = 5,
            Mode = RetryMode.Exponential
         }
    };
var client = new SecretClient(new Uri("https://<your-unique-key-vault-name>.vault.azure.net/"), new DefaultAzureCredential(),options);

KeyVaultSecret secret = client.GetSecret("<mySecret>");

string secretValue = secret.Value;
```

Atualize a linha `await context.Response.WriteAsync("Hello World!");` para se parecer com esta linha:

```csharp
await context.Response.WriteAsync(secretValue);
```

Certifique-se de que guarda as suas alterações antes de continuar para o próximo passo.

#### <a name="redeploy-your-web-app"></a>Reimplementar a aplicação Web

Agora que atualizou o seu código, pode reimplantá-lo para Azure utilizando estes comandos Git:

```bash
git add .
git commit -m "Updated web app to access my key vault"
git push azure main
```

## <a name="go-to-your-completed-web-app"></a>Vá para a sua aplicação web completa

```bash
http://<your-webapp-name>.azurewebsites.net
```

Onde antes de ver "Olá Mundo!", deve agora ver o valor do seu segredo exibido.

## <a name="next-steps"></a>Passos seguintes

- [Utilize o Cofre de Chaves Azure com aplicações implantadas numa máquina virtual em .NET](./tutorial-net-virtual-machine.md)
- Saiba mais sobre [identidades geridas para recursos Azure](../../active-directory/managed-identities-azure-resources/overview.md)
- Ver o [Guia do Desenvolvedor](./developers-guide.md)
- [Acesso seguro a um cofre de chaves](./security-features.md)