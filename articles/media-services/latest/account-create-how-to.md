---
title: Criar uma conta dos Media Services do Azure
description: Este tutorial acompanha-o através dos passos da criação de uma conta Azure Media Services.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 11/4/2020
ms.author: inhenkel
ms.custom: devx-track-azurecli
ms.openlocfilehash: b9234b27e2f08e65f569393bde342cba3f37adee
ms.sourcegitcommit: edc7dc50c4f5550d9776a4c42167a872032a4151
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2021
ms.locfileid: "105964250"
---
# <a name="create-a-media-services-account"></a>Criar uma conta dos Media Services

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Para começar a encriptar, codificar, analisar, gerir e transmitir conteúdos de mídia em Azure, é necessário criar uma conta de Media Services. A conta dos Serviços de Comunicação Social tem de ser associada a uma ou mais contas de armazenamento. Este artigo descreve passos para a criação de uma nova conta Azure Media Services.

[!INCLUDE [note 2020-05-01 API](./includes/note-2020-05-01-account-creation.md)]

 Pode utilizar o portal Azure ou o CLI para criar uma conta de Serviços de Mídia. Escolha o separador para o método que pretende utilizar.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

<!-- NOTE: The following are in the includes folder and are reused in other How To articles. All task based content should be in the includes folder with the task- prefix prepended to the file name. -->

## <a name="portal"></a>[Portal](#tab/portal/)

[!INCLUDE [the media services account and storage account must be in the same subscription](./includes/note-account-storage-same-subscription.md)]

[!INCLUDE [create a media services account in the portal](./includes/task-create-media-services-account-portal.md)]

## <a name="cli"></a>[CLI](#tab/cli/)

[!INCLUDE [the media services account and storage account must be in the same subscription](./includes/note-account-storage-same-subscription.md)]

## <a name="set-the-azure-subscription"></a>Definir a subscrição do Azure

[!INCLUDE [Set the Azure subscription with CLI](./includes/task-set-azure-subscription-cli.md)]

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

[!INCLUDE [Create a resource group with CLI](./includes/task-create-resource-group-cli.md)]

## <a name="create-a-storage-account"></a>Criar uma conta de armazenamento

[!INCLUDE [Create a storage account with CLI](./includes/task-create-storage-account-cli.md)]

## <a name="create-a-media-services-account"></a>Criar uma conta dos Media Services

[!INCLUDE [Create a Media Services account with CLI](./includes/task-create-media-services-account-cli.md)]

---

## <a name="next-steps"></a>Passos seguintes

[Transmissão de um ficheiro](stream-files-dotnet-quickstart.md)