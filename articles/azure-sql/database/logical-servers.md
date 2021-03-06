---
title: O que é um servidor na Azure SQL Database e Azure Synapse Analytics?
titleSuffix: ''
description: Saiba mais sobre os servidores SQL lógicos utilizados pela Azure SQL Database e a Azure Synapse Analytics e como geri-los.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 03/12/2019
ms.openlocfilehash: c76d3ae78bf2b9b4a71d9520f7f1c6c2c322483b
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/20/2021
ms.locfileid: "107784520"
---
# <a name="what-is-a-logical-sql-server-in-azure-sql-database-and-azure-synapse"></a>O que é um servidor SQL lógico na Base de Dados Azure SQL e Azure Synapse?
[!INCLUDE[appliesto-sqldb-asa](../includes/appliesto-sqldb-asa.md)]

Na Base de Dados Azure SQL e Azure Synapse Analytics, um servidor é uma construção lógica que funciona como um ponto administrativo central para uma coleção de bases de dados. Ao nível do servidor, pode administrar [logins,](logins-create-manage.md) [regras de firewall,](firewall-configure.md) [regras de auditoria,](../../azure-sql/database/auditing-overview.md) [políticas de deteção](threat-detection-configure.md)de ameaças e [grupos de falha automática](auto-failover-group-overview.md). Um servidor pode estar numa região diferente do seu grupo de recursos. O servidor deve existir antes de poder criar uma base de dados na Base de Dados Azure SQL ou numa base de dados de armazéns de dados em Azure Synapse Analytics. Todas as bases de dados geridas por um único servidor são criadas dentro da mesma região que o servidor.

Este servidor é distinto de uma instância sql Server que você pode estar familiarizado no mundo do local. Especificamente, não existem garantias sobre a localização das bases de dados ou base de dados de armazém de dados em relação ao servidor que as gere. Além disso, nem a Base de Dados Azure SQL nem a Azure Synapse expõem qualquer acesso ou funcionalidades ao nível de instância. Em contraste, as bases de dados de casos em um caso gerido são todas fisicamente co-localizadas - da mesma forma que você está familiarizado com o SQL Server no mundo das máquinas no local ou virtual.

Quando cria um servidor, fornece uma conta de login do servidor e uma palavra-passe que tem direitos administrativos para a base de dados principal nesse servidor e todas as bases de dados criadas nesse servidor. Esta conta inicial é uma conta de login SQL. A Azure SQL Database e Azure Synapse Analytics suportam a autenticação SQL e a autenticação do Diretório Ativo Azure para a autenticação. Para obter informações sobre logins e autenticação, consulte [Bases de Dados e Logins em Base de Dados Azure SQL](logins-create-manage.md). A autenticação do Windows não é suportada.

Um servidor na BASE de Dados SQL e Azure Synapse:

