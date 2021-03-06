---
title: incluir ficheiro
description: incluir ficheiro
services: storage
author: mhopkins-msft
ms.author: mhopkins
ms.date: 04/08/2020
ms.service: storage
ms.subservice: blobs
ms.topic: include
ms.reviewer: hux
ms.custom: include file
ms.openlocfilehash: a369eb7000fb8622a69f4205ffcc232ae9c9d242
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "95545939"
---
Para ler os dados no armazenamento de arquivo, tem de, primeiro, alterar a camada do blob para frequente ou esporádica. Este processo é conhecido como reidratação e pode levar horas a concluir. Recomendamos grandes tamanhos de bolhas para um desempenho ótimo de reidratação. “Reidratar” vários pequenos blobs em simultâneo pode provocar mais atrasos. Existem atualmente duas prioridades rehidratadas, Alta e Standard, que podem ser definidas através da propriedade opcional *x-ms-rehydrate-prioridade* numa operação [set Blob Tier](/rest/api/storageservices/set-blob-tier) ou [Copy Blob.](/rest/api/storageservices/copy-blob)

* **Prioridade padrão**: O pedido de reidratação será processado na ordem em que foi recebido e pode demorar até 15 horas.
* **Alta prioridade**: O pedido de reidratação será priorizado sobre os pedidos standard e pode terminar em menos de 1 hora para objetos com menos de dez GB de tamanho. 

> [!NOTE]
> A prioridade padrão é a opção de reidratação padrão para o arquivo. Alta prioridade é uma opção mais rápida que custará mais do que a reidratação prioritária standard e é geralmente reservada para uso em situações de restauração de dados de emergência.
>
> A alta prioridade pode demorar mais de 1 hora, dependendo do tamanho da bolha e da procura atual. É garantido que os pedidos de alta prioridade serão priorizados em relação aos pedidos prioritários da Standard.

Uma vez iniciado um pedido de reidratação, não pode ser cancelado. Durante o processo de reidratação, a propriedade blob *de nível de acesso x-ms* continuará a aparecer como arquivo até que a reidratação seja concluída para um nível on-line. Para confirmar o estado de reidratação e o progresso, pode ligar para [a Get Blob Properties](/rest/api/storageservices/get-blob-properties) para verificar o estado *x-ms-archive-status* e as propriedades *blob de prioridade x-ms-ms..* O estado do arquivo pode ler "rehidratar-se-a-quente" ou "rehidratar-pendente-para-arrefecer" dependendo do nível de destino rehidratada. A prioridade rehidrata indicará a velocidade de "Alto" ou "Standard". Após a conclusão, o estado de arquivo e as propriedades prioritárias rehidratas são removidas, e a propriedade blob de nível de acesso irá atualizar para refletir o nível quente ou fresco selecionado.