---
title: Avaliar e melhorar a precisão da fala personalizada - Serviço de fala
titleSuffix: Azure Cognitive Services
description: Neste documento aprende-se a medir quantitativamente e a melhorar a qualidade do nosso modelo de discurso-texto ou o seu modelo personalizado. Os dados de transcrição áudio + humanos são necessários para testar a precisão e devem ser fornecidos 30 minutos a 5 horas de áudio representativo.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/12/2021
ms.author: trbye
ms.openlocfilehash: b7e4ea586098ea3eb0dfd684650f798d7988e18b
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2021
ms.locfileid: "100634588"
---
# <a name="evaluate-and-improve-custom-speech-accuracy"></a>Avaliar e melhorar a precisão da Voz Personalizada

Neste artigo, aprende-se a medir quantitativamente e a melhorar a precisão dos modelos de fala-a-texto da Microsoft ou dos seus próprios modelos personalizados. Os dados de transcrição áudio + humanos são necessários para testar a precisão e devem ser fornecidos 30 minutos a 5 horas de áudio representativo.

## <a name="evaluate-custom-speech-accuracy"></a>Avaliar a precisão da Voz Personalizada

A norma da indústria para medir a precisão do modelo é [a Taxa de Erro de Texto](https://en.wikipedia.org/wiki/Word_error_rate) (WER). O WER conta o número de palavras incorretas identificadas durante o reconhecimento e, em seguida, divide-se pelo número total de palavras fornecidas na transcrição com rótulo humano (mostrada abaixo como N). Finalmente, este número é multiplicado em 100% para calcular o WER.

![Fórmula WER](./media/custom-speech/custom-speech-wer-formula.png)

As palavras incorretamente identificadas enquadram-se em três categorias:

* Inserção (I): Palavras que são incorretamente adicionadas na transcrição da hipótese
* Supressão (D): Palavras que não são detetadas na transcrição da hipótese
* Substituição (S): Palavras que foram substituídas entre referência e hipótese

Eis um exemplo:

![Exemplo de palavras mal identificadas](./media/custom-speech/custom-speech-dis-words.png)

Se quiser replicar medições de WER localmente, pode utilizar o sclite da [SCTK](https://github.com/usnistgov/SCTK).

## <a name="resolve-errors-and-improve-wer"></a>Resolver erros e melhorar o WER

Pode utilizar o WER a partir dos resultados de reconhecimento da máquina para avaliar a qualidade do modelo que está a utilizar com a sua aplicação, ferramenta ou produto. Um WER de 5%-10% é considerado de boa qualidade e está pronto a ser usado. Um WER de 20% é aceitável, no entanto, você pode considerar a formação adicional. Um WER de 30% ou mais sinaliza má qualidade e requer personalização e formação.

A forma como os erros são distribuídos é importante. Quando muitos erros de eliminação são encontrados, geralmente é devido à fraca força do sinal de áudio. Para resolver este problema, terá de recolher dados áudio mais próximos da fonte. Erros de inserção significam que o áudio foi gravado num ambiente ruidoso e a conversa cruzada pode estar presente, causando problemas de reconhecimento. Os erros de substituição são frequentemente encontrados quando uma amostra insuficiente de termos específicos do domínio foi fornecida como transcrições com rótulo humano ou texto relacionado.

Ao analisar ficheiros individuais, pode determinar que tipo de erros existem e quais os erros exclusivos de um ficheiro específico. Compreender problemas ao nível dos ficheiros irá ajudá-lo a direcionar as melhorias.

## <a name="create-a-test"></a>Criar um teste

Se quiser testar a qualidade do modelo de linha de base fala-a-texto da Microsoft ou um modelo personalizado que treinou, pode comparar dois modelos lado a lado para avaliar a precisão. A comparação inclui wer e resultados de reconhecimento. Normalmente, um modelo personalizado é comparado com o modelo de base da Microsoft.

Para avaliar os modelos lado a lado:

1. Inscreva-se no [portal de Discurso Personalizado](https://speech.microsoft.com/customspeech).
2. Navegue **para discurso a texto > > de discurso personalizado [nome do projeto] > Testes**.
3. Clique **em Adicionar Teste**.
4. Selecione **Avaliar a precisão**. Dê ao teste um nome, descrição e selecione o seu conjunto de dados de transcrição com rótulo humano + áudio + humano.
5. Selecione até dois modelos que gostaria de testar.
6. Clique em **Criar**.

Depois de o seu teste ter sido criado com sucesso, pode comparar os resultados lado a lado.

### <a name="side-by-side-comparison"></a>Comparação lado a lado

Uma vez concluído o teste, indicado pela alteração de estado para *'Sucesso',* encontrará um número WER para ambos os modelos incluídos no seu teste. Clique no nome do teste para ver a página de detalhes do teste. Esta página de detalhe lista todas as expressões no seu conjunto de dados, indicando os resultados de reconhecimento dos dois modelos juntamente com a transcrição do conjunto de dados submetido. Para ajudar a inspecionar a comparação lado a lado, pode alternar vários tipos de erros, incluindo inserção, eliminação e substituição. Ao ouvir o áudio e comparar os resultados de reconhecimento em cada coluna, que mostra a transcrição com rótulo humano e os resultados de dois modelos de discurso a texto, pode decidir qual o modelo que satisfaz as suas necessidades e onde são necessárias formações e melhorias adicionais.

## <a name="improve-custom-speech-accuracy"></a>Melhorar a precisão da Voz Personalizada

Os cenários de reconhecimento da fala variam consoante a qualidade do áudio e a linguagem (vocabulário e estilo de fala). A tabela seguinte analisa quatro cenários comuns:

| Scenario | Qualidade áudio | Vocabulário | Estilo de fala |
|----------|---------------|------------|----------------|
| Call-center | Baixo, 8 kHz, pode ser 2 humanos em 1 canal de áudio, poderia ser comprimido | Estreito, único para domínio e produtos | Conversação, vagamente estruturada |
| Assistente de voz (como Cortana, ou uma janela drive-through) | Alto, 16 kHz | Entidade pesada (títulos de música, produtos, locais) | Palavras e frases claramente ditas |
| Ditado (mensagem instantânea, notas, pesquisa) | Alto, 16 kHz | Variado | Tomada de notas |
| Legendas fechadas em vídeo | Variada, incluindo o uso variado do microfone, música adicionada | Variado, de reuniões, discurso recitado, letras musicais | Ler, preparar ou vagamente estruturado |

Diferentes cenários produzem diferentes resultados de qualidade. A tabela seguinte examina como o conteúdo destes quatro cenários é classificado na taxa de erro de [palavras (WER)](how-to-custom-speech-evaluate-data.md). A tabela mostra quais os tipos de erros mais comuns em cada cenário.

| Scenario | Qualidade de Reconhecimento de Voz | Erros de inserção | Erros de Eliminação | Erros de substituição |
|----------|----------------------------|------------------|-----------------|---------------------|
| Call-center | Médio (< 30% WER) | Baixo, exceto quando outras pessoas falam no fundo | Pode ser alto. Os call centers podem ser barulhentos, e os alto-falantes sobrepostos podem confundir o modelo | Média. Produtos e nomes de pessoas podem causar estes erros |
| Assistente de voz | Alto (pode ser < 10% WER) | Baixo | Baixo | Médio, devido a títulos de música, nomes de produtos ou locais |
| Ditado | Alto (pode ser < 10% WER) | Baixo | Baixo | Alto |
| Legendas fechadas em vídeo | Depende do tipo de vídeo (pode ser < 50% WER) | Baixo | Pode ser alto devido a música, ruídos, qualidade do microfone | Jargon pode causar estes erros |

Determinar os componentes do WER (número de erros de inserção, eliminação e substituição) ajuda a determinar que tipo de dados adicionar para melhorar o modelo. Utilize o [portal Discurso Personalizado](https://speech.microsoft.com/customspeech) para ver a qualidade de um modelo de base. O portal informa as taxas de inserção, substituição e erro de eliminação que são combinadas na taxa de qualidade WER.

## <a name="improve-model-recognition"></a>Melhorar o reconhecimento de modelos

Pode reduzir os erros de reconhecimento adicionando dados de formação no [portal Discurso Personalizado](https://speech.microsoft.com/customspeech). 

Planeie manter o seu modelo personalizado adicionando materiais de origem periodicamente. O seu modelo personalizado necessita de formação adicional para se manter atento às alterações às suas entidades. Por exemplo, pode precisar de atualizações para nomes de produtos, nomes de músicas ou novos locais de serviço.

As secções seguintes descrevem como cada tipo de dados adicionais de treino podem reduzir os erros.

### <a name="add-related-text-sentences"></a>Adicionar frases de texto relacionadas

Quando treina um novo modelo personalizado, comece por adicionar texto relacionado para melhorar o reconhecimento de palavras e frases específicas do domínio. As frases de texto relacionadas podem reduzir principalmente os erros de substituição relacionados com o reconhecimento errado das palavras comuns e das palavras específicas do domínio, mostrando-as em contexto. Palavras específicas do domínio podem ser palavras incomuns ou inventadas, mas a sua pronúncia deve ser simples de ser reconhecida.

> [!NOTE]
> Evite frases de texto relacionadas que incluam ruídos como caracteres irreconhecíveis ou palavras.

### <a name="add-audio-with-human-labeled-transcripts"></a>Adicione áudio com transcrições com rótulo humano

O áudio com transcrições com rótulo humano oferece as maiores melhorias de precisão se o áudio vier do caso de utilização do alvo. As amostras devem cobrir todo o âmbito da fala. Por exemplo, um call center para uma loja receberia a maioria das chamadas sobre roupa de banho e óculos de sol durante os meses de verão. Certifique-se de que a sua amostra inclui todo o âmbito de fala que pretende detetar.

Considere estes detalhes:

* O treino com áudio trará mais benefícios se o áudio também for difícil de entender para os humanos. Na maioria dos casos, deve começar a treinar usando apenas texto relacionado.
* Se usar uma das línguas mais utilizadas, como o US-English, há boas hipóteses de não haver necessidade de treinar com dados áudio. Para tais línguas, os modelos base oferecem já resultados de reconhecimento muito bons na maioria dos cenários; É provavelmente o suficiente para treinar com texto relacionado.
* O Custom Speech só pode capturar o contexto da palavra para reduzir erros de substituição, não inserção ou erros de eliminação.
* Evite amostras que incluam erros de transcrição, mas inclua uma diversidade de qualidade áudio.
* Evite frases que não estejam relacionadas com o seu domínio problemático. Frases não relacionadas podem prejudicar o seu modelo.
* Quando a qualidade das transcrições varia, pode duplicar frases excepcionalmente boas (como excelentes transcrições que incluem frases-chave) para aumentar o seu peso.
* O serviço Desemação utilizará automaticamente as transcrições para melhorar o reconhecimento de palavras e frases específicas do domínio, como se fossem adicionadas como texto relacionado.
* Pode levar vários dias para uma operação de treino ser concluída. Para melhorar a velocidade de formação, certifique-se de criar a sua subscrição de serviço De Discurso numa [região com o hardware dedicado](custom-speech-overview.md#set-up-your-azure-account) para a formação.

> [!NOTE]
> Nem todos os modelos base suportam o treino com áudio. Se um modelo base não o suportar, o serviço Desemaguiso utilizará apenas o texto das transcrições e ignorará o áudio. Consulte [o suporte linguístico](language-support.md#speech-to-text) para uma lista de modelos base que suportam a formação com dados áudio. Mesmo que um modelo base suporte a formação com dados áudio, o serviço pode usar apenas uma parte do áudio. Ainda assim, vai usar todas as transcrições.

> [!NOTE]
> Nos casos em que altera o modelo base utilizado para o treino, e tem áudio no conjunto de dados de treino, verifique *sempre* se o novo modelo base selecionado [suporta a formação com dados áudio](language-support.md#speech-to-text). Se o modelo base anteriormente utilizado não suportasse a formação com dados áudio, e o conjunto de dados de formação contiver áudio, o tempo de treino com o novo modelo base aumentará **drasticamente,** podendo facilmente passar de várias horas para vários dias e mais. Isto é especialmente verdade se a subscrição do seu serviço Desem declarações **não** estiver numa [região com o hardware dedicado](custom-speech-overview.md#set-up-your-azure-account) para a formação.
>
> Se encarar a questão descrita no parágrafo acima, pode diminuir rapidamente o tempo de treino reduzindo a quantidade de áudio no conjunto de dados ou removendo-o completamente e deixando apenas o texto. Esta última opção é altamente recomendada se a subscrição do serviço Speech **não** estiver numa [região com o hardware dedicado](custom-speech-overview.md#set-up-your-azure-account) para a formação.

### <a name="add-new-words-with-pronunciation"></a>Adicione novas palavras com pronúncia

Palavras que são inventadas ou altamente especializadas podem ter pronúncias únicas. Estas palavras podem ser reconhecidas se a palavra pode ser dividida em palavras menores para pronunciá-la. Por exemplo, reconhecer a **Xbox,** pronunciar-se como **caixa X**. Esta abordagem não aumentará a precisão global, mas poderá aumentar o reconhecimento destas palavras-chave.

> [!NOTE]
> Esta técnica só está disponível para algumas línguas neste momento. Consulte a personalização para pronúncia [na tabela Discurso-a-texto](language-support.md) para obter mais detalhes.

## <a name="sources-by-scenario"></a>Fontes por cenário

A tabela que se segue mostra cenários de reconhecimento de voz e lista materiais de origem a considerar nas três categorias de conteúdos de formação acima listadas.

| Scenario | Frases de texto relacionadas | Transcrições com rótulo humano + áudio + | Novas palavras com pronúncia |
|----------|------------------------|------------------------------|------------------------------|
| Call-center             | documentos de marketing, website, análises de produtos relacionadas com a atividade do call center | call center chama transcrita por humanos | termos que têm pronúncias ambíguas (ver Xbox acima) |
| Assistente de voz         | lista de frases usando todas as combinações de comandos e entidades | gravar vozes falando comandos em dispositivo, e transcrever em texto | nomes (filmes, canções, produtos) que têm pronúncias únicas |
| Ditado               | entrada escrita, como mensagens instantâneas ou e-mails | semelhante a acima | semelhante a acima |
| Legendas fechadas em vídeo | Scripts de séries de TV, filmes, conteúdos de marketing, resumos de vídeo | transcrições exatas de vídeos | semelhante a acima |

## <a name="next-steps"></a>Passos seguintes

* [Preparar e implementar um modelo](how-to-custom-speech-train-model.md)

## <a name="additional-resources"></a>Recursos adicionais

* [Prepare e teste os seus dados](./how-to-custom-speech-test-and-train.md)
* [Fiscalizar os seus dados](how-to-custom-speech-inspect-data.md)