---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/27/2021
ms.author: alkohli
ms.openlocfilehash: f92dc476d3bc79fdb522424e36e44a13644c725e
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/31/2021
ms.locfileid: "106078808"
---
O tamanho VM determina a quantidade de recursos computacional (como CPU, GPU e memória) que são disponibilizados ao VM. Deve criar máquinas virtuais utilizando um tamanho VM apropriado para a carga de trabalho. Apesar de todas as máquinas estarem a funcionar no mesmo hardware, os tamanhos das máquinas têm limites diferentes para acesso ao disco. Isto pode ajudá-lo a gerir o acesso geral ao disco através dos seus VMs. Se uma carga de trabalho aumentar, também pode redimensionar uma máquina virtual existente.

Os VMs seguintes são suportados para criação no seu dispositivo Azure Stack Edge.

### <a name="dv2-series"></a>Série Dv2
|Tamanho     |vCPU     |Memória (GiB) | Armazenamento temporário (GiB)   | Discos de dados máximos | NICs máximos |
|-------------------|----|----|-----|----|------|
|**Standard_D1_v2** |1   |3.5 |50   | 4    |2 |
|**Standard_D2_v2** |2   |7   |100  | 8    |4 |
|**Standard_D3_v2** |4   |14  |200  | 16  |4 |
|**Standard_D4_v2** |8   |28  |400  | 32  |8 |
|**Standard_D5_v2** |16  |56  |800  | 64  |8 |
|**Standard_D11_v2** |2   |14  |100 | 8     |2 |
|**Standard_D12_v2** |4   |28  |200  | 16   |4 |
|**Standard_D13_v2** |8   |56  |400  | 32  |8 |

### <a name="dsv2-series"></a>Série DSv2
|Tamanho     |vCPU     |Memória (GiB) | Armazenamento temporário (GiB)  | Discos de dados máximos| 
|--------------------|----|----|----|-----|
|**Standard_DS1_v2** |1   |3.5 |4000  |4  | 
|**Standard_DS2_v2** |2   |7   |8000  |8  | 
|**Standard_DS3_v2** |4   |14  |16000 |16 | 
|**Standard_DS4_v2** |8   |28  |32000 |32 | 
|**Standard_DS5_v2** |16  |56  |64000 |64 |  
|**Standard_DS11_v2**|2   |14  |8000  |4  | 
|**Standard_DS12_v2**|4   |28  |16000 |8  | 
|**Standard_DS13_v2**|8   |56  |32000 |16 | 


Para mais informações, consulte [a série Dv2 e DSv2](../articles/virtual-machines/dv2-dsv2-series.md#dv2-series).

### <a name="ncast4_v3-series-preview"></a>NCasT4_v3 série (Pré-visualização)

Estes tamanhos são suportados para VMs GPU no seu dispositivo e são otimizados para aplicações aceleradas por GPU com intensidade computacional. Esta série está focada em cargas de trabalho de inferência com Tesla T4 GPU da Nvidia. 

|Tamanho     |vCPU     |Memória (GiB) | Armazenamento temporário (GiB)  | GPU | Memória gpu (GiB) | NICs máximos |
|---------------------|----|----|-----|-----|-------|--------------|
|**Standard_NC4as_T4_v3** |4   |28  |180   |1 |16   |4 |
|**Standard_NC8as_T4_v3** |8   |56  |360   |1 |16  |8 |

Para mais informações, consulte [NCasT4_v3 séries.](../articles/virtual-machines/nct4-v3-series.md)

### <a name="f-series"></a>Série F

Estas séries são otimizadas para cargas de trabalho computacionais e executadas em processadores Intel Xeon. 

| Tamanho | vCPU | Memória: GiB | Armazenamento temporário (GiB) |  Discos de dados máximos | NICs máximos |
|---|---|---|---|---|---|
| Standard_F1  | 1  | 2   |16      | 4  |  2 |
| Standard_F2 | 2  | 4 |32      | 8  |  4 |
| Standard_F4  | 4  | 8 |64   | 16 |  4 |
| Standard_F8 | 8 | 16  |132    | 32 |  8 |
| Standard_F16 | 16 | 32  |262   | 64 |  8 |
| Standard_F1s | 1 | 2  | 4  | 4 | 1 |
| Standard_F2s | 2 | 4 |8   | 8 | 4 |
| Standard_F4s | 4 | 8 |16 | 16 |  4 |
| Standard_F8s | 8 | 16 |32 | 32 |  8 |
| Standard_F16s | 16 | 32 |64 | 64 |  8 |

Para mais informações, consulte [a série Fsv2.](../articles/virtual-machines/fsv2-series.md)

