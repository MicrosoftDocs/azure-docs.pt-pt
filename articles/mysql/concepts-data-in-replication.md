---
title: Replicação de dados - Base de Dados Azure para o MySQL
description: Saiba como utilizar a replicação de dados para sincronizar a partir de um servidor externo na Base de Dados Azure para o serviço MySQL.
author: savjani
ms.author: pariks
ms.service: mysql
ms.topic: conceptual
ms.date: 04/08/2021
ms.openlocfilehash: 83ad2d4f392afb6bb33c99a5449b9bc8ceeaa058
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/13/2021
ms.locfileid: "107312812"
---
# <a name="replicate-data-into-azure-database-for-mysql"></a>Replicar dados na Base de Dados Azure para o MySQL

A replicação de dados permite sincronizar dados de um servidor MySQL externo na Base de Dados Azure para o serviço MySQL. O servidor externo pode estar no local, em máquinas virtuais ou num serviço de base de dados hospedado por outros fornecedores de nuvem. A replicação de dados baseia-se na posição de registo binário (binlog) baseada na posição de ficheiro ou na replicação baseada em GTID nativa do MySQL. Para saber mais sobre a replicação do binlog, consulte a visão geral da [replicação do binlog MySQL](https://dev.mysql.com/doc/refman/5.7/en/binlog-replication-configuration-overview.html).

## <a name="when-to-use-data-in-replication"></a>Quando utilizar a replicação de dados

Os principais cenários a ter em conta sobre a utilização da replicação de dados são:

- **Sincronização de dados híbridos:** Com a replicação de dados, pode manter os dados sincronizados entre os servidores no local e a Base de Dados Azure para o MySQL. Esta sincronização é útil para criar aplicações híbridas. Este método é apelativo quando se tem um servidor de base de dados local existente, mas pretende mover os dados para uma região mais próxima dos utilizadores finais.
- **Sincronização multi-nuvem:** Para soluções complexas em nuvem, utilize a Replicação de Dados para sincronizar dados entre a Base de Dados Azure para o MySQL e diferentes fornecedores de nuvem, incluindo máquinas virtuais e serviços de base de dados alojados nessas nuvens.

Para cenários de migração, utilize o [Serviço de Migração de Bases de Dados Azure](https://azure.microsoft.com/services/database-migration/)(DMS).

## <a name="limitations-and-considerations"></a>Limitações e considerações

### <a name="data-not-replicated"></a>Dados não replicados

A base de dados do [*sistema mysql*](https://dev.mysql.com/doc/refman/5.7/en/system-schema.html) no servidor de origem não é replicada. Além disso, as alterações nas contas e permissões no servidor de origem não são replicadas. Se criar uma conta no servidor de origem e esta conta precisar de aceder ao servidor de réplicas, crie manualmente a mesma conta no servidor de réplicas. Para entender quais as tabelas contidas na base de dados do sistema, consulte o [manual MySQL](https://dev.mysql.com/doc/refman/5.7/en/system-schema.html).

### <a name="filtering"></a>Filtragem

Para saltar as tabelas de replicação do seu servidor de origem (hospedados no local, em máquinas virtuais ou num serviço de base de dados hospedado por outros fornecedores de nuvem), o `replicate_wild_ignore_table` parâmetro é suportado. Opcionalmente, atualize este parâmetro no servidor de réplica alojado no Azure utilizando o [portal Azure](howto-server-parameters.md) ou [O Azure CLI](howto-configure-server-parameters-using-cli.md).

Para saber mais sobre este parâmetro, reveja a [documentação mySQL.](https://dev.mysql.com/doc/refman/8.0/en/replication-options-replica.html#option_mysqld_replicate-wild-ignore-table)

## <a name="supported-in-general-purpose-or-memory-optimized-tier-only"></a>Suportado apenas no nível otimizado para fins gerais ou memória

A replicação de dados só é suportada nos níveis de preços otimizados para fins gerais e memória.

### <a name="requirements"></a>Requisitos

- A versão do servidor de origem deve ser pelo menos a versão 5.6 do MySQL.
- As versões de servidor de origem e réplica devem ser as mesmas. Por exemplo, ambos devem ser a versão 5.6 do MySQL ou ambas devem ser a versão 5.7 do MySQL.
- Cada mesa deve ter uma chave primária.
- O servidor de origem deve utilizar o motor MySQL InnoDB.
- O utilizador deve ter permissões para configurar a sessão de registo binário e criar novos utilizadores no servidor de origem.
- Se o servidor de origem tiver SSL ativado, certifique-se de que o certificado SSL CA fornecido para o domínio foi incluído no `mysql.az_replication_change_master` procedimento ou `mysql.az_replication_change_master_with_gtid` armazenado. Consulte os [seguintes exemplos](./howto-data-in-replication.md#link-source-and-replica-servers-to-start-data-in-replication) e o `master_ssl_ca` parâmetro.
- Certifique-se de que o endereço IP do servidor de origem foi adicionado à Base de Dados Azure para as regras de firewall do servidor de réplicas MySQL. Atualize as regras de firewall com o [portal do Azure](./howto-manage-firewall-using-portal.md) ou a [CLI do Azure](./howto-manage-firewall-using-cli.md).
- Certifique-se de que a máquina que hospeda o servidor de origem permite o tráfego de entrada e saída na porta 3306.
- Certifique-se de que o servidor de origem tem um **endereço IP público,** que o DNS é acessível ao público ou que o servidor de origem tem um nome de domínio totalmente qualificado (FQDN).

## <a name="next-steps"></a>Passos seguintes

- Saiba como configurar a [replicação de dados](howto-data-in-replication.md)
- Saiba mais [sobre a replicação em Azure com réplicas de leitura](concepts-read-replicas.md)
- Saiba como [migrar dados com tempo mínimo de inatividade usando DMS](howto-migrate-online.md)
