---
title: 'Tutorial: Azure Ative Directory integração única (SSO) com monday.com | Microsoft Docs'
description: Saiba como configurar um único sign-on entre o Azure Ative Directory e o monday.com.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/08/2021
ms.author: jeedes
ms.openlocfilehash: 06f4ddff9c897858eec27f97f8e45a3d656e951d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "101650792"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-mondaycom"></a>Tutorial: Azure Ative Directory integração única (SSO) com monday.com

Neste tutorial, você vai aprender a integrar monday.com com Azure Ative Directory (Azure AD). Quando integra monday.com com a AD Azure, pode:

* Controlo em Azure AD que tem acesso a monday.com.
* Ative os seus utilizadores a serem automaticamente inscritos para monday.com com as suas contas AD Azure.
* Gerencie as suas contas numa localização central - o portal Azure.

## <a name="prerequisites"></a>Pré-requisitos

Para começar, precisa dos seguintes itens:

* Uma assinatura AD Azure. Se não tiver uma subscrição, pode obter uma [conta gratuita.](https://azure.microsoft.com/free/)
* monday.com subscrição ativada por um único sso (SSO).

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configura e testa Azure AD SSO em um ambiente de teste.

* monday.com apoia SP **e IDP** iniciaM SSO
* monday.com suporta o fornecimento **de utilizadores just in time**

## <a name="add-mondaycom-from-the-gallery"></a>Adicione monday.com da galeria

Para configurar a integração de monday.com no AD Azure, é necessário adicionar monday.com da galeria à sua lista de aplicações geridas pelo SaaS.

1. Inscreva-se no portal Azure usando uma conta de trabalho ou escola, ou uma conta pessoal da Microsoft.
1. No painel de navegação à esquerda, selecione o serviço **Azure Ative Directory.**
1. Navegue para **aplicações empresariais** e, em seguida, selecione **Todas as Aplicações**.
1. Para adicionar nova aplicação, selecione **Nova aplicação**.
1. Na secção Adicionar a partir da secção **da galeria,** escreva **monday.com** na caixa de pesquisa.
1. Selecione **monday.com** do painel de resultados e adicione a aplicação. Aguarde alguns segundos enquanto a aplicação é adicionada ao seu inquilino.

## <a name="configure-and-test-azure-ad-sso-for-mondaycom"></a>Configure e teste Azure AD SSO para monday.com

Configure e teste Azure AD SSO com monday.com usando um utilizador de teste chamado **B.Simon**. Para que o SSO funcione, é necessário estabelecer uma relação de ligação entre um utilizador Azure AD e o utilizador relacionado em monday.com.

Para configurar e testar a Azure AD SSO com monday.com, execute os seguintes passos:

1. **[Configure Azure AD SSO](#configure-azure-ad-sso)** - para permitir que os seus utilizadores utilizem esta funcionalidade.
    1. Crie um utilizador de **[teste AD Azure](#create-an-azure-ad-test-user)** - para testar um único sinal de Azure com B.Simon.
    1. **[Atribua o utilizador de teste Azure AD](#assign-the-azure-ad-test-user)** - para permitir que b.Simon utilize um único sinal de Ad AD.
1. **[Configure monday.com SSO](#configure-mondaycom-sso)** - para configurar as definições de inscrição única no lado da aplicação.
    1. **[Crie monday.com utilizador de teste](#create-mondaycom-test-user)** - para ter uma contraparte de B.Simon em monday.com que está ligada à representação AD AD do utilizador.
1. **[Teste SSO](#test-sso)** - para verificar se a configuração funciona.

## <a name="configure-azure-ad-sso"></a>Configurar o SSO do Azure AD

Siga estes passos para ativar o Azure AD SSO no portal Azure.

1. No portal Azure, na página de integração de aplicações **monday.com,** encontre a secção **Gerir** e selecione **um único sinal de sação**.
1. Na página de método **de inscrição** única, selecione **SAML**.
1. No **set-on único com** a página SAML, clique no ícone de lápis para **configuração SAML Básica** para editar as definições.

   ![Editar Configuração BÁSICA SAML](common/edit-urls.png)

1. Na secção **de Configuração Básica SAML,** se tiver **um ficheiro de metadados do Fornecedor de Serviços** e desejar configurar no modo iniciado pelo **IDP,** execute os seguintes passos:

    a. Clique **em Carregar o ficheiro de metadados**.

    ![Carregar ficheiro de metadados](common/upload-metadata.png)

    b. Clique no **logotipo da pasta** para selecionar o ficheiro de metadados e clique em **Upload**.

    ![escolher arquivo de metadados](common/browse-upload-metadata.png)

    c. Após o carregamento com sucesso do ficheiro de metadados, os valores de URL **de identificação** e **resposta** são preenchidos automaticamente na secção de Configuração BÁSICA SAML.

    > [!Note]
    > Se os valores de URL **de identificação** e **resposta** não forem povoados automaticamente, preencha os valores manualmente. O **Identificador** e o **URL de resposta** são os mesmos e o valor está no seguinte padrão: `https://<your-domain>.monday.com/saml/saml_callback`

1. Clique **em Definir URLs adicionais** e execute o seguinte passo se desejar configurar a aplicação **no** modo iniciado sp:

    Na caixa de texto **URL de entrada de inscrição,** digite um URL utilizando o seguinte padrão:  `https://<YOUR_DOMAIN>.monday.com`

    > [!NOTE]
    > Estes valores não são reais. Atualize estes valores com o identificador real, URL de resposta e URL Sign-On. Contacte [monday.com equipa de apoio ao Cliente](https://monday.com/contact-us/) para obter estes valores. Também pode consultar os padrões indicados na secção **de Configuração BÁSICA SAML** no portal Azure.

1. monday.com aplicação espera as afirmações DE SAML num formato específico, o que requer que adicione mapeamentos de atributos personalizados à configuração de atributos de token SAML. A imagem que se segue mostra a lista de atributos predefinidos.

    ![A screenshot mostra atributos do utilizador & Reclamações com valores predefinidos, tais como givenname user.givenname e Emailaddress User.mail.](common/default-attributes.png)

1. Além de acima, monday.com aplicação espera que alguns mais atributos sejam repercutidos na resposta SAML que são mostrados abaixo. Estes atributos também são pré-povoados, mas pode revê-los de acordo com os seus requisitos.

    | Name | Atributo de origem |
    |--|--|
    | E-mail | user.mail |
    | FirstName | user.givenname |
    | LastName | utilizador.sobrenome |

1. Na **configuração de um único sessão de inscrição com** a página SAML, na secção **Certificado de Assinatura SAML,** encontre **o Certificado (Base64)** e selecione **Descarregamento** para descarregar o certificado e guardá-lo no seu computador.

    ![O link de descarregamento de certificado](common/certificatebase64.png)

1. Na secção **Configuração monday.com,** copie os URL(s) apropriados com base no seu requisito.

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

Nesta secção, você permitirá que B.Simon use a Azure single sign-on, permitindo o acesso a monday.com.

1. No portal Azure, selecione **Aplicações empresariais** e, em seguida, selecione **Todas as aplicações**.
1. Na lista de candidaturas, selecione **monday.com**.
1. Na página geral da aplicação, encontre a secção **Gerir** e selecione **Utilizadores e grupos**.
1. **Selecione Adicionar utilizador,** em seguida, selecione **Utilizadores e grupos** no diálogo **'Adicionar Atribuição'.**
1. No diálogo **de Utilizadores e grupos,** selecione **B.Simon** da lista de Utilizadores e, em seguida, clique no botão **Select** na parte inferior do ecrã.
1. Se estiver à espera que uma função seja atribuída aos utilizadores, pode selecioná-la a partir do Dropdown de **função** Select. Se não tiver sido configurada qualquer função para esta aplicação, vê a função "Acesso Predefinido" selecionada.
1. No diálogo **'Adicionar Atribuição',** clique no botão **'Atribuir'.**

## <a name="configure-mondaycom-sso"></a>Configurar monday.com SSO

1. Para automatizar a configuração dentro de monday.com, é necessário instalar a extensão do **navegador 'As aplicações' Secure Sign-in** clicando **em instalar a extensão**.

    ![Extensão das minhas aplicações](common/install-myappssecure-extension.png)

1. Depois de adicionar extensão ao navegador, clique em **Configurar monday.com** que o direcionará para a aplicação monday.com. A partir daí, forneça as credenciais de administrador para assinar monday.com. A extensão do navegador configurará automaticamente a aplicação para si e automatizará os passos 3-6.

    ![Configuração de configuração](common/setup-sso.png)

1. Se pretender configurar monday.com manualmente, abra uma nova janela do navegador web e inscreva-se para monday.com como administrador e execute os seguintes passos:

1. Vá ao **Perfil** no canto superior direito da página e clique no **Administrador**.

    ![A screenshot mostra o perfil de Administrador selecionado.](./media/mondaycom-tutorial/configuration-1.png)

1. Selecione **Segurança** e certifique-se de clicar em **Abrir** ao lado do SAML.

    ![A screenshot mostra o separador Segurança com a opção de abrir ao lado do SAML.](./media/mondaycom-tutorial/configuration-2.png)

1. Preencha os detalhes abaixo do seu IDP.

    ![A screenshot mostra o fornecedor SAML onde pode introduzir informações a partir do seu I D P.](./media/mondaycom-tutorial/configuration-3.png)

    > [!NOTE]
    > Para mais detalhes, consulte [este](https://support.monday.com/hc/articles/360000460605-SAML-Single-Sign-on?abcb=34642) artigo

### <a name="create-mondaycom-test-user"></a>Criar monday.com utilizador de teste

Nesta secção, um utilizador chamado B.Simon é criado em monday.com. monday.com suporta o provisionamento just-in-time, que é ativado por padrão. Não há nenhum item de ação para si nesta secção. Se um utilizador já não existir em monday.com, cria-se um novo quando tenta aceder a monday.com.

## <a name="test-sso"></a>Teste SSO

Nesta secção, testa a configuração de um único sinal de inscrição Azure AD com as seguintes opções. 

#### <a name="sp-initiated"></a>SP iniciado:

* Clique em **Testar esta aplicação** no portal Azure. Isto irá redirecionar para monday.com Assinar no URL onde pode iniciar o fluxo de login.  

* Vá diretamente para monday.com URL de inscrição e inicie o fluxo de login a partir daí.

#### <a name="idp-initiated"></a>IDP iniciado:

* Clique em **Testar esta aplicação** no portal Azure e deverá ser automaticamente inscrito no monday.com para o qual configura o SSO. 

Também pode utilizar o Microsoft My Apps para testar a aplicação em qualquer modo. Quando clicar no azulejo monday.com nas Minhas Apps, se configurado no modo SP, será redirecionado para o sinal de aplicação na página para iniciar o fluxo de login e se configurado no modo IDP, deverá ser automaticamente inscrito no monday.com para o qual configura o SSO. Para obter mais informações sobre as Minhas Apps, consulte [Introdução às Minhas Aplicações.](../user-help/my-apps-portal-end-user-access.md)

## <a name="next-steps"></a>Passos seguintes

Uma vez configurado monday.com pode impor o controlo da sessão, que protege a exfiltração e infiltração dos dados sensíveis da sua organização em tempo real. O controlo da sessão estende-se desde o Acesso Condicional. [Saiba como impor o controlo da sessão com o Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).