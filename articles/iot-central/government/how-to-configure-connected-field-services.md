---
title: Ligue a sua aplicação Azure IoT Central com a Dynamics 365 Field Services | Microsoft Docs
description: Saiba como construir uma solução de ponta a ponta com o Azure IoT Central e o Dynamics 365 Field Service
author: miriambrus
ms.author: miriamb
ms.date: 12/11/2020
ms.topic: how-to
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 1d098f56bbadfe115620580c8d93fb6dd021550d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "97586679"
---
# <a name="build-end-to-end-solution-with-azure-iot-central-and-dynamics-365-field-service"></a>Construa solução de ponta a ponta com o Azure IoT Central e o Dynamics 365 Field Service 
Como construtor, pode permitir a integração da sua aplicação Azure IoT Central a outros sistemas de negócio. 

Por exemplo, numa solução de gestão de resíduos conectada pode otimizar o despacho de camiões de recolha de lixo. A otimização pode ser feita com base em dados de sensores IoT de caixotes de lixo conectados. Na sua [aplicação de gestão de resíduos conectada IoT Central](./tutorial-connected-waste-management.md) pode configurar regras e ações e defini-la para desencadear a criação de alertas no Dynamics Field Service. Este cenário é realizado através da utilização da Power Automamate, que será configurada diretamente na IoT Central para automatizar fluxos de trabalho através de aplicações e serviços. Além disso, com base em atividades de serviço no Field Service, as informações podem ser enviadas de volta para a Azure IoT Central. 

## <a name="how-to-connect-your-azure-iot-central-application-with-dynamics-365-field-services"></a>Como ligar a sua aplicação Azure IoT Central com a Dynamics 365 Field Services 

Os processos de integração abaixo podem ser facilmente implementados com base numa experiência de configuração pura:
* O Azure IoT Central pode enviar informações sobre anomalias do dispositivo ao Serviço de Campo Conectado (como alerta IoT) para diagnóstico.
* O Serviço de Campo Conectado pode criar casos ou ordens de trabalho desencadeadas a partir de anomalias do dispositivo.
* O Serviço de Campo Conectado pode agendar técnicos para inspeção para evitar os incidentes de inatividade.
* O painel de instrumentos Azure IoT Central pode ser atualizado com informações de serviço e agendamento relevantes.


## <a name="next-steps"></a>Passos seguintes
* Saiba mais sobre [os modelos do governo central do IoT](./overview-iot-central-government.md)
* Saiba mais sobre [a IoT Central](../core/overview-iot-central.md)
* Saiba mais sobre [a Dynamics 365 Field Services](/dynamics365/field-service/cfs-iot-overview)
