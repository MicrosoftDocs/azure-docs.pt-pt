---
title: Pesquisa de imagens GIF utilizando a API de Pesquisa de Imagem Bing
titleSuffix: Azure Cognitive Services
description: A API de Pesquisa de Imagem Bing permite-lhe também pesquisar em toda a Web as imagens .gif mais relevantes.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: conceptual
ms.date: 04/24/2018
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: d1fcac891db240def2e7bbdcb45b45caf7f49a82
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2021
ms.locfileid: "96341908"
---
# <a name="search-for-gif-images"></a>Pesquisa de imagens GIF 

> [!WARNING]
> As APIs de Pesquisa de Bing estão a mover-se dos Serviços Cognitivos para os Serviços de Pesquisa Bing. A partir **de 30 de outubro de 2020,** quaisquer novos casos de Bing Search devem ser adquir-se na sequência do processo [aqui](/bing/search-apis/bing-web-search/create-bing-search-service-resource)documentado.
> Bing Search APIs aforados usando Serviços Cognitivos será suportado durante os próximos três anos ou até o final do seu Contrato de Empresa, o que acontecer primeiro.
> Para obter instruções de migração, consulte [os Serviços de Busca Bing.](/bing/search-apis/bing-web-search/create-bing-search-service-resource)

A API de Pesquisa de Imagem Bing permite-lhe também pesquisar em toda a Web as imagens .gif mais relevantes.  Os desenvolvedores podem integrar gifs envolventes em vários cenários de conversação. 

