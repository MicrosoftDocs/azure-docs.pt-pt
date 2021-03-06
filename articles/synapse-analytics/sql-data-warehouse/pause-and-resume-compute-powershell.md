---
title: 'Quickstart: Pausa e retomar o cálculo na piscina SQL dedicada (anteriormente SQL DW) com a Azure PowerShell'
description: Pode utilizar a Azure PowerShell para fazer uma pausa e retomar a piscina SQL dedicada (anteriormente SQL DW). recursos compute.
services: synapse-analytics
author: julieMSFT
ms.author: jrasnick
manager: craigg
ms.reviewer: igorstan
ms.date: 03/20/2019
ms.topic: quickstart
ms.service: synapse-analytics
ms.subservice: sql-dw
ms.custom:
- seo-lt-2019
- azure-synapse
- devx-track-azurepowershell
- mode-api
ms.openlocfilehash: be82b6dcc17c2850b9a35085316cd0905a5b6b75
ms.sourcegitcommit: 590f14d35e831a2dbb803fc12ebbd3ed2046abff
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2021
ms.locfileid: "107566805"
---
# <a name="quickstart-pause-and-resume-compute-in-dedicated-sql-pool-formerly-sql-dw-with-azure-powershell"></a>Quickstart: Pausa e retomar o cálculo na piscina SQL dedicada (anteriormente SQL DW) com a Azure PowerShell

Você pode usar Azure PowerShell para parar e retomar recursos de cálculo de piscina SQL (anteriormente SQL DW).
Se não tiver uma subscrição do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="before-you-begin"></a>Antes de começar

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Este quickstart pressupõe que já tem uma piscina SQL dedicada (anteriormente SQL DW) que pode parar e retomar. Se precisar de criar um, pode utilizar [o portal Create and Connect -](create-data-warehouse-portal.md) para criar uma piscina SQL dedicada (anteriormente SQL DW) chamada **mySampleDataWarehouse**.

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão na sua subscrição Azure utilizando o comando [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) e siga as instruções no ecrã.

```powershell
Connect-AzAccount
```

Para ver qual a subscrição que está a utilizar, execute [a Subscrição Get-AzSubscription](/powershell/module/az.accounts/get-azsubscription?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).

```powershell
Get-AzSubscription
```

Se precisar de utilizar uma subscrição diferente do padrão, executar [Set-AzContext](/powershell/module/az.accounts/set-azcontext?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).

```powershell
Set-AzContext -SubscriptionName "MySubscription"
```

## <a name="look-up-dedicated-sql-pool-formerly-sql-dw-information"></a>Procure informações dedicadas da piscina SQL (anteriormente SQL DW)

Localize o nome da base de dados, o nome do servidor e o grupo de recursos para a piscina de SQL dedicada (anteriormente SQL DW) que pretende fazer uma pausa e retomar.

Siga estes passos para encontrar informações de localização para a sua piscina SQL dedicada (anteriormente SQL DW):

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).
1. Clique em **Azure Synapse Analytics (anteriormente SQL DW)** na página esquerda do portal Azure.
1. Selecione **mySampleDataWarehouse** a partir da página **Azure Synapse Analytics (anteriormente SQL DW).** A piscina SQL abre.

    ![O nome do servidor e grupo de recursos](./media/pause-and-resume-compute-powershell/locate-data-warehouse-information.png)

1. Escreva o nome da piscina SQL (anteriormente SQL DW), que é o nome da base de dados. Anote também o nome do servidor e do grupo de recursos.
1. Utilize apenas a primeira parte do nome do servidor nos cmdlets PowerShell. Na imagem anterior, o nome completo do servidor é sqlpoolservername.database.windows.net. Usamos **o nome sqlpoolserver como** o nome do servidor no cmdlet PowerShell.

## <a name="pause-compute"></a>Cálculo de pausa

Para poupar custos, pode parar e retomar os recursos de computação a pedido. Por exemplo, se não estiver a utilizar a base de dados durante a noite e aos fins de semana, pode fazer uma pausa durante esses horários e retomá-la durante o dia.

>[!NOTE]
>Não há qualquer custo para os recursos de computação enquanto a base de dados é interrompida. No entanto, continua a ser cobrado para armazenamento.

