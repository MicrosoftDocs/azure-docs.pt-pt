---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-network
author: genlin
ms.service: virtual-network
ms.topic: include
ms.date: 04/13/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: 2b1f9990985951a4e6ef260954968c0e1466c298
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/18/2019
ms.locfileid: "67184175"
---
## <a name="scenario"></a>Cenário

Para ilustrar como criar uma VNet e sub-redes, este documento usa o seguinte cenário:

![Cenário de VNet](./media/virtual-networks-create-vnet-scenario-include/vnet-scenario.png)

Neste cenário, criar uma VNet com o nome **TestVNet**, com um bloco CIDR reservado de **192.168.0.0./16**. A VNet contém as seguintes sub-redes: 

* **Front-end**, utilizando **192.168.1.0/24** como bloco CIDR.
* **Back-end**, utilizando **192.168.2.0/24** como bloco CIDR.

