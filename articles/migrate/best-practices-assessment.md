---
title: Avaliação das melhores práticas na Azure Migrate Discovery e ferramenta de avaliação
description: Dicas para criar avaliações com a Azure Migrate Discovery e ferramenta de avaliação.
author: rashi-ms
ms.author: rajosh
ms.manager: abhemraj
ms.topic: conceptual
ms.date: 11/19/2019
ms.openlocfilehash: fac488ba1881b6b79139eaf2468237e546737177
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/31/2021
ms.locfileid: "106077335"
---
# <a name="best-practices-for-creating-assessments"></a>Melhores práticas para criar avaliações

[A Azure Migrate](./migrate-services-overview.md) fornece um centro de ferramentas que o ajudam a descobrir, avaliar e migrar aplicações, infraestruturas e cargas de trabalho para o Microsoft Azure. O hub inclui ferramentas Azure Migrate e ofertas de fornecedores de software independentes de terceiros (ISV).

Este artigo resume as melhores práticas ao criar avaliações utilizando a ferramenta Azure Migrate Discovery e a ferramenta de avaliação.

Avaliações que cria com a Azure Migrate: A ferramenta de descoberta e avaliação são uma imagem pontual dos dados. Existem três tipos de avaliações que pode criar usando Azure Migrate: Descoberta e avaliação:

**Tipo de Avaliação** | **Detalhes**
--- | --- 
**VM do Azure** | Avaliações para migrar os seus servidores no local para máquinas virtuais do Azure. <br/><br/> Pode avaliar os seus servidores no local em ambiente [VMware](how-to-set-up-appliance-vmware.md) e [Hiper-V](how-to-set-up-appliance-hyper-v.md) e [servidores físicos](how-to-set-up-appliance-physical.md) para migração para Azure utilizando este tipo de avaliação. [Saiba mais](concepts-assessment-calculation.md)
**SQL do Azure** | Avaliações para migrar os seus servidores SQL no local do seu ambiente VMware para Azure SQL Database ou Azure SQL Managed Instance. [Saiba mais](concepts-azure-sql-assessment-calculation.md)
**Solução VMware no Azure (AVS)** | Avaliações para migrar os seus servidores no local para o [Azure VMware Solution (AVS)](../azure-vmware/introduction.md). <br/><br/> Pode avaliar os seus [VMS VMware](how-to-set-up-appliance-vmware.md) no local para migração para Azure VMware Solution (AVS) utilizando este tipo de avaliação. [Saiba mais](concepts-azure-vmware-solution-assessment-calculation.md)

> [!NOTE]
> Se o número de avaliações de Azure VM ou AVS estiver incorreto na ferramenta Discovery e de avaliação, clique no número total de avaliações para navegar para todas as avaliações e recalcular as avaliações de Azure VM ou AVS. A ferramenta Discovery e assessment mostrará então a contagem correta para este tipo de avaliação. 

### <a name="sizing-criteria"></a>Critérios de dimensionamento
Opções de critérios de dimensionamento nas avaliações da Azure Migrate:

**Critérios de dimensionamento** | **Detalhes** | **Dados**
--- | --- | ---
**Com base no desempenho** | Avaliações que fazem recomendações com base nos dados de desempenho recolhidos | **Avaliação da VM do Azure**: A recomendação de tamanho da VM baseia-se nos dados de utilização da CPU e da memória.<br/><br/> A recomendação do tipo de disco (HDD/SSD padrão ou discos geridos premium) baseia-se no IOPS e no débito dos discos no local.<br/><br/>**Avaliação do Azure SQL**: A configuração Azure SQL baseia-se em dados de desempenho de instâncias e bases de dados SQL, que incluem: utilização de CPU, utilização de memória, IOPS (ficheiros de dados e registos), produção e latência de operações de IO<br/><br/>**Avaliação do Azure VMware Solution (AVS)** : A recomendação de nós AVS baseia-se nos dados de utilização da CPU e da memória.
**Como está no local** | Avaliações que não utilizam dados de desempenho para fazer recomendações. | **Avaliação da VM do Azure**: A recomendação de tamanho da VM baseia-se no tamanho da VM no local<br/><br> O tipo de disco recomendado baseia-se naquilo que seleciona na definição do tipo de armazenamento para a avaliação.<br/><br/> **Avaliação do Azure VMware Solution (AVS)** : A recomendação de nós AVS baseia-se no tamanho da VM no local.

#### <a name="example"></a>Exemplo
A título de exemplo, se tiver um VM no local com quatro núcleos a 20% de utilização, e memória de 8 GB com 10% de utilização, a avaliação do Azure VM será a seguinte:

- **Avaliação baseada no desempenho:**
    - Identifica núcleos e memória eficazes com base no núcleo (4 x 0,20 = 0,8) e na utilização da memória (8 GB x 0,10 = 0,8).
    - Aplica o fator de conforto especificado nas propriedades de avaliação (digamos 1,3x) para obter os valores a serem usados para dimensionamento. 
    - Recomenda o tamanho VM mais próximo em Azure que pode suportar ~1,04 núcleos (0,8 x 1.3) e ~1,04 GB (0,8 x 1.3) de memória.

