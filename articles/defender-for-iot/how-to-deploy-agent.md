---
title: Selecione e implemente agentes de segurança
description: Saiba como selecionar e implementar o Defender para agentes de segurança IoT em dispositivos IoT.
ms.topic: conceptual
ms.date: 07/23/2019
ms.openlocfilehash: c71c92ffa79c844f3529265320b46eadd0c158cf
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2021
ms.locfileid: "104778853"
---
# <a name="select-and-deploy-a-security-agent-on-your-iot-device"></a>Selecione e coloque um agente de segurança no seu dispositivo IoT

O Defender for IoT fornece arquiteturas de referência para agentes de segurança que monitorizam e recolhem dados de dispositivos IoT.
Para saber mais, consulte [a arquitetura de referência do agente de segurança.](security-agent-architecture.md)

Os agentes são desenvolvidos como projetos de código aberto, e estão disponíveis em dois sabores: <br> [C](https://aka.ms/iot-security-github-c), e [C#](https://aka.ms/iot-security-github-cs).

Neste artigo, vai aprender a:
- Compare sabores de agente de segurança
- Descubra plataformas de agentes apoiados
- Escolha o sabor do agente certo para a sua solução

## <a name="understand-security-agent-options"></a>Compreender as opções do agente de segurança

Cada sabor de agente de segurança Defender para IoT oferece o mesmo conjunto de funcionalidades, e suporta opções de configuração semelhantes.

O agente de segurança baseado em C tem uma pegada de memória mais baixa, e é a escolha ideal para dispositivos com menos recursos disponíveis.

|     | Agente de segurança baseado em C | Agente de segurança baseado em C# |
| --- | ----------- | --------- |
| **É open-source** | Disponível sob [licença do MIT](https://en.wikipedia.org/wiki/MIT_License) no [GitHub](https://aka.ms/iot-security-github-c) | Disponível sob [licença do MIT](https://en.wikipedia.org/wiki/MIT_License) no [GitHub](https://aka.ms/iot-security-github-cs) |
| **Linguagem de programação**    | C | C# |
| **Plataformas suportadas do Windows?** | No | Yes |
| **Pré-requisitos do Windows** | --- | [WMI](/windows/desktop/wmisdk/) |
| **Apoiou plataformas Linux?** | Sim, x64 e x86 | Sim, apenas x64 |
| **Pré-requisitos linux** | libunwind8, libcurl3, uuid-runtime, auditado, audispd-plugins | libunwind8, libcurl3, uuid-runtime, auditado, audispd-plugins, sudo, netstat, iptables |
| **Pegada de disco** | 10,5 MB | 90 MB |
| **Pegada de memória (em média)** | 5,5 MB | 33 MB |
| **[Autenticação](concept-security-agent-authentication-methods.md) no IoT Hub** | Yes | Yes |
| **Recolha [de](how-to-agent-configuration.md#supported-security-events) dados de segurança** | Yes | Yes |
| **Agregação de eventos** | Yes | Yes |
| **Configuração remota através [do gémeo Defender-IoT-micro-agente](concept-security-module.md)** | Yes | Yes |

## <a name="security-agent-installation-guidelines"></a>Diretrizes de instalação de agentes de segurança

Para **o Windows**: O script de instalação SecurityAgent.ps1 deve ser executado a partir de uma janela PowerShell do administrador.

Para **Linux:** O InstallSecurityAgent.sh deve ser executado como super-portador. Recomendamos a pré-fixação do comando de instalação com "sudo".

## <a name="choose-an-agent-flavor"></a>Escolha um sabor de agente

Responda às seguintes perguntas sobre os seus dispositivos IoT para selecionar o agente correto:

- Está a utilizar _o Windows Server_ ou o Windows _IoT Core?_

    [Implementar um agente de segurança baseado em C#para o Windows](how-to-deploy-windows-cs.md).

- Está a usar uma distribuição Linux com arquitetura x86?

    [Implementar um agente de segurança baseado em C para o Linux.](how-to-deploy-linux-c.md)

- Está a usar uma distribuição Linux com arquitetura x64?

    Ambos os sabores de agente podem ser usados. <br>
    [Implementar um agente de segurança baseado em C para linux](how-to-deploy-linux-c.md) e/ou [implementar um agente de segurança baseado em C#para o Linux](how-to-deploy-linux-cs.md).

Ambos os sabores do agente oferecem o mesmo conjunto de funcionalidades e suportam opções de configuração semelhantes.
Consulte a [comparação do agente de segurança](how-to-deploy-agent.md#understand-security-agent-options) para saber mais.

## <a name="supported-platforms"></a>Plataformas suportadas

A lista a seguir inclui todas as plataformas suportadas atualmente.

|Defensor para agente IoT |Sistema Operativo |Arquitetura |
|--------------|------------|--------------|
|C|Ubuntu 16.04 |    X64|
|C|Ubuntu 18.04 |    x64, ARMv7|
|C|Debian 9 |    x64, x86|
|C#|Ubuntu 16.04     |X64|
|C#|Ubuntu 18.04    |x64, ARMv7|
|C#|Debian 9    |X64|
|C#|Windows Server 2016|    x64|
|C#|Windows 10 IoT Core, construa 17763    |X64|
|

## <a name="next-steps"></a>Passos seguintes

Para saber mais sobre opções de configuração, continue a guiar como fazer para configurar o agente.
> [!div class="nextstepaction"]
> [Configuração do agente como guiar](./how-to-agent-configuration.md)