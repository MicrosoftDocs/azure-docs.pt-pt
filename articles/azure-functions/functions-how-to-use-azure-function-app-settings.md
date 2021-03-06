---
title: Configurar definições de aplicativos de função em Funções Azure
description: Saiba como configurar as definições de aplicações de função em Funções Azure.
ms.assetid: 81eb04f8-9a27-45bb-bf24-9ab6c30d205c
ms.topic: conceptual
ms.date: 04/13/2020
ms.custom: cc996988-fb4f-47, devx-track-azurecli, devx-track-azurepowershell
ms.openlocfilehash: 6775fdf8d5174600344f3c7177a3130ef63e8f76
ms.sourcegitcommit: 3c460886f53a84ae104d8a09d94acb3444a23cdc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/21/2021
ms.locfileid: "107832685"
---
# <a name="manage-your-function-app"></a>Gerir a sua aplicação de função 

Em Funções Azure, uma aplicação de função fornece o contexto de execução para as suas funções individuais. Os comportamentos da aplicação de funções aplicam-se a todas as funções hospedadas por uma determinada aplicação de função. Todas as funções numa aplicação de função devem ser do mesmo [idioma](supported-languages.md). 

As funções individuais numa aplicação de função são implementadas em conjunto e são dimensionadas em conjunto. Todas as funções na mesma função a aplicação partilham recursos, por exemplo, à medida que a tabela de aplicações de função. 

As cadeias de ligação, as variáveis ambientais e outras configurações de aplicação são definidas separadamente para cada aplicação de função. Quaisquer dados que devam ser partilhados entre aplicações de função devem ser armazenados externamente numa loja persistiu.

## <a name="get-started-in-the-azure-portal"></a>Começar a utilizar o portal do Azure

1. Para começar, vá ao [portal Azure] e inscreva-se na sua conta Azure. Na barra de pesquisa no topo do portal, insira o nome da sua aplicação de função e selecione-a da lista. 

2. Em **Definições** no painel esquerdo, **selecione Configuração**.

    :::image type="content" source="./media/functions-how-to-use-azure-function-app-settings/azure-function-app-main.png" alt-text="Visão geral da aplicação de função no portal Azure":::