- É criado numa subscrição do Azure, mas pode ser movido com os recursos nele contidos para outra subscrição
- É o recurso principal para bases de dados, conjuntos elásticos e armazéns de dados
- Fornece um espaço de nome para bases de dados, piscinas elásticas e base de dados de armazém de dados
- É um recipiente lógico com semântica vitalícia forte - elimine um servidor e elimina as suas bases de dados, piscinas elásticas e piscinas SQK
- Participa no controlo de acesso baseado em [funções Azure (Azure RBAC)](../../role-based-access-control/overview.md) - bases de dados, piscinas elásticas e base de dados de armazém de dados dentro de um servidor herdam direitos de acesso do servidor
- É um elemento de ordem elevada da identidade das bases de dados, piscinas elásticas e base de dados de armazéns de dados para efeitos de gestão de recursos Azure (ver o esquema de URL para bases de dados e piscinas)
- Coloca recursos numa região
- Fornece um ponto final de ligação para o acesso à base de dados (`<serverName>`.database.windows.net)
- Fornece acesso a metadados relativamente aos recursos contidos através de DMVs, estabelecendo ligação a uma base de dados mestra
- Fornece a margem de manobra para políticas de gestão que se aplicam às suas bases de dados - logins, firewall, auditoria, deteção de ameaças, e tal
- É restringido por uma quota dentro da subscrição principal (seis servidores por subscrição por padrão - [ver limites de subscrição aqui](../../azure-resource-manager/management/azure-subscription-service-limits.md))
- Fornece a margem de manobra para quota de base de dados e quota DTU ou vCore para os recursos que contém (tais como 45.000 DTU)
- É a possibilidade de ver versão das capacidades ativadas em recursos contidos
- Os inícios de sessão principais ao nível do servidor podem gerir todas as bases de dados num servidor
- Pode conter logins semelhantes aos de exemplos do SQL Server no seu ambiente no local que tenha acesso a uma ou mais bases de dados no servidor, e pode ser-lhe concedido direitos administrativos limitados. Para obter mais informações, veja [Inícios de sessão](logins-create-manage.md).
- A colagem padrão para todas as bases de dados criadas num servidor é `SQL_LATIN1_GENERAL_CP1_CI_AS` , onde está o inglês `LATIN1_GENERAL` (Estados Unidos), `CP1` é a página de código 1252, `CI` é insensível a caso, e é `AS` sensível ao sotaque.

## <a name="manage-servers-databases-and-firewalls-using-the-azure-portal"></a>Gerir servidores, bases de dados e firewalls utilizando o portal Azure

Pode criar o grupo de recursos para um servidor antes do tempo ou enquanto cria o próprio servidor. Existem vários métodos para chegar a um novo formulário de servidor SQL, quer através da criação de um novo servidor SQL, quer como parte da criação de uma nova base de dados.

### <a name="create-a-blank-server"></a>Criar um servidor em branco

