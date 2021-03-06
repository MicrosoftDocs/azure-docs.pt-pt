---
title: incluir ficheiro
description: incluir ficheiro
services: iot-central
author: dominicbetts
ms.service: iot-central
ms.topic: include
ms.date: 11/03/2020
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: e4f9fd537d7743a5bbb9d129b21c4bf0a529d32d
ms.sourcegitcommit: bfa7d6ac93afe5f039d68c0ac389f06257223b42
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2021
ms.locfileid: "106491157"
---
Quando executar a aplicação do dispositivo de amostra mais tarde neste tutorial, precisa dos seguintes valores de configuração:

* Âmbito de identificação: Na sua aplicação IoT Central, navegue para **a Administração > Ligação do Dispositivo**. Tome nota do valor do âmbito de **identificação.**
* Chave primária do grupo: Na sua aplicação IoT Central, navegue para **a Administração > a Ligação do Dispositivo > dispositivos SAS-IoT**. Tome nota do valor da chave **principal de** acesso partilhado.

Utilize a Cloud Shell para gerar uma chave de dispositivo a partir da chave primária do grupo que recuperou:

```azurecli-interactive
az extension add --name azure-iot
az iot central device compute-device-key --device-id sample-device-01 --pk <the group primary key value>
```

Tome nota da chave do dispositivo gerado, use-a mais tarde neste tutorial.
