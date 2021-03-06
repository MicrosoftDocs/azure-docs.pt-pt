---
title: Criar utilizadores - Azure Database for MariaDB
description: Este artigo descreve como pode criar novas contas de utilizador para interagir com uma Base de Dados Azure para o servidor MariaDB.
author: savjani
ms.author: pariks
ms.service: mariadb
ms.topic: how-to
ms.date: 01/18/2021
ms.openlocfilehash: 28ec060e95d09cb150fc699919dde6cc0e1eaf23
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "98663711"
---
# <a name="create-users-in-azure-database-for-mariadb"></a>Criar utilizadores no Azure Database for MariaDB

Este artigo descreve como pode criar utilizadores na Base de Dados Azure para MariaDB.

Quando criou a sua Base de Dados Azure para MariaDB, forneceu um nome de utilizador e senha de login de administração do servidor. Para mais informações, pode seguir o [Quickstart.](quickstart-create-mariadb-server-database-using-azure-portal.md) Pode localizar o nome de utilizador do seu servidor a partir do portal Azure.

> [!NOTE]
> Este artigo contém referências ao termo _escravo_, um termo que a Microsoft já não utiliza. Quando o termo for removido do software, vamos removê-lo deste artigo.

O utilizador de administração do servidor obtém certos privilégios para o seu servidor listado: SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, RELOAD, PROCESS, REFERENCES, INDEX, ALTER, SHOW DATABASES, CREATE TEMPORARY Tables, LOCK TABLES, EXECUTE, REPLICATION SLAVE, REPLICATION CLIENT, CREATE VIEW, SHOW VIEW, CREATE ROUTINE, ALTER ROUTINE, CREATE, EVENT, TRIGGER, REPLICATION SLAVE, REPLICATION CLIENT, CREATE VIEW, SHOW VIEW, CREATE ROUTINE, CREATE, EVENT, TRIGGER, REPLICATION SLAVE, REPLICATION CLIENT, CREATE VIEW, SHOW VIEW, CREATE, EVENT, TRIGGER, REPLICATION SLAVE, REPLICATION CLIENT, CREATE VIEW, SHOW VIEW, CREATE ROUTINE, CREATE, EVENT, TRIGGER, REPLICATION SLAVE, REPLICATION CLIENT, CREATE VIEW, SHOW VIEW, CREATE, EVENT, TRIGGER, REPLICATION SLAVE, REPLICATION CLIENT, CREATE VIEW

Após a criação da Base de Dados Azure para o servidor MariaDB, pode utilizar a primeira conta de utilizador de administração do servidor para criar mais utilizadores e conceder acesso administrativo aos mesmos. Além disso, a conta de administração do servidor pode ser usada para criar utilizadores menos privilegiados que tenham acesso a esquemas de bases de dados individuais.

