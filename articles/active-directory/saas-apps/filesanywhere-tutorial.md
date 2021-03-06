---
title: 'Tutorial: Integração do Azure Ative Directory com FicheirosOnde | Microsoft Docs'
description: Saiba como configurar um único sinal de inscrição entre o Azure Ative Directory e o FilesAnywhere.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/13/2019
ms.author: jeedes
ms.openlocfilehash: df96fae818ebe4d79c94c993d6e85a1ac48550be
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "92453489"
---
# <a name="tutorial-azure-active-directory-integration-with-filesanywhere"></a>Tutorial: Integração do Azure Ative Directory com FicheirosAnywhere

Neste tutorial, aprende-se a integrar o FilesAnywhere com o Azure Ative Directory (Azure AD).
Integração de FicheirosSOywhere com Azure AD fornece-lhe os seguintes benefícios:

* Pode controlar em Azure AD que tem acesso a FilesAnywhere.
* Pode permitir que os seus utilizadores sejam automaticamente inscritos no FilesAnywhere (Single Sign-On) com as suas contas AD Azure.
* Pode gerir as suas contas numa localização central - o portal Azure.

Se quiser saber mais detalhes sobre a integração da aplicação SaaS com o Azure AD, consulte o que é o acesso à [aplicação e o único acesso ao Azure Ative Directory](../manage-apps/what-is-single-sign-on.md).
Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração AZure AD com o FilesSSem que precisa dos seguintes itens:

* Uma assinatura AD Azure. Se não tiver um ambiente AD Azure, pode ter um mês de julgamento [aqui.](https://azure.microsoft.com/pricing/free-trial/)
* FicheirosQuando uma única subscrição ativada

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configura e testa Azure AD um único sinal de acesso em um ambiente de teste.

* FicheirosNão onde suporta **SP** e **IDP** iniciado SSO

* FicheirosQuando suporta o fornecimento do utilizador **Just In Time**

## <a name="adding-filesanywhere-from-the-gallery"></a>Adicionar FicheirosOnde da galeria

Para configurar a integração de FilesAnywhere em AD Azure, precisa adicionar FilesAnywhere da galeria à sua lista de aplicações geridas para o SaaS.

**Para adicionar FicheirosQuale da galeria, execute os seguintes passos:**

1. No **[portal Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique no ícone **Azure Ative Directory.**

    ![O botão Azure Ative Directory](common/select-azuread.png)

2. Navegue para **Aplicações Empresariais** e, em seguida, selecione a opção **Todas as Aplicações.**

    ![A lâmina de aplicações da Enterprise](common/enterprise-applications.png)

3. Para adicionar nova aplicação, clique em Novo botão de **aplicação** no topo do diálogo.

    ![O novo botão de aplicação](common/add-new-app.png)

4. Na caixa de pesquisa, **digite FicheirosOnde,** selecione **FicheirosOnde partir** do painel de resultados e, em seguida, clique no botão **Adicionar** para adicionar a aplicação.

     ![FicheirosOnde está na lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar Azure AD único sinal de inscrição

Nesta secção, configura e testa o Azure AD com um único sign-on com ficheirosAnywhere baseado num utilizador de teste chamado **Britta Simon**.
Para um único s-on para o trabalho, é necessário estabelecer uma relação de ligação entre um utilizador Azure AD e o utilizador relacionado em Ficheiros Onde é necessário estabelecer.

Para configurar e testar o Azure AD com um único sinal de acesso com ficheirosSSem que, é necessário completar os seguintes blocos de construção:

1. **[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - para permitir que os seus utilizadores utilizem esta funcionalidade.
2. **[Configure ficheirosSQuando se a sign-on única](#configure-filesanywhere-single-sign-on)** - para configurar as definições de Sign-On única no lado da aplicação.
3. Crie um utilizador de **[teste AD Azure](#create-an-azure-ad-test-user)** - para testar um único sinal de Azure com Britta Simon.
4. **[Atribua o utilizador de teste Azure AD](#assign-the-azure-ad-test-user)** - para permitir que Britta Simon utilize um único sinal de Azure.
5. **[Create FilesAnywhere test user](#create-filesanywhere-test-user)** - para ter uma contraparte de Britta Simon em FicheirosSQuando está ligado à representação AD AD do utilizador.
6. **[Teste um único sinal](#test-single-sign-on)** - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar Azure AD único sinal de inscrição

Nesta secção, você ativa a Azure AD um único sinal no portal Azure.

Para configurar o Azure AD um único sinal de inscrição com ficheirosAnywhere, execute os seguintes passos:

1. No [portal Azure](https://portal.azure.com/), na página de integração de aplicações **FilesAnywhere,** selecione **Single sign-on**.

    ![Configurar link único de inscrição](common/select-sso.png)

2. No diálogo do **método de inscrição única,** selecione o modo **SAML/WS-Fed** para ativar um único sinal de súplica.

    ![Único modo de seleção de s-on](common/select-saml-option.png)

3. Na **configuração single Sign-On com página SAML,** clique em **Editar** o ícone para abrir o diálogo **básico de configuração SAML.**

    ![Editar Configuração BÁSICA SAML](common/edit-urls.png)

4. Na secção **de Configuração Básica SAML,** se pretender configurar a aplicação no modo iniciado pelo **IDP,** execute o seguinte passo:

    ![Screenshot que mostra a secção "Configuração Básica S A M L" com o campo "Responder U R L" realçado e o botão "Guardar" selecionado.](common/both-replyurl.png)

    Na caixa de texto **URL de resposta,** digite um URL utilizando o seguinte padrão:  `https://<company name>.filesanywhere.com/saml20.aspx?c=<Client Id>`

5. Clique **em Definir URLs adicionais** e execute o seguinte passo se desejar configurar a aplicação **no** modo iniciado sp:

    ![FicheirosInde que domínio e URLs única informação de súp livre](common/both-signonurl.png)

    Na caixa de texto **URL de entrada de inscrição,** digite um URL utilizando o seguinte padrão:  `https://<sub domain>.filesanywhere.com/`

    > [!NOTE]
    > Estes valores não são reais. Atualize estes valores com o URL de resposta real e Sign-On URL. Contacte [ficheirosA equipa de suporte ao Cliente](mailto:support@FilesAnywhere.com) para obter estes valores. Também pode consultar os padrões indicados na secção **de Configuração BÁSICA SAML** no portal Azure.

6. FicheirosA aplicação onde a aplicação espera as afirmações DE SAML num formato específico, o que requer que adicione mapeamentos de atributos personalizados à configuração de atributos de token SAML. A imagem que se segue mostra a lista de atributos predefinidos. Clique no ícone Editar para adicionar os atributos.

    ![Screenshot que mostra a secção "Atributos do Utilizador" com o botão "Editar" selecionado.](common/edit-attribute.png)

    Quando os utilizadores se inscrevem no FilesOnde obtê-lo do atributo **clienteid** da [equipa FilesOnywhere](mailto:support@FilesAnywhere.com). Tem de adicionar o atributo "ID do cliente" com o valor único fornecido pelo FilesAnywhere.

7. Além de acima, a aplicação FilesAnywhere espera que alguns mais atributos sejam repercutidos na resposta SAML. Na secção **'Reclamações** de Utilizador' no diálogo **'Atributos do Utilizador',** execute os seguintes passos para adicionar o atributoken SAML, tal como mostrado na tabela abaixo:

    | Name | Atributo de origem|
    | ---------------| --------------- |    
    | clientid | *"Valor único"* |

    a. Clique **Em Adicionar nova reivindicação** para abrir o diálogo de reclamações do utilizador **Gerir.**

    ![Screenshot que mostra o diálogo "Reclamações do Utilizador" com "Adicionar nova reivindicação" e "Save" selecionados.](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. Na caixa de texto **'Nome',** digite o nome do atributo indicado para esta linha.

    c. Deixe o **Espaço Namespace** em branco.

    d. Selecione Fonte como **Atributo**.

    e. A partir da lista **de atributos Source,** digite o valor de atributo mostrado para esta linha.

    f. Clique **em Ok**

    exemplo, Clique em **Guardar**.

8. Na **configuração single Sign-On com** a página SAML, na secção **Certificado de Assinatura SAML,** clique em **Baixar** para descarregar o **Certificado (Base64)** das opções dadas de acordo com o seu requisito e guardá-lo no seu computador.

    ![O link de descarregamento de certificado](common/certificatebase64.png)

9. Na secção **Configurar FicheirosNóis onde,** copie os URL(s) apropriados de acordo com o seu requisito.

    ![URLs de configuração de cópia](common/copy-configuration-urls.png)

    a. URL de Inicio de Sessão

    b. Identificador Azure Ad

    c. Logout URL

### <a name="configure-filesanywhere-single-sign-on"></a>Configure ficheirosSem qual Sign-On único

Para configurar um único sign-on em **FicheirosLde lado,** você precisa enviar o Certificado descarregado **(Base64)** e URLs copiados apropriados do portal Azure para a equipa de [suporte FilesAnywhere](mailto:support@FilesAnywhere.com). Eles definem esta definição para ter a ligação SSO SAML corretamente definida em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste AZure AD 

O objetivo desta secção é criar um utilizador de teste no portal Azure chamado Britta Simon.

1. No portal Azure, no painel esquerdo, selecione **Azure Ative Directory**, selecione **Utilizadores**, e, em seguida, selecione **Todos os utilizadores**.

    ![Os links "Utilizadores e grupos" e "Todos os utilizadores"](common/users.png)

2. Selecione **Novo utilizador** na parte superior do ecrã.

    ![Novo botão de utilizador](common/new-user.png)

3. Nas propriedades do Utilizador, execute os seguintes passos.

    ![A caixa de diálogo do utilizador](common/user-properties.png)

    a. No campo **Nome** entra **BrittaSimon**.
  
    b. No tipo de campo **nome de utilizador** **brittasimon \@ yourcompanydomain.extension**  
    Por exemplo, BrittaSimon@contoso.com

    c. Selecione Mostrar caixa de verificação de **palavra-passe** e, em seguida, anotar o valor que é apresentado na caixa de palavra-passe.

    d. Clique em **Criar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o utilizador de teste AZure AD

Nesta secção, você permite que Britta Simon utilize o Azure single sign-on, concedendo acesso a FilesAnywhere.

1. No portal Azure, selecione **Aplicações empresariais**, selecione **Todas as aplicações** e, em seguida, selecione **FicheirosOnde .**

    ![Painel Aplicações empresariais](common/enterprise-applications.png)

2. Na lista de aplicações, **selecione FilesAnywhere**.

    ![O link FilesOnse onde está na lista de Aplicações](common/all-applications.png)

3. No menu à esquerda, selecione **Utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"](common/users-groups-blade.png)

4. Clique no botão **Adicionar utilizador** e, em seguida, selecione **Utilizadores e grupos** no diálogo **'Adicionar Atribuição'.**

    ![O painel de atribuição de adição](common/add-assign-user.png)

5. No diálogo **de Utilizadores e grupos** selecione **Britta Simon** na lista de Utilizadores e, em seguida, clique no botão **Select** na parte inferior do ecrã.

6. Se estiver à espera de qualquer valor de função na afirmação SAML, então no diálogo **'Fun's Select** selecione a função adequada para o utilizador da lista e, em seguida, clique no botão **Selecione** na parte inferior do ecrã.

7. No diálogo **'Adicionar Atribuição'** clique no botão **'Atribuir'.**

### <a name="create-filesanywhere-test-user"></a>Criar FicheirosA utilizador de teste onde está

Nesta secção, um utilizador chamado Britta Simon é criado em FilesAnywhere. FicheirosQuando suporta o fornecimento de utilizadores just-in-time, o que é ativado por padrão. Não há nenhum item de ação para si nesta secção. Se um utilizador já não existir em FicheirosSEmas, um novo é criado após a autenticação.

### <a name="test-single-sign-on"></a>Testar o início de sessão único 

Nesta secção, testa a configuração de inscrição única AZure AD utilizando o Painel de Acesso.

Quando clicar no FicheiroSOnde o azulejo do Painel de Acesso, deverá ser automaticamente inscrito no FicheirosOnde configurar sSO. Para obter mais informações sobre o Painel de Acesso, consulte [Introdução ao Painel de Acesso.](../user-help/my-apps-portal-end-user-access.md)

## <a name="additional-resources"></a>Recursos Adicionais

- [Lista de tutoriais sobre como integrar aplicações saas com diretório ativo Azure](./tutorial-list.md)

- [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

- [O que é Acesso Condicional no Diretório Ativo Azure?](../conditional-access/overview.md)