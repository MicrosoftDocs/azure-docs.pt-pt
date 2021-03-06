---
title: incluir ficheiro
description: incluir ficheiro
services: storage
author: fauhse
ms.service: storage
ms.topic: include
ms.date: 2/20/2020
ms.author: fauhse
ms.custom: include file
ms.openlocfilehash: a8dd83a9584d218d56c7350d6d634bfe4610b790
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/31/2021
ms.locfileid: "106075612"
---
Neste passo, está a avaliar quantas ações de ficheiros Azure precisa. Uma única instância do Windows Server (ou cluster) pode sincronizar até 30 ações de ficheiros Azure.

Poderá ter mais pastas nos seus volumes que partilha localmente como partilha sMB para os seus utilizadores e apps. A maneira mais fácil de imaginar este cenário é visualizar uma partilha no local que mapeia 1:1 para uma partilha de ficheiros Azure. Se tiver um número pequeno o suficiente, abaixo de 30 para uma única instância do Windows Server, recomenda-se um mapeamento de 1:1.

Se tiver mais ações do que 30, muitas vezes é desnecessário mapear uma partilha no local 1:1 para uma partilha de ficheiros Azure. Considere as seguintes opções.

#### <a name="share-grouping"></a>Agrupamento de partilha

Por exemplo, se o seu departamento de recursos humanos (RH) tiver um total de 15 ações, poderá considerar armazenar todos os dados de RH numa única partilha de ficheiros Azure. Armazenar várias ações no local numa partilha de ficheiros Azure não o impede de criar as habituais 15 ações SMB na sua instância local do Windows Server. Significa apenas que organiza as pastas de raiz destas 15 ações como sub-dobradeiras sob uma pasta comum. Em seguida, sincroniza esta pasta comum a uma partilha de ficheiros Azure. Desta forma, apenas uma única partilha de ficheiros Azure na nuvem é necessária para este grupo de ações no local.

#### <a name="volume-sync"></a>Sincronização de volume

O Azure File Sync suporta sincronizar a raiz de um volume para uma partilha de ficheiros Azure. Se sincronizar a raiz do volume, todas as sub-dobradeiras e ficheiros irão para a mesma partilha de ficheiros Azure.

Sincronizar a raiz do volume nem sempre é a melhor resposta. Existem benefícios em sincronizar vários locais. Por exemplo, fazê-lo ajuda a manter o número de itens mais baixo por âmbito de sincronização. Enquanto testamos ações de ficheiros Azure e Azure File Sync com 100 milhões de itens (ficheiros e pastas) por ação, uma boa prática é tentar manter o número abaixo de 20 milhões ou 30 milhões numa única ação. Configurar o Azure File Sync com um número mais baixo de itens não é apenas benéfico para a sincronização de ficheiros. Um menor número de itens também beneficia cenários como estes:

* A verificação inicial do conteúdo da nuvem pode ser concluída mais rapidamente, o que por sua vez diminui a espera para que o espaço de nome apareça num servidor ativado por Azure File Sync.
* A restauração do lado da nuvem a partir de uma imagem de partilha de ficheiros Azure será mais rápida.
* A recuperação de desastres de um servidor no local pode acelerar significativamente.
* As alterações efetuadas diretamente numa partilha de ficheiros Azure (sincronização externa) podem ser detetadas e sincronizadas mais rapidamente.

> [!TIP]
> Se não tiver a certeza de quantos ficheiros e pastas tem, consulte a ferramenta TreeSize da JAM Software GmbH.

#### <a name="a-structured-approach-to-a-deployment-map"></a>Uma abordagem estruturada de um mapa de implantação

Antes de implementar o armazenamento em nuvem num passo posterior, é importante criar um mapa entre as pastas no local e as partilhas de ficheiros Azure. Este mapeamento informará então quantos e quais os recursos do *grupo Azure* File Sync que irá providenciar. Um grupo de sincronização liga a partilha de ficheiros Azure e a pasta no seu servidor e estabelece uma ligação sincronizada.

Para tomar a decisão sobre quantas ações de arquivo Azure precisa, reveja os seguintes limites e boas práticas. Ao fazê-lo, irá ajudá-lo a otimizar o seu mapa.

* Um servidor com o agente Azure File Sync instalado pode sincronizar até 30 ações de ficheiros Azure.
* Uma partilha de ficheiros Azure é implantada dentro de uma conta de armazenamento. Isso faz da conta de armazenamento um alvo de escala para números de desempenho como IOPS e produção.

  Uma partilha de ficheiros Azure padrão pode teoricamente saturar o desempenho máximo que uma conta de armazenamento pode fornecer. Colocar várias ações numa única conta de armazenamento significa que está a criar um pool partilhado de IOPS e produção para estas ações. Se pretender anexar apenas o Azure File Sync a estas ações de ficheiros, agrupar várias ações de ficheiros Azure na mesma conta de armazenamento não criará problemas. Reveja os alvos de desempenho da partilha de ficheiros Azure para obter uma visão mais profunda das métricas relevantes a ter em conta. Estas limitações não se aplicam ao armazenamento premium, quando o desempenho é explicitamente previsto e garantido para cada ação.

  Se planeia levantar uma aplicação para o Azure que utilizará a partilha de ficheiros Azure de forma nativa, poderá precisar de mais desempenho da sua partilha de ficheiros Azure. Se este tipo de utilização for uma possibilidade, mesmo no futuro, então criar uma única quota de ficheiro azure padrão na sua própria conta de armazenamento é o melhor.
