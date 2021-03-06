---
title: Plano para eventos de manutenção do Azure
description: Saiba como preparar eventos de manutenção planeados na Base de Dados Azure SQL e na Azure SQL Managed Instance.
services: sql-database
ms.service: sql-db-mi
ms.subservice: service
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: aamalvea
ms.author: aamalvea
ms.reviewer: sstein
ms.date: 3/23/2021
ms.openlocfilehash: 259a8581d16f4fd6958a0d9ec2631f667d362b19
ms.sourcegitcommit: d63f15674f74d908f4017176f8eddf0283f3fac8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/07/2021
ms.locfileid: "106579465"
---
# <a name="plan-for-azure-maintenance-events-in-azure-sql-database-and-azure-sql-managed-instance"></a>Plano para eventos de manutenção Azure em Azure SQL Database e Azure SQL Managed Instance
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Saiba como se preparar para eventos de manutenção planeados na sua base de dados na Base de Dados Azure SQL e na Azure SQL Managed Instance.

## <a name="what-is-a-planned-maintenance-event"></a>O que é um evento de manutenção planeado?

Para manter os serviços Azure SQL Database e Azure SQL Managed Instance seguros, conformes, estáveis e performantes, as atualizações estão a ser realizadas através dos componentes de serviço quase continuamente. Graças à arquitetura de serviços moderna e robusta e a tecnologias inovadoras como [o hot patching, a](https://aka.ms/azuresqlhotpatching)maioria das atualizações são totalmente transparentes e não impactantes em termos de disponibilidade de serviços. Ainda assim, poucos tipos de atualizações causam interrupções de serviço curtas e requerem tratamento especial. 

Para cada base de dados, a Base de Dados Azure SQL e a Azure SQL Managed Instance mantêm um quórum de réplicas de bases de dados onde uma réplica é a principal. A todo o momento, uma réplica primária deve estar a ser a manutenção online, e pelo menos uma réplica secundária deve ser saudável. Durante a manutenção planeada, os membros do quórum da base de dados ficarão offline um de cada vez, com a intenção de que haja uma réplica primária de resposta e pelo menos uma réplica secundária on-line para garantir que nenhum cliente está em tempo de inatividade. Quando a réplica primária precisa de ser desligada, ocorrerá um processo de reconfiguração no qual uma réplica secundária se tornará a nova primária.  

## <a name="what-to-expect-during-a-planned-maintenance-event"></a>O que esperar durante um evento de manutenção planeado

O evento de manutenção pode produzir reconfigurações únicas ou múltiplas, dependendo da constelação das réplicas primárias e secundárias no início do evento de manutenção. Em média, ocorrem 1,7 reconfigurações por evento de manutenção planeado. As reconfigurações geralmente terminam dentro de 30 segundos. A média é de oito segundos. Se já estiver ligado, a sua aplicação deve voltar a ligar-se à nova réplica primária da sua base de dados. Se uma nova ligação for tentada enquanto a base de dados está a sofrer uma reconfiguração antes da nova réplica primária estar on-line, obtém-se o erro 40613 (Base de dados Indisponível): *"Base de dados '{databasename}' no servidor '{servername}' não está atualmente disponível. Por favor, re-tentar a ligação mais tarde.* Se a sua base de dados tiver uma consulta de longa duração, esta consulta será interrompida durante uma reconfiguração e terá de ser reiniciada.

## <a name="how-to-simulate-a-planned-maintenance-event"></a>Como simular um evento de manutenção planeado

Garantir que a aplicação do seu cliente é resiliente a eventos de manutenção antes de ser implementado para a produção ajudará a mitigar o risco de falhas de aplicação e contribuirá para a disponibilidade de aplicações para os seus utilizadores finais. Pode testar o comportamento da aplicação do seu cliente durante eventos de manutenção planeados [testando a resiliência da falha da aplicação](https://docs.microsoft.com/azure/azure-sql/database/high-availability-sla#testing-application-fault-resiliency) através da PowerShell, CLI ou REST API. Consulte também [o início do failover manual](https://aka.ms/mifailover-techblog) para a Instância Gerida. Produzirá um comportamento idêntico como evento de manutenção, colocando a réplica primária offline.

## <a name="retry-logic"></a>Repetir a lógica

Qualquer aplicação de produção de clientes que se conecte a um serviço de base de dados em nuvem deve implementar uma lógica robusta [de relembramento de](troubleshoot-common-connectivity-issues.md#retry-logic-for-transient-errors)conexão . Isto ajudará a tornar as reconfigurações transparentes para os utilizadores finais, ou pelo menos minimizar os efeitos negativos.

## <a name="resource-health"></a>Estado de funcionamento de recursos

Se a sua base de dados estiver a registar falhas de login, verifique a janela [de Saúde](../../service-health/resource-health-overview.md#get-started) de Recursos no [portal Azure](https://portal.azure.com) para obter o estado atual. A secção De Histórico de Saúde contém a razão de inatividade para cada evento (quando disponível).

## <a name="maintenance-window-feature"></a>Função da janela de manutenção

A função janela de manutenção permite a configuração de horários previsíveis das janelas de manutenção para bases de dados Elegívels do SQL e instâncias geridas pelo SQL. Consulte [a janela manutenção](maintenance-window.md) para obter mais informações.

## <a name="next-steps"></a>Passos seguintes

- Saiba mais sobre [a Saúde dos Recursos](resource-health-to-troubleshoot-connectivity.md) para Azure SQL Database e Azure SQL Managed Instance.
- Para obter mais informações sobre a lógica de relemissão, consulte [a lógica retry para erros transitórios](troubleshoot-common-connectivity-issues.md#retry-logic-for-transient-errors).
- Configure os horários das janelas de manutenção com a [função de janela de manutenção.](maintenance-window.md)
