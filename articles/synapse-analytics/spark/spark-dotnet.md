---
title: Utilize .NET para Faísca Apache
description: Aprenda a usar .NET e Apache Spark para fazer processamento de lotes, streaming em tempo real, machine learning e escrever consultas ad-hoc em cadernos Azure Synapse Analytics.
author: luisquintanilla
services: synapse-analytics
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: spark
ms.date: 05/01/2020
ms.author: luquinta
ms.reviewer: jrasnick
ms.openlocfilehash: 8d045c1ec96bb7b31a710a28e30e3d428922b65e
ms.sourcegitcommit: 2654d8d7490720a05e5304bc9a7c2b41eb4ae007
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/13/2021
ms.locfileid: "107378555"
---
# <a name="use-net-for-apache-spark-with-azure-synapse-analytics"></a>Utilizar o .NET para Apache Spark com o Azure Synapse Analytics

[.NET for Apache Spark](https://dot.net/spark) fornece suporte gratuito, [de código aberto](https://github.com/dotnet/spark)e cross-platform .NET para a Spark. 

Fornece encadernações .NET para a Spark, que lhe permite aceder a APIs de Faísca através de C# e F#. Com .NET para Apache Spark, também pode escrever e executar funções definidas pelo utilizador para Spark escritas em .NET. As APIs .NET para spark permitem-lhe aceder a todos os aspetos dos DataFrames de Faíscas que o ajudam a analisar os seus dados, incluindo Spark SQL, Delta Lake e Structured Streaming.

Pode analisar dados com .NET para Apache Spark através de definições de trabalho em lote spark ou com cadernos Azure Synapse Analytics interativos. Neste artigo, você aprende a usar .NET para Apache Spark com Azure Synapse usando ambas as técnicas.

## <a name="submit-batch-jobs-using-the-spark-job-definition"></a>Submeta trabalhos de lote usando a definição de trabalho spark

Visite o tutorial para aprender a usar a Azure Synapse Analytics para criar definições de [emprego apache spark para piscinas de Faíscas sinapses.](apache-spark-job-definitions.md) Se ainda não embalou a sua aplicação para submeter ao Azure Synapse, complete os seguintes passos.

1. Execute os seguintes comandos para publicar a sua aplicação. Não se esqueça de substituir *o mySparkApp* pelo caminho para a sua aplicação.
   
   ```dotnetcli
   cd mySparkApp
   dotnet publish -c Release -f netcoreapp3.1 -r ubuntu.18.04-x64
   ```

2. Zip o conteúdo da pasta de publicação, `publish.zip` por exemplo, que foi criado como resultado do Passo 1. Todos os conjuntos devem estar na primeira camada do ficheiro ZIP e não deve haver camada de pasta intermédia. Isto significa que, quando desapertar, `publish.zip` todos os conjuntos são extraídos para o seu diretório de trabalho atual.

    **Nas janelas:**

    Utilize um programa de extração, como [o 7-Zip](https://www.7-zip.org/) ou o [WinZip,](https://www.winzip.com/)para extrair o ficheiro no diretório do caixote do lixo com todos os binários publicados.

    **Em Linux:**

    Abra uma concha de bash e cd no diretório do caixote do lixo com todos os binários publicados e executar o seguinte comando.

    ```bash
    zip -r publish.zip
    ```

## <a name="net-for-apache-spark-in-azure-synapse-analytics-notebooks"></a>.NET para Apache Spark em Azure Synapse Analytics cadernos 

Os cadernos são uma ótima opção para prototipagem do seu .NET para os oleodutos e cenários apache spark. Pode começar a trabalhar com, compreender, filtrar, exibir e visualizar os seus dados de forma rápida e eficiente. 

Engenheiros de dados, cientistas de dados, analistas de negócios e engenheiros de machine learning são todos capazes de colaborar sobre um documento interativo partilhado. Você vê resultados imediatos da exploração de dados, e pode visualizar os seus dados no mesmo caderno.

### <a name="how-to-use-net-for-apache-spark-notebooks"></a>Como usar .NET para cadernos Apache Spark

Quando cria um novo caderno, escolhe um núcleo linguístico que deseja expressar a sua lógica de negócio. O suporte kernel está disponível para vários idiomas, incluindo C#.

Para utilizar .NET para Apache Spark no seu caderno Azure Synapse Analytics, selecione **.NET Spark (C#)** como o seu núcleo e prenda o caderno a uma piscina Apache Spark sem servidor existente.

O caderno .NET Spark baseia-se nas experiências [interativas .NET](https://github.com/dotnet/interactive) e proporciona experiências interativas C# com a capacidade de utilizar .NET para Spark fora da caixa com a variável de sessão Spark `spark` já predefinida.

### <a name="install-nuget-packages-in-notebooks"></a>Instalar pacotes NuGet em cadernos

Pode instalar pacotes NuGet à sua escolha no seu caderno utilizando o `#r nuget` comando mágico antes do nome do pacote NuGet. O seguinte diagrama mostra um exemplo:

![Screenshot que mostra usar #r para instalar um pacote NuGet de portátil Spark .NET](./media/apache-spark-development-using-notebooks/synapse-spark-dotnet-notebook-nuget.png)

Para saber mais sobre como trabalhar com pacotes NuGet em cadernos, consulte a [Documentação Interativa .NET](https://github.com/dotnet/interactive/blob/main/docs/nuget-overview.md).

### <a name="net-for-apache-spark-c-kernel-features"></a>.NET para apache spark c# recursos de kernel

As seguintes funcionalidades estão disponíveis quando utiliza .NET para Apache Spark no caderno Azure Synapse Analytics:

* DECLARATIVO HTML: Gere a saída das suas células utilizando a sintaxe HTML, como cabeçalhos, listas de balas e até mesmo exibição de imagens.
* Declarações simples de C# (tais como atribuições, impressão para consola, lançamento de exceções, e assim por diante).
* Blocos de código C# multi-linha (como se declarações, circuitos de foreach, definições de classe, e assim por diante).
* Acesso à biblioteca C# padrão (como System, LINQ, Enumerables, e assim por diante).
* Suporte para características linguísticas C# 8.0.
* `spark` como uma variável pré-definida para lhe dar acesso à sua sessão Apache Spark.
* Suporte para definir [funções definidas pelo utilizador .NET que podem ser executadas dentro do Apache Spark](/dotnet/spark/how-to-guides/udf-guide). Recomendamos [escrever e chamar UDFs em .NET para ambientes Apache Spark Interactive](/dotnet/spark/how-to-guides/dotnet-interactive-udf-issue) para aprender a usar UDFs em .NET para experiências Apache Spark Interactive.
* Suporte para visualizar a produção dos seus trabalhos spark usando diferentes gráficos (como linha, bar ou histograma) e layouts (como simples, sobreposto, e assim por diante) usando a `XPlot.Plotly` biblioteca.
* Capacidade de incluir pacotes NuGet no seu caderno C#.

## <a name="next-steps"></a>Passos seguintes

* [.NET para documentação Apache Spark](/dotnet/spark/)
* [.NET para guias Apache Spark Interactive](/dotnet/spark/how-to-guides/dotnet-interactive-udf-issue)
* [Azure Synapse Analytics](https://azure.microsoft.com/services/synapse-analytics/)
* [.NET Interativo](https://devblogs.microsoft.com/dotnet/creating-interactive-net-documentation/)
