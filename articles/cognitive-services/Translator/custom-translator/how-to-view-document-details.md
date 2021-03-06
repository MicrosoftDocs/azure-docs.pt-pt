---
title: Detalhes do documento - Tradutor Personalizado
titleSuffix: Azure Cognitive Services
description: A página da lista de documentos mostra o primeiro documento de 10 no seu espaço de trabalho. Para cada um dos documentos, apresenta o nome, emparelhamento, tipo, idioma, carimbo de tempo de upload e o endereço de e-mail do utilizador que fez o upload do documento.
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 08/17/2020
ms.author: lajanuar
ms.topic: conceptual
ms.openlocfilehash: 56093204516e156d670097c22b4edab42db54bde
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2021
ms.locfileid: "98895616"
---
# <a name="view-document-details"></a>Ver detalhes do documento

A página da lista de documentos mostra o primeiro documento de 10 no seu espaço de trabalho. Para cada um dos documentos, apresenta o nome, emparelhamento, tipo, idioma, carimbo de tempo de upload e o endereço de e-mail do utilizador que fez o upload do documento.

Clique num documento individual para ver a página de detalhes do documento. A página de detalhes do documento apresenta a lista de frases extraídas do documento.

- Por predefinição, o visor de origem "lado a lado" e as línguas-alvo são selecionados no campo de dropdown, mas pode alternar para ver frases na língua de origem ou alvo.
- 20 frases são exibidas por padrão por padrão. Pode utilizar o controlo de paginação para navegar entre páginas.

![detalhes do documento](media/how-to/how-to-view-document-details.png)

## <a name="delete-a-document"></a>Eliminar um documento

O utilizador deve ser um proprietário de um espaço de trabalho para apagar o documento para apagar um documento. Além disso, se um documento estiver a ser utilizado por um modelo, que esteja em qualquer parte do processo de formação ou em qualquer parte do processo de implantação, o documento não pode ser eliminado.

1. Ir para a página do documento
2. Paire em qualquer registo de documento e clique no ícone do lixo.

    ![Eliminar documento](media/how-to/how-to-delete-document-1.png)

3. Confirme eliminar.

    ![Eliminar confirmar](media/how-to/how-to-delete-document-confirm.png)

## <a name="next-steps"></a>Passos seguintes

- Aprenda [a treinar um modelo.](how-to-train-model.md)
