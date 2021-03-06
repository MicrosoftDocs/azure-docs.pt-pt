---
title: Ligue-se a um VM do Windows utilizando o Azure Bastion
description: Neste artigo, aprenda a ligar-se a uma Máquina Virtual Azure que executa o Windows utilizando o Azure Bastion.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: how-to
ms.date: 10/21/2020
ms.author: cherylmc
ms.openlocfilehash: 708bd1f61da2f3973333f8e68cabdceee0717bee
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "92521544"
---
# <a name="connect-to-a-windows-virtual-machine-using-azure-bastion"></a>Ligue-se a uma máquina virtual Windows usando Azure Bastion

Utilizando o Azure Bastion, pode ligar-se de forma segura e perfeita às suas máquinas virtuais sobre a SSL diretamente no portal Azure. Quando utiliza o Azure Bastion, os seus VMs não requerem um cliente, agente ou software adicional. Este artigo mostra-lhe como ligar-se aos seus VMs windows. Para obter informações sobre a ligação a um VM Linux, consulte [Connect to a Linux VM](bastion-connect-vm-ssh.md).

O Azure Bastion proporciona uma conectividade segura a todos os VMs na rede virtual em que é a provisionado. A utilização do Azure Bastion protege as suas máquinas virtuais de expor portas RDP/SSH ao mundo exterior, ao mesmo tempo que fornece acesso seguro utilizando RDP/SSH. Para mais informações, veja o [Bastião Azure?](bastion-overview.md)

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar, verifique se cumpriu os seguintes critérios:

* Um VNet com o anfitrião Bastion já instalado.

   Certifique-se de que criou um anfitrião Azure Bastion para a rede virtual em que o VM está localizado. Uma vez que o serviço Bastion é aloque e implementado na sua rede virtual, pode usá-lo para ligar a qualquer VM da rede virtual. Para montar um anfitrião Azure Bastion, consulte [criar um hospedeiro de bastião.](tutorial-create-host-portal.md#createhost)
* Uma máquina virtual Windows na rede virtual.
* As seguintes funções exigidas:
  * Papel do leitor na máquina virtual.
  * Função do leitor no NIC com IP privado da máquina virtual.
  * Papel do leitor no recurso Azure Bastion.
* Portas: Para ligar ao Windows VM, tem de ter as seguintes portas abertas no seu Windows VM:
  * Portas de entrada: RDP (3389)

## <a name="connect"></a><a name="rdp"></a>Ligar

[!INCLUDE [Connect to a Windows VM](../../includes/bastion-vm-rdp.md)]
 
## <a name="next-steps"></a>Passos seguintes

Leia as [FAQ do Bastião.](bastion-faq.md)