- **Avaliação as-é (como no local):**
    -  Recomenda um VM com quatro núcleos; 8 GB de memória.


## <a name="best-practices-for-creating-assessments"></a>Melhores práticas para criar avaliações

O aparelho Azure Migrate traça continuamente o seu ambiente no local e envia metadados e dados de desempenho para o Azure. Siga estas boas práticas para avaliações de servidores descobertos com um aparelho:

- **Crie avaliações como-is**: Pode criar avaliações como-is imediatamente assim que os seus servidores aparecerem no portal Azure Migrate. Não é possível criar uma avaliação Azure SQL com critérios de dimensionamento "Como no local".
- **Criar uma avaliação baseada no desempenho**: Depois de configurar a descoberta, recomendamos que espere pelo menos um dia antes de realizar uma avaliação baseada no desempenho:
    - Recolher dados de desempenho leva tempo. Esperar pelo menos um dia garante que há pontos de dados de desempenho suficientes antes de executar a avaliação.
    - Quando estiver a fazer avaliações baseadas no desempenho, certifique-se de que perfis o seu ambiente para a duração da avaliação. Por exemplo, se criar uma avaliação com uma duração de desempenho definida para uma semana, precisa esperar pelo menos uma semana após começar a descoberta, para que todos os pontos de dados sejam recolhidos. Se não o fizeres, a avaliação não terá uma classificação de cinco estrelas.
- **Recalcular avaliações**: Uma vez que as avaliações são instantâneos pontuais, não são automaticamente atualizados com os dados mais recentes. Para atualizar uma avaliação com os dados mais recentes, é necessário recalculá-lo.

Siga estas melhores práticas para avaliações de servidores importados para a Azure Migrate via . Ficheiro CSV:

- **Crie avaliações como-is**: Pode criar avaliações como-is imediatamente assim que os seus servidores aparecerem no portal Azure Migrate.
- **Crie uma avaliação baseada no desempenho**: Isto ajuda a obter uma melhor estimativa de custos, especialmente se tiver uma capacidade de servidor sobreprovisionada no local. No entanto, a precisão da avaliação baseada no desempenho depende dos dados de desempenho especificados por si para os servidores. 
- **Recalcular avaliações**: Uma vez que as avaliações são instantâneos pontuais, não são automaticamente atualizados com os dados mais recentes. Para atualizar uma avaliação com os dados mais recentes importados, é necessário recalculá-lo.
 
### <a name="ftt-sizing-parameters-for-avs-assessments"></a>Parâmetros de dimensionamento ftt para avaliações de AVS

O motor de armazenamento utilizado em AVS é vSAN. As políticas de armazenamento vSAN definem os requisitos de armazenamento das máquinas virtuais. Estas políticas garantem o nível de serviço necessário das VMs, porque determinam como o armazenamento é alocado à VM. Estas são as Combinações FTT-Raid disponíveis: 

**Falhas a Tolerar (FTT)** | **Configuração do RAID** | **Anfitriões Mínimos Necessários** | **Consideração sobre o dimensionamento**
--- | --- | --- | --- 
1 | RAID-1 (Espelhamento) | 3 | Uma VM de 100 GB consumiria 200 GB.
1 | RAID-5 (Codificação de Eliminação) | 4 | Uma VM de 100 GB consumiria 133,33 GB
2 | RAID-1 (Espelhamento) | 5 | Uma VM de 100 GB consumiria 300 GB.
2 | RAID-6 (Codificação de Eliminação) | 6 | Uma VM de 100 GB consumiria 150 GB.
3 | RAID-1 (Espelhamento) | 7 | Uma VM de 100 GB consumiria 400 GB.


## <a name="best-practices-for-confidence-ratings"></a>Melhores práticas para avaliações de confiança

Quando se faz avaliações baseadas no desempenho, é atribuída uma classificação de confiança de 1 estrela (mais baixa) a 5 estrelas (mais alta) para a avaliação. Para utilizar as classificações de confiança de forma eficaz:

- As avaliações Azure VM e AVS precisam:
    - O CPU e os dados de utilização da memória para cada um dos servidores
    - Os dados de leitura/escrita IOPS/produção para cada disco ligado ao servidor no local
    - Os dados de in/out da rede para cada adaptador de rede ligado ao servidor.
     
- As avaliações do Azure SQL precisam dos dados de desempenho dos casos e bases de dados do SQL que estão a ser avaliados, que incluem:
    - Os dados de utilização da CPU e da memória
    - Os dados de leitura/escrita IOPS/produção de dados e ficheiros de registo
    - A latência das operações de IO

Dependendo da percentagem de pontos de dados disponíveis para a duração selecionada, a classificação de confiança para uma avaliação é fornecida como resumida no quadro seguinte.

   **Disponibilidade de pontos de dados** | **Classificação de confiança**
   --- | ---
   0%-20% | 1 Estrela
   21%-40% | 2 Estrelas
   41%-60% | 3 Estrelas
   61%-80% | 4 Estrelas
   81%-100% | 5 Estrelas


## <a name="common-assessment-issues"></a>Questões comuns de avaliação

