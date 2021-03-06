---
title: Gerir contas V3 da Azure Media Services
description: Para começar a gerir, encriptar, codificar, analisar e transmitir conteúdos de mídia em Azure, é necessário criar uma conta de Media Services. Este artigo explica como gerir as contas V3 da Azure Media Services.
services: media-services
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: conceptual
ms.date: 03/29/2021
ms.author: inhenkel
ms.openlocfilehash: 5f48a60ee139cf4a89c0873633feea93cdde0940
ms.sourcegitcommit: edc7dc50c4f5550d9776a4c42167a872032a4151
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2021
ms.locfileid: "105964239"
---
# <a name="manage-azure-media-services-v3-accounts"></a>Gerir contas V3 da Azure Media Services

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Para começar a gerir, encriptar, codificar, analisar e transmitir conteúdos de mídia em Azure, é necessário criar uma conta de Media Services. Quando criar uma conta dos Serviços de Multimédia, terá de fornecer o nome de um recurso de conta de Armazenamento do Azure. A conta de armazenamento especificada está ligada à sua conta dos Serviços de Multimédia. A conta dos Serviços de Multimédia e todas as contas de armazenamento associadas têm de estar na mesma subscrição do Azure. Para mais informações, consulte [as contas de Armazenamento.](storage-account-concept.md)

[!INCLUDE [account creation note](./includes/note-2020-05-01-account-creation.md)]

## <a name="moving-a-media-services-account-between-subscriptions"></a>Movimentar uma conta de Serviços de Mídia entre subscrições 

Se precisar de mover uma conta de Serviços de Comunicação para uma nova subscrição, tem primeiro de mover todo o grupo de recursos que contém a conta de Serviços de Comunicação para a nova subscrição. Deve mover todos os recursos anexados: contas de Armazenamento Azure, perfis Azure CDN, etc. Para obter mais informações, consulte [mover recursos para novo grupo de recursos ou subscrição.](../../azure-resource-manager/management/move-resource-group-and-subscription.md) Como em qualquer recurso em Azure, movimentos de grupo de recursos podem levar algum tempo para completar.

> [!NOTE]
> O Media Services v3 suporta o modelo de vários arrendamentos.

### <a name="considerations"></a>Considerações

* Crie cópias de segurança de todos os dados da sua conta antes de migrar para uma subscrição diferente.
* Tens de parar todos os pontos de streaming e recursos de streaming ao vivo. Os seus utilizadores não poderão aceder ao seu conteúdo durante a duração do movimento do grupo de recursos. 

> [!IMPORTANT]
> Não inicie o Streaming Endpoint até que o movimento esteja concluído com sucesso.

### <a name="troubleshoot"></a>Resolução de problemas

Se uma conta de Serviços de Mídia ou uma conta de Armazenamento Azure associada ficar "desligada" após o movimento do grupo de recursos, tente rodar as teclas de Conta de Armazenamento. Se a rotação das teclas 'Conta de Armazenamento' não resolver o estado "desligado" da conta Serviços de Comunicação, apresente um novo pedido de apoio do menu "Suporte + resolução de problemas" na conta dos Serviços de Comunicação.  

## <a name="next-steps"></a>Passos seguintes

[Criar uma conta](./account-create-how-to.md)
