---
title: 'Tutorial: Azure Ative Directory integração única (SSO) com o Campus Café | Microsoft Docs'
description: Saiba como configurar um único sign-on entre o Azure Ative Directory e o Campus Café.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/19/2020
ms.author: jeedes
ms.openlocfilehash: ec6addfb14f89b7acbc3c6d4ed62120d9af4612a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "92456539"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-campus-caf"></a>Tutorial: Azure Ative Directory integração única (SSO) com o Campus Café

Neste tutorial, você vai aprender a integrar o Campus Café com o Azure Ative Directory (Azure AD). Quando integrar o Campus Café com Azure AD, pode:

* Controlo em Azure AD que tem acesso ao Campus Café.
* Permita que os seus utilizadores sejam automaticamente inscritos no Campus Café com as suas contas AD AZure.
* Gerencie as suas contas numa localização central - o portal Azure.

Para saber mais sobre a integração da aplicação SaaS com a Azure AD, consulte o que é o acesso à [aplicação e o único sign-on com o Azure Ative Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para começar, precisa dos seguintes itens:

* Uma assinatura AD Azure. Se não tiver uma subscrição, pode obter uma [conta gratuita.](https://azure.microsoft.com/free/)
* Subscrição única do Campus Café (SSO) habilitada para assinatura.

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configura e testa Azure AD SSO em um ambiente de teste.

* Campus Café apoia **SP** iniciado SSO
* Assim que configurar o Campus Café, pode impor o controlo de sessão, que protege a exfiltração e infiltração dos dados sensíveis da sua organização em tempo real. O controlo da sessão estende-se a partir do Acesso Condicional. [Saiba como impor o controlo da sessão com o Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-campus-caf-from-the-gallery"></a>Adicionar Café Campus da galeria

Para configurar a integração do Campus Café em Azure AD, você precisa adicionar o Campus Café da galeria à sua lista de aplicações geridas saaS.

1. Inscreva-se no [portal Azure](https://portal.azure.com) usando uma conta de trabalho ou escola, ou uma conta pessoal da Microsoft.
1. No painel de navegação à esquerda, selecione o serviço **Azure Ative Directory.**
1. Navegue para **aplicações empresariais** e, em seguida, selecione **Todas as Aplicações**.
1. Para adicionar nova aplicação, selecione **Nova aplicação**.
1. Na secção Add a partir da secção **da galeria,** escreva **Campus Café** na caixa de pesquisa.
1. Selecione **Campus Café** do painel de resultados e adicione a aplicação. Aguarde alguns segundos enquanto a aplicação é adicionada ao seu inquilino.


## <a name="configure-and-test-azure-ad-single-sign-on-for-campus-caf"></a>Configurar e testar Azure AD único sign-on para Campus Café

Configure e teste Azure AD SSO com Campus Café usando um utilizador de teste chamado **B.Simon**. Para que o SSO funcione, é necessário estabelecer uma relação de ligação entre um utilizador Azure AD e o utilizador relacionado no Campus Café.

Para configurar e testar o Azure AD SSO com o Campus Café, complete os seguintes blocos de construção:

1. **[Configure Azure AD SSO](#configure-azure-ad-sso)** - para permitir que os seus utilizadores utilizem esta funcionalidade.
    * Crie um utilizador de **[teste AD Azure](#create-an-azure-ad-test-user)** - para testar um único sinal de Azure com B.Simon.
    * **[Atribua o utilizador de teste Azure AD](#assign-the-azure-ad-test-user)** - para permitir que b.Simon utilize um único sinal de Ad AD.
1. **[Configure o Campus Café SSO](#configure-campus-cafe-sso)** - para configurar as definições de inscrição única no lado da aplicação.
    * **[Create Campus Café test user](#create-campus-cafe-test-user)** - ter uma contrapartida de B.Simon no Campus Café que está ligada à representação AD AD do utilizador.
1. **[Teste SSO](#test-sso)** - para verificar se a configuração funciona.

## <a name="configure-azure-ad-sso"></a>Configurar o SSO do Azure AD

Siga estes passos para ativar o Azure AD SSO no portal Azure.

1. No [portal Azure,](https://portal.azure.com/)na página de integração de aplicações do **Campus Café,** encontre a secção **Gerir** e selecione **um único sinal.**
1. Na página de método **de inscrição** única, selecione **SAML**.
1. No **set-on único com** a página SAML, clique no ícone edit/pen para **Configuração SAML Básica** para editar as definições.

   ![Editar Configuração BÁSICA SAML](common/edit-urls.png)

1. Na secção **de Configuração Básica SAML,** se tiver **um ficheiro de metadados do Fornecedor de Serviços,** execute os seguintes passos:

    a. Clique **em Carregar o ficheiro de metadados**.

    ![Carregar ficheiro de metadados](common/upload-metadata.png)

    b. Clique no **logotipo da pasta** para selecionar o ficheiro de metadados e clique em **Upload**.

    ![escolher arquivo de metadados](common/browse-upload-metadata.png)

    c. Após o carregamento do ficheiro de metadados com sucesso, o valor **do Identificador** é preenchido automaticamente na secção de Configuração BÁSICA SAML.

    ![Campus Café Domain e URLs informações únicas de súmis](common/sp-identifier.png)

    Na caixa de texto **URL de entrada de inscrição,** digite um URL utilizando o seguinte padrão:  `https://{SSO}-web.scansoftware.com/cafeweb/loginsso`

    > [!Note]
    > Se o valor **do Identificador** não for automaticamente poluído, por favor preencha o valor manualmente de acordo com o seu requisito. O valor url de inscrição não é real. Atualize este valor com o URL de inscrição real. Contacte [a equipa de apoio ao Cliente do Campus Café](mailto:support@campuscafesoftware.com) para obter este valor. Também pode consultar os padrões indicados na secção **de Configuração BÁSICA SAML** no portal Azure.

1. Na **configuração de um único sessão de inscrição com** a página SAML, na secção **Certificado de Assinatura SAML,** encontre o **Metadados XML da Federação** e selecione **Descarregue** para descarregar o certificado e guarde-o no seu computador.

    ![O link de descarregamento de certificado](common/metadataxml.png)

1. Na secção Configurar o **Campus Café,** copie os URL(s) apropriados com base na sua exigência.

    ![URLs de configuração de cópia](common/copy-configuration-urls.png)

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

Nesta secção, você permitirá que B.Simon use a Azure single sign-on, concedendo acesso ao Campus Café.

1. No portal Azure, selecione **Aplicações empresariais** e, em seguida, selecione **Todas as aplicações**.
1. Na lista de candidaturas, selecione **Campus Café.**
1. Na página geral da aplicação, encontre a secção **Gerir** e selecione **Utilizadores e grupos**.

   ![A ligação "Utilizadores e grupos"](common/users-groups-blade.png)

1. **Selecione Adicionar utilizador,** em seguida, selecione **Utilizadores e grupos** no diálogo **'Adicionar Atribuição'.**

    ![O link do utilizador adicionar](common/add-assign-user.png)

1. No diálogo **de Utilizadores e grupos,** selecione **B.Simon** da lista de Utilizadores e, em seguida, clique no botão **Select** na parte inferior do ecrã.
1. Se estiver à espera de qualquer valor de função na afirmação SAML, no diálogo **'Fun's Select,** selecione a função adequada para o utilizador da lista e, em seguida, clique no botão **Selecione** na parte inferior do ecrã.
1. No diálogo **'Adicionar Atribuição',** clique no botão **'Atribuir'.**

## <a name="configure-campus-cafe-sso"></a>Configure Campus Café SSO

Para configurar um único sign-on no lado **do Campus Café,** você precisa enviar os **metadados XML da Federação** descarregados e URLs copiados apropriados do portal Azure para a equipe de [suporte do Campus Café](mailto:support@campuscafesoftware.com). Eles definem esta definição para ter a ligação SSO SAML corretamente definida em ambos os lados.

### <a name="create-campus-cafe-test-user"></a>Criar utilizador de teste do Campus Café

Nesta secção, cria-se um utilizador chamado B.Simon no Campus Café. Trabalhe com [a equipa de apoio do Campus Café](mailto:support@campuscafesoftware.com) para adicionar os utilizadores na plataforma Campus Café. Os utilizadores devem ser criados e ativados antes de utilizar uma única s ativação.

## <a name="test-sso"></a>Teste SSO 

Nesta secção, testa a configuração de inscrição única AZure AD utilizando o Painel de Acesso.

Ao clicar no azulejo do Campus Café no Painel de Acesso, deverá ser automaticamente inscrito no Campus Café para o qual configura o SSO. Para obter mais informações sobre o Painel de Acesso, consulte [Introdução ao Painel de Acesso.](../user-help/my-apps-portal-end-user-access.md)

## <a name="additional-resources"></a>Recursos adicionais

- [ Lista de tutoriais sobre como integrar aplicações saas com diretório ativo Azure ](./tutorial-list.md)

- [O que é o acesso à aplicação e um único acesso ao Azure Ative Directory? ](../manage-apps/what-is-single-sign-on.md)

- [O que é o acesso condicional no Azure Active Directory?](../conditional-access/overview.md)

- [Experimente o Campus Café com Azure AD](https://aad.portal.azure.com/)

- [O que é o controlo de sessão no Microsoft Cloud App Security?](/cloud-app-security/proxy-intro-aad)