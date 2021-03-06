---
title: SKUs BareMetal para cargas de trabalho da Oráculo
description: Conheça as SKUs para as cargas de trabalho da Infraestrutura Oracle BareMetal.
ms.topic: reference
ms.subservice: workloads
ms.date: 04/15/2021
ms.openlocfilehash: 42ff26b9ea9bcc022f1ddbf3dddcb041b2cea3a2
ms.sourcegitcommit: 272351402a140422205ff50b59f80d3c6758f6f6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/17/2021
ms.locfileid: "107588858"
---
# <a name="baremetal-skus-for-oracle-workloads"></a>SKUs BareMetal para cargas de trabalho da Oráculo

Neste artigo, vamos dar uma olhada nos SKUs especializados da Infraestruturas DaMetal para as cargas de trabalho da Oracle.

A infraestrutura BareMetal para SKUs oracle varia de duas tomadas até quatro tomadas. Também pode escolher entre vários núcleos de CPU e tamanhos de memória para satisfazer os requisitos da sua carga de trabalho. Aqui está uma tabela que resume as funcionalidades dos SKUs disponíveis.
 
| **Hardware certificado pela Oracle**   | **Modelação** | **Memória Total** | **Armazenamento** | **Disponibilidade** |
| --- | --- | --- | --- | --- |
| SIM | SAP HANA em Azure S32m- 2 x Intel® Xeon® Processador I6234 16 núcleos CPU e 32 fios CPU | 1,5 TB | --- | Disponível |
| SIM | SAP HANA em Azure S64m- 4 x Intel® Xeon® Processador I6234 Núcleos CPU 32 e 64 fios CPU | 3.0 TB | --- | Disponível |
| SIM | SAP HANA em Azure S96– 2 x Intel® Xeon® E7-8890 v4 Processador 48 núcleos CPU e 96 fios CPU | 768 GB | 3.0 TB | Disponível |
| SIM | SAP HANA em Azure S224 – 4 x Intel® Xeon® Processador Platinum 8276 112 núcleos CPU e 224 fios CPU | 3.0 TB | 6.3 TB | Disponível |
| SIM | SAP HANA em Azure S224m– 4 x Intel® Xeon® processador Platinum 8276 112 núcleos CPU e 224 fios CPU | 6.0 TB | 10.5 TB | Disponível |

- Núcleos cpu = soma de núcleos de CPU não hiper-roscados (o número total de processadores físicos) da unidade do servidor. 
- Fios CPU = soma de fios de cálculo fornecidos por núcleos de CPU hiper roscados (o número total de processadores lógicos) da unidade do servidor. A maioria das unidades são configuradas por padrão para usar Hyper-Threading Tecnologia.
- Os servidores são dedicados aos clientes.
- O cliente tem acesso à raiz (sem hipervisor).
- Os servidores não estão diretamente nos VNETs Azure.

## <a name="next-steps"></a>Passos seguintes

Conheça o armazenamento oferecido pela BareMetal Infrastructure para a Oracle.

> [!div class="nextstepaction"]
> [Armazenamento em BareMetal para cargas de trabalho da Oráculo](oracle-baremetal-storage.md)
