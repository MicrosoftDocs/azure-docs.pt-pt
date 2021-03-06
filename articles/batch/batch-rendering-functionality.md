---
title: Capacidades de renderização
description: As capacidades standard do Lote Azure são usadas para executar cargas de trabalho e aplicações de renderização. O lote inclui funcionalidades específicas para suportar cargas de trabalho de renderização.
author: mscurrell
ms.author: markscu
ms.date: 03/12/2021
ms.topic: how-to
ms.openlocfilehash: a2e2cfb71999bd5ab83591448342d4bac1dabdd5
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2021
ms.locfileid: "103496342"
---
# <a name="azure-batch-rendering-capabilities"></a>Capacidades de renderização do Lote Azure

As capacidades standard do Lote Azure são usadas para executar cargas de trabalho e aplicações de renderização. O lote também inclui funcionalidades específicas para suportar cargas de trabalho de renderização.

Para uma visão geral dos conceitos do Lote, incluindo piscinas, empregos e tarefas, consulte [este artigo.](./batch-service-workflow-features.md)

## <a name="batch-pools-using-custom-vm-images-and-standard-application-licensing"></a>Piscinas de lote usando imagens VM personalizadas e licenciamento de aplicação padrão

Tal como acontece com outras cargas de trabalho e tipos de aplicação, uma imagem VM personalizada pode ser criada com as aplicações de renderização e plug-ins necessários. A imagem VM personalizada é colocada na Galeria de [Imagens Partilhadas](../virtual-machines/shared-image-galleries.md) e [pode ser usada para criar Piscinas de Lote.](batch-sig-images.md)

As cordas da linha de comando de tarefa terão de fazer referência às aplicações e caminhos utilizados na criação da imagem VM personalizada.

A maioria dos pedidos de renderização requer licenças obtidas a partir de um servidor de licença. Se houver um servidor de licença existente no local, então tanto o servidor de pool como o servidor de licenças precisam de estar na mesma [rede virtual.](../virtual-network/virtual-networks-overview.md) Também é possível executar um servidor de licença num Azure VM, com o Pool Batch e o servidor de licença VM na mesma rede virtual.

## <a name="batch-pools-using-rendering-vm-images"></a>Piscinas de lote usando imagens VM renderização

