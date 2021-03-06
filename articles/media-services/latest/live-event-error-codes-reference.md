---
title: Códigos de erro de evento ao vivo da Azure Media Services
description: Este artigo lista códigos de erro de evento ao vivo.
author: IngridAtMicrosoft
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: error-reference
ms.date: 03/26/2021
ms.author: inhenkel
ms.openlocfilehash: 09859a953b0127733cbf1b0876c1d2ffa6082096
ms.sourcegitcommit: 02bc06155692213ef031f049f5dcf4c418e9f509
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2021
ms.locfileid: "106279481"
---
# <a name="media-services-live-event-error-codes"></a>Códigos de erro do Evento Ao Vivo dos Serviços De Media

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

As tabelas que se seguem listam os códigos de erro [do Evento Vivo.](live-event-outputs-concept.md)

## <a name="liveeventconnectionrejected"></a>LiveEventConnectionRejected

Ao subscrever os eventos [Da Grelha de Eventos](../../event-grid/index.yml) para um evento ao vivo, poderá ver um dos seguintes erros do evento [LiveEventConnectionRejected.](monitoring/media-services-event-schemas.md\#liveeventconnectionrejected)
> [!div class="mx-tdCol2BreakAll"]
>| Erro | Informações |
>|--|--|
>|**MPE_RTMP_APPID_AUTH_FAILURE** ||
>|Descrição | URL de ingestão incorreta |
>|Solução sugerida| APPID é um símbolo GUID em RTMP ingest URL. Certifique-se de que combina com o URL Ingest da API. |
>|**MPE_INGEST_ENCODER_CONNECTION_DENIED** ||
>| Descrição |O IP do codificante não está presente na lista de autorizações IP configuradas |
>| Solução sugerida| Certifique-se de que o IP do codificader está na Lista de Autorizações IP. Utilize uma ferramenta on-line como *o whoismyip* ou a *calculadora CIDR* para definir o valor adequado.  Certifique-se de que o codificante pode chegar ao servidor antes do evento real ao vivo. |
>|**MPE_INGEST_RTMP_SETDATAFRAME_NOT_RECEIVED** ||
>| Descrição|O codificar RTMP não enviou o `setDataFrame` comando. |
>| Solução sugerida|A maioria dos codificadores comerciais enviam metadados de fluxo. Para um codificante que empurra uma única ingestão de bitrate, isto pode não ser problema. O LiveEvent é capaz de calcular a bitrate recebida quando faltam os metadados de fluxo.  Para ingerir multi-bitrate para um canal PassThru ou cenário de duplo impulso, pode tentar anexar a cadeia de consulta com 'videodatarate' e 'audiodatarate' no URL inger. O valor aproximado pode funcionar. A unidade está em Kbit. Por exemplo  `rtmp://hostname:1935/live/GUID_APPID/streamname?videodatarate=5000&audiodatarate=192` |
>|**MPE_INGEST_CODEC_NOT_SUPPORTED** ||
>| Descrição|O código especificado não é suportado.|
>| Solução sugerida| O LiveEvent recebeu um codec não suportado. Por exemplo, um RTMP ingerir, LiveEvent recebeu código de vídeo não-AVC.  Verifique a predefinição do codificação. |
>|**MPE_INGEST_DESCRIPTION_INFO_NOT_RECEIVED** ||
>| Descrição |A informação sobre a descrição dos meios de comunicação não foi recebida antes da entrega dos dados dos meios de comunicação social. |
>| Solução sugerida|O LiveEvent não recebe a descrição do fluxo (cabeçalho ou etiqueta FLV) do codificador. Isto é uma violação do protocolo. Contacte o fornecedor de codificação. |
>|**MPE_INGEST_MEDIA_QUALITIES_EXCEEDED** ||
>| Descrição|A contagem de qualidades para o tipo de áudio ou vídeo excedeu o limite máximo permitido. |
>| Solução sugerida|Quando o modo Live Event é Live Encoding, o codificadora deve empurrar um único bitrate de vídeo e áudio.  Note que é permitido um empurrão redundante da mesma bitrate. Verifique as definições de predefinição ou saída do codificadores para se certificar de que produz um único fluxo de bitrate. |
>|**MPE_INGEST_BITRATE_AGGREGATED_EXCEEDED** ||
>| Descrição|O valor total de entrada num evento ao vivo ou serviço de canais excedeu o limite máximo permitido. |
>| Solução sugerida|O codificader excedeu o bitrate máximo de entrada. Este limite agrega todos os dados recebidos do codificador contribuinte. Verifique as definições de predefinição ou saída do codificadores para reduzir o bitrate. |
>|**MPE_RTMP_FLV_TAG_TIMESTAMP_INVALID** ||
>| Descrição|O tempotad para vídeo ou áudio FLVTag é inválido do codificar RTMP. |
>| Solução sugerida|Preterido. |
>|**MPE_INGEST_FRAMERATE_EXCEEDED** ||
>| Descrição|Os fluxos ingeridos com taxas de fotogramas ultrapassaram o máximo permitido 30 fps para codificar eventos/canais ao vivo. |
>| Solução sugerida|Verifique o codificar predefinido para baixar a taxa de fotogramas para menos de 36 fps. |
>|**MPE_INGEST_VIDEO_RESOLUTION_NOT_SUPPORTED** ||
>| Descrição|Os fluxos ingeridos de entrada ultrapassaram as seguintes resoluções permitidas: 1920x1088 para codificação de eventos/canais ao vivo e 4096 x 2160 para eventos/canais ao vivo. |
>| Solução sugerida|Verifique o codificar predefinido para reduzir a resolução de vídeo para que não exceda o limite. |
>|**MPE_INGEST_RTMP_TOO_LARGE_UNPROCESSED_FLV** |
>| Descrição|O evento ao vivo recebeu uma grande quantidade de dados áudio ao mesmo tempo, ou uma grande quantidade de dados de vídeo sem qualquer frames chave. Desligamos o codificar para lhe dar a oportunidade de voltar a tentar com dados corretos. |
>| Solução sugerida|Certifique-se de que o codificar envia um quadro-chave para cada intervalo de quadro de tecla (GOP).  Ativar definições como "Bitrate constante (CBR)" ou "Alinhar quadros de teclas". Às vezes, repor o codificador que contribui pode ajudar. Se não ajudar, contacte o fornecedor de codificação. |

## <a name="liveeventencoderdisconnected"></a>LiveEventEncoderDis ligados

Pode ver um dos seguintes erros do [evento LiveEventEncoderDis.](monitoring/media-services-event-schemas.md\#liveeventencoderdisconnected)

> [!div class="mx-tdCol2BreakAll"]
>| Erro | Informações |
>|--|--|
>|**MPE_RTMP_SESSION_IDLE_TIMEOUT** |
>| Descrição|A sessão de RTMP foi cronometrada depois de ter ficado inativa por um limite de tempo permitido. |
>|Solução sugerida|Isto acontece normalmente quando um codificadora deixa de receber o feed de entrada para que a sessão fique inativa porque não há dados para empurrar para fora. Verifique se o codificar ou o estado do feed de entrada estão em estado saudável. |
>|**MPE_RTMP_FLV_TAG_TIMESTAMP_INVALID** |
>|Descrição| A marca de tempo para o vídeo ou áudio FLVTag é inválida do codificar RTMP. |
>| Solução sugerida| Preterido. |
>|**MPE_CAPACITY_LIMIT_REACHED** |
>| Descrição|Codificação enviando dados muito rápido. |
>| Solução sugerida|Isto acontece quando o codificar rebenta um grande conjunto de fragmentos num breve período.  Isto pode teoricamente acontecer quando o codificante não consegue empurrar dados durante algum tempo devido a um problema de rede e os dados rebentam quando a rede está disponível. Encontre a razão do registo de codificação ou do registo do sistema. |
>|**Códigos de erro desconhecidos** |
>| Descrição| Estes códigos de erro podem variar de erro de memória a entradas duplicadas no mapa de haxixe. Isto pode acontecer quando o codificar envia um grande conjunto de fragmentos num breve período.  Isto também pode acontecer quando o codificante não conseguiu empurrar os dados durante algum tempo devido a um problema de rede e, em seguida, envia todos os fragmentos atrasados de uma só vez quando a rede fica disponível. |
>|Solução sugerida| Verifique os registos do codificadores.|

## <a name="other-error-codes"></a>Outros códigos de erro

> [!div class="mx-tdCol2BreakAll"]
>| Erro | Informações |Evento rejeitado/desligado|
>|--|--|--|
>|**ERROR_END_OF_MEDIA** ||Sim|
>| Descrição|Isto é um erro geral. ||
>|Solução sugerida| Nenhum.||
>|**MPI_SYSTEM_MAINTENANCE** ||Sim|
>| Descrição|O codificaor desligado devido à atualização do serviço ou à manutenção do sistema. ||
>|Solução sugerida|Certifique-se de que o codificador ativa a ligação automática. Permite que o codificadores se reconecte ao ponto final redundante do evento que não está em manutenção. ||
>|**MPE_BAD_URL_SYNTAX** ||Sim|
>| Descrição|O URL inger está incorretamente formatado. ||
>|Solução sugerida|Certifique-se de que o URL inger está corretamente formatado. Para a RTMP, deve ser `rtmp[s]://hostname:port/live/GUID_APPID/streamname` ||
>|**MPE_CLIENT_TERMINATED_SESSION** ||Sim|
>| Descrição|O codificar desligou a sessão.  ||
>|Solução sugerida|Isto não é um erro. O codificante iniciou a desconexão, incluindo a desconexão graciosa. Se isto for uma desconexão inesperada, verifique os registos do codificadores. |
>|**MPE_INGEST_BITRATE_NOT_MATCH** ||Não|
>| Descrição|A taxa de dados recebida não corresponde à bitrate esperada. ||
>|Solução sugerida|Este é um aviso que acontece quando a taxa de dados recebida é demasiado lenta ou rápida. Verifique o registo do codificação ou o registo do sistema.||
>|**MPE_INGEST_DISCONTINUITY** ||Não|
>| Descrição| Há descontinuidade nos dados recebidos.||
>|Solução sugerida| Este é um aviso de que o codificante deixa cair dados devido a um problema de rede ou a um problema de recursos do sistema. Verifique o registo de codificação ou o registo do sistema. Monitorize também o recurso do sistema (CPU, memória ou rede). Se o CPU do sistema for demasiado elevado, tente baixar o bitrate ou utilize a opção codificação H/W a partir da placa gráfica do sistema.||

## <a name="see-also"></a>Ver também

[Códigos de erro de streaming endpoint (origem)](stream-streaming-endpoint-error-codes-reference.md)

## <a name="next-steps"></a>Passos seguintes

[Tutorial: Stream ao vivo com serviços de mídia](stream-live-tutorial-with-api.md)
