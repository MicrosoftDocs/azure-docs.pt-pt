---
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 09/18/2019
ms.author: cephalin
ms.openlocfilehash: ed53cb60f6fb3bca93900941a8a6a53bed99d779
ms.sourcegitcommit: d40ffda6ef9463bb75835754cabe84e3da24aab5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/07/2021
ms.locfileid: "107073493"
---
Quando clona a configuração a partir de outra ranhura de implantação, a configuração clonada é editável. Alguns elementos de configuração seguem o conteúdo através de uma troca (não específica para a ranhura), enquanto outros elementos de configuração permanecem na mesma ranhura após uma troca (específica slot). As listas que se seguem mostram as definições que mudam quando troca slots.

**Definições que são trocadas:**

* Configurações gerais, tais como versão-quadro, tomadas web de 32/64 bits
* Definições de aplicativos (pode ser configurado para manter uma ranhura)
* Cordas de ligação (podem ser configuradas para se colarem a uma ranhura)
* Mapeamentos de manipulador
* Certificados públicos
* Conteúdo da WebJobs
* Ligações híbridas *
* Pontos finais de serviço *
* Rede de Entrega de Conteúdos Azure *

As características marcadas com um asterisco (*) estão planeadas para não seremgrafadas. 

**Configurações que não são trocadas:**

* Pontos finais de publicação
* Nomes de domínio personalizados
* Certificados não públicos e definições de TLS/SSL
* Configurações de escala
* Agendadores WebJobs
* Restrições ip
* Sempre ligado
* Definições de diagnóstico
* Partilha de recursos transversais à origem (CORS)
* Integração da rede virtual
* Mapeamentos de caminhos
* Configurações que terminam com o sufixo _EXTENSION_VERSION

> [!NOTE]
> Para tornar estas definições permutáveis, adicione a definição da aplicação `WEBSITE_OVERRIDE_PRESERVE_DEFAULT_STICKY_SLOT_SETTINGS` em cada ranhura da aplicação e defina o seu valor para ou `0` `false` . Estas definições são todas permutáveis ou não são nada trocadas. Não podes fazer apenas algumas definições trocadas e não as outras.

> Algumas definições de aplicações que se aplicam a definições não mapeadas também não são trocadas. Por exemplo, uma vez que as definições de diagnóstico não são trocadas, as definições de aplicações relacionadas como `WEBSITE_HTTPLOGGING_RETENTION_DAYS` e também não são `DIAGNOSTICS_AZUREBLOBRETENTIONDAYS` trocadas, mesmo que não apareçam como configurações de slot.
>