Aqui está como abordar algumas questões ambientais comuns que afetam as avaliações.

###  <a name="out-of-sync-assessments"></a>Avaliações fora de sincronização

Se adicionar ou remover servidores de um grupo após a criação de uma avaliação, a avaliação que criou será marcada **fora de sincronização**. Volte a fazer a avaliação (**Recalculare)** para refletir as alterações de grupo.

### <a name="outdated-assessments"></a>Avaliações desatualizadas

#### <a name="azure-vm-assessment-and-avs-assessment"></a>Avaliação do Azure VM e avaliação do AVS
Se houver alterações nos servidores no local que estão num grupo que foi avaliado, a avaliação está marcada **desatualizada**. Uma avaliação pode ser marcada como "Ultrapassada" devido a uma ou mais alterações nas propriedades abaixo:
- Número de núcleos de processador
- Memória alocada
- Tipo de bota ou firmware
- Nome do sistema operativo, versão e arquitetura
- Número de discos
- Número de adaptador de rede
- Alteração do tamanho do disco (GB Atribuído)
- Atualização de propriedades nic. Exemplo: Alterações no endereço mac, adição de endereço IP, etc.
    
Volte a fazer a avaliação (**Recalcular)** para refletir as alterações.
    
#### <a name="azure-sql-assessment"></a>Avaliação do SQL do Azure
Se houver alterações em casos e bases de dados SQL no local que estejam num grupo que foi avaliado, a avaliação está marcada **desatualizada.** Uma avaliação pode ser marcada como "Ultrapassada" devido a uma ou mais razões abaixo:
- Foi adicionada ou removida uma instância do SQL de um servidor
- Foi adicionada ou removida uma base de dados SQL de uma instância do SQL
- O tamanho total da base de dados numa instância do SQL sofreu uma alteração superior a 20%
- Alteração do número de núcleos de processador
- Alteração da memória alocada        
  
    Volte a fazer a avaliação (**Recalcular)** para refletir as alterações.

### <a name="low-confidence-rating"></a>Baixa classificação de confiança

Uma avaliação pode não ter todos os pontos de dados por uma série de razões:

- Não analisou o ambiente durante o tempo para a qual está a criar a avaliação. Por exemplo, se estiver a criar uma avaliação com a duração de desempenho definida para uma semana, terá de aguardar, pelo menos, uma semana após iniciar a deteção de todos os pontos de dados serem recolhidos. Se não puder esperar pela duração, altere a duração do desempenho para um período mais curto e “Recalcule” a avaliação.
 
- A avaliação não é capaz de recolher os dados de desempenho de alguns ou todos os servidores no período de avaliação. Para uma alta classificação de confiança, certifique-se de que: 
    - Os servidores são ligados durante a duração da avaliação
    - São permitidas ligações de saída nas portas 443
    - Para os servidores hiper-V a memória dinâmica está ativada 
    - O estado de ligação dos agentes em Azure Migrate são 'Conectados' e verificam o último batimento cardíaco
    - Para avaliações de Azure SQL, o estado de ligação do Azure Migrate para todos os casos SQL é "Conectado" na lâmina de instância SQL descoberta

    “Recalcule” a avaliação para refletir as últimas alterações na classificação de confiança.

- Para avaliações de Azure VM e AVS, poucos servidores foram criados após a descoberta ter começado. Por exemplo, se estiver a criar uma avaliação para o histórico de desempenho do último mês, mas poucos servidores foram criados no ambiente apenas há uma semana. Neste caso, os dados de desempenho dos novos servidores não estarão disponíveis durante toda a duração e a classificação de confiança será baixa.

- Para avaliações do SQL do Azure, foram criadas poucas bases de dados ou instâncias do SQL após o início da deteção. Por exemplo, se estiver a criar uma avaliação para o histórico de desempenho do último mês, mas foram criados poucos casos ou bases de dados SQL no ambiente há apenas uma semana. Neste caso, os dados de desempenho dos novos servidores não estarão disponíveis durante toda a duração e a classificação de confiança será baixa.

### <a name="migration-tool-guidance-for-avs-assessments"></a>Orientação da Ferramenta de Migração para avaliações de AVS

No relatório de preparação para o Azure da avaliação do Azure VMware Solution (AVS), pode ver as seguintes ferramentas sugeridas: 
- **VMware HCX ou Enterprise**: Para servidores VMware, a solução VMware Hybrid Cloud Extension (HCX) é a ferramenta de migração sugerida para migrar a sua carga de trabalho no local para a sua nuvem privada Azure VMware Solution (AVS). [Saiba Mais](../azure-vmware/tutorial-deploy-vmware-hcx.md).
- **Desconhecido**: Para os servidores importados através de um ficheiro CSV, a ferramenta de migração padrão é desconhecida. No entanto, para servidores em ambiente VMware, recomenda-se a utilização da solução VMware Hybrid Cloud Extension (HCX).


## <a name="next-steps"></a>Passos seguintes

- [Saiba](concepts-assessment-calculation.md) como as avaliações são calculadas.
- [Saiba](how-to-modify-assessment.md) como personalizar uma avaliação.