Para fazer uma pausa numa base de dados, utilize o cmdlet [Suspend-AzSqlDatabase.](/powershell/module/az.sql/suspend-azsqldatabase?toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) O exemplo seguinte faz uma pausa numa piscina SQL chamada **mySampleDataWarehouse** hospedada num servidor chamado **sqlpoolservername**. O servidor está num grupo de recursos Azure chamado **myResourceGroup**.

```Powershell
Suspend-AzSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "sqlpoolservername" –DatabaseName "mySampleDataWarehouse"
```

O exemplo a seguir recupera a base de dados no objeto $database. Em seguida, canaliza o objeto para [suspender a base de dados AzSql](/powershell/module/az.sql/suspend-azsqldatabase?toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json). Os resultados são armazenados no resultado do objectoDatabase. O comando final mostra os resultados.

```Powershell
$database = Get-AzSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "sqlpoolservername" –DatabaseName "mySampleDataWarehouse"
$resultDatabase = $database | Suspend-AzSqlDatabase
$resultDatabase
```

## <a name="resume-compute"></a>Retomar o cálculo

Para iniciar uma base de dados, utilize o [cmdlet Resume-AzSqlDatabase.](/powershell/module/az.sql/resume-azsqldatabase?toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) O exemplo a seguir inicia uma base de dados chamada **mySampleDataWarehouse** hospedada num servidor chamado **sqlpoolservername**. O servidor está num grupo de recursos Azure chamado **myResourceGroup**.

```Powershell
Resume-AzSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "sqlpoolservername" -DatabaseName "mySampleDataWarehouse"
```

O próximo exemplo recupera a base de dados no objeto $database. Em seguida, canaliza o objeto para [a Resume-AzSqlDatabase](/powershell/module/az.sql/resume-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) e armazena os resultados em $resultDatabase. O comando final mostra os resultados.

```Powershell
$database = Get-AzSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "sqlpoolservername" –DatabaseName "mySampleDataWarehouse"
$resultDatabase = $database | Resume-AzSqlDatabase
$resultDatabase
```

## <a name="check-status-of-your-sql-pool-operation"></a>Verifique o estado da sua operação de piscina SQL

Para verificar o estado da sua piscina SQL dedicada (anteriormente SQL DW), utilize o cmdlet [Get-AzSqlDatabaseActivity.](/powershell/module/az.sql/Get-AzSqlDatabaseActivity?toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)

```Powershell
Get-AzSqlDatabaseActivity -ResourceGroupName "myResourceGroup" -ServerName "sqlpoolservername" -DatabaseName "mySampleDataWarehouse"
```

## <a name="clean-up-resources"></a>Limpar os recursos

Está a ser cobrado por unidades de armazém de dados e os dados armazenaram a sua piscina DE SQL dedicada (anteriormente SQL DW). Estes recursos de computação e armazenamento são faturados em separado.

- Se quiser manter os dados armazenados, faça uma pausa no cálculo.
- Se quiser remover as cargas futuras, pode apagar a piscina SQL.

Siga estes passos para limpar os recursos conforme quiser.

1. Inscreva-se no [portal Azure](https://portal.azure.com)e clique na sua piscina SQL.

    ![Limpar os recursos](./media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

2. Para interromper a computação, clique no botão **Pausar**. Quando a piscina SQL é pausada, vê-se um botão **Iniciar.**  Para retomar a computação, clique em **Iniciar**.

3. Para remover a piscina SQL para que não esteja cobrado para calcular ou armazenar, clique em **Eliminar**.

4. Para remover o servidor SQL que criou, clique **em sqlpoolservername.database.windows.net** e, em seguida, clique em **Apagar**.  Tenha cuidado com esta eliminação, uma vez que eliminar o servidor também elimina todas as bases de dados atribuídas ao mesmo.

5. Para remover o grupo de recursos, clique em **myResourceGroup** e, em seguida, clique em **Eliminar grupo de recursos**.

## <a name="next-steps"></a>Passos seguintes

Para saber mais sobre o pool SQL, continue os dados de Load em um artigo [dedicado da piscina SQL (anteriormente SQL DW).](./load-data-from-azure-blob-storage-using-copy.md) Para obter informações adicionais sobre a gestão das capacidades de computação, consulte o artigo [de visão geral do cálculo do](sql-data-warehouse-manage-compute-overview.md) cálculo.
