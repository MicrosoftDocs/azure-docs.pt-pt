---
title: Privacidade diferencial no machine learning (pré-visualização)
titleSuffix: Azure Machine Learning
description: Saiba o que é a privacidade diferencial e como pode implementar sistemas diferencialmente privados que preservem a privacidade dos dados.
author: luisquintanilla
ms.author: luquinta
ms.date: 01/21/2020
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: responsible-ml
ms.openlocfilehash: de85c8184e05c9d662a2854e6138f3a61bfba8de
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/31/2021
ms.locfileid: "106062459"
---
# <a name="what-is-differential-privacy-in-machine-learning-preview"></a>O que é privacidade diferencial no machine learning (pré-visualização)

Saiba mais sobre privacidade diferencial na aprendizagem automática e como funciona.

À medida que a quantidade de dados que uma organização recolhe e utiliza para análises aumenta, também as preocupações com a privacidade e segurança aumentam. As análises requerem dados. Normalmente, quanto mais dados forem usados para formar modelos, mais precisos são. Quando as informações pessoais são usadas para estas análises, é especialmente importante que os dados se mantenham privados durante todo o seu uso.

## <a name="how-differential-privacy-works"></a>Como funciona a privacidade diferencial

A privacidade diferencial é um conjunto de sistemas e práticas que ajudam a manter os dados dos indivíduos seguros e privados.

> [!div class="mx-imgBorder"]
> ![Processo diferencial de aprendizagem de máquinas de privacidade](./media/concept-differential-privacy/differential-privacy-machine-learning.jpg)

Nos cenários tradicionais, os dados brutos são armazenados em ficheiros e bases de dados. Quando os utilizadores analisam os dados, normalmente utilizam os dados brutos. Isto é uma preocupação porque pode violar a privacidade de um indivíduo. A privacidade diferencial tenta lidar com este problema adicionando "ruído" ou aleatoriedade aos dados para que os utilizadores não possam identificar quaisquer pontos de dados individuais. Pelo menos, tal sistema proporciona uma negação plausível. Portanto, a privacidade dos indivíduos é preservada com um impacto limitado na precisão dos dados.

Em sistemas diferencialmente privados, os dados são partilhados através de pedidos chamados **consultas.** Quando um utilizador submete uma consulta para dados, as operações conhecidas como **mecanismos de privacidade** adicionam ruído aos dados solicitados. Os mecanismos de privacidade devolvem uma *aproximação dos dados* em vez dos dados brutos. Este resultado de preservação da privacidade aparece num **relatório**. Os relatórios consistem em duas partes, os dados reais calculados e uma descrição de como os dados foram criados.

## <a name="differential-privacy-metrics"></a>Métricas de privacidade diferenciais

A privacidade diferencial tenta proteger contra a possibilidade de um utilizador poder produzir um número indefinido de relatórios para eventualmente revelar dados sensíveis. Um valor conhecido como **epsilon** mede o quão ruidoso ou privado um relatório é. Epsilon tem uma relação inversa com o ruído ou privacidade. Quanto mais baixo o epsilon, mais barulhento (e privado) os dados são.

Os valores de Epsilon não são negativos. Valores abaixo de 1 proporcionam total negação plausível. Qualquer coisa acima de 1 tem um maior risco de exposição dos dados reais. Ao implementar sistemas diferencialmente privados, pretende produzir relatórios com valores epsilon entre 0 e 1.

Outro valor diretamente correlacionado com o epsilon é **o delta.** Delta é uma medida da probabilidade de um relatório não ser totalmente privado. Quanto maior o delta, maior o epsilon. Como estes valores estão correlacionados, o epsilon é usado com mais frequência.

## <a name="limit-queries-with-a-privacy-budget"></a>Limite as consultas com um orçamento de privacidade

Para garantir a privacidade em sistemas onde são permitidas múltiplas consultas, a privacidade diferencial define um limite de taxa. Este limite é conhecido como um orçamento de **privacidade.** Os orçamentos de privacidade impedem que os dados sejam recriados através de múltiplas consultas. Os orçamentos de privacidade são atribuídos um montante epsilon, tipicamente entre 1 e 3 para limitar o risco de reidentificação. À medida que os relatórios são gerados, os orçamentos de privacidade acompanham o valor epsilon dos relatórios individuais, bem como o agregado para todos os relatórios. Depois de gastos ou esgotados os orçamentos de privacidade, os utilizadores deixaram de poder aceder aos dados. 

