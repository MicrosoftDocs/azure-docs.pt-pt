---
title: 'Tutorial: Integração do Diretório Ativo Azure com | Nimblex Microsoft Docs'
description: Saiba como configurar um único sign-on entre o Azure Ative Directory e o Nimblex.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/18/2019
ms.author: jeedes
ms.openlocfilehash: 528411f7ef662e90bce98a52b67caa9123fad6ad
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "92522478"
---
# <a name="tutorial-azure-active-directory-integration-with-nimblex"></a>Tutorial: Integração do Diretório Ativo Azure com o Nimblex

Neste tutorial, aprende-se a integrar o Nimblex com o Azure Ative Directory (Azure AD).
A integração do Nimblex com a AD Azure proporciona-lhe os seguintes benefícios:

* Pode controlar em Azure AD que tem acesso ao Nimblex.
* Pode permitir que os seus utilizadores sejam automaticamente inscritos no Nimblex (Single Sign-On) com as suas contas AD Azure.
* Pode gerir as suas contas numa localização central - o portal Azure.

Se quiser saber mais detalhes sobre a integração da aplicação SaaS com o Azure AD, consulte o que é o acesso à [aplicação e o único acesso ao Azure Ative Directory](../manage-apps/what-is-single-sign-on.md).
Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração AZure AD com o Nimblex, precisa dos seguintes itens:

* Uma assinatura AD Azure. Se não tiver um ambiente AD Azure, pode ter um mês de julgamento [aqui.](https://azure.microsoft.com/pricing/free-trial/)
* Subscrição ativa ativada por nimblex

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configura e testa Azure AD um único sinal de acesso em um ambiente de teste.

* Nimblex suporta **SSO** iniciado sp

* Nimblex suporta **provisão de** utilizadores just in time

## <a name="adding-nimblex-from-the-gallery"></a>Adicionar Nimblex da galeria

Para configurar a integração do Nimblex no AD Azure, é necessário adicionar o Nimblex da galeria à sua lista de aplicações geridas pelo SaaS.

**Para adicionar Nimblex da galeria, execute os seguintes passos:**

1. No **[portal Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique no ícone **Azure Ative Directory.**

    ![O botão Azure Ative Directory](common/select-azuread.png)

2. Navegue para **Aplicações Empresariais** e, em seguida, selecione a opção **Todas as Aplicações.**

    ![A lâmina de aplicações da Enterprise](common/enterprise-applications.png)

3. Para adicionar nova aplicação, clique em Novo botão de **aplicação** no topo do diálogo.

    ![O novo botão de aplicação](common/add-new-app.png)

4. Na caixa de pesquisa, **digite Nimblex,** selecione **Nimblex** do painel de resultados e clique em Adicionar o botão **Adicionar** a aplicação.

     ![Nimblex na lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar Azure AD único sinal de inscrição

Nesta secção, você configura e testa Azure AD single sign-on com Nimblex com base em um utilizador de teste chamado **Britta Simon**.
Para um único sinal de trabalho, é necessário estabelecer uma relação de ligação entre um utilizador Azure AD e o utilizador relacionado no Nimblex.

Para configurar e testar o único sinal de Ad AD com Nimblex, você precisa completar os seguintes blocos de construção:

1. **[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - para permitir que os seus utilizadores utilizem esta funcionalidade.
2. **[Configure o Nimblex Single Sign-On](#configure-nimblex-single-sign-on)** - para configurar as definições de Sign-On única no lado da aplicação.
3. Crie um utilizador de **[teste AD Azure](#create-an-azure-ad-test-user)** - para testar um único sinal de Azure com Britta Simon.
4. **[Atribua o utilizador de teste Azure AD](#assign-the-azure-ad-test-user)** - para permitir que Britta Simon utilize um único sinal de Azure.
5. **[Create Nimblex test user](#create-nimblex-test-user)** - para ter uma contraparte de Britta Simon em Nimblex que está ligada à representação AD AD do utilizador.
6. **[Teste um único sinal](#test-single-sign-on)** - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar Azure AD único sinal de inscrição

Nesta secção, você ativa a Azure AD um único sinal no portal Azure.

Para configurar o Azure AD single sign-on com Nimblex, execute os seguintes passos:

1. No [portal Azure](https://portal.azure.com/), na página de integração de aplicações **Nimblex,** selecione **Single sign-on**.

    ![Configurar link único de inscrição](common/select-sso.png)

2. No diálogo do **método de inscrição única,** selecione o modo **SAML/WS-Fed** para ativar um único sinal de súplica.

    ![Único modo de seleção de s-on](common/select-saml-option.png)

3. Na **configuração Single Sign-On com** a página SAML, clique em **Editar** o ícone para abrir o diálogo **básico de configuração SAML.**

    ![Editar Configuração BÁSICA SAML](common/edit-urls.png)

4. Na secção **de Configuração Básica SAML,** execute os seguintes passos:

    ![Informações de súmblex domain e URLs únicas](common/sp-identifier-reply.png)

    a. Na caixa de texto **URL de entrada de inscrição,** digite um URL utilizando o seguinte padrão: `https://<YOUR APPLICATION PATH>/Login.aspx`

    b. Na caixa **identifier,** digite um URL utilizando o seguinte padrão: `https://<YOUR APPLICATION PATH>/`

    c. Na caixa de texto **URL de resposta,** digite um URL utilizando o seguinte padrão: `https://<path-to-application>/SamlReply.aspx`

    > [!NOTE]
    > Estes valores não são reais. Atualize estes valores com o URL de Sign-On real, identifier e responder URL. Contacte [a equipa de suporte do Cliente Nimblex](mailto:support@ebms.com.au) para obter estes valores. Também pode consultar os padrões indicados na secção **de Configuração BÁSICA SAML** no portal Azure.

4. Na **configuração Single Sign-On com** a página SAML, na secção **Certificado de Assinatura SAML,** clique em **Baixar** para descarregar o **Certificado (Base64)** das opções dadas de acordo com o seu requisito e guardá-lo no seu computador.

    ![O link de descarregamento de certificado](common/certificatebase64.png)

6. Na secção **Configurar Nimblex,** copie os URL(s) apropriados de acordo com o seu requisito.

    ![URLs de configuração de cópia](common/copy-configuration-urls.png)

    a. URL de Inicio de Sessão

    b. Identificador de Azure Ad

    c. Logout URL

### <a name="configure-nimblex-single-sign-on"></a>Configurar Sign-On Single Nimblex

1. Numa janela diferente do navegador web, inscreva-se no Nimblex como Administrador de Segurança.

2. No lado superior direito da página, clique no logotipo **Definições.**

    ![A screenshot mostra o ícone Definições.](./media/nimblex-tutorial/tutorial_nimblex_settings.png)

3. Na página **do Painel de Controlo,** na secção **de Segurança** clique **em 'S.Sign-on' único**.

    ![A screenshot mostra o único sinal de inscrição selecionado no menu Security.](./media/nimblex-tutorial/tutorial_nimblex_single.png)

4. Na página **'Gerir',** selecione o nome da sua instância e clique em **Editar.**

    ![A screenshot mostra gerir o Single Sign-On onde pode selecionar Editar.](./media/nimblex-tutorial/tutorial_nimblex_saml.png)

5. Na página **Editar SSO Provider,** execute os seguintes passos:

    ![A screenshot mostra editar S O Provider onde pode introduzir os valores descritos.](./media/nimblex-tutorial/tutorial_nimblex_sso.png)

    a. Na caixa de texto **Descrição,** digite o nome da sua instância.

    b. No Bloco de Notas, abra o certificado codificado base-64 que descarregou a partir do portal Azure, copie o seu conteúdo e, em seguida, cole-o na caixa **de Certificado.**

    c. Na caixa de texto **do Url do Fornecedor de Identidade Sso Target,** cole o valor do URL de **login,** que copiou a partir do portal Azure.

    d. Clique em **Guardar**.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste AZure AD 

O objetivo desta secção é criar um utilizador de teste no portal Azure chamado Britta Simon.

1. No portal Azure, no painel esquerdo, selecione **Azure Ative Directory**, selecione **Utilizadores**, e, em seguida, selecione **Todos os utilizadores**.

    ![Os links "Utilizadores e grupos" e "Todos os utilizadores"](common/users.png)

2. Selecione **Novo utilizador** na parte superior do ecrã.

    ![Novo botão de utilizador](common/new-user.png)

3. Nas propriedades do Utilizador, execute os seguintes passos.

    ![A caixa de diálogo do utilizador](common/user-properties.png)

    a. No campo **Nome,** **insira BrittaSimon**.
  
    b. No campo **nome do utilizador,** escreva **brittasimon@yourcompanydomain.extension**  
    Por exemplo, BrittaSimon@contoso.com

    c. Selecione Mostrar caixa de verificação de **palavra-passe** e, em seguida, anotar o valor que é apresentado na caixa de palavra-passe.

    d. Clique em **Criar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o utilizador de teste AZure AD

Nesta secção, você permite que Britta Simon use Azure single sign-on, concedendo acesso ao Nimblex.

1. No portal Azure, selecione **Aplicações empresariais**, selecione **Todas as aplicações** e, em seguida, selecione **Nimblex**.

    ![Painel Aplicações empresariais](common/enterprise-applications.png)

2. Na lista de candidaturas, selecione **Nimblex**.

    ![O link Nimblex na lista de Aplicações](common/all-applications.png)

3. No menu à esquerda, selecione **Utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"](common/users-groups-blade.png)

4. Clique no botão **Adicionar utilizador** e, em seguida, selecione **Utilizadores e grupos** no diálogo **'Adicionar Atribuição'.**

    ![O painel de atribuição de adição](common/add-assign-user.png)

5. No diálogo **de Utilizadores e grupos** selecione **Britta Simon** na lista de Utilizadores e, em seguida, clique no botão **Select** na parte inferior do ecrã.

6. Se estiver à espera de qualquer valor de função na afirmação SAML, então no diálogo **'Escolha' de Função,** selecione a função adequada para o utilizador da lista e, em seguida, clique no botão **Selecione** na parte inferior do ecrã.

7. No diálogo **'Adicionar Atribuição',** clique no botão **'Atribuir'.**

### <a name="create-nimblex-test-user"></a>Criar utilizador de teste Nimblex

Nesta secção, um utilizador chamado Britta Simon é criado em Nimblex. O Nimblex suporta o provisionamento do utilizador just-in-time, o que é ativado por padrão. Não há nenhum item de ação para si nesta secção. Se um utilizador já não existir no Nimblex, um novo é criado após a autenticação.

>[!Note]
>Se precisar de criar um utilizador manualmente, contacte a [equipa de suporte do Cliente Nimblex.](mailto:support@ebms.com.au)

### <a name="test-single-sign-on"></a>Testar o início de sessão único 

Nesta secção, testa a configuração de inscrição única AZure AD utilizando o Painel de Acesso.

Quando clicar no azulejo Nimblex no Painel de Acesso, deverá ser automaticamente inscrito no Nimblex para o qual configura o SSO. Para obter mais informações sobre o Painel de Acesso, consulte [Introdução ao Painel de Acesso.](../user-help/my-apps-portal-end-user-access.md)

## <a name="additional-resources"></a>Recursos Adicionais

- [Lista de tutoriais sobre como integrar aplicações saas com diretório ativo Azure](./tutorial-list.md)

- [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

- [O que é Acesso Condicional no Diretório Ativo Azure?](../conditional-access/overview.md)