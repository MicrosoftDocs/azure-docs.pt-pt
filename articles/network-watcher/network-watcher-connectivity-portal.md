---
title: Conexões de resolução de problemas - Portal Azure
titleSuffix: Azure Network Watcher
description: Saiba como utilizar a capacidade de resolução de problemas de ligação do Observador de Redes Azure utilizando o portal Azure.
services: network-watcher
documentationcenter: na
author: damendo
ms.service: network-watcher
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/04/2021
ms.author: damendo
ms.openlocfilehash: f33c5f0fdf69737df0d8bd83499ded1e0e0f4f88
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "97898118"
---
# <a name="troubleshoot-connections-with-azure-network-watcher-using-the-azure-portal"></a>Ligações de resolução de problemas com o Observador de Redes Azure utilizando o portal Azure

> [!div class="op_single_selector"]
> - [Portal](network-watcher-connectivity-portal.md)
> - [PowerShell](network-watcher-connectivity-powershell.md)
> - [CLI do Azure](network-watcher-connectivity-cli.md)
> - [API REST do Azure](network-watcher-connectivity-rest.md)

Saiba como utilizar a resolução de problemas de ligação para verificar se pode ser estabelecida uma ligação TCP direta de uma máquina virtual para um determinado ponto final.

## <a name="before-you-begin"></a>Antes de começar

Este artigo pressupõe que tem os seguintes recursos:

* Um caso de Observador de Rede na região que pretende resolver uma ligação.
* Máquinas virtuais para resolver ligações com.

> [!IMPORTANT]
> A resolução de problemas de ligação requer que a resolução de problemas de VM de onde provém a `AzureNetworkWatcherExtension` extensão VM tenha a extensão VM instalada. Para instalar a extensão num Windows VM visite [a extensão da máquina virtual do Observador de Redes Azure para windows](../virtual-machines/extensions/network-watcher-windows.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) e para Linux VM visite a [extensão virtual do Agente de Observadores de Rede Azure para o Linux](../virtual-machines/extensions/network-watcher-linux.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json). A extensão não é necessária no ponto final de destino.

## <a name="check-connectivity-to-a-virtual-machine"></a>Verifique a conectividade com uma máquina virtual

Este exemplo verifica a conectividade com uma máquina virtual de destino sobre a porta 80.

Navegue no seu Observador de Rede e clique na **resolução de problemas de ligação**. Selecione a máquina virtual para verificar a conectividade. Na secção **Destino** escolha **Selecionar uma máquina virtual** e escolher a máquina virtual e a porta corretas para testar.

Uma vez clicada **Verifique-se**, verifica-se a conectividade entre as máquinas virtuais na porta especificada. No exemplo, o VM de destino é inacessível, uma lista de lúpulo é mostrada.

![Verifique os resultados da conectividade de uma máquina virtual][1]

## <a name="check-remote-endpoint-connectivity"></a>Verificar a conectividade do ponto final remoto

Para verificar a conectividade e a latência num ponto final remoto, escolha o botão de rádio **Specify manualmente** na secção **Destino,** insira o url e a porta e clique em **Verificar**.  Isto é usado para pontos finais remotos como websites e pontos finais de armazenamento.

![Verifique os resultados da conectividade de um site][2]

## <a name="next-steps"></a>Passos seguintes

Saiba como automatizar capturas de pacotes com alertas de máquina virtual ao visualizar [Criar uma captura de pacotes desencadeados por alerta](network-watcher-alert-triggered-packet-capture.md)

Descubra se determinado tráfego é permitido dentro ou fora do seu VM visitando [verificar o fluxo IP](diagnose-vm-network-traffic-filtering-problem.md)

[1]: ./media/network-watcher-connectivity-portal/figure1.png
[2]: ./media/network-watcher-connectivity-portal/figure2.png