Pode navegar para tudo o que necessita para gerir a sua aplicação de função a partir da página de visão geral, em particular as **[definições](#settings)** de Aplicação e as **[funcionalidades da Plataforma.](#platform-features)**

## <a name="work-with-application-settings"></a><a name="settings"></a>Trabalhar com configurações de aplicações

As definições de aplicação podem ser geridas a partir do [portal Azure](functions-how-to-use-azure-function-app-settings.md?tabs=portal#settings) e utilizando o [Azure CLI](functions-how-to-use-azure-function-app-settings.md?tabs=azurecli#settings) e [o Azure PowerShell](functions-how-to-use-azure-function-app-settings.md?tabs=powershell#settings). Também pode gerir as definições de aplicações a partir do [Código do Estúdio Visual](functions-develop-vs-code.md#application-settings-in-azure) e do Visual [Studio](functions-develop-vs.md#function-app-settings). 

Estas definições são armazenadas encriptadas. Para saber mais, consulte [a segurança das definições de aplicação](security-concepts.md#application-settings).

# <a name="portal"></a>[Portal](#tab/portal)

Para encontrar as definições da aplicação, consulte [Começar a trabalhar no portal Azure](#get-started-in-the-azure-portal). 

O **separador de definições de aplicação** mantém as definições que são utilizadas pela sua aplicação de função. Tem de selecionar **valores de 'Mostrar'** para ver os valores no portal. Para adicionar uma definição no portal, selecione **nova definição** de aplicação e adicione o novo par de valor-chave.

![Definições de aplicações de função no portal Azure.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-settings-tab.png)

# <a name="azure-cli"></a>[CLI do Azure](#tab/azurecli)

O [`az functionapp config appsettings list`](/cli/azure/functionapp/config/appsettings#az_functionapp_config_appsettings_list) comando devolve as definições de aplicação existentes, como no exemplo seguinte:

```azurecli-interactive
az functionapp config appsettings list --name <FUNCTION_APP_NAME> \
--resource-group <RESOURCE_GROUP_NAME>
```

O [`az functionapp config appsettings set`](/cli/azure/functionapp/config/appsettings#az_functionapp_config_appsettings_set) comando adiciona ou atualiza uma definição de aplicação. O exemplo a seguir cria uma definição com uma chave nomeada `CUSTOM_FUNCTION_APP_SETTING` e um valor `12345` de:


```azurecli-interactive
az functionapp config appsettings set --name <FUNCTION_APP_NAME> \
--resource-group <RESOURCE_GROUP_NAME> \
--settings CUSTOM_FUNCTION_APP_SETTING=12345
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/powershell)

O [`Get-AzFunctionAppSetting`](/powershell/module/az.functions/get-azfunctionappsetting) cmdlet devolve as definições de aplicação existentes, como no exemplo seguinte: 

```azurepowershell-interactive
Get-AzFunctionAppSetting -Name <FUNCTION_APP_NAME> -ResourceGroupName <RESOURCE_GROUP_NAME>
```

O [`Update-AzFunctionAppSetting`](/powershell/module/az.functions/update-azfunctionappsetting) comando adiciona ou atualiza uma definição de aplicação. O exemplo a seguir cria uma definição com uma chave nomeada `CUSTOM_FUNCTION_APP_SETTING` e um valor `12345` de:

```azurepowershell-interactive
Update-AzFunctionAppSetting -Name <FUNCTION_APP_NAME> -ResourceGroupName <RESOURCE_GROUP_NAME> -AppSetting @{"CUSTOM_FUNCTION_APP_SETTING" = "12345"}
```

---

### <a name="use-application-settings"></a>Utilizar as definições de aplicações

[!INCLUDE [functions-environment-variables](../../includes/functions-environment-variables.md)]

Quando desenvolve uma aplicação de função localmente, deve manter cópias locais destes valores no local.settings.jsno ficheiro do projeto. Para saber mais, consulte [o ficheiro de definições locais](functions-run-local.md#local-settings-file).

## <a name="hosting-plan-type"></a>Tipo de plano de hospedagem

Quando cria uma aplicação de função, também cria um plano de hospedagem no qual a aplicação é executado. Um plano pode ter uma ou mais aplicações de função. A funcionalidade, a escala e o preço das suas funções dependem do tipo de plano. Para saber mais, consulte [as opções de hospedagem do Azure Functions](functions-scale.md).

Pode determinar o tipo de plano que está a ser utilizado pela sua aplicação de função a partir do portal Azure, ou utilizando as APIs Azure CLI ou Azure PowerShell. 

Os seguintes valores indicam o tipo de plano:

| Tipo de plano | Portal | Azure CLI/PowerShell |
| --- | --- | --- |
| [Consumo](consumption-plan.md) | **Consumo** | `Dynamic` |
| [Premium](functions-premium-plan.md) | **Premónio Elástico** | `ElasticPremium` |
| [Dedicado (Serviço de Aplicações)](dedicated-plan.md) | Vários | Vários |

# <a name="portal"></a>[Portal](#tab/portal)

Para determinar o tipo de plano utilizado pela sua aplicação de função, consulte o **plano de Serviço de Aplicações** no **separador Visão Geral** para a aplicação de função no portal [Azure](https://portal.azure.com). Para ver o nível de preços, selecione o nome do Plano de Serviço de **Aplicações** e, em seguida, selecione **Propriedades** a partir do painel esquerdo.

![Ver plano de escala no portal](./media/functions-scale/function-app-overview-portal.png)

# <a name="azure-cli"></a>[CLI do Azure](#tab/azurecli)

Executar o seguinte comando Azure CLI para obter o seu tipo de plano de hospedagem:

```azurecli-interactive
functionApp=<FUNCTION_APP_NAME>
resourceGroup=FunctionMonitoringExamples
appServicePlanId=$(az functionapp show --name $functionApp --resource-group $resourceGroup --query appServicePlanId --output tsv)
az appservice plan list --query "[?id=='$appServicePlanId'].sku.tier" --output tsv

```  

No exemplo anterior substitua `<RESOURCE_GROUP>` e `<FUNCTION_APP_NAME>` pelos nomes de aplicações do grupo de recursos e funções, respetivamente. 

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/powershell)

Executar o seguinte comando Azure PowerShell para obter o seu tipo de plano de hospedagem:

```azurepowershell-interactive
$FunctionApp = '<FUNCTION_APP_NAME>'
$ResourceGroup = '<RESOURCE_GROUP>'

$PlanID = (Get-AzFunctionApp -ResourceGroupName $ResourceGroup -Name $FunctionApp).AppServicePlan
(Get-AzFunctionAppPlan -Name $PlanID -ResourceGroupName $ResourceGroup).SkuTier
```
No exemplo anterior substitua `<RESOURCE_GROUP>` e `<FUNCTION_APP_NAME>` pelos nomes de aplicações do grupo de recursos e funções, respetivamente. 

---

## <a name="plan-migration"></a>Plano de migração

Pode utilizar comandos Azure CLI para migrar uma aplicação de função entre um plano de Consumo e um plano Premium no Windows. Os comandos específicos dependem da direção da migração. A migração direta para um plano dedicado (App Service) não é suportada atualmente.

Esta migração não é apoiada em Linux.

### <a name="consumption-to-premium"></a>Consumo a Prémio

Utilize o seguinte procedimento para migrar de um plano de consumo para um plano Premium no Windows:

1. Executar o seguinte comando para criar um novo plano de Serviço de Aplicações (Elastic Premium) na mesma região e grupo de recursos que a sua app de função existente.  

    ```azurecli-interactive
    az functionapp plan create --name <NEW_PREMIUM_PLAN_NAME> --resource-group <MY_RESOURCE_GROUP> --location <REGION> --sku EP1
    ```

1. Executar o seguinte comando para migrar a app de função existente para o novo plano Premium

    ```azurecli-interactive
    az functionapp update --name <MY_APP_NAME> --resource-group <MY_RESOURCE_GROUP> --plan <NEW_PREMIUM_PLAN>
    ```

1. Se já não precisar do seu plano de aplicações de função Consumo anterior, elimine o seu plano de aplicações de função original depois de confirmar que emigrou com sucesso para o novo. Execute o seguinte comando para obter uma lista de todos os planos de consumo no seu grupo de recursos.

    ```azurecli-interactive
    az functionapp plan list --resource-group <MY_RESOURCE_GROUP> --query "[?sku.family=='Y'].{PlanName:name,Sites:numberOfSites}" -o table
    ```

    Pode eliminar com segurança o plano com zero sites, que é aquele de onde emigrou.

1. Executar o seguinte comando para eliminar o plano de Consumo de que emigrou.

    ```azurecli-interactive
    az functionapp plan delete --name <CONSUMPTION_PLAN_NAME> --resource-group <MY_RESOURCE_GROUP>
    ```

### <a name="premium-to-consumption"></a>Prémio ao Consumo

Utilize o seguinte procedimento para migrar de um plano Premium para um plano de consumo no Windows:

1. Executar o seguinte comando para criar uma nova app de função (Consumo) na mesma região e grupo de recursos que a sua app de função existente. Este comando também cria um novo plano de Consumo no qual a aplicação de função é executado.

    ```azurecli-interactive
    az functionapp create --resource-group <MY_RESOURCE_GROUP> --name <NEW_CONSUMPTION_APP_NAME> --consumption-plan-location <REGION> --runtime dotnet --functions-version 3 --storage-account <STORAGE_NAME>
    ```

1. Executar o seguinte comando para migrar a app de função existente para o novo plano de Consumo.

    ```azurecli-interactive
    az functionapp update --name <MY_APP_NAME> --resource-group <MY_RESOURCE_GROUP> --plan <NEW_CONSUMPTION_PLAN>
    ```

1. Elimine a aplicação de função que criou no passo 1, uma vez que só precisa do plano criado para executar a aplicação de função existente.

    ```azurecli-interactive
    az functionapp delete --name <NEW_CONSUMPTION_APP_NAME> --resource-group <MY_RESOURCE_GROUP>
    ```

1. Se já não precisar do seu plano de aplicações de função Premium anterior, elimine o seu plano de aplicações de função original depois de confirmar que emigrou com sucesso para o novo. Por favor, note que se o plano não for apagado, continuará a ser cobrado pelo plano Premium. Execute o seguinte comando para obter uma lista de todos os planos Premium no seu grupo de recursos.

    ```azurecli-interactive
    az functionapp plan list --resource-group <MY_RESOURCE_GROUP> --query "[?sku.family=='EP'].{PlanName:name,Sites:numberOfSites}" -o table
    ```

1. Executar o seguinte comando para eliminar o plano Premium de que emigrou.

    ```azurecli-interactive
    az functionapp plan delete --name <PREMIUM_PLAN> --resource-group <MY_RESOURCE_GROUP>
    ```

## <a name="platform-features"></a>Funcionalidades da plataforma

As aplicações de função são executadas e mantidas pela plataforma Azure App Service. Como tal, as aplicações da sua função têm acesso à maioria das funcionalidades da plataforma de hospedagem web core da Azure. O painel esquerdo é onde acede às muitas funcionalidades da plataforma De Serviço de Aplicações que pode utilizar nas suas aplicações de função. 

> [!NOTE]
> Nem todas as funcionalidades do Serviço de Aplicações estão disponíveis quando uma aplicação de função funciona no plano de hospedagem De Consumo.

O resto deste artigo centra-se nas seguintes funcionalidades do Serviço de Aplicações no portal Azure que são úteis para funções:

+ [Editor de Serviço de Aplicações](#editor)
+ [Consola](#console)
+ [Ferramentas avançadas (Kudu)](#kudu)
+ [Opções de implementação](#deployment)
+ [CORS](#cors)
+ [Autenticação](#auth)

Para obter mais informações sobre como trabalhar com as definições do Serviço de Aplicações, consulte configurações do [Serviço de Aplicações Configure Azure](../app-service/configure-common.md).

### <a name="app-service-editor"></a><a name="editor"></a>Editor de Serviço de Aplicações

![O editor do Serviço de Aplicações](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-appservice-editor.png)

O editor do Serviço de Aplicações é um editor avançado no portal que pode usar para modificar ficheiros de configuração JSON e ficheiros de código. A escolha desta opção lança um separado separado do navegador com um editor básico. Isto permite-lhe integrar-se com o código de repositório, executar e depurar Git e modificar as definições de aplicações de função. Este editor proporciona um ambiente de desenvolvimento melhorado para as suas funções em comparação com o editor de funções incorporado.  

Recomendamos que considere desenvolver as suas funções no seu computador local. Quando desenvolve localmente e publica para o Azure, os seus ficheiros de projeto são apenas lidos no portal. Para saber mais, consulte [Código e teste as Funções Azure localmente.](functions-develop-local.md)

### <a name="console"></a><a name="console"></a>Consola

![Consola de aplicativos de função](./media/functions-how-to-use-azure-function-app-settings/configure-function-console.png)

A consola no portal é uma ferramenta de desenvolvimento ideal quando prefere interagir com a sua aplicação de função a partir da linha de comando. Os comandos comuns incluem diretório e criação de ficheiros e navegação, bem como a execução de ficheiros e scripts de lote. 

Ao desenvolver-se localmente, recomendamos a utilização das [Ferramentas Centrais de Funções Azure](functions-run-local.md) e do [CLI Azure].

### <a name="advanced-tools-kudu"></a><a name="kudu"></a>Ferramentas avançadas (Kudu)

![Configuure Kudu](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-kudu.png)

As ferramentas avançadas para o Serviço de Aplicações (também conhecido como Kudu) fornecem acesso a funcionalidades administrativas avançadas da sua aplicação de função. A partir de Kudu, gere informações do sistema, configurações de aplicações, variáveis ambientais, extensões de site, cabeçalhos HTTP e variáveis de servidor. Também pode lançar **Kudu** navegando no ponto final SCM para a sua aplicação de função, como `https://<myfunctionapp>.scm.azurewebsites.net/` 


### <a name="deployment-center"></a><a name="deployment"></a>Centro de Implementação

Quando utiliza uma solução de controlo de fontes para desenvolver e manter o código de funções, o Centro de Implantação permite-lhe construir e implantar a partir do controlo de origem. O seu projeto é construído e implantado para a Azure quando faz atualizações. Para obter mais informações, consulte [as tecnologias de implantação em Funções Azure](functions-deployment-technologies.md).

### <a name="cross-origin-resource-sharing"></a><a name="cors"></a>Partilha de recursos de várias origens

Para evitar a execução de código malicioso no cliente, os navegadores modernos bloqueiam pedidos de aplicações web a recursos executados em um domínio separado. [A partilha de recursos de origem cruzada (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS) permite que um `Access-Control-Allow-Origin` cabeçalho declare quais as origens que podem chamar pontos finais na sua aplicação de função.

#### <a name="portal"></a>Portal

Quando configurar a lista **de origens permitidas** para a sua aplicação de função, o `Access-Control-Allow-Origin` cabeçalho é automaticamente adicionado a todas as respostas dos pontos finais HTTP na sua aplicação de função. 

![Configurar a lista DE CORS da app de funções configurar](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-cors.png)

Quando o wildcard `*` é usado, todos os outros domínios são ignorados. 

Utilize o [`az functionapp cors add`](/cli/azure/functionapp/cors#az_functionapp_cors_add) comando para adicionar um domínio à lista de origens permitidas. O seguinte exemplo adiciona o domínio contoso.com:

```azurecli-interactive
az functionapp cors add --name <FUNCTION_APP_NAME> \
--resource-group <RESOURCE_GROUP_NAME> \
--allowed-origins https://contoso.com
```

Utilize o [`az functionapp cors show`](/cli/azure/functionapp/cors#az_functionapp_cors_show) comando para listar as origens atuais permitidas.

### <a name="authentication"></a><a name="auth"></a>Autenticação

![Configurar a autenticação para uma aplicação de função](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-authentication.png)

Quando as funções utilizarem um gatilho HTTP, pode exigir que as chamadas sejam autenticadas primeiro. O App Service suporta a autenticação e o sômposições do Azure Ative Directory com fornecedores sociais, como facebook, Microsoft e Twitter. Para obter detalhes sobre a configuração de fornecedores de autenticação específica, consulte [a visão geral da autenticação do Azure App Service](../app-service/overview-authentication-authorization.md). 


## <a name="next-steps"></a>Passos seguintes

+ [Configurações de serviço de aplicações Azure configurações](../app-service/configure-common.md)
+ [Implantação contínua para funções Azure](functions-continuous-deployment.md)

[CLI do Azure]: /cli/azure/
[Portal do Azure]: https://portal.azure.com
