---
title: Ligue o DLP do Forcepoint ao Azure Sentinel| Microsoft Docs
description: Saiba como ligar o Forcepoint DLP ao Azure Sentinel.
services: sentinel
author: yelevin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/20/2020
ms.author: yelevin
ms.openlocfilehash: 62ed3915dcaf596d144a2f59817626cdf8ec47e5
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "100092779"
---
# <a name="connect-your-forcepoint-dlp-to-azure-sentinel"></a>Ligue o seu Forcepoint DLP ao Azure Sentinel

> [!IMPORTANT]
> O conector de dados de prevenção de dados de pontos de força (DLP) em Azure Sentinel está atualmente em pré-visualização pública. Esta funcionalidade é fornecida sem um contrato de nível de serviço, e não é recomendado para cargas de trabalho de produção. Algumas funcionalidades poderão não ser suportadas ou poderão ter capacidades limitadas. Para obter mais informações, veja [Termos Suplementares de Utilização para Pré-visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).



O conector Forcepoint DLP permite exportar automaticamente dados de incidentes DLP para Azure Sentinel. Pode usá-lo para obter visibilidade nas atividades do utilizador e incidentes de perda de dados. Também permite correlações com dados de cargas de trabalho Azure e outros feeds, e melhora a capacidade de monitorização com livros de trabalho dentro de Azure Sentinel.

> [!NOTE]
> Os dados serão armazenados na localização geográfica do espaço de trabalho em que está a executar o Azure Sentinel.

## <a name="configure-and-connect-forcepoint-dlp"></a>Configure e ligue o Forcepoint DLP

Configure o Forcepoint DLP para encaminhar dados de incidentes em formato JSON para o seu espaço de trabalho Azure via REST API, conforme explicado no Guia de [Integração do DLP do Forcepoint](https://frcpnt.com/dlp-sentinel).


## <a name="find-your-data"></a>Encontre os seus dados

Após a configuração do conector DLP do Forcepoint, os dados aparecem no Log Analytics em "CustomLogs" **ForcepointDLPEvents_CL**.


Para utilizar o esquema relevante no Log Analytics para Forcepoint DLP, procure **ForcepointDLPEvents_CL**.


## <a name="validate-connectivity"></a>Validar conectividade

Pode demorar até que os seus registos comecem a aparecer no Log Analytics.

## <a name="next-steps"></a>Passos seguintes

Neste documento, aprendeu a ligar o Forcepoint DLP ao Azure Sentinel. Para saber mais sobre Azure Sentinel, consulte os seguintes artigos:

- Saiba como [obter visibilidade nos seus dados e potenciais ameaças.](quickstart-get-visibility.md)
- Começa [a detetar ameaças com o Azure Sentinel.](tutorial-detect-threats-built-in.md)
- [Utilize livros para](tutorial-monitor-your-data.md) monitorizar os seus dados.