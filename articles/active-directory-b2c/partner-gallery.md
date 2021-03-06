---
title: Galeria ISV Partner para Azure AD B2C
titleSuffix: Azure AD B2C
description: Saiba como se integrar com os nossos parceiros ISV para adaptar a sua experiência de utilizador final às suas necessidades. A nossa rede de parceiros alarga as nossas capacidades de solução; ativar a autenticação do cliente, a autenticação segura do cliente, o controlo de acesso baseado em funções; combater a fraude através da verificação de identidade.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 01/11/2021
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: b555c8651c6e1608193a6ae06c39b20f633e4ea9
ms.sourcegitcommit: 260a2541e5e0e7327a445e1ee1be3ad20122b37e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/21/2021
ms.locfileid: "107813669"
---
# <a name="azure-active-directory-b2c-isv-partners"></a>Parceiros isv do Azure Ative Directory B2C

A nossa rede de parceiros ISV alarga as nossas capacidades de solução para o ajudar a construir experiências de utilizador final sem emenda. Com o Azure AD B2C, pode integrar-se com parceiros ISV para permitir métodos de autenticação multi-factor (MFA), fazer controlo de acesso baseado em funções, permitir a verificação e a impermeabilização de identidade, melhorar a segurança com a deteção de bots e proteção contra fraudes, e cumprir os requisitos da Direção 2 (PSD2) de Autenticação Segura do Cliente (SCA). Use as nossas amostras detalhadas para aprender a integrar aplicações com os parceiros ISV.

