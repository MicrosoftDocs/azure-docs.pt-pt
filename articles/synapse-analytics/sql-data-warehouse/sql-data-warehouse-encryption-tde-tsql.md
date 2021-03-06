---
title: Encriptação de dados transparentes (T-SQL)
description: Encriptação de dados transparente (TDE) em Azure Synapse Analytics (T-SQL)
services: synapse-analytics
author: julieMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 04/30/2019
ms.author: jrasnick
ms.reviewer: rortloff
ms.custom: seo-lt-2019
ms.openlocfilehash: 80e5da6bb281806afe6bc980e35d70732bcd609c
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "98676277"
---
# <a name="get-started-with-transparent-data-encryption-tde"></a>Começar com encriptação de dados transparentes (TDE)

> [!div class="op_single_selector"]
>
> * [Visão geral da segurança](sql-data-warehouse-overview-manage-security.md)
> * [Autenticação](sql-data-warehouse-authentication.md)
> * [Encriptação (Portal)](sql-data-warehouse-encryption-tde.md)
> * [Encriptação (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

## <a name="required-permissions"></a>Permissões Obrigatórias

Para ativar a Encriptação de Dados Transparente (TDE), tem de ser administrador ou membro da função dbmanager.

## <a name="enabling-encryption"></a>Habilitar a encriptação

Siga estes passos para ativar o TDE:

1. Conecte-se à base de dados *principal* no servidor que hospeda a base de dados utilizando um login que seja um administrador ou um membro da função **dbmanager** na base de dados principal
2. Execute a seguinte declaração para encriptar a base de dados.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Encriptação incapacitante

Siga estes passos para desativar o TDE:

1. Ligue-se à base de dados *principal* usando um login que seja um administrador ou um membro da função **dbmanager** na base de dados principal
2. Execute a seguinte declaração para encriptar a base de dados.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION OFF;
```

> [!NOTE]
> Uma piscina SQL dedicada a pausa deve ser retomada antes de errar nas definições do TDE.

## <a name="verifying-encryption"></a>Verificação da encriptação

Para verificar o estado da encriptação, siga os passos abaixo:

1. Ligue-se à base de *dados principal* ou de instância usando um login que seja um administrador ou um membro da função **dbmanager** na base de dados principal
2. Execute a seguinte declaração para encriptar a base de dados.

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

O resultado indica uma base de ```1``` dados encriptada, ```0``` indica uma base de dados não encriptada.

## <a name="encryption-dmvs"></a>DMVs de encriptação

* [sys.databases](/sql/relational-databases/system-catalog-views/sys-databases-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true)
* [sys.dm_pdw_nodes_database_encryption_keys](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-database-encryption-keys-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true)
