---
title: 'Tutorial: Azure Ative Directory integração única de sign-on (SSO) com SharingCloud | Microsoft Docs'
description: Saiba como configurar um único sign-on entre o Azure Ative Directory e o Instant Suite.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/10/2021
ms.author: jeedes
ms.openlocfilehash: f663a81a86aae2fe11611aa1d6ab19f89ebcf6d1
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2021
ms.locfileid: "104950047"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-sharingcloud"></a>Tutorial: Azure Ative Directory integração única (SSO) com SharingCloud

Neste tutorial, você vai aprender a integrar SharingCloud com Azure Ative Directory (Azure AD). Quando integra o SharingCloud com a AD Azure, pode:

* Controle em Azure AD que tem acesso ao SharingCloud.
* Capacitar os seus utilizadores a serem automaticamente inscritos no SharingCloud com as suas contas AD Azure.
* Gerencie as suas contas numa localização central - o portal Azure.

## <a name="prerequisites"></a>Pré-requisitos

Para começar, precisa dos seguintes itens:

* Uma assinatura AD Azure. Se não tiver uma subscrição, pode obter uma [conta gratuita.](https://azure.microsoft.com/free/)
* Assinatura única de sso (SSO) ativada.


## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configura e testa Azure AD SSO em um ambiente de teste.

* SharingCloud suporta SSO iniciado **sp e IDP**
* SharingCloud suporta **provisão de** utilizadores just in time

## <a name="adding-sharingcloud-from-the-gallery"></a>Adicionar SharingCloud da galeria

Para configurar a integração do SharingCloud no AD Azure, precisa adicionar sharingCloud da galeria à sua lista de aplicações geridas pelo SaaS.

1. Inscreva-se no portal Azure usando uma conta de trabalho ou escola, ou uma conta pessoal da Microsoft.
1. No painel de navegação à esquerda, selecione o serviço **Azure Ative Directory.**
1. Navegue para **aplicações empresariais** e, em seguida, selecione **Todas as Aplicações**.
1. Para adicionar nova aplicação, selecione **Nova aplicação**.
1. Na secção Adicionar a partir da secção **de galeria,** **escreva SharingCloud** na caixa de pesquisa.
1. Selecione **SharingCloud** do painel de resultados e adicione a aplicação. Aguarde alguns segundos enquanto a aplicação é adicionada ao seu inquilino.


## <a name="configure-and-test-azure-ad-sso-for-sharingcloud"></a>Configurar e testar Azure AD SSO para sharingCloud

Configure e teste Azure AD SSO com SharingCloud usando um utilizador de teste chamado **B.Simon**. Para que o SSO funcione, é necessário estabelecer uma relação de ligação entre um utilizador AZure AD e o utilizador relacionado no SharingCloud.

Para configurar e testar O Azure AD SSO com SharingCloud, execute os seguintes passos:

1. **[Configure Azure AD SSO](#configure-azure-ad-sso)** - para permitir que os seus utilizadores utilizem esta funcionalidade.
    1. Crie um utilizador de **[teste AD Azure](#create-an-azure-ad-test-user)** - para testar um único sinal de Azure com B.Simon.
    1. **[Atribua o utilizador de teste Azure AD](#assign-the-azure-ad-test-user)** - para permitir que b.Simon utilize um único sinal de Ad AD.
1. **[Configure SharingCloud SSO](#configure-sharingcloud-sso)** - para configurar as definições de inscrição única no lado da aplicação.
    1. **[Create SharingCloud test user](#create-sharingcloud-test-user)** - para ter uma contraparte de B.Simon em SharingCloud que está ligada à representação AD AD do utilizador.
1. **[Teste SSO](#test-sso)** - para verificar se a configuração funciona.

## <a name="configure-azure-ad-sso"></a>Configurar o SSO do Azure AD

Siga estes passos para ativar o Azure AD SSO no portal Azure.

1. No portal Azure, na página de integração da aplicação **SharingCloud,** encontre a secção **Gerir** e selecione **um único sinal de sação**.
1. Na página de método **de inscrição** única, selecione **SAML**.
1. No **set-on único com** a página SAML, clique no ícone de lápis para **configuração SAML Básica** para editar as definições.

   ![Editar Configuração BÁSICA SAML](common/edit-urls.png)

1. Na secção **de Configuração Básica SAML,** execute os seguintes passos:

    Faça o upload do ficheiro de metadados com o ficheiro XML fornecido pelo SharingCloud. Contacte a equipa de suporte do [Cliente SharingCloud](mailto:support@sharingcloud.com) para obter o ficheiro.

    ![Screenshot da interface de utilizador de configuração básica SAML com o link **Upload metadados** realçado.](common/upload-metadata.png)
    
    Selecione o ficheiro de metadados fornecido e clique no **Upload**.

    ![A screenshot do ficheiro metadados forneceu interface de utilizador, com o ícone de ficheiro selecionado e o botão **Upload** realçado.](common/browse-upload-metadata.png)

1. A aplicação SharingCloud espera as afirmações DO SAML num formato específico, o que requer que adicione mapeamentos de atributos personalizados à configuração de atributos de token SAML. A imagem que se segue mostra a lista de atributos predefinidos.

    ![Screenshot da interface de utilizador user Atributos do utilizador com o ícone de edição realçado.](common/edit_attribute.png)

1. Além de acima, a aplicação SharingCloud espera que alguns mais atributos sejam repercutidos na resposta SAML que são mostrados abaixo. Estes atributos também são pré-povoados, mas pode revê-los de acordo com o seu requisito.

    | Name | Atributo de origem|
    | ---------------| --------- |
    | urn:sharingcloud:sso:firstname | user.givenname |
    | urn:sharingcloud:sso:lastname | utilizador.sobrenome |
    | urn:sharingcloud:sso:email | user.mail |

1. No **set-on único com** a página SAML, na secção **Certificado de Assinatura SAML,** clique no ícone **Copy** para copiar o Url de **Metadados** da Federação a partir das opções dadas de acordo com o seu requisito.

    ![O Url de Metadados para copiar](common/copy_metadataurl.png)

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

Nesta secção, você permitirá que B.Simon use a Azure single sign-on, concedendo acesso ao SharingCloud.

1. No portal Azure, selecione **Aplicações empresariais** e, em seguida, selecione **Todas as aplicações**.
1. Na lista de aplicações, selecione **SharingCloud**.
1. Na página geral da aplicação, encontre a secção **Gerir** e selecione **Utilizadores e grupos**.
1. **Selecione Adicionar utilizador,** em seguida, selecione **Utilizadores e grupos** no diálogo **'Adicionar Atribuição'.**
1. No diálogo **de Utilizadores e grupos,** selecione **B.Simon** da lista de Utilizadores e, em seguida, clique no botão **Select** na parte inferior do ecrã.
1. Se estiver à espera que uma função seja atribuída aos utilizadores, pode selecioná-la a partir do Dropdown de **função** Select. Se não tiver sido configurada qualquer função para esta aplicação, vê a função "Acesso Predefinido" selecionada.
1. No diálogo **'Adicionar Atribuição',** clique no botão **'Atribuir'.**

## <a name="configure-sharingcloud-sso"></a>Configurar partilharCloud SsO

Para configurar um único sinal no lado **do SharingCloud,** é necessário enviar o **url de metadados** da Federação copiado do portal Azure para a equipa de suporte [sharingCloud](mailto:support@sharingcloud.com). Eles definem esta definição para ter a ligação SSO SAML corretamente definida em ambos os lados.

### <a name="create-sharingcloud-test-user"></a>Criar utilizador de teste SharingCloud

Nesta secção, um utilizador chamado Britta Simon é criado no SharingCloud. O SharingCloud suporta o fornecimento do utilizador Just In Time, que é ativado por padrão. Não há nenhum item de ação para si nesta secção. Se um utilizador já não existir no SharingCloud, um novo é criado após a autenticação.

## <a name="test-sso"></a>Teste SSO 

Nesta secção, testa a configuração de um único sinal de inscrição Azure AD com as seguintes opções. 

#### <a name="sp-initiated"></a>SP iniciado:

* Clique em **Testar esta aplicação** no portal Azure. Isto irá redirecionar para SharingCloud Sign no URL onde pode iniciar o fluxo de login.  

* Vá diretamente ao URL de assinatura do SharingCloud e inicie o fluxo de login a partir daí.

#### <a name="idp-initiated"></a>IDP iniciado:

* Clique em **Testar esta aplicação** no portal Azure e deverá ser automaticamente inscrito no SharingCloud para o qual configura o SSO 

Também pode utilizar o Microsoft My Apps para testar a aplicação em qualquer modo. Quando clicar no azulejo SharingCloud nas Minhas Apps, se configurado no modo SP, será redirecionado para o sinal de aplicação na página para iniciar o fluxo de login e se configurado no modo IDP, deverá ser automaticamente inscrito no SharingCloud para o qual configura o SSO. Para obter mais informações sobre as Minhas Apps, consulte [Introdução às Minhas Aplicações.](../user-help/my-apps-portal-end-user-access.md)


## <a name="next-steps"></a>Passos seguintes

Uma vez configurado o SharingCloud, pode impor o controlo da sessão, que protege a exfiltração e infiltração dos dados sensíveis da sua organização em tempo real. O controlo da sessão estende-se desde o Acesso Condicional. [Saiba como impor o controlo da sessão com o Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).