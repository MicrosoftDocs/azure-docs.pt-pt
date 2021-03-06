---
title: Colaboração B2B API e personalização - Azure Ative Directory
description: A colaboração do Azure Ative Directory B2B apoia as suas relações intercompreitárias, permitindo aos parceiros de negócio aceder seletivamente às suas aplicações corporativas.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 02/03/2021
ms.author: mimart
author: msmimart
manager: celestedg
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8160859bb782ee8ffc4fef5ee03b61b6f54be1bb
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "99548666"
---
# <a name="azure-active-directory-b2b-collaboration-api-and-customization"></a>Azure Ative Directy B2B colaboração API e personalização

Muitos clientes dizem-nos que querem personalizar o processo de convite de uma forma que funcione melhor para as suas organizações. Com a nossa API, podes fazer isso. [https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation](/graph/api/resources/invitation)

## <a name="capabilities-of-the-invitation-api"></a>Capacidades do convite API

A API oferece as seguintes capacidades:

1. Convide um utilizador externo com *qualquer* endereço de e-mail.

    ```
    "invitedUserDisplayName": "Sam"
    "invitedUserEmailAddress": "gsamoogle@gmail.com"
    ```

2. Personalize onde quer que os seus utilizadores aterrem depois de aceitarem o convite.

    ```
    "inviteRedirectUrl": "https://myapps.microsoft.com/"
    ```

3. Opte por enviar o correio de convite padrão através de nós

    ```
    "sendInvitationMessage": true
    ```

   com uma mensagem para o destinatário que pode personalizar

    ```
    "customizedMessageBody": "Hello Sam, let's collaborate!"
    ```

4. E escolha cc: pessoas que quer manter ao corrente sobre o seu convite a este colaborador.

5. Ou personalize completamente o seu fluxo de trabalho de convite e de embarque, optando por não enviar notificações através do Azure AD.

    ```
    "sendInvitationMessage": false
    ```

   Neste caso, você recebe de volta um URL de resgate da API que você pode incorporar em um modelo de e-mail, IM, ou outro método de distribuição à sua escolha.

6. Finalmente, se for administrador, pode optar por convidar o utilizador como membro.

    ```
    "invitedUserType": "Member"
    ```

## <a name="determine-if-a-user-was-already-invited-to-your-directory"></a>Determine se um utilizador já foi convidado para o seu diretório

Pode utilizar o convite da API para determinar se um utilizador já existe no seu inquilino de recursos. Isto pode ser útil quando está a desenvolver uma aplicação que usa o convite da API para convidar um utilizador. Se o utilizador já existir no seu diretório de recursos, eles não receberão um convite, para que você possa executar uma consulta primeiro para determinar se o e-mail já existe como um UPN ou outro imóvel de inscrição.

1. Certifique-se de que o domínio de e-mail do utilizador não faz parte do domínio verificado do seu inquilino de recursos.
2. No inquilino de recursos, utilize a seguinte consulta do utilizador onde {0} está o endereço de e-mail que está convidando:

   ```
   “userPrincipalName eq '{0}' or mail eq '{0}' or proxyAddresses/any(x:x eq 'SMTP:{0}') or signInNames/any(x:x eq '{0}') or otherMails/any(x:x eq '{0}')"
   ```

## <a name="authorization-model"></a>Modelo de autorização

A API pode ser executada nos seguintes modos de autorização:

### <a name="app--user-mode"></a>App + Modo de utilizador

Neste modo, quem estiver a usar a API precisa de ter as permissões para criar convites B2B.

### <a name="app-only-mode"></a>Modo apenas app

No contexto apenas da aplicação, a aplicação precisa do Utilizador.Convidar.Todas as possibilidades para o convite ter sucesso.

Para mais informações, consulte: https://developer.microsoft.com/graph/docs/authorization/permission_scopes


## <a name="powershell"></a>PowerShell

Pode utilizar o PowerShell para adicionar e convidar facilmente utilizadores externos para uma organização. Crie um convite utilizando o cmdlet:

```powershell
New-AzureADMSInvitation
```

Pode utilizar as seguintes opções:

* -Nome do Nome do Casamento Convidado
* -ConvidadoUserEmailAddress
* -Enviar AColhimento de Mensagens
* -ConvidadoUserMessageInfo

### <a name="invitation-status"></a>Estatuto do convite

Depois de enviar um convite a um utilizador externo, pode utilizar o **cmdlet Get-AzureADUser** para ver se o aceitaram. As seguintes propriedades de Get-AzureADUser são povoadas quando um utilizador externo é enviado um convite:

* **O UserState** indica se o convite é **pendente** ou **aceite.**
* **UserStateChangedOn** mostra o tempo de pontuação para a última alteração na propriedade **UserState.**

Pode utilizar a opção **Filtro** para filtrar os resultados pelo **UserState**. O exemplo abaixo mostra como filtrar resultados para mostrar apenas utilizadores que tenham um convite pendente. O exemplo também mostra a opção **Lista de Formatos,** que permite especificar as propriedades a exibir. 
 

```powershell
Get-AzureADUser -Filter "UserState eq 'PendingAcceptance'" | Format-List -Property DisplayName,UserPrincipalName,UserState,UserStateChangedOn
```

> [!NOTE]
> Certifique-se de que tem a versão mais recente do módulo AzureAD PowerShell ou do módulo AzureADPreview PowerShell. 

## <a name="see-also"></a>Ver também

Confira a referência a API do convite em [https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation](/graph/api/resources/invitation) .

## <a name="next-steps"></a>Passos seguintes

- [O que é a colaboração B2B do Azure AD?](what-is-b2b.md)
- [Os elementos do e-mail de convite de colaboração B2B](invitation-email-elements.md)
- [Resgate de convite de colaboração B2B](redemption-experience.md)
- [Adicione utilizadores de colaboração B2B sem convite](add-user-without-invite.md)