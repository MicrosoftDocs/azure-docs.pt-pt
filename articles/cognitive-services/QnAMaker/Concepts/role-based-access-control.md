---
title: Colaborar com outros - QnA Maker
description: Aprenda a colaborar com outros autores e editores usando o controlo de acesso baseado em funções Azure.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 05/15/2020
ms.openlocfilehash: 512ae470d3c9068b9d284095a18013fc84dbf36e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2021
ms.locfileid: "103232550"
---
# <a name="collaborate-with-other-authors-and-editors"></a>Colaborar com outros autores e editores

Colabore com outros autores e editores utilizando o controlo de acesso baseado em funções Azure (Azure RBAC) colocado no seu recurso QnA Maker.

## <a name="access-is-provided-on-the-qna-maker-resource"></a>O acesso é fornecido no recurso QnA Maker

Todas as permissões são controladas pelas permissões colocadas no recurso QnA Maker. Estas permissões alinham-se para ler, escrever, publicar e aceder plenamente. Pode permitir a colaboração entre vários utilizadores [atualizando o acesso do RBAC](../how-to/manage-qna-maker-app.md) para o recurso QnA Maker.

Esta funcionalidade Azure RBAC inclui:
* O Azure Ative Directory (AAD) é 100% compatível com a autenticação baseada em chaves para proprietários e contribuintes. Os clientes podem utilizar a autenticação baseada em chaves ou a autenticação baseada no Azure RBAC nos seus pedidos.
* Adicione rapidamente autores e editores a todas as bases de conhecimento no recurso porque o controlo está ao nível dos recursos, não ao nível da base de conhecimento.

> [!NOTE]
> Certifique-se de adicionar um subdomínio personalizado para o recurso. [Subdomínio personalizado](../../cognitive-services-custom-subdomains.md) deve estar presente por padrão, mas se não, por favor, adicione

## <a name="access-is-provided-by-a-defined-role"></a>O acesso é fornecido por uma função definida

[!INCLUDE [Azure RBAC permissions table](../includes/role-based-access-control.md)]

## <a name="authentication-flow"></a>Fluxo de autenticação

O diagrama seguinte mostra o fluxo, do ponto de vista do autor, para iniciar a sessão no portal QnA Maker e utilizar as APIs de autoria.

> [!div class="mx-imgBorder"]
> ![O diagrama seguinte mostra o fluxo, do ponto de vista do autor, para iniciar a sessão no portal QnA Maker e utilizar as APIs de autoria.](../media/qnamaker-how-to-collaborate-knowledge-base/rbac-flow-from-portal-to-service.png)

|Passos|Descrição|
|--|--|
|1|Portal adquire ficha para recurso QnA Maker.|
|2|Portal Chama o fabricante QnA apropriado autor de API (APIM) passando o token em vez de teclas.|
|3|A API do Fabricante QNA valida o token.|
|4 |A API do Fabricante QNA chama o Serviço QnAMaker.|

Se pretender ligar para as [APIs de autoria,](../index.yml)saiba mais sobre como configurar a autenticação.

## <a name="authenticate-by-qna-maker-portal"></a>Autenticado por portal QnA Maker

Se você autore e colabore usando o portal QnA Maker, depois de adicionar o papel apropriado ao recurso para um colaborador, o portal QnA Maker gere todas as permissões de acesso.

## <a name="authenticate-by-qna-maker-apis-and-sdks"></a>Autenticado por APIs e SDKs do Criador de QnA

Se você autorizar e colaborar usando as APIs, seja através do REST ou dos SDKs, precisa [de criar um principal de serviço](../../authentication.md#assign-a-role-to-a-service-principal) para gerir a autenticação.

## <a name="next-step"></a>Passo seguinte

* Desenhe uma base de conhecimento para idiomas e para aplicações de clientes
