---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines-linux
author: cynthn
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 02/11/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 4bfac9be5041fdf4ebfe7ea56f064b8b85806703
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "98860009"
---
## <a name="supported-distributions-and-drivers"></a>Distribuições e controladores suportados

### <a name="nvidia-cuda-drivers"></a>Motoristas da NVIDIA CUDA

Os condutores da NVIDIA CUDA para NC, NCv2, NCv3, ND e NDv2 série VMs (opcional para série NV) são suportados apenas nas distribuições Linux listadas na tabela seguinte. A informação do condutor da CUDA está atual no momento da publicação. Para os mais recentes controladores da CUDA e sistemas operativos suportados, visite o site da [NVIDIA.](https://developer.nvidia.com/cuda-zone) Certifique-se de que instala ou faz upgrade para os controladores CUDA mais recentes para a sua distribuição. 

> [!TIP]
> Como alternativa à instalação manual do controlador CUDA num Linux VM, pode implementar uma imagem [da Máquina Virtual Azure Data Science.](../articles/machine-learning/data-science-virtual-machine/overview.md) As edições DSVM para Ubuntu 16.04 LTS ou CentOS 7.4 pré-instalar os controladores NVIDIA CUDA, a Biblioteca da Rede Neural Profunda CUDA, e outras ferramentas.


### <a name="nvidia-grid-drivers"></a>Condutores da NVIDIA GRID

A Microsoft redistribui os instaladores de controladores NVIDIA GRID para VMs da série NV e NVv3 utilizados como estações de trabalho virtuais ou para aplicações virtuais. Instale apenas estes controladores GRID em VMs Azure NV, apenas nos sistemas operativos listados na tabela seguinte. Estes controladores incluem licenciamento para GRID Virtual GPU Software em Azure. Não precisa de configurar um servidor de licença de software NVIDIA vGPU.

Os controladores GRID redistribuídos pela Azure não funcionam em VMs não-NV séries como NC, NCv2, NCv3, ND e NDv2 série VMs.

|Distribuição|Controlador|
| --- | -- |
|Ubuntu 18.04 LTS<br/><br/>Ubuntu 16.04 LTS<br/><br/>Red Hat Enterprise Linux 7.7 a 7.9, 8.0, 8.1<br/><br/>SUSE Linux Enterprise Server 12 SP2 <br/><br/>SUSE Linux Enterprise Server 15 SP2 | NVIDIA GRID 12.0, ramo do condutor [R460](https://go.microsoft.com/fwlink/?linkid=874272)(.exe)|

Visite o [GitHub](https://github.com/Azure/azhpc-extensions/blob/master/NvidiaGPU/resources.json) para obter a lista completa de todas as ligações anteriores do controlador Nvidia GRID.

> [!WARNING] 
> A instalação de software de terceiros em produtos Red Hat pode afetar os termos de suporte do Red Hat. Veja o [Red Hat Knowledgebase](https://access.redhat.com/articles/1067) (Base de Dados de Conhecimento do Red Hat).
>
