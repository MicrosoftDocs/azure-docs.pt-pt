---
title: Navegação e história da versão IoT Edge - Azure IoT Edge
description: Descubra as novidades no IoT Edge com informações sobre novas funcionalidades e capacidades nas últimas versões.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 04/07/2021
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 1d3473e975e7c69a83c359b040a2de0defaac69b
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/13/2021
ms.locfileid: "107310479"
---
# <a name="azure-iot-edge-versions-and-release-notes"></a>Versões Azure IoT Edge e notas de lançamento

[!INCLUDE [iot-edge-version-all-supported](../../includes/iot-edge-version-all-supported.md)]

Azure IoT Edge é um produto construído a partir do projeto IoT Edge de código aberto hospedado no GitHub. Todos os novos lançamentos são disponibilizados no [projeto Azure IoT Edge.](https://github.com/Azure/azure-iotedge) As contribuições e os relatórios de bugs podem ser feitos no [projeto IoT Edge de código aberto.](https://github.com/Azure/iotedge)

## <a name="documented-versions"></a>Versões documentadas

A documentação IoT Edge neste site está disponível para duas versões diferentes do produto, para que possa escolher o conteúdo que se aplica ao seu ambiente IoT Edge. Atualmente, as duas versões suportadas são:

* **O IoT Edge 1.2** contém conteúdo adicional para novas funcionalidades e capacidades que estão na mais recente versão estável.
* **IoT Edge 1.1 (LTS)** é a primeira versão de suporte a longo prazo (LTS) do IoT Edge. A documentação desta versão abrange todas as funcionalidades e capacidades de todas as versões anteriores até 1.1. Esta versão de documentação será estável através do tempo de vida suportado da versão 1.1, e não refletirá novas funcionalidades lançadas em versões posteriores.

Para obter mais informações sobre as versões IoT Edge, consulte [os sistemas suportados Azure IoT Edge](support.md).

## <a name="version-history"></a>Histórico de versões

Esta tabela fornece o histórico de versão recente para lançamentos de pacotes IoT Edge e destaca as atualizações de documentação feitas para cada versão.

| Notas de lançamento e ativos | Tipo | Data | Destaques |
| ------------------------ | ---- | ---- | ---------- |
| [1.2](https://github.com/Azure/azure-iotedge/releases/tag/1.2.0) | Estável | abril de 2021 | [Dispositivos IoT Edge por trás de gateways](how-to-connect-downstream-iot-edge-device.md?view=iotedge-2020-11&preserve-view=true)<br>[Corretor MQTT IoT Edge (pré-visualização)](how-to-publish-subscribe.md?view=iotedge-2020-11&preserve-view=true)<br>Novos pacotes IoT Edge introduzidos, com novos passos de instalação e configuração. Para mais informações, consulte [atualização de 1.0 ou 1.1 a 1.2](how-to-update-iot-edge.md#special-case-update-from-10-or-11-to-12)
| [1.1](https://github.com/Azure/azure-iotedge/releases/tag/1.1.0) | Suporte de longo prazo (LTS) | Fevereiro de 2021 | [Plano de apoio a longo prazo e atualizações de sistemas suportados](support.md) |
| [1.0.10](https://github.com/Azure/azure-iotedge/releases/tag/1.0.10) | Estável | Outubro de 2020 | [Método direto uploadSupportBundle](how-to-retrieve-iot-edge-logs.md#upload-support-bundle-diagnostics)<br>[Carregar métricas de tempo de execução](how-to-access-built-in-metrics.md)<br>[Prioridade de rota e tempo a viver](module-composition.md#priority-and-time-to-live)<br>[Pedido de arranque de módulos](module-composition.md#configure-modules)<br>[Fornecimento manual X.509](how-to-register-device.md) |
| [1.0.9](https://github.com/Azure/azure-iotedge/releases/tag/1.0.9) | Estável | Março de 2020 | [X.509 auto-provisionamento com DPS](how-to-auto-provision-x509-certs.md)<br>[Método direto RestartModule](how-to-edgeagent-direct-method.md#restart-module)<br>[comando de pacote de apoio](troubleshoot.md#gather-debug-information-with-support-bundle-command) |

## <a name="next-steps"></a>Passos seguintes

* [Ver todos os lançamentos do Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases)
* [Faça ou reveja os pedidos de recurso no fórum de feedback](https://feedback.azure.com/forums/907045-azure-iot-edge)