>[!NOTE]
>O [site comunitário Azure Ative Directory B2C no GitHub](https://azure-ad-b2c.github.io/azureadb2ccommunity.io/) também fornece políticas personalizadas de amostra da comunidade.

## <a name="identity-verification-and-proofing"></a>Verificação e impermeabilização de identidade

A Microsoft associa-se aos seguintes ISVs para verificação e revisão de identidade.

| Parceiro ISV | Descrição e integração de caminhadas |
|:-------------------------|:--------------|
|![Imagem de um logotipo da Experian.](./media/partner-gallery/experian-logo.png) | [A Experian](./partner-experian.md) é um fornecedor de verificação de identidade e de revisão que realiza avaliações de risco com base em atributos do utilizador para prevenir fraudes. |
|![Screenshot de um logotipo de IDology.](./media/partner-gallery/idology-logo.png) | [A IDology](./partner-idology.md) é um fornecedor de verificação de identidade e de revisão com soluções de verificação de ID, soluções de prevenção de fraudes, soluções de conformidade, entre outras.|
|![Imagem de um logotipo do Jumio.](./media/partner-gallery/jumio-logo.png) | [O Jumio](./partner-jumio.md) é um serviço de verificação de identificação, que permite a verificação automática de ID em tempo real, salvaguardando os dados dos clientes. |
| ![Imagem de um logótipo lexisNexis.](./media/partner-gallery/lexisnexis-logo.png) | [A LexisNexis](./partner-lexisnexis.md) é um fornecedor de perfis e validação de identidade que verifica a identificação do utilizador e fornece uma avaliação abrangente de risco com base no dispositivo do utilizador. |
| ![Screenshot de um logotipo Onfido](./media/partner-gallery/onfido-logo.png) | [O Onfido](./partner-onfido.md) é uma solução de verificação de ID e biometria facial que permite às empresas satisfazer em tempo real os requisitos *de Know Your Customer* e identidade.  |

## <a name="mfa-and-passwordless-authentication"></a>Autenticação MFA e Password

A Microsoft associa-se aos seguintes ISVs para autenticação MFA e Password sem palavras-passe.

| Parceiro ISV | Descrição e integração de caminhadas |
|:-------------------------|:--------------|
| ![Screenshot de um logotipo hypr](./media/partner-gallery/hypr-logo.png) | [A Hypr](./partner-hypr.md) é um fornecedor de autenticação sem palavras-passe, que substitui palavras-passe por encriptações de chaves públicas que eliminam fraude, phishing e reutilização credencial. |
| ![Screenshot de um logotipo do seu nome](./media/partner-gallery/itsme-logo.png) | [o seu nome](./partner-itsme.md) é uma solução de ID digital compatível com identificação eletrónica, autenticação e fidedignidade (eiDAS) para permitir que os utilizadores assinem de forma segura sem leitores de cartões, palavras-passe, autenticação de dois fatores e múltiplos códigos PIN. |
|![Screenshot de um logotipo keyless.](./media/partner-gallery/keyless-logo.png) | [Keyless](./partner-keyless.md) é um fornecedor de autenticação sem palavras-passe que fornece autenticação sob a forma de uma varredura biométrica facial e elimina fraude, phishing e reutilização credencial.
| ![Screenshot de um logotipo nevis](./media/partner-gallery/nevis-logo.png) | [O Nevis](./partner-nevis.md) permite a autenticação sem palavras-passe e fornece uma experiência de utilizador final de primeira e totalmente marcada com a aplicação Nevis Access para autenticação forte do cliente e para cumprir os requisitos de transação PSD2. |
| ![Screenshot de um logotipo de trusona](./media/partner-gallery/trusona-logo.png) | A integração [da Trusona](./partner-trusona.md) ajuda-o a assinar de forma segura e permite a autenticação sem palavras-passe, o MFA e a digitalização de licenças digitais. |
| ![Imagem de um logotipo de twilio.](./media/partner-gallery/twilio-logo.png) | [A aplicação Twilio Check](./partner-twilio.md) fornece múltiplas soluções para permitir o MFA através de sms senha única (OTP), senha única baseada no tempo (TOTP), e notificações push, e para cumprir os requisitos sca para PSD2. |
| ![Screenshot de um logotipo da dactilografiaDNA](./media/partner-gallery/typingdna-logo.png) | [Digitar O ADN](./partner-typingdna.md) permite uma autenticação forte do cliente analisando o padrão de dactilografia de um utilizador. Ajuda as empresas a viabilizarem um MFA silencioso e a cumprirem os requisitos da SCA para o PSD2. |
| ![Screenshot de um logotipo whoiam](./media/partner-gallery/whoiam-logo.png) | [A WhoIAM](./partner-whoiam.md) é uma aplicação do Sistema de Gestão de Identidade de Marca (BRIMS) que permite às organizações verificarem a sua base de utilizadores por voz, SMS e e-mail. |

## <a name="role-based-access-control"></a>Controlo de acesso baseado em funções 
 
A Microsoft associa-se aos seguintes ISVs para controlo de acesso baseado em funções.

| Parceiro ISV | Descrição e integração de caminhadas |
|:-------------------------|:--------------|
| ![Screenshot de um logotipo da n8identity](./media/partner-gallery/n8identity-logo.png) | [A N8Identity](./partner-n8identity.md) é uma plataforma de governação de Identidade como serviço que fornece solução para resolver a migração de contas de clientes e a administração de Pedidos de Atendimento ao Cliente (CSR) em execução no Microsoft Azure. |
| ![Screenshot de um logotipo Saviynt](./media/partner-gallery/saviynt-logo.png) | A plataforma [saviynt](./partner-Saviynt.md) cloud-native promove uma melhor segurança, conformidade e governação através de análises inteligentes e integração de aplicações cruzadas para agilizar a modernização das TI. |

## <a name="secure-hybrid-access-to-on-premises-application"></a>Acesso híbrido seguro à aplicação no local

A Microsoft associa-se aos seguintes ISVs para fornecer acesso híbrido seguro à aplicação no local. 

| Parceiro ISV | Descrição e integração de caminhadas |
|:-------------------------|:--------------|
| ![Screenshot de um logotipo de Ping](./media/partner-gallery/ping-logo.png) | [A Ping Identity](./partner-ping-identity.md) permite o acesso híbrido seguro a aplicações antigas no local através de várias nuvens. |
| ![Screenshot de um logotipo de estratos](./media/partner-gallery/strata-logo.png) | [Strata](./partner-strata.md) fornece acesso híbrido seguro a aplicações no local, aplicando políticas de acesso consistentes, mantendo as identidades em sincronização, e tornando simples a transição de aplicações de sistemas de identidade antigas para a autenticação baseada em padrões e controlo de acesso fornecido pela Azure AD B2C. |
| ![Screenshot de um logotipo de zscaler](./media/partner-gallery/zscaler-logo.png) | [A Zscaler](./partner-zscaler.md) oferece acesso seguro e baseado em políticas a aplicações e ativos privados sem o custo, a agioso ou riscos de segurança de uma VPN. |

## <a name="fraud-protection"></a>Proteção contra fraudes

A Microsoft associa-se aos seguintes ISVs para deteção e prevenção de fraudes. 

| Parceiro ISV | Descrição e integração de caminhadas |
|:-------------------------|:--------------|
| ![Screenshot de um logotipo de laboratório Arkose](./media/partner-gallery/arkose-logo.png) | [A Arkose Labs](./partner-arkose-labs.md) é um fornecedor de soluções de prevenção de fraudes que ajuda as organizações a proteger contra ataques de bots, ataques de aquisição de contas e aberturas fraudulentas de contas. |
| ![Screenshot de um logotipo bioCatch](./media/partner-gallery/biocatch-logo.png) | [A BioCatch](./partner-biocatch.md) é um fornecedor de soluções de prevenção de fraudes que analisa os comportamentos digitais físicos e cognitivos de um utilizador para gerar insights que distinguem entre clientes legítimos e criminosos virtuais. |
| ![Screenshot de um logótipo Microsoft Dynamics 365](./media/partner-gallery/microsoft-dynamics365-logo.png) | [O Microsoft Dynamics 365 Fraud Protection](./partner-dynamics-365-fraud-protection.md) é uma solução que ajuda as organizações a protegerem-se contra aberturas fraudulentas de contas através da recolha de impressões digitais do dispositivo. |


## <a name="additional-information"></a>Informações adicionais

- [Políticas personalizadas no Azure AD B2C](./custom-policy-overview.md)

- [Começar com políticas personalizadas em Azure AD B2C](tutorial-create-user-flows.md?pivots=b2c-custom-policy)

## <a name="next-steps"></a>Passos seguintes

Selecione um parceiro nas tabelas mencionadas para aprender a integrar a sua solução com a Azure AD B2C.
