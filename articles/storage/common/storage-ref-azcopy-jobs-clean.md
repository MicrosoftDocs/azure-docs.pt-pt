---
title: trabalhos azcopia limpar | Microsoft Docs
description: Este artigo fornece informações de referência para o comando limpo de trabalhos de azcopia.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 07/24/2020
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 01bb7e37965abacac8105689bcb5ae52c548d647
ms.sourcegitcommit: 3b5cb7fb84a427aee5b15fb96b89ec213a6536c2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107503427"
---
# <a name="azcopy-jobs-clean"></a>azcopy jobs clean

Remova todos os registos e planos para todos os trabalhos

```
azcopy jobs clean [flags]
```

## <a name="related-conceptual-articles"></a>Artigos conceptuais relacionados

- [Introdução ao AzCopy](storage-use-azcopy-v10.md)
- [Dados de transferência com armazenamento AzCopy e Blob](./storage-use-azcopy-v10.md#transfer-data)
- [Transferir dados com o AzCopy e armazenamento de ficheiros](storage-use-azcopy-files.md)

## <a name="examples"></a>Exemplos

```
  azcopy jobs clean --with-status=completed
```

## <a name="options"></a>Opções

**...-ajuda**                Ajuda para limpar.

**--com o status** string Apenas remova os postos de trabalho com este estatuto, valores disponíveis: `Canceled` , , , , `Completed` `Failed` `InProgress` `All` (padrão `All` )

## <a name="options-inherited-from-parent-commands"></a>Opções herdadas dos comandos dos pais

**---cap-mbps flutuar**      Reduz a taxa de transferência, em megabits por segundo. A produção momentesa pode variar ligeiramente da tampa. Se esta opção for definida para zero, ou for omitida, a produção não está limitada.

**...-tipo de saída** formato da saída do comando. As escolhas incluem: texto, json. O valor predefinido é 'texto'. ("texto" predefinido)

**--cadeia de sufixos fidedignos-microsoft-sufixos** Especifica sufixos de domínio adicionais onde podem ser enviados tokens de login do Azure Ative Directory.  O padrão é '*.core.windows.net;*. core.chinacloudapi.cn; *.core.cloudapi.de;* core.usgovcloudapi.net.' Qualquer listado aqui é adicionado ao padrão. Para a segurança, só deve colocar os domínios microsoft Azure aqui. Separe várias entradas com pontos e vírgulas.

## <a name="see-also"></a>Ver também

- [azcopy jobs](storage-ref-azcopy-jobs.md)