O URL seguinte é uma consulta para imagens animadas .gif.
```
https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=interesting&imageType=AnimatedGif&mkt=en-us
```
O parâmetro [q](/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#query) especifica os termos de pesquisa.  A consulta anterior também especifica `animatedGif` a utilização do parâmetro do filtro [de imagemType.](/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#imagetype)

Para ver exemplos de resultados, utilize o seguinte URL para pesquisar bing.com.
```
https://www.bing.com/images/search?q=interesting&qft=%20filterui%3Aphoto-animatedgif

```
## <a name="query-parameters"></a>Parâmetros de consulta

Para obter mais informações sobre parâmetros e opções de consulta, consulte a [referência API de pesquisa de imagem](/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#query-parameters). Segue-se um exemplo sob o título [Exemplo de pesquisa de gif animado usando Java](#gifExample).

## <a name="tips-and-suggestions"></a>Dicas e sugestões

- Pode especificar [os parâmetros maxFileSize](/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#maxfilesize) e [minFileSize.](/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#minfilesize) Recomendamos que se estabeleça o maxFileSize=20000000 como a maioria dos gifs no nosso índice estão abaixo de 2MB.  Isto também ajuda a controlar o tamanho dos dados se a largura de banda é uma preocupação, como em cenários celulares móveis.
- Para ajudar a melhorar o desempenho percebido, carregue a miniatura primeiro antes de carregar o url de origem.  
- Para experiência de primeira execução ou página de aterragem onde ainda não tenha uma consulta de utilizador, experimente usar as nossas pesquisas gif trending para ajudar nas [imagens de tendência API](trending-images.md).
- Existem três configurações para o parâmetro [safeSearch.](/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#safesearch)  A `strict` opção bloqueia o conteúdo adulto.
- Consulte [o mkt](./language-support.md) para obter a lista completa de idiomas e locais suportados.
- *AnimatedGifHttps* apenas devolve imagens gif animadas que são de um endereço https. Para a segurança, muitas aplicações requerem ligação a links web externos em https. Por exemplo, a Apple App Store requer ligação a serviços web em HTTPS, que encripta os dados dos utilizadores seguros durante o transporte.

<a name="gifExample"></a>

## <a name="example-search-for-animated-gif-using-java"></a>Pesquisa de exemplo para gif animado usando Java

As seguintes pesquisas de URL para imagens animadas .gif: `q=interesting`
```
https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=interesting&imageType=AnimatedGif&mkt=en-us

```
Como mostrado no exemplo seguinte, a consulta URL requer o cabeçalho [Ocp-Apim-Subscrição-Chave.](/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#headers)

O exemplo java seguinte constrói e envia o pedido.

```
package gifSearch;
import java.net.*;
import java.util.*;
import java.io.*;
import javax.net.ssl.HttpsURLConnection;

import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

/*
 * Gson: https://github.com/google/gson
 * Maven info:
 *     groupId: com.google.code.gson
 *     artifactId: gson
 *     version: 2.8.1
 *
 * Once you have compiled or downloaded gson-2.8.1.jar, assuming you have placed it in the
 * same folder as this file (BingImageSearch.java), you can compile and run this program at
 * the command line as follows.
 *
 * javac GIFsearch.java -classpath .;gson-2.8.1.jar -encoding UTF-8
 * java -cp .;gson-2.8.1.jar GIFsearch
 */


public class GIFsearch {

    // Replace the subscriptionKey string value with your valid subscription key.
    static String subscriptionKey = "YOUR-ACCESS-KEY";

    static String host = "https://api.cognitive.microsoft.com";
    static String path = "/bing/v7.0/images/search";

    static String searchTerm = "interesting";

    public static SearchResults SearchImages (String searchQuery) throws Exception {
        // construct URL of search request (endpoint + query string)
        URL url = new URL(host + path + "?q=" +  URLEncoder.encode(searchQuery, "UTF-8") + "&imageType=AnimatedGif&mkt=en-us");
        HttpsURLConnection connection = (HttpsURLConnection)url.openConnection();
        connection.setRequestProperty("Ocp-Apim-Subscription-Key", subscriptionKey);

        // receive JSON body
        InputStream stream = connection.getInputStream();
        String response = new Scanner(stream).useDelimiter("\\A").next();

        // construct result object for return
        SearchResults results = new SearchResults(new HashMap<String, String>(), response);

        // extract Bing-related HTTP headers
        Map<String, List<String>> headers = connection.getHeaderFields();
        for (String header : headers.keySet()) {
            if (header == null) continue;      // may have null key
            if (header.startsWith("BingAPIs-") || header.startsWith("X-MSEdge-")) {
                results.relevantHeaders.put(header, headers.get(header).get(0));
            }
        }

        stream.close();
        return results;
    }

    // pretty-printer for JSON; uses GSON parser to parse and re-serialize
    public static String prettify(String json_text) {
        JsonParser parser = new JsonParser();
        JsonObject json = parser.parse(json_text).getAsJsonObject();
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }

    public static void main (String[] args) {
        if (subscriptionKey.length() != 32) {
            System.out.println("Invalid Bing Search API subscription key!");
            System.out.println("Please paste yours into the source code.");
            System.exit(1);
        }

        try {
            System.out.println("Searching the Web for: " + searchTerm);

            SearchResults result = SearchImages(searchTerm);

            System.out.println("\nRelevant HTTP Headers:\n");
            for (String header : result.relevantHeaders.keySet())
                System.out.println(header + ": " + result.relevantHeaders.get(header));

            System.out.println("\nJSON Response:\n");
            System.out.println(prettify(result.jsonResponse));
        }
        catch (Exception e) {
            e.printStackTrace(System.out);
            System.exit(1);
        }
    }

}

//Container class for search results encapsulates relevant headers and JSON data
class SearchResults{
 HashMap<String, String> relevantHeaders;
 String jsonResponse;
 SearchResults(HashMap<String, String> headers, String json) {
     relevantHeaders = headers;
     jsonResponse = json;
 }
}

```

## <a name="results"></a>Resultados
O código obtém os seguintes resultados como objetos JSON:

```json
    {
      "webSearchUrl": "https://www.bing.com/images/search?view\u003ddetai...",
      "name": "Very Interesting GIF - Thats Very Interesting - ...",
      "thumbnailUrl": "https://tse1.mm.bing.net/th?id\u003dOIP.yJX6Vz345JPK...",
      "datePublished": "2017-03-12T01:35:00.0000000Z",
      "contentUrl": "https://media.contoso.co/images/c895fa573df8e493ca8d0dec7d93b/raw",
      "hostPageUrl": "https://www.contoso.co/view/thats-very-interesting-christi...",
      "contentSize": "1295633 B",
      "encodingFormat": "animatedgif",
      "hostPageDisplayUrl": "https://www.contoso.co/view/thats-very-christian...",
      "width": 440,
      "height": 186,
      "thumbnail": {
        "width": 474,
        "height": 200
      },
      "imageInsightsToken": "ccid_yJX6Vz34*mid_9FF0FFA42AADA1357F042443D2103B40EA...",
      "insightsMetadata": {
        "recipeSourcesCount": 0,
        "bestRepresentativeQuery": {
          "text": "That\u0027s Very Interesting",
          "displayText": "That\u0027s Very Interesting",
          "webSearchUrl": "https://www.bing.com/images/search?q\u003dThat..."
        },
        "pagesIncludingCount": 19,
        "availableSizesCount": 2
      },
      "imageId": "9FF0FFA42AADA1357F042443D21030EAAA225F",
      "accentColor": "62452D"
    },

```

## <a name="next-steps"></a>Passos seguintes
- [Início rápido do C#](quickstarts/csharp.md)
- [Pedido de pesquisa de imagem tutorial de uma página](tutorial-bing-image-search-single-page-app.md)