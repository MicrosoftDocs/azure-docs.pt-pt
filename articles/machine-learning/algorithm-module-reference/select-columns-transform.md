---
title: 'Selecione Colunas Transformar: Referência do módulo'
titleSuffix: Azure Machine Learning
description: Aprenda a utilizar o módulo Select Columns Transform no designer Azure Machine Learning para realizar uma transformação selecionada.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/10/2020
ms.openlocfilehash: f03840e55366d7f105ca4b57bd60061c82833e72
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2021
ms.locfileid: "93420721"
---
# <a name="select-columns-transform"></a>Selecionar Transformação de Colunas

Este artigo descreve como utilizar o módulo Select Columns Transform no designer de aprendizagem automática Azure. O objetivo do módulo Select Columns Transform é garantir que um conjunto previsível e consistente de colunas seja utilizado em operações de aprendizagem automática a jusante.

Este módulo é útil para tarefas como a pontuação, que requerem colunas específicas. As alterações nas colunas disponíveis podem quebrar o gasoduto ou alterar os resultados.

Utilize a Select Columns Transform para criar e guardar um conjunto de colunas. Em seguida, utilize o módulo [De Transformação De Aplicar](apply-transformation.md) para aplicar essas seleções a novos dados.

## <a name="how-to-use-select-columns-transform"></a>Como utilizar a seleção de colunas transformam

Este cenário pressupõe que pretende utilizar a seleção de funcionalidades para gerar um conjunto dinâmico de colunas que serão usadas para treinar um modelo. Para garantir que as seleções de colunas são as mesmas para o processo de pontuação, utilize o módulo Select Columns Transform para capturar as seleções das colunas e aplicá-las em outros locais do oleoduto.

1. Adicione um conjunto de dados de entrada ao seu pipeline no designer.

2. Adicione uma instância da seleção de [recursos baseados em filtros.](filter-based-feature-selection.md)

3. Ligue os módulos e configuure o módulo de seleção de funcionalidades para encontrar automaticamente uma série de melhores funcionalidades no conjunto de dados de entrada.

4. Adicione uma instância do Modelo de [Comboio](train-model.md) e use a saída da Seleção de [Recursos Baseados em Filtros](filter-based-feature-selection.md) como entrada para o treino.

    > [!IMPORTANT]
    > Uma vez que a importância da característica se baseia nos valores da coluna, não pode saber com antecedência quais as colunas que podem estar disponíveis para entrada no [Modelo de Comboio.](train-model.md)  
5. Fixe uma instância do módulo Select Columns Transform. 

    Este passo gera uma seleção de colunas como uma transformação que pode ser guardada ou aplicada a outros conjuntos de dados. Este passo garante que as colunas identificadas na seleção de recursos são guardadas para que outros módulos sejam reutilizados.

6. Adicione o módulo ['Modelo de Pontuação'.](score-model.md) 

   *Não ligue o conjunto de dados de entrada.* Em vez disso, adicione o módulo [De Transformação Aplicar](apply-transformation.md) e ligue a saída da transformação da seleção de recursos.

   A estrutura do gasoduto deve ser a seguinte:

   > [!div class="mx-imgBorder"]
   > ![Gasoduto de amostra](media/module/filter-based-feature-selection-score.png)

   > [!IMPORTANT]
   > Não pode esperar aplicar a [Seleção de Recursos Baseados em Filtros](filter-based-feature-selection.md) no conjunto de dados de pontuação e obter os mesmos resultados. Como a seleção de recursos é baseada em valores, pode escolher um conjunto diferente de colunas, o que faria com que a operação de pontuação falhasse.
    
7. Envie o oleoduto.

Este processo de poupança e, em seguida, aplicação de uma seleção de colunas garante que o mesmo esquema de dados está disponível para treino e pontuação.


## <a name="next-steps"></a>Passos seguintes

Consulte o [conjunto de módulos disponíveis](module-reference.md) para Azure Machine Learning. 
