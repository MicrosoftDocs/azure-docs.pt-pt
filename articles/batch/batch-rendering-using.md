---
title: Utilização de capacidades de renderização
description: Como utilizar as capacidades de renderização do Azure Batch. Tente utilizar a aplicação Batch Explorer, direta ou invocada a partir de um plug-in de aplicação do cliente.
author: mscurrell
ms.author: markscu
ms.date: 03/12/2020
ms.topic: how-to
ms.openlocfilehash: dc3d2cc53b478b1ec955d8f4b3717b0407772849
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2021
ms.locfileid: "103496631"
---
# <a name="using-azure-batch-rendering"></a>Usando a renderização do lote Azure

> [!IMPORTANT]
> As imagens VM de renderização e o licenciamento pay-for-use foram [depreciados e serão retirados no dia 29 de fevereiro de 2024](https://azure.microsoft.com/updates/azure-batch-rendering-vm-images-licensing-will-be-retired-on-29-february-2024/). Para utilizar o Batch para renderização, [deve ser utilizado um licenciamento de imagem VM e aplicação padrão.](batch-rendering-functionality.md#batch-pools-using-custom-vm-images-and-standard-application-licensing)

Existem várias formas de utilizar a renderização do Azure Batch:

* APIs:
  * Escreva código usando qualquer uma das APIs do lote.  Os desenvolvedores podem integrar as capacidades do Azure Batch nas suas aplicações ou fluxo de trabalho existentes, seja em nuvem ou com base nas instalações.
* Ferramentas da linha de comando:
  * A [linha de comando Azure](/cli/azure/) ou [PowerShell](/powershell/azure/) pode ser usada para script Batch.
  * Em particular, o suporte ao [modelo do Grupo CLI](./batch-cli-templates.md) facilita muito a criação de piscinas e a apresentação de empregos.
* UI do Explorador de Lote:
  * [O Batch Explorer](https://github.com/Azure/BatchLabs) é uma ferramenta de clientes de plataforma cruzada que também permite que as contas do Batch sejam geridas e monitorizadas.
  * Para cada uma das aplicações de renderização, são fornecidos vários modelos de pool e de trabalho que podem ser usados para criar facilmente piscinas e para apresentar empregos.  Um conjunto de modelos está listado na UI da aplicação, com os ficheiros de modelo a serem acedidos a partir do GitHub.
  * Os modelos personalizados podem ser da autoria do zero ou os modelos fornecidos do GitHub podem ser copiados e modificados.
* Plug-ins de aplicação do cliente:
  * Estão disponíveis plug-ins que permitem a utilização de Batching diretamente dentro das aplicações de design e modelação do cliente.  Os plug-ins invocam principalmente a aplicação Batch Explorer com informações contextuais sobre o modelo 3D atual e inclui funcionalidades para ajudar a gerir os ativos.

A melhor maneira de experimentar a renderização do Azure Batch e a forma mais simples para os utilizadores finais, que não são desenvolvedores e não especialistas em Azure, é utilizar a aplicação Batch Explorer, direta ou invocada a partir de um plug-in de aplicação do cliente.

## <a name="using-batch-explorer"></a>Usando o Explorador de Lotes

Para um tutorial passo a passo para utilizar o Batch Explorer para executar a renderização consulte o tutorial do [Blender](./tutorial-rendering-batchexplorer-blender.md).

### <a name="download-and-install"></a>Descarregar e Instalar

Os downloads do Batch Explorer [estão disponíveis](https://azure.github.io/BatchExplorer/) para Windows, OSX e Linux.

### <a name="using-templates-to-create-pools-and-run-jobs"></a>Usando modelos para criar piscinas e executar empregos

Um conjunto abrangente de modelos está disponível para uso com Batch Explorer que facilita a criação de piscinas e submeter empregos para as várias aplicações de renderização sem ter que especificar todas as propriedades necessárias para criar piscinas, empregos e tarefas diretamente com Batch.  Os modelos disponíveis no Batch Explorer são armazenados e visíveis [num repositório GitHub.](https://github.com/Azure/BatchExplorer-data/tree/master/ncj)

![Galeria do Explorador de Lotes](./media/batch-rendering-using/batch-explorer-gallery.png)

São fornecidos modelos que atendem a todas as aplicações presentes no Marketplace, renderizando imagens VM.  Para cada aplicação existem vários modelos, incluindo modelos de piscina para atender piscinas de CPU e GPU, piscinas Windows e Linux; os modelos de trabalho incluem a renderização total do quadro ou do blender em azulejo e a renderização distribuída por V-Ray. O conjunto de modelos fornecidos será expandido ao longo do tempo para atender a outras capacidades do Lote, tais como o auto-dimensionamento da piscina.

Também é possível que os modelos personalizados sejam produzidos, a partir do zero ou modificando os modelos fornecidos. Os modelos personalizados podem ser utilizados selecionando o item 'Modelos Locais' na secção 'Galeria' do Explorador de Lotes.

### <a name="file-system-and-data-movement"></a>Sistema de ficheiros e movimento de dados

A secção 'Dados' no Batch Explorer permite que os ficheiros sejam copiados entre um sistema de ficheiros local e contas de Armazenamento Azure.

## <a name="next-steps"></a>Passos seguintes

Por exemplo, a renderização do Lote experimente os dois tutoriais:

* [Renderização utilizando o CLI Azure](./tutorial-rendering-cli.md)
* [Compor com o Batch Explorer](./tutorial-rendering-batchexplorer-blender.md)
