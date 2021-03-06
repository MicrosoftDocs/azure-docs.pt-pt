---
title: 'Tutorial: Azure Ative Directory integração única de sign-on (SSO) com a Plataforma de Segurança da Aplicação Cequence | Microsoft Docs'
description: Saiba como configurar um único sinal entre o Azure Ative Directory e a Plataforma de Segurança da Aplicação cequence.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 11/25/2020
ms.author: jeedes
ms.openlocfilehash: 967ad66434a09252c1b7afa195facee20279f4dd
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "98735285"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-cequence-application-security-platform"></a>Tutorial: Azure Ative Directy integração única (SSO) com a Plataforma de Segurança da Aplicação Cequence

Neste tutorial, você vai aprender como integrar a Plataforma de Segurança de Aplicações Cequence com O Diretório Ativo Azure (Azure AD). Quando integrar a Plataforma de Segurança da Aplicação Cequence com a AD Azure, pode:

* Control em Azure AD que tem acesso à Plataforma de Segurança de Aplicações Cequence.
* Capacitar os seus utilizadores a serem automaticamente inscritos na Plataforma de Segurança da Aplicação Cequence com as suas contas AD Azure.
* Gerencie as suas contas numa localização central - o portal Azure.

## <a name="prerequisites"></a>Pré-requisitos

Para começar, precisa dos seguintes itens:

