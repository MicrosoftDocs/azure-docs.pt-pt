---
title: 'Tutorial: Azure Ative Directory integração única (SSO) com visivelmente | Microsoft Docs'
description: Saiba como configurar um único sinal entre o Azure Ative Directory e o Visivelmente.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/14/2020
ms.author: jeedes
ms.openlocfilehash: de41e1ef021ea3da2b3a5bef4268469aab448a21
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "92637943"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-visibly"></a>Tutorial: Azure Ative Directory integração única (SSO) com visivelmente

Neste tutorial, você vai aprender a integrar visivelmente com Azure Ative Direy (Azure AD). Quando se integra visivelmente com Azure AD, pode:

* Controlo em Azure AD que tem acesso visivelmente.
* Ative os seus utilizadores a serem automaticamente inscritos na Visivelmente com as suas contas AD Azure.
* Gerencie as suas contas numa localização central - o portal Azure.

Para saber mais sobre a integração da aplicação SaaS com a Azure AD, consulte o que é o acesso à [aplicação e o único sign-on com o Azure Ative Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para começar, precisa dos seguintes itens:

* Uma assinatura AD Azure. Se não tiver uma subscrição, pode obter uma [conta gratuita.](https://azure.microsoft.com/free/)
* Assinatura ativada visivelmente única (SSO).

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configura e testa Azure AD SSO em um ambiente de teste.

* Apoia visivelmente **o SSO** iniciado pela SP

* Uma vez configurado Visivelmente, pode impor o controlo da sessão, que protege a exfiltração e infiltração dos dados sensíveis da sua organização em tempo real. O controlo da sessão estende-se desde o Acesso Condicional. [Saiba como impor o controlo da sessão com o Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-visibly-from-the-gallery"></a>Adicionando Visivelmente da galeria

Para configurar a integração de Visivelmente em Azure AD, é necessário adicionar Visivelmente da galeria à sua lista de aplicações geridas pelo SaaS.

1. Inscreva-se no [portal Azure](https://portal.azure.com) usando uma conta de trabalho ou escola, ou uma conta pessoal da Microsoft.
1. No painel de navegação à esquerda, selecione o serviço **Azure Ative Directory.**
1. Navegue para **aplicações empresariais** e, em seguida, selecione **Todas as Aplicações**.
1. Para adicionar nova aplicação, selecione **Nova aplicação**.
1. Na secção Adicionar a partir da secção **da galeria,** **escreva visivelmente** na caixa de pesquisa.
1. Selecione **Visivelmente** do painel de resultados e adicione a aplicação. Aguarde alguns segundos enquanto a aplicação é adicionada ao seu inquilino.


## <a name="configure-and-test-azure-ad-sso-for-visibly"></a>Configure e teste Azure AD SSO para Visivelmente

Configure e teste Azure AD SSO com visivelmente usando um utilizador de teste chamado **B.Simon**. Para que o SSO funcione, é necessário estabelecer uma relação de ligação entre um utilizador Azure AD e o utilizador relacionado em Visivelmente.

Para configurar e testar a Azure AD SSO com visivelmente, complete os seguintes blocos de construção:

1. **[Configure Azure AD SSO](#configure-azure-ad-sso)** - para permitir que os seus utilizadores utilizem esta funcionalidade.
    1. Crie um utilizador de **[teste AD Azure](#create-an-azure-ad-test-user)** - para testar um único sinal de Azure com B.Simon.
    1. **[Atribua o utilizador de teste Azure AD](#assign-the-azure-ad-test-user)** - para permitir que b.Simon utilize um único sinal de Ad AD.
1. **[Configure visivelmente SSO](#configure-visibly-sso)** - para configurar as definições de inscrição única no lado da aplicação.
    1. **[Create Visivelmente utilizador de teste](#create-visibly-test-user)** - para ter uma contraparte de B.Simon em Visivelmente que esteja ligada à representação AD AD do utilizador.
1. **[Teste SSO](#test-sso)** - para verificar se a configuração funciona.

## <a name="configure-azure-ad-sso"></a>Configurar o SSO do Azure AD

Siga estes passos para ativar o Azure AD SSO no portal Azure.

1. No [portal Azure,](https://portal.azure.com/)na página de integração da aplicação **Visivelmente,** encontre a secção **Gerir** e selecione **um único sinal de sação**.
1. Na página de método **de inscrição** única, selecione **SAML**.
1. No **set-on único com** a página SAML, clique no ícone edit/pen para **Configuração SAML Básica** para editar as definições.

   ![Editar Configuração BÁSICA SAML](common/edit-urls.png)

1. Na secção **Configuração Básica SAML,** insira os valores para os seguintes campos:

    a. Na caixa de texto **URL de entrada de inscrição,** digite o URL: `https://app.visibly.io/`

    b. Na caixa de texto **URL de resposta,** digite o URL: `https://api.visibly.io/api/v1/verifyResponse`


1. A aplicação espera visivelmente as afirmações DO SAML num formato específico, o que requer que adicione mapeamentos de atributos personalizados à configuração de atributos de token SAML. A imagem que se segue mostra a lista de atributos predefinidos.

    ![image](common/default-attributes.png)

1. Além de acima, a aplicação Visivelmente espera que alguns mais atributos sejam repercutidos na resposta SAML que são mostrados abaixo. Estes atributos também são pré-povoados, mas pode revê-los de acordo com os seus requisitos.
    
    | Name |  Atributo de origem|
    | ----------- | --------- |
    | city | user.city |
    | apelido | utilizador.sobrenome |
    | state | user.state |
    | departamento | user.department |
    | e-mail | user.mail |

1. Na **configuração de um único sessão de inscrição com** a página SAML, na secção **Certificado de Assinatura SAML,** encontre **o Certificado (Base64)** e selecione **Descarregamento** para descarregar o certificado e guardá-lo no seu computador.

    ![O link de descarregamento de certificado](common/certificatebase64.png)

1. Na secção **Configurar Visivelmente,** copie os URL(s) apropriados com base no seu requisito.

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

Nesta secção, você permitirá que B.Simon use a Azure single sign-on, concedendo acesso a Visivelmente.

1. No portal Azure, selecione **Aplicações empresariais** e, em seguida, selecione **Todas as aplicações**.
1. Na lista de candidaturas, selecione **Visivelmente**.
1. Na página geral da aplicação, encontre a secção **Gerir** e selecione **Utilizadores e grupos**.

   ![A ligação "Utilizadores e grupos"](common/users-groups-blade.png)

1. **Selecione Adicionar utilizador,** em seguida, selecione **Utilizadores e grupos** no diálogo **'Adicionar Atribuição'.**

    ![O link do utilizador adicionar](common/add-assign-user.png)

1. No diálogo **de Utilizadores e grupos,** selecione **B.Simon** da lista de Utilizadores e, em seguida, clique no botão **Select** na parte inferior do ecrã.
1. Se estiver à espera de qualquer valor de função na afirmação SAML, no diálogo **'Fun's Select,** selecione a função adequada para o utilizador da lista e, em seguida, clique no botão **Selecione** na parte inferior do ecrã.
1. No diálogo **'Adicionar Atribuição',** clique no botão **'Atribuir'.**

## <a name="configure-visibly-sso"></a>Configurar Visivelmente SSO

1. Faça login em Visivelmente usando as suas credenciais.

1. Navegue para a opção **de definições** a partir do menu de navegação.

    ![A screenshot mostra a opção de definições selecionada.](./media/visibly-tutorial/settings.png)

1. Clique em **Integrações** dentro de Definições.

    ![O Screenshot mostra integrações selecionadas no menu Definições.](./media/visibly-tutorial/integrations.png)

1. Nas **Integrações,** selecione **SSO**.

    ![A screenshot mostra S S O selecionado a partir das Integrações.](./media/visibly-tutorial/sso.png)

1. Execute os seguintes passos na página seguinte.

    ![A screenshot mostra a página de Integração S S O onde pode introduzir os valores descritos.](./media/visibly-tutorial/configuration.png)

    a. Na **caixa de** texto iD da Entidade, cole o valor **de ID** da Entidade que copiou do portal Azure.

    b. Na caixa de texto **SSO URL,** cole o valor URL de **login** que copiou a partir do portal Azure.

    c. Na caixa de texto do **nome SSO,** forneça qualquer nome válido.

    d. Abra o Certificado descarregado **(Base64)** do portal Azure para o Bloco de Notas e cole o conteúdo na caixa de texto **do Certificado** ou também pode fazer o upload do **Certificado** selecionando o Certificado **de Upload**.

    e. Clique em **Guardar**

### <a name="create-visibly-test-user"></a>Criar utilizador de teste visivelmente

Nesta secção, um utilizador chamado B.Simon é criado em Visivelmente. Suporta visivelmente o provisionamento do utilizador just-in-time, o que é ativado por padrão. Não há nenhum item de ação para si nesta secção. Se um utilizador já não existir em Visivelmente, um novo é criado após a autenticação.

## <a name="test-sso"></a>Teste SSO 

Nesta secção, testa a configuração de inscrição única AZure AD utilizando o Painel de Acesso.

Quando clicar no azulejo visivelmente no Painel de Acesso, deverá ser automaticamente inscrito no Visivelmente para o qual configura sSO. Para obter mais informações sobre o Painel de Acesso, consulte [Introdução ao Painel de Acesso.](../user-help/my-apps-portal-end-user-access.md)

## <a name="additional-resources"></a>Recursos adicionais

- [ Lista de tutoriais sobre como integrar aplicações saas com diretório ativo Azure ](./tutorial-list.md)

- [O que é o acesso à aplicação e um único acesso ao Azure Ative Directory? ](../manage-apps/what-is-single-sign-on.md)

- [O que é o acesso condicional no Azure Active Directory?](../conditional-access/overview.md)

- [Experimente visivelmente com Azure AD](https://aad.portal.azure.com/)

- [O que é o controlo de sessão no Microsoft Cloud App Security?](/cloud-app-security/proxy-intro-aad)