---
title: 'Tutorial: Azure Ative Directory integração única de sign-on (SSO) com skysite | Microsoft Docs'
description: Saiba como configurar um único sinal entre o Azure Ative Directory e o SKYSITE.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/27/2019
ms.author: jeedes
ms.openlocfilehash: 9d4413c47e40a611fb1559b32aac1e32a71d1998
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "92515989"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-skysite"></a>Tutorial: Azure Ative Directory integração única (SSO) com SKYSITE

Neste tutorial, você vai aprender a integrar SKYSITE com Azure Ative Directory (Azure AD). Quando integra o SKYSITE com a AD Azure, pode:

* Controlo em Azure AD que tem acesso ao SKYSITE.
* Permita que os seus utilizadores sejam automaticamente inscritos no SKYSITE com as suas contas AD Azure.
* Gerencie as suas contas numa localização central - o portal Azure.

Para saber mais sobre a integração da aplicação SaaS com a Azure AD, consulte o que é o acesso à [aplicação e o único sign-on com o Azure Ative Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para começar, precisa dos seguintes itens:

* Uma assinatura AD Azure. Se não tiver uma subscrição, pode obter uma [conta gratuita.](https://azure.microsoft.com/free/)
* Subscrição ativada por SKYSITE (SSO).

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configura e testa Azure AD SSO em um ambiente de teste.

* SKYSITE suporta **IDP** iniciado SSO

* SKYSITE suporta **provisão de** utilizadores just in time

## <a name="adding-skysite-from-the-gallery"></a>Adicionar SKYSITE da galeria

Para configurar a integração do SKYSITE no AD Azure, precisa adicionar SKYSITE da galeria à sua lista de aplicações geridas pelo SaaS.

1. Inscreva-se no [portal Azure](https://portal.azure.com) usando uma conta de trabalho ou escola, ou uma conta pessoal da Microsoft.
1. No painel de navegação à esquerda, selecione o serviço **Azure Ative Directory.**
1. Navegue para **aplicações empresariais** e, em seguida, selecione **Todas as Aplicações**.
1. Para adicionar nova aplicação, selecione **Nova aplicação**.
1. Na secção Adicionar a partir da secção **da galeria,** digite **SKYSITE** na caixa de pesquisa.
1. Selecione **SKYSITE** do painel de resultados e adicione a aplicação. Aguarde alguns segundos enquanto a aplicação é adicionada ao seu inquilino.


## <a name="configure-and-test-azure-ad-single-sign-on-for-skysite"></a>Configurar e testar Azure AD único sinal para SKYSITE

Configure e teste Azure AD SSO com SKYSITE usando um utilizador de teste chamado **B.Simon**. Para que o SSO funcione, é necessário estabelecer uma relação de ligação entre um utilizador AZure AD e o utilizador relacionado no SKYSITE.

Para configurar e testar o Azure AD SSO com o SKYSITE, complete os seguintes blocos de construção:

1. **[Configure Azure AD SSO](#configure-azure-ad-sso)** - para permitir que os seus utilizadores utilizem esta funcionalidade.
    1. Crie um utilizador de **[teste AD Azure](#create-an-azure-ad-test-user)** - para testar um único sinal de Azure com B.Simon.
    1. **[Atribua o utilizador de teste Azure AD](#assign-the-azure-ad-test-user)** - para permitir que b.Simon utilize um único sinal de Ad AD.
1. **[Configure o SKYSITE SSO](#configure-skysite-sso)** - para configurar as definições de inscrição única no lado da aplicação.
    1. **[Create SKYSITE test user](#create-skysite-test-user)** - para ter uma contraparte de B.Simon na SKYSITE que está ligada à representação AD AZure do utilizador.
1. **[Teste SSO](#test-sso)** - para verificar se a configuração funciona.

## <a name="configure-azure-ad-sso"></a>Configurar o SSO do Azure AD

Siga estes passos para ativar o Azure AD SSO no portal Azure.

1. No [portal Azure,](https://portal.azure.com/)na página de integração de aplicações **SKYSITE,** clique no **separador Propriedades** e execute o seguinte passo: 

    ![Propriedades de inscrição única](./media/skysite-tutorial/config05.png)

    * Copie o **URL de acesso** ao Utilizador e terá de colá-lo na secção **Configure SKYSITE SSO**, que é explicada mais tarde no tutorial.

1. Na página de integração de aplicações **SKYSITE,** navegue para **um único s-on**.
1. Na página de método **de inscrição** única, selecione **SAML**.
1. No **set-on único com** a página SAML, clique no ícone edit/pen para **Configuração SAML Básica** para editar as definições.

   ![Editar Configuração BÁSICA SAML](common/edit-urls.png)

1. Na secção **de Configuração Básica SAML,** a aplicação está pré-configurada no modo iniciado pelo **IDP** e os URLs necessários já estão pré-povoados com Azure. O utilizador precisa de guardar a configuração clicando no botão **Guardar.**

1. A aplicação SKYSITE espera as afirmações SAML num formato específico, o que requer que adicione mapeamentos de atributos personalizados à configuração de atributos de token SAML. A imagem que se segue mostra a lista de atributos predefinidos. Clique **em Editar** o ícone para abrir o diálogo dos Atributos do Utilizador.

    ![A screenshot mostra atributos do utilizador com o ícone editar selecionado.](common/edit-attribute.png)

1. Além de acima, a aplicação SKYSITE espera que alguns mais atributos sejam repercutidos na resposta SAML. Na secção **"Atributos & Reclamações** do Utilizador no diálogo **'Pré-visualização)** do Grupo, execute os seguintes passos:

    a. Clique na **caneta** ao lado **de Grupos devolvidos em reivindicação**.

    ![A Screenshot mostra as reclamações do Utilizador com a opção de adicionar uma nova reclamação.](./media/skysite-tutorial/config01.png)

    ![A screenshot mostra a caixa de diálogo de reclamações do utilizador Manage onde pode introduzir os valores descritos.](./media/skysite-tutorial/config02.png)

    b. Selecione **Todos os Grupos** da lista de rádio.

    c. Selecione **Atributo de Origem** do **ID do Grupo**.

    d. Clique em **Guardar**.

1. Na **configuração de um único sessão de inscrição com** a página SAML, na secção **Certificado de Assinatura SAML,** encontre **o Certificado (Base64)** e selecione **Descarregamento** para descarregar o certificado e guardá-lo no seu computador.

    ![O link de descarregamento de certificado](common/certificatebase64.png)

1. Na secção **Configurar SKYSITE,** copie os URL(s) apropriados com base no seu requisito.

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

Nesta secção, você permitirá que B.Simon use a Azure single sign-on, concedendo acesso ao SKYSITE.

1. No portal Azure, selecione **Aplicações empresariais** e, em seguida, selecione **Todas as aplicações**.
1. Na lista de candidaturas, selecione **SKYSITE**.
1. Na página geral da aplicação, encontre a secção **Gerir** e selecione **Utilizadores e grupos**.

   ![A ligação "Utilizadores e grupos"](common/users-groups-blade.png)

1. **Selecione Adicionar utilizador,** em seguida, selecione **Utilizadores e grupos** no diálogo **'Adicionar Atribuição'.**

    ![O link do utilizador adicionar](common/add-assign-user.png)

1. No diálogo **de Utilizadores e grupos,** selecione **B.Simon** da lista de Utilizadores e, em seguida, clique no botão **Select** na parte inferior do ecrã.
1. Se estiver à espera de qualquer valor de função na afirmação SAML, no diálogo **'Fun's Select,** selecione a função adequada para o utilizador da lista e, em seguida, clique no botão **Selecione** na parte inferior do ecrã.
1. No diálogo **'Adicionar Atribuição',** clique no botão **'Atribuir'.**

### <a name="configure-skysite-sso"></a>Configure SKYSITE SSO

1. Abra uma nova janela do navegador web e inscreva-se no site da empresa SKYSITE como administrador e execute os seguintes passos:

4. Clique em **Definições** no lado superior direito da página e, em seguida, navegue para a **definição de Conta**.

    ![A screenshot mostra a definição de conta selecionada a partir de Definições.](./media/skysite-tutorial/config03.png)

5. Mude para o separador **'SSO'** de assinatura única, execute os seguintes passos:

    ![A screenshot mostra o sinal único no separador onde pode introduzir os valores descritos.](./media/skysite-tutorial/config04.png)

    a. No sinal do Fornecedor de Identidade na caixa de texto **URL,** cole o valor do URL de acesso ao **Utilizador,** que copiou a partir do separador **propriedades** no portal Azure.

    b. Clique **no certificado upload**, para carregar o certificado codificado Base64 que descarregou a partir do portal Azure.

    c. Clique em **Guardar**.

### <a name="create-skysite-test-user"></a>Criar utilizador de teste SKYSITE

Nesta secção, um utilizador chamado Britta Simon é criado no SKYSITE. O SKYSITE suporta o provisionamento do utilizador just-in-time, o que é ativado por padrão. Não há nenhum item de ação para si nesta secção. Se um utilizador já não existir no SKYSITE, um novo é criado após a autenticação.

## <a name="test-sso"></a>Teste SSO 

Nesta secção, testa a configuração de inscrição única AZure AD utilizando o Painel de Acesso.

Quando clicar no azulejo SKYSITE no Painel de Acesso, deverá ser automaticamente inscrito no SKYSITE para o qual configura o SSO. Para obter mais informações sobre o Painel de Acesso, consulte [Introdução ao Painel de Acesso.](../user-help/my-apps-portal-end-user-access.md)

## <a name="additional-resources"></a>Recursos adicionais

- [ Lista de tutoriais sobre como integrar aplicações saas com diretório ativo Azure ](./tutorial-list.md)

- [O que é o acesso à aplicação e um único acesso ao Azure Ative Directory? ](../manage-apps/what-is-single-sign-on.md)

- [O que é o acesso condicional no Azure Active Directory?](../conditional-access/overview.md)

- [Experimente SKYSITE com Azure AD](https://aad.portal.azure.com/)