---
title: 'Tutorial: Integração do Diretório Ativo Azure com Nova Relíquia por Conta | Microsoft Docs'
description: Saiba como configurar um único sign-on entre o Azure Ative Directory e a New Relic by Account.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/02/2021
ms.author: jeedes
ms.openlocfilehash: a2c149bfdf79102779abf7544fed9fb78796a50e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "101649977"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-new-relic-by-account"></a>Tutorial: Azure Ative Directory integração única (SSO) com Nova Relíquia por Conta

Neste tutorial, você vai aprender a integrar Nova Relíquia por Conta com Azure Ative Direy (Azure AD). Quando integra a Nova Relíquia por Conta com a Azure AD, pode:

* Controlo em Azure AD que tem acesso a Nova Relíquia por Conta.
* Permitir que os seus utilizadores sejam automaticamente inscritos na Nova Relíquia por Conta com as suas contas AD Azure.
* Gerencie as suas contas numa localização central - o portal Azure.

## <a name="prerequisites"></a>Pré-requisitos

Para começar, precisa dos seguintes itens:

* Uma assinatura AD Azure. Se não tiver uma subscrição, pode obter uma [conta gratuita.](https://azure.microsoft.com/free/)
* Nova subscrição ativada por nova relíquia por conta (SSO).

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configura e testa Azure AD SSO em um ambiente de teste.

* Nova Relíquia por Conta suporta **SSO** iniciado sp

## <a name="add-new-relic-by-account-from-the-gallery"></a>Adicione nova relíquia por conta da galeria

Para configurar a integração de Nova Relíquia por Conta no AZure AD, é necessário adicionar Nova Relíquia por Conta da galeria à sua lista de aplicações geridas pelo SaaS.

1. Inscreva-se no portal Azure usando uma conta de trabalho ou escola, ou uma conta pessoal da Microsoft.
1. No painel de navegação à esquerda, selecione o serviço **Azure Ative Directory.**
1. Navegue para **aplicações empresariais** e, em seguida, selecione **Todas as Aplicações**.
1. Para adicionar nova aplicação, selecione **Nova aplicação**.
1. Na secção Adicionar a partir da secção **da galeria,** digite **Nova Relíquia por Conta** na caixa de pesquisa.
1. Selecione **Nova Relíquia por Conta** a partir do painel de resultados e adicione a aplicação. Aguarde alguns segundos enquanto a aplicação é adicionada ao seu inquilino.

## <a name="configure-and-test-azure-ad-sso-for-new-relic-by-account"></a>Configurar e testar Azure AD SSO para nova relíquia por conta

Configure e teste Azure AD SSO com Nova Relíquia por Conta usando um utilizador de teste chamado **B.Simon**. Para que o SSO funcione, é necessário estabelecer uma relação de ligação entre um utilizador Azure AD e o utilizador relacionado em Nova Relíquia por Conta.

Para configurar e testar a Azure AD SSO com Nova Relíquia por Conta, execute os seguintes passos:

1. **[Configure Azure AD SSO](#configure-azure-ad-sso)** - para permitir que os seus utilizadores utilizem esta funcionalidade.
    * Crie um utilizador de **[teste AD Azure](#create-an-azure-ad-test-user)** - para testar um único sinal de Azure com B.Simon.
    * **[Atribua o utilizador de teste Azure AD](#assign-the-azure-ad-test-user)** - para permitir que b.Simon utilize um único sinal de Ad AD.
1. **[Configure Nova Relíquia por Conta SSO](#configure-new-relic-by-account-sso)** - para configurar as definições de inscrição única no lado da aplicação.
    * **[Criar novo utilizador](#create-new-relic-by-account-test-user)** de teste de relíquia por conta - para ter uma contraparte de B.Simon em Nova Relíquia por Conta que está ligada à representação AD AD do utilizador.
1. **[Teste SSO](#test-sso)** - para verificar se a configuração funciona.

## <a name="configure-azure-ad-sso"></a>Configurar o SSO do Azure AD

Siga estes passos para ativar o Azure AD SSO no portal Azure.

1. No portal Azure, na página de integração da aplicação **New Relic by Account,** encontre a secção **Gerir** e selecione **um único sinal de sação**.
1. Na página de método **de inscrição** única, selecione **SAML**.
1. No **set-on único com** a página SAML, clique no ícone de lápis para **configuração SAML Básica** para editar as definições.

   ![Editar Configuração BÁSICA SAML](common/edit-urls.png)
   
1. Na secção **de Configuração Básica SAML,** execute os seguintes passos:

    a. Na caixa de texto **URL, digite** o URL utilizando o seguinte padrão:

    `https://rpm.newrelic.com:443/accounts/{acc_id}/sso/saml/finalize` - Substitua-se `acc_id` pela sua própria Conta ID de Nova Relíquia por Conta.

    b. Na caixa de texto **identifier (Entity ID),** digite o URL: `rpm.newrelic.com`

1. Na **configuração single Sign-On com** a página SAML, na secção **Certificado de Assinatura SAML,** clique em **Baixar** para descarregar o **Certificado (Base64)** das opções dadas de acordo com o seu requisito e guardá-lo no seu computador.

    ![O link de descarregamento de certificado](common/certificatebase64.png)

1. Na secção **Configurar Nova Relíquia por Conta,** copie os URL(s) apropriados de acordo com o seu requisito.

    ![URLs de configuração de cópia](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user&quot;></a>Criar um utilizador de teste AZure AD

Nesta secção, irá criar um utilizador de teste no portal Azure chamado B.Simon.

1. A partir do painel esquerdo no portal Azure, selecione **Azure Ative Directory**, selecione **Utilizadores**, e, em seguida, selecione **Todos os utilizadores**.
1. Selecione **Novo utilizador** na parte superior do ecrã.
1. Nas propriedades do **Utilizador,** siga estes passos:
   1. No campo **Nome**, introduza `B.Simon`.  
   1. No campo **nome do utilizador,** insira o username@companydomain.extension . Por exemplo, `B.Simon@contoso.com`.
   1. Selecione a caixa **de verificação de palavra-passe Show** e, em seguida, anote o valor que é apresentado na caixa **palavra-passe.**
   1. Clique em **Criar**.

### <a name=&quot;assign-the-azure-ad-test-user&quot;></a>Atribuir o utilizador de teste AZure AD

Nesta secção, você permitirá que B.Simon use a Azure single sign-on, concedendo acesso a Nova Relíquia por Conta.

1. No portal Azure, selecione **Aplicações empresariais** e, em seguida, selecione **Todas as aplicações**.
1. Na lista de candidaturas, selecione **Nova Relíquia por Conta**.
1. Na página geral da aplicação, encontre a secção **Gerir** e selecione **Utilizadores e grupos**.
1. **Selecione Adicionar utilizador,** em seguida, selecione **Utilizadores e grupos** no diálogo **'Adicionar Atribuição'.**
1. No diálogo **de Utilizadores e grupos,** selecione **B.Simon** da lista de Utilizadores e, em seguida, clique no botão **Select** na parte inferior do ecrã.
1. Se estiver à espera que uma função seja atribuída aos utilizadores, pode selecioná-la a partir do Dropdown de **função** Select. Se não tiver sido configurada qualquer função para esta aplicação, vê a função &quot;Acesso Predefinido&quot; selecionada.
1. No diálogo **'Adicionar Atribuição',** clique no botão **'Atribuir'.**

## <a name=&quot;configure-new-relic-by-account-sso&quot;></a>Configure nova relíquia por conta SSO

1. Numa janela diferente do navegador web, inscreva-se no site da empresa **New Relic by Account** como administrador.

2. No menu em cima, clique em **Definições de Conta**.
   
    ![A screenshot mostra a página Welcome com as definições de Conta selecionadas.](./media/new-relic-tutorial/settings.png &quot;Definições de conta")

3. Clique no separador **Segurança e autenticação** e, em seguida, clique **no sinal único no** separador.
   
    ![Único sign-on](./media/new-relic-tutorial/single-sign-on-tab.png "Início de Sessão Único")

4. Na página de diálogo SAML, execute os seguintes passos:
   
    ![SAML](./media/new-relic-tutorial/save.png "SAML")
   
    a. Clique **em Escolher Ficheiro** para fazer o upload do certificado de Diretório Azure Ative descarregado.

    b. Na caixa de texto **url de login remoto,** cole o valor do URL de **login,** que copiou do portal Azure.
   
    c. Na caixa de texto **URL de aterragem logout,** cole o valor do **URL logout,** que copiou do portal Azure.

    d. Clique **em Guardar as minhas alterações.**

### <a name="create-new-relic-by-account-test-user"></a>Criar novo utilizador de teste de relíquia por conta

1. Inscreva-se no site da empresa **New Relic by Account** como administrador.

2. No menu em cima, clique em **Definições de Conta**.
   
    ![A screenshot mostra as definições de conta selecionadas na página Welcome.](./media/new-relic-tutorial/account.png "Definições de conta")

3. No painel **conta** no lado esquerdo, clique em **Resumo** e, em seguida, clique em **Adicionar utilizador**.
   
    ![A screenshot mostra o painel resumo onde pode selecionar Adicionar utilizador.](./media/new-relic-tutorial/add.png "Definições de conta")

4. No diálogo dos **utilizadores Ativos,** execute os seguintes passos:
   
    ![Utilizadores Ativos](./media/new-relic-tutorial/user.png "Utilizadores Ativos")
   
    a. Na caixa de texto **do Email,** digite o endereço de e-mail de um utilizador válido do Azure Ative Directory que pretende.

    b. Como **Fun** select **User**.

    c. Clique **em Adicionar este utilizador**.

> [!NOTE]
> Pode utilizar quaisquer outras ferramentas de criação de conta de utilizador de Novas Relíquias por Conta fornecidas pela New Relic by Account para fornecer contas de utilizador Azure AD.

## <a name="test-sso"></a>Teste SSO 

Nesta secção, testa a configuração de um único sinal de inscrição Azure AD com as seguintes opções. 

* Clique em **Testar esta aplicação** no portal Azure. Isto irá redirecionar para Nova Relíquia por URL de início de Sessão de Sessão, onde pode iniciar o fluxo de login. 

* Vá diretamente para A Nova Relíquia por Conta URL de inscrição e inicie o fluxo de login a partir daí.

* Pode utilizar as minhas apps do Microsoft. Quando clicar na Nova Relíquia por Conta nas Minhas Apps, este será redirecionado para Nova Relíquia por URL de inscrição na conta. Para obter mais informações sobre as Minhas Apps, consulte [Introdução às Minhas Aplicações.](../user-help/my-apps-portal-end-user-access.md)

## <a name="next-steps"></a>Passos seguintes

Uma vez configurado Nova Relíquia por Conta, pode impor o controlo da sessão, que protege a exfiltração e infiltração dos dados sensíveis da sua organização em tempo real. O controlo da sessão estende-se desde o Acesso Condicional. [Saiba como impor o controlo da sessão com o Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).