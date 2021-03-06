---
title: Crie um ponto de restauro definido pelo utilizador para uma piscina SQL dedicada
description: Aprenda a usar o portal Azure para criar um ponto de restauro definido pelo utilizador para piscina SQL dedicada em Azure Synapse Analytics.
author: joannapea
manager: igorstan
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: sql
ms.date: 10/29/2020
ms.author: joanpo
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: b695f6c7aabc21541fcc48efed54bbecd022f65a
ms.sourcegitcommit: 590f14d35e831a2dbb803fc12ebbd3ed2046abff
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2021
ms.locfileid: "107567932"
---
# <a name="user-defined-restore-points"></a>Pontos de restauro definidos pelo utilizador

Neste artigo, você aprenderá a criar um novo ponto de restauro definido pelo utilizador para uma piscina SQL dedicada em Azure Synapse Analytics usando o portal Azure.

## <a name="create-user-defined-restore-points-through-the-azure-portal"></a>Criar pontos de restauro definidos pelo utilizador através do portal Azure

Os pontos de restauro definidos pelo utilizador também podem ser criados através do portal Azure.

1. Inscreva-se na sua conta [do portal Azure.](https://portal.azure.com/)

2. Navegue para a piscina de SQL dedicada para a que pretende criar um ponto de restauração.

3. Selecione **Visão geral** do painel esquerdo, selecione **+ Novo ponto de restauro**. Se o botão New Restore Point não estiver ativado, certifique-se de que a piscina SQL dedicada não está em pausa.

    ![Novo ponto de restauro](../media/sql-pools/create-sqlpool-restore-point-01.png)

4. Especifique um nome para o seu ponto de restauro definido pelo utilizador e clique **em Aplicar**. Os pontos de restauro definidos pelo utilizador têm um período de retenção predefinido de sete dias.

    ![Nome do Ponto de Restauro](../media/sql-pools/create-sqlpool-restore-point-02.png)

## <a name="next-steps"></a>Passos seguintes

- [Restaurar uma piscina SQL dedicada existente](restore-sql-pool.md)