> [!NOTE]
> O papel de SUPER privilégio e DBA não são apoiados. Reveja os [privilégios](concepts-limits.md#privileges--data-manipulation-support) no artigo de limitações para entender o que não é suportado no serviço.<br><br>
> Os plugins de palavras-passe como "validate_password" e "caching_sha2_password" não são suportados pelo serviço.

## <a name="create-more-admin-users"></a>Criar mais utilizadores administrativos

1. Obtenha a informação de ligação e o nome de utilizador administrativo.
   Para ligar ao seu servidor de bases de dados, precisa do nome completo do servidor e das credenciais de início de sessão de administrador. Pode encontrar facilmente o nome do servidor e informações de inscrição a partir da página **'Vista Geral'** do servidor ou da página **Propriedades** no portal Azure.

2. Utilize a conta de administração e a palavra-passe para ligar ao servidor de base de dados. Utilize a sua ferramenta cliente preferida, como mySQL Workbench, mysql.exe, HeidiSQL, ou outros.
   Se não tiver a certeza de como se conectar, consulte [a workbench MySQL para ligar e consultar dados](./connect-workbench.md)

3. Editar e executar o seguinte código SQL. Substitua o seu novo nome de utilizador pelo valor do espaço reservado `new_master_user` . Esta sintaxe concede os privilégios listados em todos os esquemas de base de dados *(.*) ao nome de utilizador (new_master_user neste exemplo). 

   ```sql
   CREATE USER 'new_master_user'@'%' IDENTIFIED BY 'StrongPassword!';
   
   GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, RELOAD, PROCESS, REFERENCES, INDEX, ALTER, SHOW DATABASES, CREATE TEMPORARY TABLES, LOCK TABLES, EXECUTE, REPLICATION SLAVE, REPLICATION CLIENT, CREATE VIEW, SHOW VIEW, CREATE ROUTINE, ALTER ROUTINE, CREATE USER, EVENT, TRIGGER ON *.* TO 'new_master_user'@'%' WITH GRANT OPTION; 
   
   FLUSH PRIVILEGES;
   ```

4. Verifique os subsídios.

   ```sql
   USE sys;
   
   SHOW GRANTS FOR 'new_master_user'@'%';
   ```

## <a name="create-database-users"></a>Criar utilizadores da base de dados

1. Obtenha a informação de ligação e o nome de utilizador administrativo.
   Para ligar ao seu servidor de bases de dados, precisa do nome completo do servidor e das credenciais de início de sessão de administrador. Pode encontrar facilmente o nome do servidor e informações de inscrição a partir da página **'Vista Geral'** do servidor ou da página **Propriedades** no portal Azure. 

2. Utilize a conta de administração e a palavra-passe para ligar ao servidor de base de dados. Utilize a sua ferramenta cliente preferida, como mySQL Workbench, mysql.exe, HeidiSQL, ou outros.
   Se não tiver a certeza de como se conectar, consulte [a workbench MySQL para ligar e consultar dados](./connect-workbench.md)

3. Editar e executar o seguinte código SQL. Substitua o valor do espaço reservado `db_user` pelo novo nome de utilizador pretendido e o valor do espaço reservado pelo seu próprio nome de base `testdb` de dados.

   Esta sintaxe de código sql cria uma nova base de dados chamada testdb para fins de exemplo. Em seguida, cria um novo utilizador na Base de Dados Azure para o serviço MariaDB, e concede todos os privilégios ao novo esquema de base de dados (testdb) \* para esse utilizador. 

   ```sql
   CREATE DATABASE testdb;
   
   CREATE USER 'db_user'@'%' IDENTIFIED BY 'StrongPassword!';
   
   GRANT ALL PRIVILEGES ON testdb . * TO 'db_user'@'%';
   
   FLUSH PRIVILEGES;
   ```

4. Verifique os subsídios dentro da base de dados.

   ```sql
   USE testdb;
   
   SHOW GRANTS FOR 'db_user'@'%';
   ```

5. Faça login no servidor, especificando a base de dados designada utilizando o novo nome de utilizador e senha. Este exemplo mostra a linha de comando mysql. Com este comando, é solicitado a senha para o nome de utilizador. Substitua o nome do seu próprio servidor, nome de base de dados e nome de utilizador.

   ```bash
   mysql --host mydemoserver.mariadb.database.azure.com --database testdb --user db_user@mydemoserver -p
   ```

   Para obter mais informações sobre a gestão da conta de utilizador, consulte a documentação MariaDB para [gestão de conta de utilizador,](https://mariadb.com/kb/en/library/user-account-management/) [Grant Syntax](https://mariadb.com/kb/en/library/grant/)e [Privileges](https://mariadb.com/kb/en/library/grant/#privilege-levels).

## <a name="azure_superuser"></a>azure_superuser

Todas as bases de dados Azure para servidores MySQL são criadas com um utilizador chamado "azure_superuser". Esta é uma conta de sistema criada pela Microsoft para gerir o servidor para realizar monitorização, backups e outras manutenção regulares. Os engenheiros de serviço também podem usar esta conta para aceder ao servidor durante um incidente com autenticação de certificados e devem solicitar o acesso usando processos just-in-time (JIT).

## <a name="next-steps"></a>Passos seguintes

Abra a firewall para os endereços IP das máquinas dos novos utilizadores para permitir a sua ligação: [Criar e gerir a base de dados Azure para regras de firewall MariaDB utilizando o portal Azure](howto-manage-firewall-portal.md)  

<!--or [Azure CLI](howto-manage-firewall-using-cli.md).-->
