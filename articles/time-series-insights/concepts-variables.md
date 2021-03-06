---
title: Variáveis de modelo - Azure Time Series Insights Gen2 | Microsoft Docs
description: Variáveis de modelo
author: shreyasharmamsft
ms.author: shresha
ms.service: time-series-insights
ms.topic: conceptual
ms.date: 01/22/2021
ms.openlocfilehash: 01184a4eb2aac81bbcabcebf89ef10afeabddbe8
ms.sourcegitcommit: 2aeb2c41fd22a02552ff871479124b567fa4463c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/22/2021
ms.locfileid: "107872970"
---
# <a name="time-series-model-variables"></a>Variáveis de modelo de série de tempo

Este artigo descreve as variáveis do Modelo série de tempo que especificam regras de fórmula e computação em eventos.

Cada variável pode ser um de três tipos: *numérico,* *categórico* e *agregado.*

* Os tipos **numéricos** funcionam com valores numéricos contínuos.
* Os tipos **categóricos** funcionam com um conjunto definido de valores discretos.
* Os tipos **agregados** combinam múltiplas variáveis de um único tipo (ou todos numéricos ou todos categóricos).

Os seguintes visores de tabela que propriedades são relevantes para cada tipo variável.

[![Tabela variável modelo série de tempo](media/v2-update-tsm/time-series-model-variable-table.png)](media/v2-update-tsm/time-series-model-variable-table.png#lightbox)

## <a name="numeric-variables"></a>Variáveis numéricas

| Propriedade variável | Description |
| --- | ---|
| Filtro variável | Os filtros são cláusulas condicionais opcionais para restringir o número de linhas que estão a ser consideradas para cálculo. |
| Valor variável | Valores de telemetria utilizados para a computação provenientes do dispositivo ou sensores ou transformados através de Expressões de Séries Tempotacurtas. As variáveis do tipo numérico devem ser `Double` ou corresponder ao tipo de `Long` dados dos dados que chegam.|
| Interpolação variável | A interpolação especifica como reconstruir um sinal utilizando dados existentes. As opções de interpolação *passo* e *linear* estão disponíveis para variáveis numéricas. |
| Agregação variável | Execute computações através das funções de agregação suportadas [para tipos de variáveis numéricas](/rest/api/time-series-insights/reference-time-series-expression-syntax#numeric-variable-kind). |

As variáveis estão em conformidade com o seguinte exemplo JSON:

```JSON
"Interpolated Speed": {
  "kind": "numeric",
  "value": {
    "tsx": "$event['Speed-Sensor'].Double"
  },
  "filter": null,
  "interpolation": {
    "kind": "step",
    "boundary": {
      "span": "P1D"
    }
  },
  "aggregation": {
    "tsx": "right($value)"
  }
}
```

## <a name="categorical-variables"></a>Variáveis categóricas

| Propriedade variável | Description |
| --- | ---|
| Filtro variável | Os filtros são cláusulas condicionais opcionais para restringir o número de linhas que estão a ser consideradas para cálculo. |
| Valor variável | Valores de telemetria utilizados para a computação provenientes do dispositivo ou dos sensores. As variáveis de tipo categórico devem ser `Long` ou corresponder ao tipo de `String` dados dos dados que chegam. |
| Interpolação variável | A interpolação especifica como reconstruir um sinal utilizando dados existentes. A opção de interpolação *Step* está disponível para variáveis categóricas. |
| Categorias variáveis | As categorias criam um mapeamento entre os valores provenientes do dispositivo ou sensores para uma etiqueta. |
| Categoria de padrão variável | A categoria padrão é para todos os valores que não estão sendo mapeados na propriedade "categorias". |

As variáveis estão em conformidade com o seguinte exemplo JSON:

```JSON
"Status": {
  "kind": "categorical",
  "value": {
     "tsx": "$event.Status.Long"
},
  "interpolation": {
    "kind": "step",
    "boundary": {
      "span" : "PT1M"
    }
  },
  "categories": [
    {
      "values": [0, 1, 2, 3],
      "label": "Good"
    },
    {
      "values": [4],
      "label": "Bad"
    }
  ],
  "defaultCategory": {
    "label": "Not Applicable"
  }
}
```

## <a name="aggregate-variables"></a>Variáveis agregadas

| Propriedade variável | Description |
| --- | ---|
| Filtro variável | Os filtros são cláusulas condicionais opcionais para restringir o número de linhas que estão a ser consideradas para cálculo. |
| Agregação variável | Execute computações através das funções de agregação suportadas [para tipos variáveis agregados](/rest/api/time-series-insights/reference-time-series-expression-syntax#aggregate-variable-kind). |

As variáveis estão em conformidade com o seguinte exemplo JSON:

```JSON
"Speed Range": {
  "kind": "aggregate",
  "filter": null,
  "aggregation": {
    "tsx": "max($event.Speed.Double) - min($event.Speed.Double)"
  }
}
```

As variáveis são armazenadas na definição tipo de modelo de série sonora e podem ser fornecidas em linha através de APIs para substituir ou complementar a definição armazenada.

## <a name="next-steps"></a>Passos seguintes

* Saiba mais sobre [o Modelo série de tempo.](./concepts-model-overview.md)

* Leia mais sobre como definir variáveis inline usando [APIs de consulta](./concepts-query-overview.md).
