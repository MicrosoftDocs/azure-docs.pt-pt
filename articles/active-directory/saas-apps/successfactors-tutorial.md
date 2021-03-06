---
title: 'Tutorial: Azure Ative Directory integração única de sign-on (SSO) com SuccessFactors | Microsoft Docs'
description: Saiba como configurar um único sign-on entre o Azure Ative Directory e o SuccessFactors.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 12/26/2020
ms.author: jeedes
ms.openlocfilehash: a903eb6000cdd5212ad358cae4952e27a4719070
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "98725265"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-successfactors"></a>Tutorial: Azure Ative Directory integração única (SSO) com SuccessFactors

Neste tutorial, você vai aprender a integrar SuccessFactors com Azure Ative Directory (Azure AD). Quando integras o SuccessFactors com a AD Azure, podes:

* Controlo em Azure AD que tem acesso a SuccessFactors.
* Capacitar os seus utilizadores a serem automaticamente inscritos nos SuccessFactors com as suas contas AD Azure.
* Gerencie as suas contas numa localização central - o portal Azure.


## <a name="prerequisites"></a>Pré-requisitos

Para começar, precisa dos seguintes itens:

* Uma assinatura AD Azure. Se não tiver uma subscrição, pode obter uma [conta gratuita.](https://azure.microsoft.com/free/)
* SucessoFactors uma única subscrição (SSO) ativada.

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configura e testa Azure AD SSO em um ambiente de teste.

* SuccessFactors apoia sSO iniciado **SP.**

## <a name="adding-successfactors-from-the-gallery"></a>Adicionar SuccessFactors da galeria

Para configurar a integração de SuccessFactors em AD Azure, você precisa adicionar SuccessFactors da galeria à sua lista de aplicações geridas saaS.

1. Inscreva-se no portal Azure usando uma conta de trabalho ou escola, ou uma conta pessoal da Microsoft.
1. No painel de navegação à esquerda, selecione o serviço **Azure Ative Directory.**
1. Navegue para **aplicações empresariais** e, em seguida, selecione **Todas as Aplicações**.
1. Para adicionar nova aplicação, selecione **Nova aplicação**.
1. Na secção Adicionar a partir da secção **de galeria,** digite **SuccessFactors** na caixa de pesquisa.
1. Selecione **SuccessFactors** do painel de resultados e, em seguida, adicione a aplicação. Aguarde alguns segundos enquanto a aplicação é adicionada ao seu inquilino.


## <a name="configure-and-test-azure-ad-sso-for-successfactors"></a>Configurar e testar Azure AD SSO para successFactors

Configure e teste Azure AD SSO com SuccessFactors usando um utilizador de teste chamado **B.Simon**. Para que o SSO funcione, é necessário estabelecer uma relação de ligação entre um utilizador AZure AD e o utilizador relacionado em SuccessFactors.

Para configurar e testar a Azure AD SSO com successFactors, execute os seguintes passos:

1. **[Configure Azure AD SSO](#configure-azure-ad-sso)** - para permitir que os seus utilizadores utilizem esta funcionalidade.
    1. Crie um utilizador de **[teste AD Azure](#create-an-azure-ad-test-user)** - para testar um único sinal de Azure com B.Simon.
    1. **[Atribua o utilizador de teste Azure AD](#assign-the-azure-ad-test-user)** - para permitir que b.Simon utilize um único sinal de Ad AD.
2. **[Configure SuccessFactors SSO](#configure-successfactors-sso)** - para configurar as definições de Sign-On única no lado da aplicação.
    1. **[Create SuccessFactors test user](#create-successfactors-test-user)** - para ter uma contraparte de B.Simon em SuccessFactors que está ligada à representação AD Ad Azure do utilizador.
3. **[Teste SSO](#test-sso)** - para verificar se a configuração funciona.

## <a name="configure-azure-ad-sso"></a>Configurar o SSO do Azure AD

Siga estes passos para ativar o Azure AD SSO no portal Azure.

1. No portal Azure, na página de integração de aplicações **SuccessFactors,** encontre a secção **Gerir** e selecione **'Único sinal de súb'.**
1. Na página **de método de inscrição única,** selecione **SAML**.
1. Na **configuração single Sign-On com página SAML,** clique no ícone de lápis para **configuração SAML básica** para editar as definições.

   ![Editar Configuração BÁSICA SAML](common/edit-urls.png)

1. Na secção **de Configuração Básica SAML,** execute os seguintes passos:

    a. Na caixa de texto **url de entrada de sinais,** digite um URL utilizando um dos seguintes padrões:

    - `https://<companyname>.successfactors.com/<companyname>`
    - `https://<companyname>.sapsf.com/<companyname>`
    - `https://<companyname>.successfactors.eu/<companyname>`
    - `https://<companyname>.sapsf.eu`

    b. Na caixa de texto **identifier,** digite um URL utilizando um dos seguintes padrões:

    - `https://www.successfactors.com/<companyname>`
    - `https://www.successfactors.com`
    - `https://<companyname>.successfactors.eu`
    - `https://www.successfactors.eu/<companyname>`
    - `https://<companyname>.sapsf.com`
    - `https://hcm4preview.sapsf.com/<companyname>`
    - `https://<companyname>.sapsf.eu`
    - `https://www.successfactors.cn`
    - `https://www.successfactors.cn/<companyname>`

    c. Na caixa de texto **URL de resposta,** digite um URL utilizando um dos seguintes padrões:

    - `https://<companyname>.successfactors.com/<companyname>`
    - `https://<companyname>.successfactors.com`
    - `https://<companyname>.sapsf.com/<companyname>`
    - `https://<companyname>.sapsf.com`
    - `https://<companyname>.successfactors.eu/<companyname>`
    - `https://<companyname>.successfactors.eu`
    - `https://<companyname>.sapsf.eu`
    - `https://<companyname>.sapsf.eu/<companyname>`
    - `https://<companyname>.sapsf.cn`
    - `https://<companyname>.sapsf.cn/<companyname>`

    > [!NOTE]
    > Estes valores não são reais. Atualize estes valores com o URL de inscrição real, o identificador e o URL de resposta. Contacte [a equipa de suporte do Cliente SuccessFactors](https://www.sap.com/support.html) para obter estes valores.

4. Na **configuração single Sign-On com página SAML,** na secção Certificado de Assinatura **SAML,** encontre **o Certificado (Base64)** e selecione **Descarregamento** para descarregar o certificado e guardá-lo no seu computador.

    ![O link de descarregamento de certificado](common/certificatebase64.png)

6. Na secção **Configuração de Resultados,** copie os URL(s) apropriados com base na sua exigência.

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

Nesta secção, você permitirá que B.Simon use a Azure single sign-on, concedendo acesso a SuccessFactors.

1. No portal Azure, selecione **Aplicações empresariais** e, em seguida, selecione **Todas as aplicações**.
1. Na lista de candidaturas, selecione **SuccessFactors**.
1. Na página geral da aplicação, encontre a secção **Gerir** e selecione **Utilizadores e grupos**.
1. **Selecione Adicionar utilizador,** em seguida, selecione **Utilizadores e grupos** no diálogo **'Adicionar Atribuição'.**
1. No diálogo **de Utilizadores e grupos,** selecione **B.Simon** da lista de Utilizadores e, em seguida, clique no botão **Select** na parte inferior do ecrã.
1. Se estiver à espera que uma função seja atribuída aos utilizadores, pode selecioná-la a partir do Dropdown de **função** Select. Se não tiver sido configurada qualquer função para esta aplicação, vê a função "Acesso Predefinido" selecionada.
1. No diálogo **'Adicionar Atribuição',** clique no botão **'Atribuir'.**

## <a name="configure-successfactors-sso"></a>Configure SucessoFactors SSO

1. Numa janela diferente do navegador web, inicie sessão no seu **portal de administração SuccessFactors** como administrador.

2. Visite **a Segurança da Aplicação** e nativo de um único sinal na **funcionalidade**.

3. Coloque qualquer valor no **Token reset** e clique em **Guardar Token** para ativar o SSO SAML.

    ![O Screenshot mostra o separador de Segurança da Aplicação com sinal único nas funcionalidades chamadas onde pode introduzir um token.][11]

    > [!NOTE]
    > Este valor é utilizado como o interruptor de ligar/desligar. Se algum valor for guardado, o SSO SAML está LIGADO. Se um valor em branco for salvo, o SSO SAML está desligado.

4. Nativo abaixo da imagem e executar as seguintes ações:

    ![A screenshot mostra o painel S S O baseado em SAML onde pode introduzir os valores descritos.][12]
  
    a. Selecione o botão de rádio **SSO SAML v2**
  
    b. Deite o nome do **partido assertante SAML**(por exemplo, emitente SAML + nome da empresa).

    c. Na caixa de texto URL do **emitente,** cole o valor do **identificador Azure AD** que copiou do portal Azure.

    d. Selecione **a afirmação** como **Requer a Assinatura Obrigatória**.

    e. Selecione **Ativado** como **Enable SAML Flag**.

    f. Selecione **Não** como **Assinatura de Pedido de Início de Sessão (SF Gerado/SP/RP)**.

    exemplo, Selecione **o perfil de navegador/post** como **perfil SAML**.

    h. Selecione **Não** como **Prazo Válido de Certificado de Execução**.

    i. Copie o conteúdo do ficheiro de certificado descarregado do portal Azure e, em seguida, cole-o na caixa de texto **do Certificado de Verificação SAML.**

    > [!NOTE] 
    > O conteúdo do certificado deve ter etiquetas de certificado e de fim de certificado.

5. Navegue até SAML V2 e, em seguida, execute os seguintes passos:

    ![A screenshot mostra o painel de logout iniciado SAML v2 S P onde pode introduzir os valores descritos.][13]

    a. Selecione **Sim** como **Suporte SP iniciado Logotipout Global**.

    b. Na caixa de texto **Global Logout Service URL (destino LogoutRequest),** cole o valor **URL de Sign-Out** que copiou forma o portal Azure.

    c. Selecione **Não** como **Requere sp deve encriptar todos os elementos NameID**.

    d. Selecione **não especificado** como **Formato NameID**.

    e. Selecione **Sim** como **Enable sp iniciado login (AuthnRequest)**.

    f. No pedido enviar como Company-Wide caixa de texto **emitente,** cole o valor URL de **login** que copiou a partir do portal Azure.

6. Execute estes passos se quiser tornar os nomes de utilizador de login Insensíveis.

    ![Configurar Sign-On Individuais][29]

    a. Visite **as Definições da Empresa**(perto do fundo).

    b. Selecione caixa de verificação perto **de ativar o nome de utilizador não sensível a casos**.

    c. Clique em **Guardar**.

    > [!NOTE]
    > Se tentar ativar isto, o sistema verifica se cria um nome de login DE SAML duplicado. Por exemplo, se o cliente tiver nomes de utilizador, o Utilizador1 e o utilizador1. Tirar a sensibilidade ao caso faz com que estes duplicados. O sistema dá-lhe uma mensagem de erro e não ativa a funcionalidade. O cliente precisa de alterar um dos nomes de utilizador para que seja escrito diferente.

### <a name="create-successfactors-test-user"></a>Criar utilizadores de testes SuccessFactors

Para permitir que os utilizadores de Azure AD inscrevam-se no SuccessFactors, devem ser aprovisionados em SuccessFactors. No caso dos SuccessFactors, o provisionamento é uma tarefa manual.

Para conseguir que os utilizadores são criados em SuccessFactors, é necessário contactar a equipa de [suporte do SuccessFactors.](https://www.sap.com/support.html)

## <a name="test-sso"></a>Teste SSO 

Nesta secção, testa a configuração de um único sinal de inscrição Azure AD com as seguintes opções. 

* Clique em **Testar esta aplicação** no portal Azure. Isto irá redirecionar para o URL de login do SuccessFactors, onde pode iniciar o fluxo de login. 

* Vá diretamente ao URL de login do SuccessFactors e inicie o fluxo de login a partir daí.

* Pode utilizar as minhas apps do Microsoft. Quando clicar no azulejo SuccessFactors nas Minhas Apps, este irá redirecionar para o URL de assinatura do SuccessFactors. Para obter mais informações sobre as Minhas Apps, consulte [Introdução às Minhas Aplicações.](../user-help/my-apps-portal-end-user-access.md)


## <a name="next-steps"></a>Passos seguintes

Uma vez configurados os SuccessFactors, pode impor controlos de sessão, o que protege a exfiltração e infiltração dos dados sensíveis da sua organização em tempo real. Os controlos de sessão estendem-se desde o Acesso Condicional. [Saiba como impor o controlo da sessão com a Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad)

<!--Image references-->

[11]: ./media/successfactors-tutorial/tutorial_successfactors_07.png
[12]: ./media/successfactors-tutorial/tutorial_successfactors_08.png
[13]: ./media/successfactors-tutorial/tutorial_successfactors_09.png
[29]: ./media/successfactors-tutorial/tutorial_successfactors_10.png