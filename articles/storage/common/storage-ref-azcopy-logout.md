---
title: logout azcopy | Microsoft Docs
description: Este artigo fornece informações de referência para o comando de logout azcopy.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 07/24/2020
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 20d1ee643f9864fb95ebdc9215cd0dab13a0fb5c
ms.sourcegitcommit: 3b5cb7fb84a427aee5b15fb96b89ec213a6536c2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107503104"
---
# <a name="azcopy-logout"></a>azcopy logout

Regista o utilizador e termina o acesso aos recursos de Armazenamento Azure.

## <a name="synopsis"></a>Sinopse

Este comando removerá todas as informações de login em cache para o utilizador atual.

```azcopy
azcopy logout [flags]
```

## <a name="related-conceptual-articles"></a>Artigos conceptuais relacionados

- [Introdução ao AzCopy](storage-use-azcopy-v10.md)
- [Dados de transferência com armazenamento AzCopy e Blob](./storage-use-azcopy-v10.md#transfer-data)
- [Transferir dados com o AzCopy e armazenamento de ficheiros](storage-use-azcopy-files.md)

## <a name="options"></a>Opções

|Opção|Descrição|
|--|--|
|-h, -- ajuda|Mostre conteúdo de ajuda para o comando logout.|

## <a name="options-inherited-from-parent-commands"></a>Opções herdadas dos comandos dos pais

|Opção|Descrição|
|---|---|
|---cap-mbps flutuar|Reduz a taxa de transferência, em megabits por segundo. A produção momentesa pode variar ligeiramente da tampa. Se esta opção for definida para zero, ou for omitida, a produção não está limitada.|
|--cadeia do tipo de saída|Formato da saída do comando. As escolhas incluem: texto, json. O valor predefinido é "texto".|
|--cadeia de sufixos de confiança da Microsoft-Sufixes   |Especifica sufixos de domínio adicionais onde podem ser enviados tokens de login do Azure Ative Directory.  O padrão é '*.core.windows.net;*. core.chinacloudapi.cn; *.core.cloudapi.de;* core.usgovcloudapi.net.' Qualquer listado aqui é adicionado ao padrão. Para a segurança, só deve colocar os domínios microsoft Azure aqui. Separe várias entradas com pontos e vírgulas.|

## <a name="see-also"></a>Ver também

- [azcopia](storage-ref-azcopy.md)
