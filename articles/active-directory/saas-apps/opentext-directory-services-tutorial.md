---
title: 'Tutorial: Azure Ative Directory integração única de sign-on (SSO) com serviços de diretório OpenText | Microsoft Docs'
description: Saiba como configurar um único sign-on entre o Azure Ative Directory e o OpenText Directory Services.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/11/2021
ms.author: jeedes
ms.openlocfilehash: 71c69ffa91e5841ec03ed836ab2562af2ec01959
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "101645110"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-opentext-directory-services"></a>Tutorial: Azure Ative Directy integração única de sign-on (SSO) com serviços de diretório OpenText

Neste tutorial, você vai aprender a integrar os Serviços de Diretório OpenText com o Azure Ative Directory (Azure AD). Quando integrar os Serviços de Diretório OpenText com Azure AD, pode:

* Controlo em Azure AD que tem acesso aos Serviços de Diretório OpenText.
* Capacitar os seus utilizadores a serem automaticamente inscritos nos Serviços de Diretório OpenText com as suas contas AD Azure.
* Gerencie as suas contas numa localização central - o portal Azure.

## <a name="prerequisites"></a>Pré-requisitos

Para começar, precisa dos seguintes itens:

* Uma assinatura AD Azure. Se não tiver uma subscrição, pode obter uma [conta gratuita.](https://azure.microsoft.com/free/)
* OpenText Directory Services assinatura única (SSO) ativada.

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configura e testa Azure AD SSO em um ambiente de teste.

* Os Serviços de Diretório OpenText suportam **a SP e a IDP** iniciada sSO.
* Os Serviços de Diretório OpenText suportam o fornecimento do utilizador **Just In Time.**

## <a name="add-opentext-directory-services-from-the-gallery"></a>Adicionar Serviços de Diretório OpenText da galeria

Para configurar a integração dos Serviços de Diretório OpenText em Azure AD, é necessário adicionar serviços de diretório OpenText da galeria à sua lista de aplicações geridas pelo SaaS.

1. Inscreva-se no portal Azure usando uma conta de trabalho ou escola, ou uma conta pessoal da Microsoft.
1. No painel de navegação à esquerda, selecione o serviço **Azure Ative Directory.**
1. Navegue para **aplicações empresariais** e, em seguida, selecione **Todas as Aplicações**.
1. Para adicionar nova aplicação, selecione **Nova aplicação**.
1. Na secção Adicionar a partir da secção **de galeria,** **escreva Serviços de Diretório OpenText** na caixa de pesquisa.
1. Selecione **Os Serviços de Diretório OpenText** do painel de resultados e, em seguida, adicione a aplicação. Aguarde alguns segundos enquanto a aplicação é adicionada ao seu inquilino.

## <a name="configure-and-test-azure-ad-sso-for-opentext-directory-services"></a>Configurar e testar Azure AD SSO para serviços de diretório OpenText

Configure e teste Azure AD SSO com OpenText Directory Services usando um utilizador de teste chamado **B.Simon**. Para que o SSO funcione, é necessário estabelecer uma relação de ligação entre um utilizador Azure AD e o utilizador relacionado nos Serviços de Diretório OpenText.

Para configurar e testar o Azure AD SSO com os Serviços de Diretório OpenText, execute os seguintes passos:

1. **[Configure Azure AD SSO](#configure-azure-ad-sso)** - para permitir que os seus utilizadores utilizem esta funcionalidade.
    1. Crie um utilizador de **[teste AD Azure](#create-an-azure-ad-test-user)** - para testar um único sinal de Azure com B.Simon.
    1. **[Atribua o utilizador de teste Azure AD](#assign-the-azure-ad-test-user)** - para permitir que b.Simon utilize um único sinal de Ad AD.
1. **[Configure o OpenText Directory Services SSO](#configure-opentext-directory-services-sso)** - para configurar as definições de inscrição única no lado da aplicação.
    1. Crie o utilizador de **[teste openText Directory Services](#create-opentext-directory-services-test-user)** - para ter uma contrapartida de B.Simon nos Serviços de Diretório OpenText que está ligada à representação Azure AD do utilizador.
1. **[Teste SSO](#test-sso)** - para verificar se a configuração funciona.

## <a name="configure-azure-ad-sso"></a>Configurar o SSO do Azure AD

Siga estes passos para ativar o Azure AD SSO no portal Azure.

1. No portal Azure, na página de integração de aplicações **OpenText Directory Services,** encontre a secção **Gerir** e selecione **um único sinal de sação**.
1. Na página de método **de inscrição** única, selecione **SAML**.
1. No **set-on único com** a página SAML, clique no ícone de lápis para **configuração SAML Básica** para editar as definições.

   ![Editar Configuração BÁSICA SAML](common/edit-urls.png)

1. Na secção **Configuração Básica SAML,** se pretender configurar a aplicação no modo iniciado pelo **IDP,** insira os valores para os seguintes campos:

    a. Na caixa de texto **do identificador,** digite um URL utilizando o seguinte padrão: `https://<SUBDOMAIN>.opentext.com/<OTDSTENANT>/<TENANTID>/login`

    b. Na caixa de texto **URL de resposta,** digite um URL utilizando o seguinte padrão: `https://<SUBDOMAIN>.opentext.com/<OTDSTENANT>/<TENANTID>/login?authhandler=<HANDLERID>`

1. Clique **em Definir URLs adicionais** e execute o seguinte passo se desejar configurar a aplicação **no** modo iniciado sp:

    Na caixa de texto **URL de entrada de inscrição,** digite um URL utilizando o seguinte padrão:  `https://<SUBDOMAIN>.opentext.com/<OTDSTENANT>/<TENANTID>/login?authhandler=<HANDLERID>`

    > [!NOTE]
    > Estes valores não são reais. Atualize estes valores com o identificador real, URL de resposta e URL de inscrição. Contacte [a equipa de suporte do cliente openText Directory Services](mailto:support@opentext.com) para obter estes valores. Também pode consultar os padrões indicados na secção **de Configuração BÁSICA SAML** no portal Azure.

1. Na **configuração de um único sessão de inscrição com** a página SAML, na secção **Certificado de Assinatura SAML,** clique no botão de cópia para copiar o Url de **metadados da Federação de Aplicações** e guarde-o no seu computador.

    ![O link de descarregamento de certificado](common/copy-metadataurl.png)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste AZure AD

Nesta secção, irá criar um utilizador de teste no portal Azure chamado B.Simon.

1. A partir do painel esquerdo no portal Azure, selecione **Azure Ative Directory**, selecione **Utilizadores**, e, em seguida, selecione **Todos os utilizadores**.
1. Selecione **Novo utilizador** na parte superior do ecrã.
1. Nas propriedades do **Utilizador,** siga estes passos:
   1. No campo **Nome**, introduza `B.Simon`.  
   1. No campo **nome do utilizador,** insira o username@companydomain.extension . Por exemplo, `B.Simon@contoso.com`.
   1. Selecione a caixa **de verificação de palavra-passe Show** e, em seguida, anote o valor que é apresentado na caixa **palavra-passe.**
   1. Clique em **Criar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o utilizador de teste AZure AD

Nesta secção, você permitirá que B.Simon use a Azure single sign-on, concedendo acesso aos Serviços de Diretório OpenText.

1. No portal Azure, selecione **Aplicações empresariais** e, em seguida, selecione **Todas as aplicações**.
1. Na lista de candidaturas, selecione **OpenText Directory Services**.
1. Na página geral da aplicação, encontre a secção **Gerir** e selecione **Utilizadores e grupos**.
1. **Selecione Adicionar utilizador,** em seguida, selecione **Utilizadores e grupos** no diálogo **'Adicionar Atribuição'.**
1. No diálogo **de Utilizadores e grupos,** selecione **B.Simon** da lista de Utilizadores e, em seguida, clique no botão **Select** na parte inferior do ecrã.
1. Se estiver à espera que uma função seja atribuída aos utilizadores, pode selecioná-la a partir do Dropdown de **função** Select. Se não tiver sido configurada qualquer função para esta aplicação, vê a função "Acesso Predefinido" selecionada.
1. No diálogo **'Adicionar Atribuição',** clique no botão **'Atribuir'.**

## <a name="configure-opentext-directory-services-sso"></a>Configure Serviços de Diretório OpenText SSO

Para configurar um único sinal no lado **dos Serviços de Diretório OpenText,** é necessário enviar o **Url de Metadados da Federação de Aplicações** para a [equipa de suporte dos Serviços de Diretório OpenText](mailto:support@opentext.com). Eles definem esta definição para ter a ligação SSO SAML corretamente definida em ambos os lados.

### <a name="create-opentext-directory-services-test-user"></a>Criar utilizador de teste de serviços de diretório OpenText

Nesta secção, um utilizador chamado B.Simon é criado nos Serviços de Diretório OpenText. Os Serviços de Diretório OpenText suportam o fornecimento de utilizadores just-in-time, o que é ativado por padrão. Não há nenhum item de ação para si nesta secção. Se um utilizador já não existir nos Serviços de Diretório OpenText, um novo é criado após a autenticação.

## <a name="test-sso"></a>Teste SSO 

Nesta secção, testa a configuração de um único sinal de inscrição Azure AD com as seguintes opções. 

#### <a name="sp-initiated"></a>SP iniciado:

* Clique em **Testar esta aplicação** no portal Azure. Isto irá redirecionar para OpenText Directory Services Sign on URL onde pode iniciar o fluxo de login.  

* Vá diretamente ao URL de acesso ao Diretório OpenText e inicie o fluxo de login a partir daí.

#### <a name="idp-initiated"></a>IDP iniciado:

* Clique em **Testar esta aplicação** no portal Azure e deverá ser automaticamente inscrito nos Serviços de Diretório OpenText para os quais configura o SSO. 

Também pode utilizar o Microsoft My Apps para testar a aplicação em qualquer modo. Quando clicar no azulejo do OpenText Directory Services nas Minhas Apps, se configurado no modo SP, será redirecionado para o sinal de aplicação na página para iniciar o fluxo de login e se configurado no modo IDP, deverá ser automaticamente inscrito nos Serviços de Diretório OpenText para os quais configura o SSO. Para obter mais informações sobre as Minhas Apps, consulte [Introdução às Minhas Aplicações.](../user-help/my-apps-portal-end-user-access.md)

## <a name="next-steps"></a>Passos seguintes

Uma vez configurados serviços de diretório OpenText, pode impor o controlo da sessão, que protege a exfiltração e infiltração dos dados sensíveis da sua organização em tempo real. O controlo da sessão estende-se desde o Acesso Condicional. [Saiba como impor o controlo da sessão com o Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).