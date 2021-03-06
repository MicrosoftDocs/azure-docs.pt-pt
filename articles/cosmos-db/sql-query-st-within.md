---
title: ST_WITHIN na linguagem de consulta DB do Azure Cosmos
description: Saiba mais sobre a função do sistema SQL ST_WITHIN em Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 02/17/2021
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 9372da349d0ea9169bb59570b7e6dd0e597d1cdf
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "100558230"
---
# <a name="st_within-azure-cosmos-db"></a>ST_WITHIN (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 Devolve uma expressão booleana indicando se o objeto GeoJSON (Point, Polygon, MultiPolygon ou LineString) especificado no primeiro argumento está dentro do GeoJSON (Point, Polygon, MultiPolygon, ou LineString) no segundo argumento.  
  
## <a name="syntax"></a>Sintaxe
  
```sql
ST_WITHIN (<spatial_expr>, <spatial_expr>)  
```  
  
## <a name="arguments"></a>Argumentos
  
*spatial_expr*  
   É uma expressão de objeto GeoJSON Point, Polygon ou LineString.  
  
## <a name="return-types"></a>Tipos de retorno
  
  Devolve um valor Boolean.  
  
## <a name="examples"></a>Exemplos
  
  O exemplo a seguir mostra como encontrar todos os documentos familiares dentro de um polígono utilizando `ST_WITHIN` .  
  
```sql
SELECT f.id
FROM Families f
WHERE ST_WITHIN(f.location, {  
    'type':'Polygon',
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 Aqui está o conjunto de resultados.  
  
```json
[{ "id": "WakefieldFamily" }]  
```  

## <a name="remarks"></a>Observações

Esta função do sistema beneficiará de um [índice geoespacial,](index-policy.md#spatial-indexes) exceto em consultas com agregados.

## <a name="next-steps"></a>Passos seguintes

- [Funções espaciais Azure Cosmos DB](sql-query-spatial-functions.md)
- [Funciona O sistema Azure Cosmos DB](sql-query-system-functions.md)
- [Introdução ao Azure Cosmos DB](introduction.md)
