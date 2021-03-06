---
title: Ativar as informações do SQL
description: Ativar insights SQL no Azure Monitor
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/15/2021
ms.openlocfilehash: 012aa364fe9e379455b6b63f7c9e541d2d5b97ed
ms.sourcegitcommit: 6f1aa680588f5db41ed7fc78c934452d468ddb84
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/19/2021
ms.locfileid: "107726903"
---
# <a name="enable-sql-insights-preview"></a>Ativar insights SQL (pré-visualização)
Este artigo descreve como permitir [insights SQL](sql-insights-overview.md) para monitorizar as suas implementações SQL. A monitorização é realizada a partir de uma máquina virtual Azure que faz uma ligação às suas implementações SQL e utiliza Vistas de Gestão Dinâmica (DMVs) para recolher dados de monitorização. Pode controlar quais conjuntos de dados são recolhidos e a frequência de recolha utilizando um perfil de monitorização.

> [!NOTE]
> Para ativar os insights DO SQL criando o perfil de monitorização e a máquina virtual utilizando um modelo de gestor de recursos, consulte [amostras de modelo de Gestor de Recursos para insights SQL](resource-manager-sql-insights.md).

## <a name="create-log-analytics-workspace"></a>Criar uma área de trabalho do Log Analytics
Os insights SQL armazenam os seus dados em um ou mais [espaços de trabalho do Log Analytics.](../logs/data-platform-logs.md#log-analytics-workspaces)  Antes de ativar o SQL Insights, tem de [criar um espaço de trabalho](../logs/quick-create-workspace.md) ou selecionar um existente. Um espaço de trabalho único pode ser usado com múltiplos perfis de monitorização, mas o espaço de trabalho e os perfis devem estar localizados na mesma região de Azure. Para ativar e aceder às funcionalidades em insights SQL, tem de ter o [papel de colaborador](../logs/manage-access.md) do Log Analytics no espaço de trabalho. 

## <a name="create-monitoring-user"></a>Criar utilizador de monitorização 
Precisa de um utilizador nas implementações SQL que pretende monitorizar. Siga os procedimentos abaixo para diferentes tipos de implementações SQL.

As instruções abaixo cobrem o processo por tipo de SQL que pode monitorizar.  Para o conseguir com um script em vários SQL resouces de uma só vez, consulte o seguinte [ficheiro README](https://github.com/microsoft/Application-Insights-Workbooks/blob/master/Workbooks/Workloads/SQL/SQL%20Insights%20Onboarding%20Scripts/Permissions_LoginUser_Account_Creation-README.txt) e [o exemplo script](https://github.com/microsoft/Application-Insights-Workbooks/blob/master/Workbooks/Workloads/SQL/SQL%20Insights%20Onboarding%20Scripts/Permissions_LoginUser_Account_Creation.ps1).


### <a name="azure-sql-database"></a>Base de dados SQL do Azure
Abrir base de dados Azure SQL com [SQL Server Management Studio](../../azure-sql/database/connect-query-ssms.md) ou [Editor de Consulta (pré-visualização)](../../azure-sql/database/connect-query-portal.md) no portal Azure.

Execute o seguinte script para criar um utilizador com as permissões necessárias. Substitua *o utilizador* por um nome de utilizador e *uma palavra-passe com* uma palavra-passe.

```sql
CREATE USER [user] WITH PASSWORD = N'mystrongpassword'; 
GO 
GRANT VIEW DATABASE STATE TO [user]; 
GO 
```

:::image type="content" source="media/sql-insights-enable/telegraf-user-database-script.png" alt-text="Crie o script do utilizador da telegraf." lightbox="media/sql-insights-enable/telegraf-user-database-script.png":::

Verifique se o utilizador foi criado.

:::image type="content" source="media/sql-insights-enable/telegraf-user-database-verify.png" alt-text="Verifique o script do utilizador da telegraf." lightbox="media/sql-insights-enable/telegraf-user-database-verify.png":::

```sql
select name as username,
       create_date,
       modify_date,
       type_desc as type,
       authentication_type_desc as authentication_type
from sys.database_principals
where type not in ('A', 'G', 'R', 'X')
       and sid is not null
order by username
```

### <a name="azure-sql-managed-instance"></a>Instância Gerida do Azure SQL
Inicie sessão no seu Azure SQL Managed Instance e utilize [o SQL Server Management Studio](../../azure-sql/database/connect-query-ssms.md) ou uma ferramenta similar para executar o seguinte script para criar o utilizador de monitorização com as permissões necessárias. Substitua *o utilizador* por um nome de utilizador e *uma palavra-passe com* uma palavra-passe.

 
```sql
USE master; 
GO 
CREATE LOGIN [user] WITH PASSWORD = N'mystrongpassword'; 
GO 
GRANT VIEW SERVER STATE TO [user]; 
GO 
GRANT VIEW ANY DEFINITION TO [user]; 
GO 
```

### <a name="sql-server"></a>SQL Server
Inicie sessão na sua máquina virtual Azure que executa o SQL Server e utilize [o SQL Server Management Studio](../../azure-sql/database/connect-query-ssms.md) ou uma ferramenta similar para executar o seguinte script para criar o utilizador de monitorização com as permissões necessárias. Substitua *o utilizador* por um nome de utilizador e *uma palavra-passe com* uma palavra-passe.

 
```sql
USE master; 
GO 
CREATE LOGIN [user] WITH PASSWORD = N'mystrongpassword'; 
GO 
GRANT VIEW SERVER STATE TO [user]; 
GO 
GRANT VIEW ANY DEFINITION TO [user]; 
GO
```

Verifique se o utilizador foi criado.

```sql
select name as username,
       create_date,
       modify_date,
       type_desc as type
from sys.server_principals
where type not in ('A', 'G', 'R', 'X')
       and sid is not null
order by username
```

## <a name="create-azure-virtual-machine"></a>Criar máquina virtual Azure 
Terá de criar uma ou mais máquinas virtuais Azure que serão usadas para recolher dados para monitorizar o SQL.  

> [!NOTE]
>  Os [perfis de monitorização](#create-sql-monitoring-profile) especificam quais os dados que irá recolher dos diferentes tipos de SQL que pretende monitorizar. Cada máquina virtual de monitorização pode ter apenas um perfil de monitorização associado. Se tiver necessidade de múltiplos perfis de monitorização, então precisa de criar uma máquina virtual para cada um.

### <a name="azure-virtual-machine-requirements"></a>Requisitos das máquinas Virtuais do Azure
As máquinas virtuais Azure têm os seguintes requisitos.

- Sistema operativo: Ubuntu 18.04 
- Tamanhos de máquina virtual Azure recomendados: Standard_B2s (2 cpus, 4 memória GiB) 
- Regiões apoiadas: Qualquer [região apoiada pelo agente Azure Monitor](../agents/azure-monitor-agent-overview.md#supported-regions)

> [!NOTE]
> O tamanho da máquina virtual Standard_B2s (2 cpus, 4 GiB) irá suportar até 100 cordas de ligação. Não deve atribuir mais de 100 ligações a uma única máquina virtual.

Dependendo das definições de rede dos seus recursos SQL, as máquinas virtuais podem ter de ser colocadas na mesma rede virtual que os seus recursos SQL para que possam fazer ligações de rede para recolher dados de monitorização.  

## <a name="configure-network-settings"></a>Configurar as definições de rede
Cada tipo de SQL oferece métodos para a sua máquina virtual de monitorização para aceder de forma segura ao SQL.  As secções abaixo cobrem as opções com base no tipo de SQL.

### <a name="azure-sql-databases"></a>Bases de Dados SQL do Azure  

Os insights SQL suportam o acesso à sua Base de Dados Azure SQL através do seu ponto final público, bem como da sua rede virtual.

Para acesso através do ponto final público, adicionaria uma regra na página de **definições** de Firewall e na secção de definições de [firewall IP.](https://docs.microsoft.com/azure/azure-sql/database/network-access-controls-overview#ip-firewall-rules)  Para especificar o acesso a partir de uma rede virtual, pode definir [regras de firewall de rede virtuais](https://docs.microsoft.com/azure/azure-sql/database/network-access-controls-overview#virtual-network-firewall-rules) e definir as [etiquetas de serviço exigidas pelo agente Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/agents/azure-monitor-agent-overview#networking).  [Este artigo](https://docs.microsoft.com/azure/azure-sql/database/network-access-controls-overview#ip-vs-virtual-network-firewall-rules) descreve as diferenças entre estes dois tipos de regras de firewall.

:::image type="content" source="media/sql-insights-enable/set-server-firewall.png" alt-text="Definir firewall do servidor" lightbox="media/sql-insights-enable/set-server-firewall.png":::

:::image type="content" source="media/sql-insights-enable/firewall-settings.png" alt-text="Definições de firewall." lightbox="media/sql-insights-enable/firewall-settings.png":::


### <a name="azure-sql-managed-instances"></a>Azure SQL Managed Instance 

Se a sua máquina virtual de monitorização estiver no mesmo VNet que os seus recursos SQL MI, consulte [Connect dentro do mesmo VNet](https://docs.microsoft.com/azure/azure-sql/managed-instance/connect-application-instance#connect-inside-the-same-vnet). Se a sua máquina virtual de monitorização estiver nos diferentes VNet do que os seus recursos SQL MI, consulte [Connect dentro de um VNet diferente](https://docs.microsoft.com/azure/azure-sql/managed-instance/connect-application-instance#connect-inside-a-different-vnet).


### <a name="azure-virtual-machine-and-azure-sql-virtual-machine"></a>Máquina virtual Azure e máquina virtual Azure SQL  
Se a sua máquina virtual de monitorização estiver no mesmo VNet que os recursos da sua máquina virtual SQL, consulte [o Connect to SQL Server dentro de uma rede virtual](https://docs.microsoft.com/azure/azure-sql/virtual-machines/windows/ways-to-connect-to-sql#connect-to-sql-server-within-a-virtual-network). Se a sua máquina virtual de monitorização estiver nos diferentes VNet do que os recursos da sua máquina virtual SQL, consulte  [o Connect to SQL Server através da internet](https://docs.microsoft.com/azure/azure-sql/virtual-machines/windows/ways-to-connect-to-sql#connect-to-sql-server-over-the-internet).

## <a name="store-monitoring-password-in-key-vault"></a>Armazenar senha de monitorização no Cofre de Chaves
Deve armazenar as suas palavras-passe de ligação do utilizador SQL num Cofre de Chaves em vez de as introduzir diretamente nas cordas de ligação do perfil de monitorização.

Ao configurar o seu perfil para monitorização SQL, necessitará de uma das seguintes permissões no recurso Key Vault que pretende utilizar:

- Microsoft.Authorization/roleAssignments/write 
- Microsoft.Autorização/funAsignments/eliminar permissões como Administrador de Acesso ao Utilizador ou Proprietário 

Uma nova política de acesso será criada automaticamente como parte da criação do seu perfil de monitorização SQL que utiliza o Cofre-Chave especificado. Utilizar *Permitir o acesso de todas as redes* para definições de rede de cofres de chave.


## <a name="create-sql-monitoring-profile"></a>Criar perfil de monitorização SQL
Introdem insights DE SQL selecionando **SQL (pré-visualização)** a partir da secção **Insights** do menu **Azure Monitor** no portal Azure. Clique **em Criar novo perfil.** 

:::image type="content" source="media/sql-insights-enable/create-new-profile.png" alt-text="Criar um novo perfil." lightbox="media/sql-insights-enable/create-new-profile.png":::

O perfil armazenará as informações que pretende recolher nos seus sistemas SQL.  Tem configurações específicas para: 

- Base de Dados SQL do Azure 
- Azure SQL Managed Instance 
- SQL Server em execução em máquinas virtuais  

Por exemplo, pode criar um perfil chamado *SQL Production* e outro chamado *SQL Staging* com diferentes configurações para a frequência da recolha de dados, quais os dados a recolher e para que espaço de trabalho enviar os dados. 

O perfil é armazenado como um recurso [de regra de recolha de dados](../agents/data-collection-rule-overview.md) na subscrição e grupo de recursos que seleciona. Cada perfil precisa do seguinte:

- O nome. Não pode ser editado uma vez criado.
- Localização. Esta é uma região de Azure.
- Log Analytics espaço de trabalho para armazenar os dados de monitorização.
- Definições de recolha para a frequência e tipo de dados de monitorização sql para recolher.

> [!NOTE]
> A localização do perfil deve estar no mesmo local que o espaço de trabalho Log Analytics para o quais planeia enviar os dados de monitorização.


:::image type="content" source="media/sql-insights-enable/profile-details.png" alt-text="Detalhes do perfil." lightbox="media/sql-insights-enable/profile-details.png":::

Clique **em Criar o perfil de monitorização** assim que tiver introduzido os detalhes para o seu perfil de monitorização. Pode levar até um minuto para o perfil ser implantado.  Se não vir o novo perfil listado na caixa de **combinação de perfis de monitorização,** clique no botão de atualização e deverá aparecer assim que a implementação estiver concluída.  Uma vez selecionado o novo perfil, selecione o **separador de perfil 'Gerir'** para adicionar uma máquina de monitorização que estará associada ao perfil.

### <a name="add-monitoring-machine"></a>Adicionar máquina de monitorização
**Selecione Adicionar a máquina de monitorização** para abrir um painel de contexto para escolher a máquina virtual para configurar para monitorizar as suas instâncias SQL e fornecer as cordas de ligação.

Selecione a subscrição e o nome da sua máquina virtual de monitorização. Se estiver a utilizar o Key Vault para armazenar a sua palavra-passe para o utilizador de monitorização, selecione os recursos do Cofre chave com estes segredos e introduza o URL e o nome secreto a utilizar nas cordas de ligação. Consulte a secção seguinte para obter mais informações sobre a identificação da cadeia de ligação para diferentes implementações SQL.


:::image type="content" source="media/sql-insights-enable/add-monitoring-machine.png" alt-text="Adicione a máquina de monitorização." lightbox="media/sql-insights-enable/add-monitoring-machine.png":::


### <a name="add-connection-strings"></a>Adicionar cordas de ligação 
A cadeia de ligação especifica o nome de utilizador que os insights SQL devem utilizar ao iniciar sessão em SQL para executar as Vistas de Gestão Dinâmica. Se estiver a utilizar um Cofre-Chave para armazenar a palavra-passe para o utilizador de monitorização, forneça o URL e o nome do segredo para utilizar. 

A cadeia de ligações variará para cada tipo de recurso SQL:

#### <a name="azure-sql-databases"></a>Bases de Dados SQL do Azure 
Introduza o fio de ligação no formulário:

```
sqlAzureConnections": [ 
   "Server=mysqlserver.database.windows.net;Port=1433;Database=mydatabase;User Id=$username;Password=$password;" 
}
```

Obtenha os detalhes do item do menu de cordas De Ligação para a base de **dados.**

:::image type="content" source="media/sql-insights-enable/connection-string-sql-database.png" alt-text="Cadeia de conexão de base de dados SQL" lightbox="media/sql-insights-enable/connection-string-sql-database.png":::

Para monitorizar um secundário legível, inclua o valor-chave na cadeia de `ApplicationIntent=ReadOnly` ligação. A SQL Insights suporta a monitorização de um único secundário. Os dados recolhidos serão marcados para refletir o primário ou o secundário. 


#### <a name="azure-virtual-machines-running-sql-server"></a>Máquinas virtuais Azure que executam o SQL Server 
Introduza o fio de ligação no formulário:

```
"sqlVmConnections": [ 
   "Server=MyServerIPAddress;Port=1433;User Id=$username;Password=$password;" 
] 
```

Se a sua máquina virtual de monitorização estiver no mesmo VNET, utilize o endereço IP privado do Servidor.  Caso contrário, utilize o endereço IP público. Se estiver a utilizar a máquina virtual Azure SQL, pode ver qual a porta a utilizar aqui na página **De Segurança** para o recurso.

:::image type="content" source="media/sql-insights-enable/sql-vm-security.png" alt-text="Segurança da máquina virtual SQL" lightbox="media/sql-insights-enable/sql-vm-security.png":::


### <a name="azure-sql-managed-instances"></a>Azure SQL Managed Instance 
Introduza o fio de ligação no formulário:

```
"sqlManagedInstanceConnections": [ 
      "Server= mysqlserver.database.windows.net;Port=1433;User Id=$username;Password=$password;", 
    ] 
```
Obtenha os detalhes do item do menu **de cordas De Ligação** para a instância gerida.


:::image type="content" source="media/sql-insights-enable/connection-string-sql-managed-instance.png" alt-text="Cadeia de conexão sql Managed Instance" lightbox="media/sql-insights-enable/connection-string-sql-managed-instance.png":::

Para monitorizar um secundário legível, inclua o valor-chave na cadeia de `ApplicationIntent=ReadOnly` ligação. A SQL Insights suporta a monitorização de um único secundário e os dados recolhidos serão marcados para refletir o Primário ou o Secundário. 


## <a name="monitoring-profile-created"></a>Perfil de monitorização criado 

**Selecione Adicionar a máquina virtual de monitorização** para configurar a máquina virtual para recolher dados dos seus recursos SQL. Não volte ao **separador 'Vista Geral'.**  Em poucos minutos, a coluna Status deve ser alterada para ler "Recolha", deve ver os dados dos recursos SQL que escolheu monitorizar.

Se não vir dados, consulte [insights SQL de resolução de problemas](sql-insights-troubleshoot.md) para identificar o problema. 

:::image type="content" source="media/sql-insights-enable/profile-created.png" alt-text="Perfil criado" lightbox="media/sql-insights-enable/profile-created.png":::

> [!NOTE]
> Se precisar de atualizar o seu perfil de monitorização ou as cadeias de ligação nos seus VMs de monitorização, poderá fazê-lo através do separador de perfil SQL **Manage.**  Uma vez guardadas as atualizações, as alterações serão aplicadas em aproximadamente 5 minutos.

## <a name="next-steps"></a>Passos seguintes

- Consulte [insights SQL de resolução de problemas](sql-insights-troubleshoot.md) se as informações do SQL não estiverem a funcionar corretamente depois de ativadas.