## <a name="reliability-of-data"></a>Fiabilidade dos dados

Embora a preservação da privacidade deva ser o objetivo, há uma compensação no que diz respeito à usabilidade e fiabilidade dos dados. Na análise de dados, a precisão pode ser considerada como uma medida de incerteza introduzida por erros de amostragem. Esta incerteza tende a cair dentro de certos limites. **A precisão** de uma perspetiva de privacidade diferencial mede, em vez disso, a fiabilidade dos dados, que é afetada pela incerteza introduzida pelos mecanismos de privacidade. Em suma, um nível mais elevado de ruído ou privacidade traduz-se em dados que têm um epsilon mais baixo, precisão e fiabilidade. 

## <a name="open-source-differential-privacy-libraries"></a>Bibliotecas de privacidade diferenciais de código aberto

O SmartNoise é um projeto de código aberto que contém diferentes componentes para a construção de sistemas privados diferenciais globais. O SmartNoise é composto pelos seguintes componentes de nível superior:

- Biblioteca SmartNoise Core
- Biblioteca SmartNoise SDK

### <a name="smartnoise-core"></a>SmartNoise Core

A biblioteca central inclui os seguintes mecanismos de privacidade para a implementação de um sistema diferencialmente privado:

|Componente  |Descrição  |
|---------|---------|
|Análise     | Uma descrição de gráfico de cálculos arbitrários. |
|Validador     | Uma biblioteca Rust que contém um conjunto de ferramentas para verificar e derivar as condições necessárias para que uma análise seja diferenciadamente privada.          |
|Runtime     | O meio para executar a análise. O tempo de referência está escrito em Rust, mas os tempos de execução podem ser escritos utilizando qualquer estrutura de cálculo, como SQL e Spark, dependendo das suas necessidades de dados.        |
|Enlaces     | Ligações linguísticas e bibliotecas auxiliares para construir análises. Atualmente o SmartNoise fornece encadernações Python. |

### <a name="smartnoise-sdk"></a>SmartNoise SDK

A biblioteca do sistema fornece as seguintes ferramentas e serviços para trabalhar com dados tabulares e relacionais:

|Componente  |Descrição  |
|---------|---------|
|Acesso a Dados     | Biblioteca que interceta e processa consultas SQL e produz relatórios. Esta biblioteca é implementada em Python e suporta as seguintes fontes de dados ODBC e DBAPI:<ul><li>PostgreSQL</li><li>SQL Server</li><li>Spark</li><li>Rio Preston</li><li>Pandas</li></ul>|
|Serviço     | Serviço de execução que fornece um ponto final REST para servir pedidos ou consultas contra fontes de dados partilhadas. O serviço foi concebido para permitir a composição de módulos de privacidade diferenciais que operam em pedidos que contenham diferentes valores delta e epsilon, também conhecidos como pedidos heterogéneos. Esta implementação de referência explica o impacto adicional das consultas sobre dados correlacionados. |
|Avaliador     | Avaliador estocástico que verifica por violações de privacidade, precisão e preconceito. O avaliador suporta os seguintes testes: <ul><li>Teste de Privacidade - Determina se um relatório adere às condições de privacidade diferencial.</li><li>Teste de Precisão - Mede se a fiabilidade dos relatórios se enquadra nos limites superior e inferior, dado um nível de confiança de 95%.</li><li>Teste de Utilidade - Determina se os limites de confiança de um relatório estão suficientemente próximos dos dados, maximizando ainda a privacidade.</li><li>Teste de Enviesamento - Mede a distribuição de relatórios para consultas repetidas para garantir que não são desequilibrados</li></ul> |

## <a name="next-steps"></a>Passos seguintes

[Como construir um sistema diferencialmente privado](how-to-differential-privacy.md) em Azure Machine Learning.

Para saber mais sobre os componentes do SmartNoise, consulte os repositórios GitHub para [smartNoise Core,](https://github.com/opendifferentialprivacy/smartnoise-core) [SmartNoise SDK](https://github.com/opendifferentialprivacy/smartnoise-sdk)e [smartNoise.](https://github.com/opendifferentialprivacy/smartnoise-samples)