* Uma assinatura AD Azure. Se não tiver uma subscrição, pode obter uma [conta gratuita.](https://azure.microsoft.com/free/)
* Cequence Application Security Platform single sign-on (SSO) enabled subscrição.

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configura e testa Azure AD SSO em um ambiente de teste.

* Plataforma de Segurança de Aplicações cequence suporta **SSO** iniciado SP

* Plataforma de Segurança de Aplicações cequence suporta **provisão de** utilizadores just in time


## <a name="adding-cequence-application-security-platform-from-the-gallery"></a>Adicionar Plataforma de Segurança de Aplicações cequence da galeria

Para configurar a integração da Plataforma de Segurança da Aplicação Cequence em AD Azure, é necessário adicionar a Plataforma de Segurança da Aplicação Cequence da galeria à sua lista de aplicações geridas para o SaaS.

1. Inscreva-se no portal Azure usando uma conta de trabalho ou escola, ou uma conta pessoal da Microsoft.
1. No painel de navegação à esquerda, selecione o serviço **Azure Ative Directory.**
1. Navegue para **aplicações empresariais** e, em seguida, selecione **Todas as Aplicações**.
1. Para adicionar nova aplicação, selecione **Nova aplicação**.
1. Na secção Adicionar a partir da secção **de galeria,** digite a Plataforma de **Segurança da Aplicação Cequence** na caixa de pesquisa.
1. Selecione a Plataforma de Segurança da **Aplicação Cequence** do painel de resultados e adicione a aplicação. Aguarde alguns segundos enquanto a aplicação é adicionada ao seu inquilino.


## <a name="configure-and-test-azure-ad-sso-for-cequence-application-security-platform"></a>Configurar e testar Azure AD SSO para a Plataforma de Segurança da Aplicação cequence

Configure e teste Azure AD SSO com Plataforma de Segurança da Aplicação Cequence usando um utilizador de teste chamado **B.Simon**. Para que o SSO funcione, é necessário estabelecer uma relação de ligação entre um utilizador Azure AD e o utilizador relacionado na Plataforma de Segurança da Aplicação Cequence.

Para configurar e testar o Azure AD SSO com a Plataforma de Segurança da Aplicação Cequence, execute os seguintes passos:

1. **[Configure Azure AD SSO](#configure-azure-ad-sso)** - para permitir que os seus utilizadores utilizem esta funcionalidade.
    1. Crie um utilizador de **[teste AD Azure](#create-an-azure-ad-test-user)** - para testar um único sinal de Azure com B.Simon.
    1. **[Atribua o utilizador de teste Azure AD](#assign-the-azure-ad-test-user)** - para permitir que b.Simon utilize um único sinal de Ad AD.
1. **[Configure a Plataforma de Segurança da Aplicação de Cequence SSO](#configure-cequence-application-security-platform-sso)** - para configurar as definições de inscrição única no lado da aplicação.
    1. Crie o utilizador de teste da Plataforma de **[Segurança da Aplicação Cequence](#create-cequence-application-security-platform-test-user)** - para ter uma contrapartida de B.Simon na Plataforma de Segurança da Aplicação Cequence que está ligada à representação AD AD do utilizador.
1. **[Teste SSO](#test-sso)** - para verificar se a configuração funciona.

## <a name="configure-azure-ad-sso"></a>Configurar o SSO do Azure AD

Siga estes passos para ativar o Azure AD SSO no portal Azure.

1. No portal Azure, na página de integração da plataforma de segurança da **aplicação Cequence,** encontre a secção **Gerir** e selecione **um único sinal de sação**.
1. Na página de método **de inscrição** única, selecione **SAML**.
1. No **set-on único com** a página SAML, clique no ícone edit/pen para **Configuração SAML Básica** para editar as definições.

   ![Editar Configuração BÁSICA SAML](common/edit-urls.png)

1. Na secção **Configuração Básica SAML,** insira os valores para os seguintes campos:

    a. Na caixa de texto **URL, digite** um URL utilizando o seguinte padrão: `https://<CUSTOMERNAME>.s.cequence.cloud`

    b. Na caixa de texto **identifier (Entity ID),** digite um URL utilizando o seguinte padrão: `https://<CUSTOMERNAME>.s.cequence.cloud:443/saml/metadata`

    > [!NOTE]
    > Estes valores não são reais. Atualize estes valores com o sinal real no URL e no identificador. Contacte [a equipa de suporte do cliente da Plataforma de Segurança da Aplicação Cequence](mailto:support@cequence.ai) para obter estes valores. Também pode consultar os padrões indicados na secção **de Configuração BÁSICA SAML** no portal Azure.

1. A aplicação Da Plataforma de Segurança da Aplicação cequence espera as afirmações DO SAML num formato específico, o que requer que adicione mapeamentos de atributos personalizados à configuração de atributos de token SAML. A imagem que se segue mostra a lista de atributos predefinidos.

    ![image](common/default-attributes.png)

1. Além de acima, a aplicação Da Plataforma de Segurança de Aplicações Cequence espera que alguns mais atributos sejam repercutidos na resposta SAML que são mostrados abaixo. Estes atributos também são pré-povoados, mas pode revê-los de acordo com os seus requisitos.
    
    | Name |  Atributo de origem|
    | --------------- | --------- |
    | Group | utilizador.grupos |

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

Nesta secção, você permitirá que B.Simon use a Azure single sign-on, concedendo acesso à Plataforma de Segurança da Aplicação Cequence.

1. No portal Azure, selecione **Aplicações empresariais** e, em seguida, selecione **Todas as aplicações**.
1. Na lista de aplicações, selecione **Plataforma de Segurança da Aplicação Cequence**.
1. Na página geral da aplicação, encontre a secção **Gerir** e selecione **Utilizadores e grupos**.
1. **Selecione Adicionar utilizador,** em seguida, selecione **Utilizadores e grupos** no diálogo **'Adicionar Atribuição'.**
1. No diálogo **de Utilizadores e grupos,** selecione **B.Simon** da lista de Utilizadores e, em seguida, clique no botão **Select** na parte inferior do ecrã.
1. Se estiver à espera que uma função seja atribuída aos utilizadores, pode selecioná-la a partir do Dropdown de **função** Select. Se não tiver sido configurada qualquer função para esta aplicação, vê a função "Acesso Predefinido" selecionada.
1. No diálogo **'Adicionar Atribuição',** clique no botão **'Atribuir'.**

## <a name="configure-cequence-application-security-platform-sso"></a>Configure plataforma de segurança de aplicação de cequence SSO

Para configurar um único sinal no lado da Plataforma de **Segurança da Aplicação cequence,** é necessário enviar o **Url de Metadados da Federação de Aplicações** para a equipa de [suporte da Plataforma de Segurança da Aplicação cequence](mailto:support@cequence.ai). Eles definem esta definição para ter a ligação SSO SAML corretamente definida em ambos os lados.

### <a name="create-cequence-application-security-platform-test-user"></a>Criar utilizador de teste da Plataforma de Segurança da Aplicação Cequence

Nesta secção, um utilizador chamado Britta Simon é criado na Plataforma de Segurança da Aplicação Cequence. A Plataforma de Segurança da Aplicação cequence suporta o fornecimento de utilizadores just-in-time, o que é ativado por padrão. Não há nenhum item de ação para si nesta secção. Se um utilizador ainda não existir na Plataforma de Segurança da Aplicação Cequence, um novo é criado após a autenticação.

## <a name="test-sso"></a>Teste SSO 

Nesta secção, testa a configuração de um único sinal de inscrição Azure AD com as seguintes opções. 

1. Clique em **Testar esta aplicação** no portal Azure. Isto irá redirecionar para o URL de acesso da plataforma de segurança da aplicação cequence, onde pode iniciar o fluxo de login. 

2. Vá diretamente ao URL de segurança da plataforma de segurança da aplicação cequence e inicie o fluxo de login a partir daí.

3. Pode utilizar o Microsoft Access Panel. Quando clicar no azulejo da Plataforma de Segurança da Aplicação Cequence no Painel de Acesso, este irá redirecionar para o URL de acesso da Plataforma de Segurança da Aplicação Cequence. Para obter mais informações sobre o Painel de Acesso, consulte [Introdução ao Painel de Acesso.](../user-help/my-apps-portal-end-user-access.md)


## <a name="next-steps"></a>Passos seguintes

Uma vez configurada a Plataforma de Segurança de Aplicações de Cequence, pode impor o controlo da sessão, que protege a exfiltração e infiltração dos dados sensíveis da sua organização em tempo real. O controlo da sessão estende-se desde o Acesso Condicional. [Saiba como impor o controlo da sessão com o Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).