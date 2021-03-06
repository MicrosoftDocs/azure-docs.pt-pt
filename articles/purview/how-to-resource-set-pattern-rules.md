---
title: Como criar regras de padrão definidas por recursos
description: Aprenda a criar uma regra de padrão de conjunto de recursos para substituir como os ativos são agrupados em conjuntos de recursos
author: djpmsft
ms.author: daperlov
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 04/15/2021
ms.openlocfilehash: 61de2cf2e3ad9175d97378234d62f72ab3517b51
ms.sourcegitcommit: 272351402a140422205ff50b59f80d3c6758f6f6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/17/2021
ms.locfileid: "107587838"
---
# <a name="create-resource-set-pattern-rules"></a>Crie regras de padrão de conjunto de recursos

Os sistemas de processamento de dados à escala normalmente armazenam uma única tabela num disco como vários ficheiros. Este conceito é representado em Azure Purview utilizando conjuntos de recursos. Um conjunto de recursos é um único objeto no catálogo de dados que representa um grande número de ativos em armazenamento. Para saber mais, consulte [conjuntos de recursos de compreensão.](concept-resource-sets.md)

Ao digitalizar uma conta de armazenamento, o Azure Purview utiliza um conjunto de padrões definidos para determinar se um grupo de ativos é um conjunto de recursos. Em alguns casos, o agrupamento de conjuntos de recursos da Azure Purview pode não refletir com precisão a sua propriedade de dados. As regras de padrão definidas por recursos permitem-lhe personalizar ou sobrepor a forma como o Azure Purview deteta quais os ativos agrupados como conjuntos de recursos e como são exibidos dentro do catálogo.

As regras de padrão são atualmente suportadas nos seguintes tipos de origem:
- Armazenamento do Azure Data Lake Ger2
- Armazenamento de Blobs do Azure
- Ficheiros do Azure


## <a name="how-to-create-a-resource-set-pattern-rule"></a>Como criar uma regra de padrão de conjunto de recursos

Siga os passos abaixo para criar uma nova regra de padrão definido por recursos:

1. Vá ao centro de gestão. Selecione **as regras** de padrão do menu sob o título de conjuntos de recursos. Selecione **+ Novo** para criar um novo conjunto de regras.

   :::image type="content" source="media/how-to-resource-set-pattern-rules/create-new-scoped-resource-set-rule.png" alt-text="Criar nova regra de padrão de conjunto de recursos" border="true":::

1. Introduza o âmbito da regra do padrão definido pelo recurso. Selecione o tipo de conta de armazenamento e o nome da conta de armazenamento que deseja criar uma regra definida. Cada conjunto de regras é aplicado em relação a um âmbito de trajetória de pasta especificado no campo de caminhos da **pasta.**

   :::image type="content" source="media/how-to-resource-set-pattern-rules/create-new-scoped-resource-set-scope.png" alt-text="Criar configurações de regra de padrão de conjunto de recursos" border="true":::

1. Para introduzir uma regra para um âmbito de configuração, selecione **+ Nova Regra**.