Para criar um servidor (sem base de dados, piscina elástica ou base de dados de armazém de dados) utilizando o [portal Azure,](https://portal.azure.com)navegue para um formulário de servidor SQL em branco (servidor lógico SQL).

### <a name="create-a-blank-or-sample-database-in-azure-sql-database"></a>Criar uma base de dados em branco ou amostra na Base de Dados Azure SQL

Para criar uma base de dados na Base de Dados SQL utilizando o [portal Azure,](https://portal.azure.com)navegue para um formulário de Base de Dados SQL em branco e forneça as informações solicitadas. Pode criar o grupo de recursos e o servidor antes do tempo ou enquanto cria a própria base de dados. Pode criar uma base de dados em branco ou criar uma base de dados de amostras baseada em Adventure Works LT.

  ![criar base de dados-1](./media/logical-servers/create-database-1.png)

> [!IMPORTANT]
> Para obter informações sobre a seleção do nível de preços da sua base de dados, consulte [o modelo de compra baseado em DTU](service-tiers-dtu.md) e o modelo de compra baseado em [vCore.](service-tiers-vcore.md)

Para criar um caso gerido, consulte [Criar um caso gerido](../managed-instance/instance-create-quickstart.md)

### <a name="manage-an-existing-server"></a>Gerir um servidor existente

Para gerir um servidor existente, navegue para o servidor utilizando uma série de métodos - tais como a partir de uma página específica de base de dados, a página de **servidores SQL** ou a página **de todos os recursos.**

Para gerir uma base de dados existente, navegue na página **de bases de dados SQL** e clique na base de dados que pretende gerir. A imagem que se segue mostra como começar a configurar uma firewall de nível de servidor para uma base de dados a partir da página **'Vista Geral'** para uma base de dados.

   ![regra de firewall do servidor](./media/single-database-create-quickstart/server-firewall-rule.png)

> [!IMPORTANT]
> Para configurar propriedades de desempenho para uma base de dados, consulte [o modelo de compra baseado em DTU](service-tiers-dtu.md) e o modelo de compra baseado em [vCore.](service-tiers-vcore.md)
> [!TIP]
> Para obter um arranque rápido do portal Azure, consulte [criar uma base de dados na Base de Dados SQL no portal Azure](single-database-create-quickstart.md).

## <a name="manage-servers-databases-and-firewalls-using-powershell"></a>Gerir servidores, bases de dados e firewalls usando PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
> [!IMPORTANT]
> O módulo PowerShell Azure Resource Manager ainda é suportado, mas todo o desenvolvimento futuro é para o módulo Az.Sql. Para estes cmdlets, consulte [AzureRM.Sql](/powershell/module/AzureRM.Sql/). Os argumentos para os comandos no módulo Az e nos módulos AzureRm são substancialmente idênticos.

Para criar e gerir servidores, bases de dados e firewalls com a Azure PowerShell, utilize os seguintes cmdlets PowerShell. Se precisar de instalar ou atualizar o PowerShell, consulte [instalar o módulo Azure PowerShell](/powershell/azure/install-az-ps). Para criar e gerir piscinas elásticas, consulte [piscinas elásticas.](elastic-pool-overview.md)

| Cmdlet | Descrição |
| --- | --- |
|[New-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase)|Cria uma base de dados |
|[Base de Dados Get-AzSql](/powershell/module/az.sql/get-azsqldatabase)|Obtém uma ou mais bases de dados|
|[Set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase)|Define propriedades para uma base de dados, ou move uma base de dados existente em uma piscina elástica|
|[Remove-AzSqlDatabase](/powershell/module/az.sql/remove-azsqldatabase)|Remove uma base de dados|
|[New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup)|Cria um grupo de recursos|
|[New-AzSqlServer](/powershell/module/az.sql/new-azsqlserver)|Cria um servidor|
|[Get-AzSqlServer](/powershell/module/az.sql/get-azsqlserver)|Devolve informações sobre servidores|
|[Set-AzSqlServer](/powershell/module/az.sql/set-azsqlserver)|Modifica propriedades de um servidor|
|[Remover-AzSqlServer](/powershell/module/az.sql/remove-azsqlserver)|Remove um servidor|
|[New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule)|Cria uma regra de firewall de nível de servidor |
|[Get-AzSqlServerFirewallRule](/powershell/module/az.sql/get-azsqlserverfirewallrule)|Obtém regras de firewall para um servidor|
|[Set-AzSqlServerFirewallRule](/powershell/module/az.sql/set-azsqlserverfirewallrule)|Modifica uma regra de firewall num servidor|
|[Remover-AzSqlServerFirewallRule](/powershell/module/az.sql/remove-azsqlserverfirewallrule)|Elimina uma regra de firewall de um servidor.|
| New-AzSqlServerVirtualNetworkRule | Cria uma [*regra de rede virtual*](vnet-service-endpoint-rule-overview.md), baseada numa sub-rede que é um ponto final de serviço de Rede Virtual. |

> [!TIP]
> Para obter um arranque rápido PowerShell, consulte [Criar uma base de dados na Base de Dados Azure SQL utilizando o PowerShell](single-database-create-quickstart.md). Para scripts de exemplo PowerShell, consulte [Use PowerShell para criar uma base de dados na Base de Dados Azure SQL e configurar uma regra de firewall](scripts/create-and-configure-database-powershell.md) e Monitor e escalar uma base de [dados na Base de Dados Azure SQL utilizando o PowerShell](scripts/monitor-and-scale-database-powershell.md).
>

## <a name="manage-servers-databases-and-firewalls-using-the-azure-cli"></a>Gerir servidores, bases de dados e firewalls utilizando o Azure CLI

Para criar e gerir servidores, bases de dados e firewalls com o [Azure CLI,](/cli/azure)utilize os seguintes comandos [Azure CLI SQL Database.](/cli/azure/sql/db) Utilize o [Cloud Shell](../../cloud-shell/overview.md) para executar a CLI no seu browser ou [instale-a](/cli/azure/install-azure-cli) no macOS, Linux ou Windows. Para criar e gerir piscinas elásticas, consulte [piscinas elásticas.](elastic-pool-overview.md)

| Cmdlet | Descrição |
| --- | --- |
|[az sql db create](/cli/azure/sql/db#az_sql_db_create) |Cria uma base de dados|
|[lista de az sql db](/cli/azure/sql/db#az_sql_db_list)|Lista todas as bases de dados geridas por um servidor, ou todas as bases de dados numa piscina elástica|
|[az sql db lista-edições](/cli/azure/sql/db#az_sql_db_list_editions)|Lista objetivos de serviço disponíveis e limites de armazenamento|
|[az sql db lista-usos](/cli/azure/sql/db#az_sql_db_list_usages)|Devoluções de utilizações de bases de dados|
|[az sql db show](/cli/azure/sql/db#az_sql_db_show)|Obtém uma base de dados
|[az sql db update](/cli/azure/sql/db#az_sql_db_update)|Atualiza uma base de dados|
|[az sql db apagar](/cli/azure/sql/db#az_sql_db_delete)|Remove uma base de dados|
|[az group create](/cli/azure/group#az_group_create)|Cria um grupo de recursos|
|[az sql server create](/cli/azure/sql/server#az_sql_server_create)|Cria um servidor|
|[lista de servidores az sql](/cli/azure/sql/server#az_sql_server_list)|Lista servidores|
|[lista de servidores az sql](/cli/azure/sql/server#az_sql_server_list-usages)|Retorna os usos do servidor|
|[show de servidor az sql](/cli/azure/sql/server#az_sql_server_show)|Obtém um servidor|
|[atualização do servidor az sql](/cli/azure/sql/server#az_sql_server_update)|Atualiza um servidor|
|[servidor az sql eliminar](/cli/azure/sql/server#az_sql_server_delete)|Elimina um servidor|
|[az sql servidor firewall-rule criar](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_create)|Cria uma regra de firewall do servidor|
|[lista de regras de firewall do servidor az sql](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_list)|Lista as regras de firewall num servidor|
|[az sql servidor firewall-rule show](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_show)|Mostra o detalhe de uma regra de firewall|
|[az sql servidor firewall-rule](/cli/azure/sql/server/firewall-rule##az_sql_server_firewall_rule_update)|Atualiza uma regra de firewall|
|[az sql servidor firewall-regra eliminar](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_delete)|Elimina uma regra de firewall|

> [!TIP]
> Para obter um quickstart Azure CLI, consulte [criar uma base de dados na Base de Dados Azure SQL utilizando o Azure CLI](az-cli-script-samples-content-guide.md). Para scripts de exemplo Azure CLI, consulte [utilizar o CLI para criar uma base de dados na Base de Dados Azure SQL e configurar uma regra de firewall](scripts/create-and-configure-database-cli.md) e utilizar o [CLI para monitorizar e escalar uma base de dados na Base de Dados Azure SQL](scripts/monitor-and-scale-database-cli.md).
>

## <a name="manage-servers-databases-and-firewalls-using-transact-sql"></a>Gerir servidores, bases de dados e firewalls utilizando o Transact-SQL

Para criar e gerir servidores, bases de dados e firewalls com Transact-SQL, utilize os seguintes comandos T-SQL. Pode emitir estes comandos utilizando o portal Azure, [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code,](https://code.visualstudio.com/docs)ou qualquer outro programa que possa ligar-se a um servidor e passar comandos Transact-SQL. Para gerir piscinas elásticas, consulte [piscinas elásticas.](elastic-pool-overview.md)

> [!IMPORTANT]
> Não é possível criar ou eliminar um servidor utilizando o Transact-SQL.

| Comando | Descrição |
| --- | --- |
|[CREATE DATABASE (Base de dados Azure SQL)](/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-current&preserve-view=true) | Cria uma nova base de dados na Base de Dados Azure SQL. Tem de estar ligado à base de dados principal para criar uma nova base de dados.|
|[CRIAR BASE DE DADOS (Azure Synapse)](/sql/t-sql/statements/create-database-transact-sql?view=azure-sqldw-latest&preserve-view=true) | Cria uma nova base de dados de armazém de dados em Azure Synapse. Tem de estar ligado à base de dados principal para criar uma nova base de dados.|
| [ALTER DATABASE (Base de dados Azure SQL)](/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current&preserve-view=true) |Modifica a base de dados ou piscina elástica. |
|[ALTER DATABASE (Azure Synapse Analytics)](/sql/t-sql/statements/alter-database-transact-sql?view=azure-sqldw-latest&preserve-view=true&tabs=sqlpool)|Modifica uma base de dados de armazém de dados em Azure Synapse.|
|[BASE DE DADOS DROP (Transact-SQL)](/sql/t-sql/statements/drop-database-transact-sql)|Elimina uma base de dados.|
|[sys.database_service_objetives (Base de Dados Azure SQL)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Devolve a edição (nível de serviço), o objetivo do serviço (nível de preços) e o nome elástico da piscina, se houver, para uma base de dados. Se for iniciado na base de dados principal de um servidor, retorna a informação em todas as bases de dados. Para a Azure Synapse, deve estar ligado à base de dados principal.|
|[sys.dm_db_resource_stats (Base de Dados Azure SQL)](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database)| Devoluções cpu, IO e consumo de memória para uma base de dados na Base de Dados Azure SQL. Existe uma linha por cada 15 segundos, mesmo que não haja atividade na base de dados.|
|[sys.resource_stats (Base de Dados Azure SQL)](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)|Devolve dados de utilização e armazenamento de CPU para uma base de dados na Base de Dados Azure SQL. Os dados são recolhidos e agregados dentro de intervalos de cinco minutos.|
|[sys.database_connection_stats (Base de Dados Azure SQL)](/sql/relational-databases/system-catalog-views/sys-database-connection-stats-azure-sql-database)|Contém estatísticas para eventos de conectividade de base de dados para a Base de Dados Azure SQL, fornecendo uma visão geral dos sucessos e falhas da ligação de base de dados. |
|[sys.event_log (Base de Dados Azure SQL)](/sql/relational-databases/system-catalog-views/sys-event-log-azure-sql-database)|Devolve ligações bem sucedidas da base de dados Azure SQL Database, falhas de ligação e impasses para a Base de Dados Azure SQL. Pode utilizar esta informação para rastrear ou resolver problemas na sua atividade de base de dados.|
|[sp_set_firewall_rule (Base de Dados Azure SQL)](/sql/relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database)|Cria ou atualiza as definições de firewall ao nível do servidor para o seu servidor. Este procedimento armazenado só está disponível na base de dados principal para o início de sessão principal de nível de servidor. Uma regra de firewall ao nível do servidor só pode ser criada usando o Transact-SQL depois de a primeira regra de firewall de nível de servidor ter sido criada por um utilizador com permissões ao nível do Azure|
|[sys.firewall_rules (Base de Dados Azure SQL)](/sql/relational-databases/system-catalog-views/sys-firewall-rules-azure-sql-database)|Retorna informações sobre as definições de firewall ao nível do servidor associadas a um servidor.|
|[sp_delete_firewall_rule (Base de Dados Azure SQL)](/sql/relational-databases/system-stored-procedures/sp-delete-firewall-rule-azure-sql-database)|Remove as definições de firewall ao nível do servidor de um servidor. Este procedimento armazenado só está disponível na base de dados principal para o início de sessão principal de nível de servidor.|
|[sp_set_database_firewall_rule (Base de Dados Azure SQL)](/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database)|Cria ou atualiza as regras de firewall ao nível da base de dados para uma base de dados na Base de Dados Azure SQL. As regras de firewall da base de dados podem ser configuradas para a base de dados principal e para bases de dados de utilizadores na Base de Dados SQL. As regras de firewall da base de dados são úteis quando se utilizam utilizadores de bases de dados contidos. As regras de firewall da base de dados não são suportadas em Azure Synapse.|
|[sys.database_firewall_rules (Base de Dados Azure SQL)](/sql/relational-databases/system-catalog-views/sys-database-firewall-rules-azure-sql-database)|Retorna informações sobre as definições de firewall ao nível da base de dados para uma base de dados na Base de Dados Azure SQL. |
|[sp_delete_database_firewall_rule (Base de Dados Azure SQL)](/sql/relational-databases/system-stored-procedures/sp-delete-database-firewall-rule-azure-sql-database)|Remove a definição de firewall ao nível da base de dados para uma base de dados sua na Base de Dados Azure SQL. |

> [!TIP]
> Para iniciar um quickstart utilizando o SQL Server Management Studio no Microsoft Windows, consulte [a Base de Dados Azure SQL: Utilize o SQL Server Management Studio para ligar e consultar dados](connect-query-ssms.md). Para iniciar um quickstart utilizando o Código do Estúdio Visual no macOS, Linux ou Windows, consulte [a Base de Dados Azure SQL: Utilize o Código do Estúdio Visual para ligar e consultar dados](connect-query-vscode.md).

## <a name="manage-servers-databases-and-firewalls-using-the-rest-api"></a>Gerir servidores, bases de dados e firewalls utilizando a API REST

Para criar e gerir servidores, bases de dados e firewalls, utilize estes pedidos de API REST.

| Comando | Descrição |
| --- | --- |
|[Servidores - Criar ou atualizar](/rest/api/sql/servers/createorupdate)|Cria ou atualiza um novo servidor.|
|[Servidores - Eliminar](/rest/api/sql/servers/delete)|Elimina um servidor.|
|[Servidores - Obter](/rest/api/sql/servers/get)|Arranja um servidor.|
|[Servidores - Lista](/rest/api/sql/servers/list)|Devolve uma lista de servidores.|
|[Servidores - Lista por grupo de recursos](/rest/api/sql/servers/listbyresourcegroup)|Devolve uma lista de servidores num grupo de recursos.|
|[Servidores - Atualização](/rest/api/sql/servers/update)|Atualiza um servidor existente.|
|[Bases de dados - Criar ou atualizar](/rest/api/sql/databases/createorupdate)|Cria uma nova base de dados ou atualiza uma base de dados existente.|
|[Bases de dados - Eliminar](/rest/api/sql/databases/delete)|Elimina uma base de dados.|
|[Bases de dados - Obter](/rest/api/sql/databases/get)|Tem uma base de dados.|
|[Bases de dados - Lista por piscina elástica](/rest/api/sql/databases/listbyelasticpool)|Devolve uma lista de bases de dados numa piscina elástica.|
|[Bases de dados - Lista por servidor](/rest/api/sql/databases/listbyserver)|Retorna uma lista de bases de dados num servidor.|
|[Bases de dados - Atualização](/rest/api/sql/databases/update)|Atualiza uma base de dados existente.|
|[Regras de firewall - Criar ou atualizar](/rest/api/sql/firewallrules/createorupdate)|Cria ou atualiza uma regra de firewall.|
|[Regras de firewall - Eliminar](/rest/api/sql/firewallrules/delete)|Elimina uma regra de firewall.|
|[Regras de firewall - Obter](/rest/api/sql/firewallrules/get)|Tem uma regra de firewall.|
|[Regras de firewall - Lista por servidor](/rest/api/sql/firewallrules/listbyserver)|Devolve uma lista de regras de firewall.|

## <a name="next-steps"></a>Passos seguintes

- Para saber sobre a migração de uma base de dados do SQL Server para a Base de Dados Azure SQL, consulte [a Migração para a Base de Dados SQL Azure](migrate-to-database-from-sql-server.md).
- Para obter informações sobre as funcionalidades suportadas, veja [Funcionalidades](features-comparison.md).