* Há um limite de 250 contas de armazenamento por subscrição por região de Azure.

> [!TIP]
> Com esta informação em mente, muitas vezes torna-se necessário agrupar várias pastas de alto nível nos seus volumes num novo diretório de raiz comum. Em seguida, sincroniza este novo diretório de raiz, e todas as pastas que agrupou nele, para uma única partilha de ficheiros Azure. Esta técnica permite-lhe permanecer dentro do limite de 30 sincronizações de partilha de ficheiros Azure por servidor.
>
> Este agrupamento sob uma raiz comum não tem impacto no acesso aos seus dados. Os teus ACLs ficam como estão. Só precisa de ajustar quaisquer caminhos de partilha (como ações SMB ou NFS) que possa ter nas pastas do servidor local que agora se transformou numa raiz comum. Nada mais muda.

> [!IMPORTANT]
> O vetor de escala mais importante para O Azure File Sync é o número de itens (ficheiros e pastas) que precisam de ser sincronizados. Reveja os alvos da [escala de Sincronização de Ficheiros Azure](../articles/storage/files/storage-files-scale-targets.md#azure-file-sync-scale-targets) para obter mais detalhes.

É uma boa prática manter o número de itens por sincronização baixo. É um fator importante a ter em conta no seu mapeamento de pastas para ações de ficheiros Azure. O Azure File Sync é testado com 100 milhões de itens (ficheiros e pastas) por ação. No entanto, muitas vezes é melhor manter o número de itens abaixo de 20 milhões ou 30 milhões numa única parte. Divida o seu espaço de nome em várias ações se começar a exceder estes números. Pode continuar a agrupar várias ações no local na mesma partilha de ficheiros Azure se ficar aproximadamente abaixo destes números. Esta prática vai dar-lhe espaço para crescer.

Na sua situação, é possível que um conjunto de pastas possa logicamente sincronizar-se com a mesma partilha de ficheiros Azure (utilizando a nova abordagem comum da pasta raiz mencionada anteriormente). Mas ainda pode ser melhor reagrupar pastas de modo a sincronizar em duas em vez de uma partilha de ficheiros Azure. Pode utilizar esta abordagem para manter o número de ficheiros e pastas por partilha de ficheiros equilibrado em todo o servidor. Também pode dividir as suas ações no local e sincronizar-se com mais servidores no local, adicionando a capacidade de sincronizar com mais 30 partilhas de ficheiros Azure por servidor extra.

#### <a name="create-a-mapping-table"></a>Criar uma mesa de mapeamento

:::row:::
    :::column:::
        [![Um exemplo de uma mesa de mapeamento. Descarregue o seguinte ficheiro para experimentar e utilizar o conteúdo desta imagem.](media/storage-files-migration-namespace-mapping/namespace-mapping.png)](media/storage-files-migration-namespace-mapping/namespace-mapping-expanded.png#lightbox)
    :::column-end:::
    :::column:::
        Utilize uma combinação dos conceitos anteriores para ajudar a determinar quantas ações de ficheiros Azure precisa e quais as partes dos seus dados existentes que acabarão em que a ação de ficheiros Azure.
        
        Crie uma tabela que grava os seus pensamentos para que possa referir-se a ela quando necessário. Manter-se organizado é importante porque pode ser fácil perder detalhes do seu plano de mapeamento quando está a autar muitos recursos Azure ao mesmo tempo. Para ajudá-lo a criar um mapeamento completo, pode descarregar um ficheiro Microsoft Excel como modelo.

[//]: # (HTML aparece como a única maneira de conseguir adicionar uma tabela aninhada de duas colunas com a análise de imagem de trabalho e texto/hiperligação na mesma linha.)

<br>
<table>
    <tr>
        <td>
            <img src="media/storage-files-migration-namespace-mapping/excel.png" alt="Microsoft Excel file icon that helps to set the context for the type of file download for the link next to it.">
        </td>
        <td>
            <a href="https://download.microsoft.com/download/1/8/D/18DC8184-E7E2-45EF-823F-F8A36B9FF240/Azure File Sync - Namespace Mapping.xlsx">Descarregue um modelo de mapeamento de espaço de nome.</a>
        </td>
    </tr>
</table>
    :::column-end:::
:::row-end:::