> [!IMPORTANT]
> As imagens VM de renderização e o licenciamento pay-for-use foram [depreciados e serão retirados no dia 29 de fevereiro de 2024](https://azure.microsoft.com/updates/azure-batch-rendering-vm-images-licensing-will-be-retired-on-29-february-2024/). Para utilizar o Batch para renderização, [deve ser utilizado um licenciamento de imagem VM e aplicação padrão.](batch-rendering-functionality.md#batch-pools-using-custom-vm-images-and-standard-application-licensing)

### <a name="rendering-application-installation"></a>Instalação de aplicação de renderização

Uma imagem VM de renderização do Azure Marketplace pode ser especificada na configuração da piscina se apenas as aplicações pré-instaladas precisarem de ser utilizadas.

Existe uma imagem do Windows e uma imagem CentOS.  No [Mercado Azure,](https://azuremarketplace.microsoft.com)as imagens VM podem ser encontradas procurando por "renderização de lote".

Para uma configuração de piscina por exemplo, consulte o [tutorial de renderização Azure CLI](./tutorial-rendering-cli.md).  O portal Azure e o Batch Explorer fornecem ferramentas GUI para selecionar uma imagem VM renderizadora quando criar uma piscina.  Se utilizar uma API de lote, especifique os seguintes valores de propriedade para [ImageReference](/rest/api/batchservice/pool/add#imagereference) ao criar uma piscina:

| Publisher | Oferta | Sku | Versão |
|---------|---------|---------|--------|
| lote | renderização-centos73 | renderização | mais recente |
| lote | renderização-windows2016 | renderização | mais recente |

Outras opções estão disponíveis se forem necessárias aplicações adicionais nos VMs da piscina:

* Uma imagem personalizada da Galeria de Imagens Partilhadas:
  * Com esta opção, pode configurar a VM com as aplicações e versões específicas exatas de que precisa. Para mais informações, consulte [Criar uma piscina com a Galeria de Imagens Partilhadas.](batch-sig-images.md) A Autodesk e o Chaos Group modificaram a Arnold e a V-Ray, respectivamente, para validar contra um serviço de licenciamento Azure Batch. Certifique-se de que tem as versões destas aplicações com este suporte, caso contrário o licenciamento pay-per-use não funcionará. As versões atuais de Maya ou 3ds Max não requerem um servidor de licença quando estiver em funcionamento sem cabeça (no modo lote/linha de comando). Contacte o suporte do Azure se não tiver a certeza de como proceder com esta opção.
* [Pacotes de aplicação:](./batch-application-packages.md)
  * Embale os ficheiros de aplicação utilizando um ou mais ficheiros ZIP, faça upload através do portal Azure e especifique o pacote na configuração do pool. Quando os VMs de piscina são criados, os ficheiros ZIP são descarregados e os ficheiros extraídos.
* Ficheiros de recursos:
  * Os ficheiros de aplicações são enviados para o armazenamento de blob Azure e especifica referências de ficheiros na [tarefa inicial](/rest/api/batchservice/pool/add#starttask)da piscina . Quando os VMs de piscina são criados, os ficheiros de recursos são descarregados em cada VM.

### <a name="pay-for-use-licensing-for-pre-installed-applications"></a>Licenciamento pay-for-use para aplicações pré-instaladas

As aplicações que serão utilizadas e têm uma taxa de licenciamento precisam de ser especificadas na configuração da piscina.

* Especifique a `applicationLicenses` propriedade ao criar uma [piscina.](/rest/api/batchservice/pool/add#request-body)  Os seguintes valores podem ser especificados na matriz de cordas - "vray", "arnold", "3dsmax", "maya".
* Quando especificar uma ou mais aplicações, o custo dessas aplicações é adicionado ao custo dos VMs.  Os preços da aplicação estão listados na página de preços do [Azure Batch](https://azure.microsoft.com/pricing/details/batch/#graphic-rendering).

> [!NOTE]
> Se, em vez disso, ligar a um servidor de licença para utilizar as aplicações de renderização, não especifique a `applicationLicenses` propriedade.

Pode utilizar o portal Azure ou o Batch Explorer para selecionar aplicações e mostrar os preços da aplicação.

Se for feita uma tentativa de utilização de uma aplicação, mas a aplicação não tiver sido especificada na `applicationLicenses` propriedade da configuração da piscina ou não chegar a um servidor de licença, então a execução da aplicação falha com um erro de licenciamento e código de saída não zero.

### <a name="environment-variables-for-pre-installed-applications"></a>Variáveis ambientais para aplicações pré-instaladas

Para ser capaz de criar a linha de comando para tarefas de renderização, deve ser especificada a localização de instalação dos executáveis da aplicação de renderização.  Foram criadas variáveis ambientais do sistema nas imagens VM do Azure Marketplace, que podem ser usadas em vez de terem de especificar caminhos reais.  Estas variáveis ambientais são além das [variáveis padrão do ambiente batch criadas](./batch-compute-node-environment-variables.md) para cada tarefa.

|Aplicação|Aplicação executável|Variável ambiente|
|---------|---------|---------|
|Autodesk 3ds Max 2021|3dsmaxcmdio.exe|3DSMAX_2021_EXEC|
|Autodesk Maya 2020|render.exe|MAYA_2020_EXEC|
|Grupo Caos V-Ray Autónomo|vray.exe|VRAY_4.10.03_EXEC|
|Linha de comando Arnold 2020|kick.exe|ARNOLD_2020_EXEC|
|Liquidificador|blender.exe|BLENDER_2018_EXEC|

## <a name="azure-vm-families"></a>Famílias Azure VM

Tal como acontece com outras cargas de trabalho, os requisitos do sistema de aplicação de renderização variam e os requisitos de desempenho variam para empregos e projetos.  Uma grande variedade de famílias VM estão disponíveis em Azure dependendo dos seus requisitos - menor custo, melhor preço/desempenho, melhor desempenho, e assim por diante.
Algumas aplicações de renderização, como Arnold, são baseadas em CPU; outros, como v-ray e ciclos liquidificadores, podem usar CPUs e/ou GPUs.
Para obter uma descrição das famílias VM disponíveis e tamanhos VM, [consulte os tipos e tamanhos VM](../virtual-machines/sizes.md).

## <a name="low-priority-vms"></a>VMs de baixa prioridade

Tal como acontece com outras cargas de trabalho, os VMs de baixa prioridade podem ser utilizados em piscinas de Lote para renderização.  Os VM de baixa prioridade executam o mesmo que os VMs dedicados regulares, mas utilizam a capacidade excedentária do Azure e estão disponíveis para um grande desconto.  A compensação pela utilização de VMs de baixa prioridade é que esses VM podem não estar disponíveis para serem atribuídos ou podem ser antecipados a qualquer momento, dependendo da capacidade disponível. Por esta razão, os VM de baixa prioridade não serão adequados para todos os trabalhos de renderização. Por exemplo, se as imagens demoram muitas horas a renderizar, então é provável que ter a renderização dessas imagens interrompida e reiniciada devido à antecipação dos VMs não seria aceitável.

Para obter mais informações sobre as características dos VMs de baixa prioridade e as várias formas de os configurar utilizando o Lote, consulte [utilizar VMs de baixa prioridade com o Lote](./batch-low-pri-vms.md).

## <a name="jobs-and-tasks"></a>Trabalhos e tarefas

Não é necessário um apoio específico para a prestação de empregos e tarefas.  O item de configuração principal é a linha de comando de tarefa, que precisa de referenciar a aplicação necessária.
Quando as imagens VM do Azure Marketplace são utilizadas, então a melhor prática é usar as variáveis ambientais para especificar o caminho e a aplicação executáveis.

## <a name="next-steps"></a>Passos seguintes

Por exemplo, a renderização do Lote experimente os dois tutoriais:

* [Renderização utilizando o CLI Azure](./tutorial-rendering-cli.md)
* [Compor com o Batch Explorer](./tutorial-rendering-batchexplorer-blender.md)