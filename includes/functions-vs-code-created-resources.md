---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 01/31/2021
ms.author: glenga
ms.openlocfilehash: 9a5c143927f549124cb6e69a4c8eb4829709a9e9
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "99493496"
---
+ Um [grupo de recursos](../articles/azure-resource-manager/management/overview.md), que é um recipiente lógico para recursos relacionados.
+ Uma [conta padrão de Armazenamento Azure,](../articles/storage/common/storage-account-create.md)que mantém informações estatais e outras sobre os seus projetos.
+ Um plano de consumo, que define o anfitrião subjacente para a sua aplicação de função sem servidor. 
+ Uma aplicação de função, que fornece o ambiente para a execução do seu código de função. Uma aplicação de função permite-lhe agrupar funções como uma unidade lógica para facilitar a gestão, implementação e partilha de recursos dentro do mesmo plano de hospedagem.
+ Uma instância de Insights de Aplicação ligada à aplicação de função, que rastreia o uso da sua função sem servidor.