1. Insira nos seguintes campos para criar uma regra:

   1. **Nome da regra:** O nome da regra de configuração. Este campo não tem qualquer efeito sobre os ativos a que a regra se aplica.

   1. **Nome qualificado:** Um caminho qualificado que usa uma combinação de texto, substitutos dinâmicos e substitutos estáticos para combinar os ativos com a regra de configuração. Este caminho é relativo ao âmbito da regra de configuração. Consulte a secção [de sintaxe](#syntax) abaixo para obter instruções detalhadas sobre como especificar nomes qualificados.

   1. **Nome do visor:** O nome de exibição do ativo. Este campo é opcional. Utilize texto simples e substitutos estáticos para personalizar a forma como um ativo é apresentado no catálogo. Para obter instruções mais detalhadas, consulte a secção [de sintaxe](#syntax) abaixo.

   1. **Não agrupar como conjunto de recursos:** Se ativado, o recurso combinado não será agrupado num conjunto de recursos.

      :::image type="content" source="media/how-to-resource-set-pattern-rules/scoped-resource-set-rule-example.png" alt-text="Criar nova regra de configuração." border="true":::

1. Guarde a regra clicando **em Adicionar**.

> [!NOTE]
> Após a criação de uma regra de padrão, todos os novos exames aplicarão a regra durante a ingestão. Os ativos existentes no catálogo de dados serão atualizados através de um processo de fundo que pode demorar até algumas horas. 

## <a name="pattern-rule-syntax"></a><a name="syntax"></a> Sintaxe de regra de padrão

Ao criar regras de padrão de definição de recursos, utilize a seguinte sintaxe para especificar quais as regras do ativo aplicáveis.

### <a name="dynamic-replacers-single-brackets"></a>Substitutos dinâmicos (suportes individuais)

Os suportes individuais são utilizados como **substitutos dinâmicos** numa regra de padrão. Especifique um substituto dinâmico no nome qualificado utilizando o formato `{<replacerName:<replacerType>}` . Se combinados, os substitutos dinâmicos são usados como uma condição de agrupamento que indica que os ativos devem ser representados como um conjunto de recursos. Se os ativos forem agrupados num conjunto de recursos, o caminho qualificado definido pelo recurso conterá `{replacerName}` onde o substituto foi especificado.

Por exemplo, se dois ativos `folder1/file-1.csv` e `folder2/file-2.csv` compatíveis com a `{folder:string}/file-{NUM:int}.csv` regra, o conjunto de recursos seria uma única entidade `{folder}/file-{NUM}.csv` .

#### <a name="special-case-dynamic-replacers-when-not-grouping-into-resource-set"></a>Caso especial: Substitutos dinâmicos quando não agrupar em conjunto de recursos

Se *Não agrupar como conjunto de recursos* está ativado para uma regra de padrão, o nome de substituidor é um campo opcional. `{:<replacerType>}` é sintaxe válida. Por exemplo, `file-{:int}.csv` combinaria com sucesso `file-1.csv` e `file-2.csv` criaria dois ativos diferentes em vez de um conjunto de recursos.

### <a name="static-replacers-double-brackets"></a>Substituidores estáticos (suportes duplos)

Os suportes duplos são utilizados como **substitutos estáticos** no nome qualificado de uma regra de padrão. Especifique um substituto estático no nome qualificado utilizando o formato `{{<replacerName>:<replacerType>}}` . Se combinado, cada conjunto de valores únicos de substituição estática criará diferentes agrupamentos de conjuntos de recursos.

Por exemplo, se dois ativos `folder1/file-1.csv` e `folder2/file-2.csv` compatíveis com a `{{folder:string}}/file-{NUM:int}.csv` regra, dois conjuntos de recursos seriam criados `folder1/file-{NUM}.csv` e `folder2/file-{NUM}.csv` .

Os substitutos estáticos podem ser utilizados para especificar o nome de visualização de um ativo correspondente a uma regra de padrão. A utilização `{{<replacerName>}}` no nome de exibição de uma regra utilizará o valor combinado no nome do ativo.

### <a name="available-replacement-types"></a>Tipos de substituição disponíveis

Abaixo estão os tipos disponíveis que podem ser usados em substituidores estáticos e dinâmicos:

| Tipo | Estrutura |
| ---- | --------- |
| string | Uma série de caracteres Unicode ou 1 ou mais, incluindo delimiters como espaços. |
| int | Uma série de caracteres ASCII de 1 ou mais 0-9, pode ser 0 prefixado (por exemplo, 0001). |
| guid | Uma série de 32 ou 8-4-4-4-12 representação de uma UUID como definida em [RFC 4122](https://tools.ietf.org/html/rfc4122). |
| data | Uma série de caracteres ASCII de 6 ou 8 0-9 com separadores opcionais: yyymmdd, yyy-mm-dd, yymmdd, yy-mm-dd, especificados no [RFC 3339](https://tools.ietf.org/html/rfc3339). |
| hora | Uma série de caracteres ASCII de 4 ou 6 0-9 com separadores opcionais: HHmm, HH:mm, HHmmss, HH:mm:ss especificados no [RFC 3339](https://tools.ietf.org/html/rfc3339). |
| carimbo de data/hora | Uma série de caracteres ASCII de 12 ou 14 0-9 com separadores opcionais: yyyy-mm-ddTHH:mm, yyyymmddhmm, yyyy-mm-ddTHH:mm:mm:ss, yyymmdDHmmss especificado em [RFC 3339](https://tools.ietf.org/html/rfc3339). |
| boolean | Pode conter "verdadeiro" ou "falso", caso insensível. |
| número | Uma série de caracteres 0 ou mais 0-9 ASCII, pode ser 0 prefixados (por exemplo, 0001) seguidos opcionalmente por um ponto '.' e uma série de caracteres 1 ou mais 0-9 ASCII, pode ser 0 postfixado (por exemplo, .100) |
| hex | Uma série de caracteres ASCII ou 1 ou mais do conjunto 0-1 e A-F, o valor pode ser 0 prefixado |
| região | Uma corda que corresponde à sintaxe especificada no [RFC 5646](https://tools.ietf.org/html/rfc5646). |

## <a name="order-of-resource-set-pattern-rules-getting-applied"></a>Ordem das regras de padrão definidas por recursos a ser aplicada

Segue-se a ordem de operações para a aplicação das regras de padrão:

1. Os âmbitos mais específicos terão prioridade se um ativo corresponder a duas regras. Por exemplo, as regras num âmbito `container/folder` aplicar-se-ão antes de regras de âmbito `container` .

1. Ordem de regras dentro de um âmbito específico. Isto pode ser editado no UX.

1. Se um ativo não corresponder a nenhuma regra especificada, aplica-se a heurística do conjunto de recursos predefinido.

## <a name="examples"></a>Exemplos

### <a name="example-1"></a>Exemplo 1

Extração de dados SAP em cargas completas e delta

#### <a name="inputs"></a>Entradas

Ficheiros:

- `https://myazureblob.blob.core.windows.net/bar/customer/full/2020/01/13/saptable_customer_20200101_20200102_01.txt`
- `https://myazureblob.blob.core.windows.net/bar/customer/full/2020/01/13/saptable_customer_20200101_20200102_02.txt`
- `https://myazureblob.blob.core.windows.net/bar/customer/delta/2020/01/15/saptable_customer_20200101_20200102_01.txt`
- `https://myazureblob.blob.core.windows.net/bar/customer/full/2020/01/17/saptable_customer_20200101_20200102_01.txt`
- `https://myazureblob.blob.core.windows.net/bar/customer/full/2020/01/17/saptable_customer_20200101_20200102_02.txt`

#### <a name="pattern-rule"></a>Regra de padrão

**Âmbito:**`https://myazureblob.blob.core.windows.net/bar/`

**Nome do visor:** 'Cliente Externo'

**Nome qualificado:**`customer/{extract:string}/{year:int}/{month:int}/{day:int}/saptable_customer_{date_from:date}_{date_to:time}_{sequence:int}.txt`

**Conjunto de recursos:** verdadeiro

#### <a name="output"></a>Saída

Um ativo conjunto de recursos

**Nome do visor:** Cliente Externo

**Nome qualificado:**`https://myazureblob.blob.core.windows.net/bar/customer/{extract}/{year}/{month}/{day}/saptable_customer_{date_from}_{date_to}_{sequence}.txt`

### <a name="example-2"></a>Exemplo 2

Dados IoT em formato avro

#### <a name="inputs"></a>Entradas

Ficheiros:

- `https://myazureblob.blob.core.windows.net/bar/raw/machinename-89/01-01-2020/22:33:22-001.avro`
- `https://myazureblob.blob.core.windows.net/bar/raw/machinename-89/01-01-2020/22:33:22-002.avro`
- `https://myazureblob.blob.core.windows.net/bar/raw/machinename-89/02-01-2020/22:33:22-001.avro`
- `https://myazureblob.blob.core.windows.net/bar/raw/machinename-90/01-01-2020/22:33:22-001.avro`

#### <a name="pattern-rules"></a>Regras de padrão

**Âmbito:**`https://myazureblob.blob.core.windows.net/bar/`

Regra 1

**Nome do visor:** 'máquina-89'

**Nome qualificado:**`raw/machinename-89/{date:date}/{time:time}-{id:int}.avro`

**Conjunto de recursos:** verdadeiro

Regra 2

**Nome do visor:** 'máquina-90'

**Nome qualificado:**`raw/machinename-90/{date:date}/{time:time}-{id:int}.avro`

**Conjunto de recursos:** verdadeiro

#### <a name="outputs"></a>Saídas

2 conjuntos de recursos

Conjunto de recursos 1

**Nome do visor:** máquina-89

**Nome qualificado:**`https://myazureblob.blob.core.windows.net/bar/raw/machinename-89/{date}/{time}-{id}.avro`

Conjunto de recursos 2

**Nome do visor:** máquina-90

**Nome qualificado:**`https://myazureblob.blob.core.windows.net/bar/raw/machinename-90/{date}/{time}-{id}.avro`

### <a name="example-3"></a>Exemplo 3

Dados IoT em formato avro

#### <a name="inputs"></a>Entradas

Ficheiros:

- `https://myazureblob.blob.core.windows.net/bar/raw/machinename-89/01-01-2020/22:33:22-001.avro`
- `https://myazureblob.blob.core.windows.net/bar/raw/machinename-89/01-01-2020/22:33:22-002.avro`
- `https://myazureblob.blob.core.windows.netbar/raw/machinename-89/02-01-2020/22:33:22-001.avro`
- `https://myazureblob.blob.core.windows.net/bar/raw/machinename-90/01-01-2020/22:33:22-001.avro`

#### <a name="pattern-rule"></a>Regra de padrão

**Âmbito:**`https://myazureblob.blob.core.windows.net/bar/`

**Nome do visor:** 'Machine-{{machineid}}

**Nome qualificado:**`raw/machinename-{{machineid:int}}/{date:date}/{time:time}-{id:int}.avro`

**Conjunto de recursos:** verdadeiro

#### <a name="outputs"></a>Saídas

Conjunto de recursos 1

**Nome do visor:** máquina-89

**Nome qualificado:**`https://myazureblob.blob.core.windows.net/bar/raw/machinename-89/{date}/{time}-{id}.avro`

Conjunto de recursos 2

**Nome do visor:** máquina-90

**Nome qualificado:**`https://myazureblob.blob.core.windows.net/bar/raw/machinename-90/{date}/{time}-{id}.avro`

### <a name="example-4"></a>Exemplo 4

Não agrupar em conjuntos de recursos

#### <a name="inputs"></a>Entradas

Ficheiros:

- `https://myazureblob.blob.core.windows.net/bar/raw/machinename-89/01-01-2020/22:33:22-001.avro`
- `https://myazureblob.blob.core.windows.net/bar/raw/machinename-89/01-01-2020/22:33:22-002.avro`
- `https://myazureblob.blob.core.windows.net/bar/raw/machinename-89/02-01-2020/22:33:22-001.avro`
- `https://myazureblob.blob.core.windows.net/bar/raw/machinename-90/01-01-2020/22:33:22-001.avro`

#### <a name="pattern-rule"></a>Regra de padrão

**Âmbito:**`https://myazureblob.blob.core.windows.net/bar/`

**Nome do visor:**`Machine-{{machineid}}`

**Nome qualificado:**`raw/machinename-{{machineid:int}}/{{:date}}/{{:time}}-{{:int}}.avro`

**Conjunto de recursos:** falso

#### <a name="outputs"></a>Saídas

4 ativos individuais

Ativo 1

**Nome do visor:** máquina-89

**Nome qualificado:**`https://myazureblob.blob.core.windows.net/bar/raw/machinename-89/01-01-2020/22:33:22-001.avro`

Ativo 2

**Nome do visor:** máquina-89

**Nome qualificado:**`https://myazureblob.blob.core.windows.net/bar/raw/machinename-89/01-01-2020/22:33:22-002.avro`

Ativo 3

**Nome do visor:** máquina-89

**Nome qualificado:**`https://myazureblob.blob.core.windows.net/bar/raw/machinename-89/02-01-2020/22:33:22-001.avro`

Ativo 4

**Nome do visor:** máquina-90

**Nome qualificado:**`https://myazureblob.blob.core.windows.net/bar/raw/machinename-90/01-01-2020/22:33:22-001.avro`

## <a name="next-steps"></a>Passos seguintes

Começa por [registar e digitalizar uma conta de armazenamento Azure Data Lake Gen2](register-scan-adls-gen2.md).