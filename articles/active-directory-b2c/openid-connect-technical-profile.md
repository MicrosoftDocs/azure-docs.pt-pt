---
title: Defina um perfil técnico OpenID Connect numa política personalizada
titleSuffix: Azure AD B2C
description: Defina um perfil técnico OpenID Connect numa política personalizada no Azure Ative Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/04/2021
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: fea42cb89dce717431c188deeb2ce83f9413f560
ms.sourcegitcommit: c6a2d9a44a5a2c13abddab932d16c295a7207d6a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/09/2021
ms.locfileid: "107283886"
---
# <a name="define-an-openid-connect-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Defina um perfil técnico OpenID Connect numa política personalizada do Azure Ative Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

O Azure Ative Directory B2C (Azure AD B2C) fornece suporte para o fornecedor de identidade do protocolo [OpenID Connect.](https://openid.net/2015/04/17/openid-connect-certification-program/) O OpenID Connect 1.0 define uma camada de identidade em cima do OAuth 2.0 e representa o estado da arte nos protocolos modernos de autenticação. Com um perfil técnico OpenID Connect, pode federar com um fornecedor de identidade baseado em OpenID Connect, como a Azure AD. A federação com um fornecedor de identidade permite que os utilizadores assinem com as suas identidades sociais ou empresariais existentes.

## <a name="protocol"></a>Protocolo

O **atributo nome** do elemento **Protocolo** tem de ser definido para `OpenIdConnect` . Por exemplo, o protocolo para o perfil técnico **MSA-OIDC** `OpenIdConnect` é:

```xml
<TechnicalProfile Id="MSA-OIDC">
  <DisplayName>Microsoft Account</DisplayName>
  <Protocol Name="OpenIdConnect" />
  ...
```

## <a name="input-claims"></a>Reclamações de entrada

Os **elementos InputClaims** e **InputClaimsTransformations** não são necessários. Mas pode querer enviar parâmetros adicionais ao seu fornecedor de identidade. O exemplo a seguir adiciona o **parâmetro de** cadeia de consulta domain_hint com o valor do pedido de `contoso.com` autorização.

```xml
<InputClaims>
  <InputClaim ClaimTypeReferenceId="domain_hint" DefaultValue="contoso.com" />
</InputClaims>
```

## <a name="output-claims"></a>Reclamações de saída

O elemento **OutputClaims** contém uma lista de reclamações devolvidas pelo fornecedor de identidade OpenID Connect. Poderá ser necessário mapear o nome da reclamação definida na sua política para o nome definido no fornecedor de identidade. Também pode incluir reclamações que não são devolvidas pelo fornecedor de identidade, desde que desagressem o `DefaultValue` atributo.

O elemento **OutputClaimsTransformations** pode conter uma coleção de elementos de **saídaClaimsTransformation** que são utilizados para modificar as alegações de saída ou gerar novos.

O exemplo a seguir mostra as reclamações devolvidas pelo fornecedor de identidade da Conta Microsoft:

- A  sub-alegação que está mapeada para a reivindicação **da EmitenteUS.**
- A alegação do **nome** que está mapeada para a **alegação do nome de exibição.**
- O **e-mail** sem mapeamento de nome.

O perfil técnico também devolve alegações que não são devolvidas pelo fornecedor de identidade:

- A **identidadeProvider** afirma que contém o nome do fornecedor de identidade.
- A **autenticaçãoProvação com** um valor predefinido de **SocialIdpAuthentication**.

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="live.com" />
  <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
  <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="sub" />
  <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
  <OutputClaim ClaimTypeReferenceId="email" />
</OutputClaims>
```

## <a name="metadata"></a>Metadados

| Atributo | Obrigatório | Descrição |
| --------- | -------- | ----------- |
| client_id | Yes | O identificador de aplicação do fornecedor de identidade. |
| IdTokenAudience | No | O público do id_token. Se especificado, a Azure AD B2C verifica se a `aud` reclamação num token devolvido pelo fornecedor de identidade é igual à especificada nos metadados IdTokenAudience.  |
| METADADOS | Yes | Um URL que aponta para um documento de configuração do fornecedor de identidade OpenID Connect, que também é conhecido como ponto final de configuração bem conhecido do OpenID. O URL pode conter a `{tenant}` expressão, que é substituída pelo nome do inquilino.  |
| authorization_endpoint | No | Um URL que aponta para um ponto final de autorização de configuração do fornecedor de identidade OpenID Connect. O valor dos metadados authorization_endpoint prevalece sobre o `authorization_endpoint` especificado no ponto final de configuração bem conhecido do OpenID. O URL pode conter a `{tenant}` expressão, que é substituída pelo nome do inquilino. |
| end_session_endpoint | No | O URL do ponto final da sessão final. O valor dos metadados authorization_endpoint prevalece sobre o `end_session_endpoint` especificado no ponto final de configuração bem conhecido do OpenID. |
| issuer | No | O identificador único de um fornecedor de identidade OpenID Connect. O valor dos metadados emitentes tem precedência sobre o `issuer` especificado no ponto final de configuração bem conhecido do OpenID.  Se especificado, a Azure AD B2C verifica se a `iss` reclamação num token devolvido pelo fornecedor de identidade é igual à especificada nos metadados emitentes. |
| ProviderName | No | O nome do fornecedor de identidade.  |
| response_types | No | O tipo de resposta de acordo com a especificação OpenID Connect Core 1.0. Valores possíveis: `id_token` `code` , ou . `token` . |
| response_mode | No | O método que o fornecedor de identidade utiliza para enviar o resultado de volta para Azure AD B2C. Valores possíveis: `query` `form_post` (padrão), ou `fragment` . |
| scope | No | O âmbito do pedido que é definido de acordo com a especificação OpenID Connect Core 1.0. Como, `openid` `profile` `email` e. |
| HttpBinding | No | A ligação HTTP esperada ao token de acesso e reclama pontos finais simbólicos. Valores possíveis: `GET` ou `POST` .  |
| ValidTokenIssuerPrefixes | No | Uma chave que pode ser usada para iniciar sedutar a cada um dos inquilinos quando se utiliza um fornecedor de identidade multi-inquilino, como o Azure Ative Directory. |
| UsePolicyInRedirectUri | No | Indica se deve utilizar uma política ao construir o URI de redirecionamento. Ao configurar a sua aplicação no fornecedor de identidade, tem de especificar o URI de redirecionamento. O redirecionamento URI aponta para Azure AD B2C, `https://{your-tenant-name}.b2clogin.com/{your-tenant-name}.onmicrosoft.com/oauth2/authresp` .  Se `true` especificar, tem de adicionar um URI de redirecionamento para cada apólice que utilizar. Por exemplo: `https://{your-tenant-name}.b2clogin.com/{your-tenant-name}.onmicrosoft.com/{policy-name}/oauth2/authresp`. |
| MarkAsFailureOnStatusCode5xx | No | Indica se um pedido a um serviço externo deve ser marcado como uma falha se o código de estado http estiver na gama 5xx. A predefinição é `false`. |
| DiscoverMetadataByTokenIssuer | No | Indica se os metadados OIDC devem ser descobertos utilizando o emitente no token JWT. |
| IncluirClaimResolvingInClaimsHandling  | No | Para pedidos de entradas e saídas, especifica se a [resolução de sinistros](claim-resolver-overview.md) está incluída no perfil técnico. Valores possíveis: `true` , ou `false` (predefinição). Se pretender utilizar uma reclamação no perfil técnico, desa um pouco `true` para . |
|token_endpoint_auth_method| No | Especifica como a Azure AD B2C envia o cabeçalho de autenticação para o ponto final simbólico. Valores possíveis: `client_secret_post` (padrão) e `client_secret_basic` (visualização pública), `private_key_jwt` (visualização pública). Para mais informações, consulte [a secção de autenticação do cliente OpenID Connect](https://openid.net/specs/openid-connect-core-1_0.html#ClientAuthentication). |
|token_signing_algorithm| No | Especifica o algoritmo de assinatura para utilizar quando `token_endpoint_auth_method` estiver definido para `private_key_jwt` . Valores possíveis: `RS256` (padrão) ou `RS512` . .|
| SingleLogoutEnabled | No | Indica se durante a entrada no perfil técnico tenta-se assinar dos fornecedores de identidade federados. Para mais informações, consulte [a sessão de Sessão Azure AD B2C](./session-behavior.md#sign-out).  Valores possíveis: `true` (padrão), ou `false` . |
|ReadBodyClaimsOnIdpRedirect| No| Definir `true` para ler reclamações do organismo de resposta no redirecionamento do fornecedor de identidade. Estes metadados são utilizados com [o Apple ID,](identity-provider-apple-id.md)onde as reclamações regressam na carga útil da resposta.|

```xml
<Metadata>
  <Item Key="ProviderName">https://login.live.com</Item>
  <Item Key="METADATA">https://login.live.com/.well-known/openid-configuration</Item>
  <Item Key="response_types">code</Item>
  <Item Key="response_mode">form_post</Item>
  <Item Key="scope">openid profile email</Item>
  <Item Key="HttpBinding">POST</Item>
  <Item Key="UsePolicyInRedirectUri">false</Item>
  <Item Key="client_id">Your Microsoft application client ID</Item>
</Metadata>
```

### <a name="ui-elements"></a>Elementos da IU
 
As seguintes definições podem ser utilizadas para configurar a mensagem de erro exibida após a falha. Os metadados devem ser configurados no perfil técnico OpenID Connect. As mensagens de erro podem ser [localizadas](localization-string-ids.md#sign-up-or-sign-in-error-messages).

| Atributo | Obrigatório | Descrição |
| --------- | -------- | ----------- |
| UserMessageIfClaimsPrincipalDoesNotExist | No | A mensagem a apresentar ao utilizador se uma conta com o nome de utilizador fornecido não for encontrada no diretório. |
| UserMessageIfInvalidPassword | No | A mensagem a apresentar ao utilizador se a palavra-passe estiver incorreta. |
| UserMessageIfOldPasswordUsed| No |  A mensagem a apresentar ao utilizador se for utilizada uma senha antiga.|

## <a name="cryptographic-keys"></a>Chaves criptográficas

O elemento **CryptographicKeys** contém o seguinte atributo:

| Atributo | Obrigatório | Descrição |
| --------- | -------- | ----------- |
| client_secret | Yes | O segredo do cliente da aplicação do fornecedor de identidade. Esta chave criptográfica só é necessária se os **metadados response_types** estiverem definidos `code` e **token_endpoint_auth_method** estiver definido para `client_secret_post` ou `client_secret_basic` . Neste caso, a Azure AD B2C faz outra chamada para trocar o código de autorização por um token de acesso. Se os metadados estiverem definidos, `id_token` pode omitir a tecla criptográfica.  |
| assertion_signing_key | Yes | A chave privada RSA que será usada para assinar a afirmação do cliente. Esta chave criptográfica só é necessária se os **metadados token_endpoint_auth_method** estiverem definidos para `private_key_jwt` . |

## <a name="redirect-uri"></a>Redirecionar Uri

Quando configurar o URI de redirecionamento do seu fornecedor de identidade, insira `https://{your-tenant-name}.b2clogin.com/{your-tenant-name}.onmicrosoft.com/oauth2/authresp` . Certifique-se de substituir `{your-tenant-name}` pelo nome do seu inquilino. O URI de redirecionamento tem de estar em todas as minúsculas.

Exemplos:

- [Adicione a Conta Microsoft (MSA) como um fornecedor de identidade usando políticas personalizadas](identity-provider-microsoft-account.md)
- [Inscreva-se usando contas AD Azure](identity-provider-azure-ad-single-tenant.md)
- [Permitir que os utilizadores inscrevam-se num fornecedor de identidade Azure AD multi-inquilino usando políticas personalizadas](identity-provider-azure-ad-multi-tenant.md)
