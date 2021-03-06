---
title: 'Tutorial: Integração do Azure Ative Directory com a GoToMeeting | Microsoft Docs'
description: Aprenda os passos que precisa de executar para integrar o GoToMeeting com o Azure Ative Directory (Azure AD).
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/16/2020
ms.author: jeedes
ms.openlocfilehash: ffc87b311cc1eeaa9c0d5bb66602b824b5ee9803
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "92456008"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-gotomeeting"></a>Tutorial: Azure Ative Directory integração única (SSO) com GoToMeeting

Neste tutorial, você vai aprender a integrar GoToMeeting com Azure Ative Directory (Azure AD). Quando integrar o GoToMeeting com a Ad Azure, pode:

* Controle em Azure AD que tem acesso ao GoToMeeting.
* Permita que os seus utilizadores sejam automaticamente inscritos no GoToMeeting com as suas contas AD Azure.
* Gerencie as suas contas numa localização central - o portal Azure.

Para saber mais sobre a integração da aplicação SaaS com a Azure AD, consulte o que é o acesso à [aplicação e o único sign-on com o Azure Ative Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para começar, precisa dos seguintes itens:

* Uma assinatura AD Azure. Se não tiver uma subscrição, pode obter uma [conta gratuita.](https://azure.microsoft.com/free/)
* GoToMeeting assinatura única ativada (SSO).

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configura e testa Azure AD SSO em um ambiente de teste.

* GoToMeeting suporta **IDP** iniciado SSO.
* Uma vez configurado o GoToMeeting, pode impor controlos de sessão, que protegem a exfiltração e infiltração dos dados sensíveis da sua organização em tempo real. Os controlos de sessão estendem-se desde o Acesso Condicional. [Saiba como impor o controlo da sessão com a Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad)

## <a name="adding-gotomeeting-from-the-gallery"></a>Adicionar GoToMeeting da galeria

Para configurar a integração do GoToMeeting no AD Azure, precisa adicionar GoToMeeting da galeria à sua lista de aplicações geridas pelo SaaS.

1. Inscreva-se no [portal Azure](https://portal.azure.com) usando uma conta de trabalho ou escola, ou uma conta pessoal da Microsoft.
1. No painel de navegação à esquerda, selecione o serviço **Azure Ative Directory.**
1. Navegue para **aplicações empresariais** e, em seguida, selecione **Todas as Aplicações**.
1. Para adicionar nova aplicação, selecione **Nova aplicação**.
1. Na secção Adicionar da secção **da galeria,** **digite GoToMeeting** na caixa de pesquisa.
1. Selecione **GoToMeeting do** painel de resultados e, em seguida, adicione a aplicação. Aguarde alguns segundos enquanto a aplicação é adicionada ao seu inquilino.


## <a name="configure-and-test-azure-ad-single-sign-on-for-gotomeeting"></a>Configurar e testar Azure AD único sinal para GoToMeeting

Configure e teste Azure AD SSO com GoToMeeting usando um utilizador de teste chamado **B.Simon**. Para que o SSO funcione, é necessário estabelecer uma relação de ligação entre um utilizador AZure AD e o utilizador relacionado no GoToMeeting.

Para configurar e testar o Azure AD SSO com GoToMeeting, complete os seguintes blocos de construção:

1. **[Configure Azure AD SSO](#configure-azure-ad-sso)** - para permitir que os seus utilizadores utilizem esta funcionalidade.
    * Crie um utilizador de **[teste AD Azure](#create-an-azure-ad-test-user)** - para testar um único sinal de Azure com B.Simon.
    * **[Atribua o utilizador de teste Azure AD](#assign-the-azure-ad-test-user)** - para permitir que b.Simon utilize um único sinal de Ad AD.
1. **[Configure o GoToMeeting SSO](#configure-gotomeeting-sso)** - para configurar as definições de inscrição única no lado da aplicação.
    * **[Create GoToMeeting test user](#create-gotomeeting-test-user)** - para ter uma contraparte de B.Simon em GoToMeeting que está ligada à representação AD AD do utilizador.
1. **[Teste SSO](#test-sso)** - para verificar se a configuração funciona.

## <a name="configure-azure-ad-sso"></a>Configurar o SSO do Azure AD

Siga estes passos para ativar o Azure AD SSO no portal Azure.

1. No [portal Azure](https://portal.azure.com/), na página de integração de aplicações **GoToMeeting,** encontre a secção **Gerir** e selecione **um único sinal de sing**.
1. Na página de método **de inscrição** única, selecione **SAML**.
1. No **set-on único com** a página SAML, clique no ícone edit/pen para **Configuração SAML Básica** para editar as definições.

   ![Editar Configuração BÁSICA SAML](common/edit-urls.png)

1. Na secção **Configuração Básica SAML,** insira os valores para os seguintes campos:

    a. Na caixa de texto **do identificador,** digite um URL utilizando o seguinte padrão: `https://authentication.logmeininc.com/saml/sp`

    b. Na caixa de texto **URL de resposta,** digite um URL utilizando o seguinte padrão: `https://authentication.logmeininc.com/saml/acs`

    c. Clique **em URLs adicionais** e configuure os URLs abaixo

    d. **Assine no URL** (mantenha este em branco)

    e. Na caixa de texto **RelayState,** digite um URL utilizando o seguinte padrão:

   - Para App GoToMeeting, use `https://global.gotomeeting.com`

   - Para GoToTraining, use `https://global.gototraining.com`

   - Para GoToWebinar, use `https://global.gotowebinar.com` 

   - Para GoToAssist, use `https://app.gotoassist.com`

     > [!NOTE]
     > Estes valores não são reais. Atualize estes valores com o URL de identificação e resposta real. Contacte [a equipa de suporte do Cliente GoToMeeting](../manage-apps/view-applications-portal.md) para obter estes valores. Também pode consultar os padrões indicados na secção **de Configuração BÁSICA SAML** no portal Azure.

5. Na **configuração single Sign-On com** a página SAML, na secção **Certificado de Assinatura SAML,** clique em **Baixar** para descarregar o **Certificado (Base64)** das opções dadas de acordo com o seu requisito e guardá-lo no seu computador.

    ![O link de descarregamento de certificado](common/certificatebase64.png)

6. Na secção **set up GoToMeeting,** copie os URL(s) apropriados de acordo com o seu requisito.

    ![URLs de configuração de cópia](common/copy-configuration-urls.png)

    a. URL de Inicio de Sessão

    b. Identificador Azure Ad

    c. Logout URL


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

Nesta secção, você permitirá que B.Simon use a Azure single sign-on, concedendo acesso ao GoToMeeting.

1. No portal Azure, selecione **Aplicações empresariais** e, em seguida, selecione **Todas as aplicações**.
1. Na lista de aplicações, selecione **GoToMeeting**.
1. Na página geral da aplicação, encontre a secção **Gerir** e selecione **Utilizadores e grupos**.

   ![A ligação "Utilizadores e grupos"](common/users-groups-blade.png)

1. **Selecione Adicionar utilizador,** em seguida, selecione **Utilizadores e grupos** no diálogo **'Adicionar Atribuição'.**

    ![O link do utilizador adicionar](common/add-assign-user.png)

1. No diálogo **de Utilizadores e grupos,** selecione **B.Simon** da lista de Utilizadores e, em seguida, clique no botão **Select** na parte inferior do ecrã.
1. Se estiver à espera de qualquer valor de função na afirmação SAML, no diálogo **'Fun's Select,** selecione a função adequada para o utilizador da lista e, em seguida, clique no botão **Selecione** na parte inferior do ecrã.
1. No diálogo **'Adicionar Atribuição',** clique no botão **'Atribuir'.**

## <a name="configure-gotomeeting-sso"></a>Configurar GoToMeeting SSO

1. Numa janela de navegador diferente, inicie sessão no seu [Centro de Organização GoToMeeting](https://organization.logmeininc.com/). Será solicitado para confirmar que o IdP foi atualizado.

2. Ativar a caixa de verificação "My Identity Provider" foi atualizada com a nova caixa de verificação de domínio. Clique **em Fazer** quando terminar.

### <a name="create-gotomeeting-test-user"></a>Criar utilizador de teste GoToMeeting

Nesta secção, um utilizador chamado Britta Simon é criado no GoToMeeting. O GoToMeeting suporta o provisionamento just-in-time, que é ativado por padrão.

Não há nenhum item de ação para si nesta secção. Se um utilizador já não existir no GoToMeeting, um novo é criado quando tenta aceder ao GoToMeeting.

> [!NOTE]
> Se necessitar de criar um utilizador manualmente, contacte a [equipa de suporte GoToMeeting](https://support.logmeininc.com/gotomeeting).

## <a name="test-sso"></a>Teste SSO 

Nesta secção, testa a configuração de inscrição única AZure AD utilizando o Painel de Acesso.

Quando clicar no azulejo GoToMeeting no Painel de Acesso, deverá ser automaticamente inscrito no GoToMeeting para o qual configura sSO. Para obter mais informações sobre o Painel de Acesso, consulte [Introdução ao Painel de Acesso.](../user-help/my-apps-portal-end-user-access.md)

## <a name="additional-resources"></a>Recursos adicionais

- [ Lista de tutoriais sobre como integrar aplicações saas com diretório ativo Azure ](./tutorial-list.md)

- [O que é o acesso à aplicação e um único acesso ao Azure Ative Directory? ](../manage-apps/what-is-single-sign-on.md)

- [O que é o acesso condicional no Azure Active Directory?](../conditional-access/overview.md)

- [Experimente GoToMeeting com Azure AD](https://aad.portal.azure.com/)

- [O que é o controlo de sessão no Microsoft Cloud App Security?](/cloud-app-security/proxy-intro-aad)

- [Como proteger o GoToMeeting com visibilidade e controlos avançados](/cloud-app-security/proxy-intro-aad)