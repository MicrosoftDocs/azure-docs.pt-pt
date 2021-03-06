---
title: Azure App Configuration REST API - versão
description: Páginas de referência para a versão utilizando a API de Configuração de Aplicação Azure
author: AlexandraKemperMS
ms.author: alkemper
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: a869531860942e5a8b2b05212e778aca2170c89b
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "96932426"
---
# <a name="versioning"></a>Controlo de versões

Cada pedido de cliente deve fornecer uma versão API explícita como parâmetro de cadeia de consulta. Por exemplo: `https://{myconfig}.azconfig.io/kv?api-version=1.0`.

`api-version` é expresso no formato SemVer (major.minor). A negociação de alcance ou versão não é apoiada.

Este artigo aplica-se à versão API 1.0.

O seguinte descreve um resumo das possíveis respostas de erro devolvidas pelo servidor quando a versão API solicitada não pode ser correspondida.

## <a name="api-version-unspecified"></a>Versão API não especificada

Este erro ocorre quando um cliente faz um pedido sem fornecer uma versão API.

```http
HTTP/1.1 400 Bad Request
Content-Type: application/problem+json; charset=utf-8
{
  "type": "https://azconfig.io/errors/invalid-argument",
  "title": "API version is not specified",
  "name": "api-version",
  "detail": "An API version is required, but was not specified.",
  "status": 400
}
```

## <a name="unsupported-api-version"></a>Versão API não suportada

Este erro ocorre quando uma versão API solicitada pelo cliente não corresponde a nenhuma das versões API suportadas pelo servidor.

```http
HTTP/1.1 400 Bad Request
Content-Type: application/problem+json; charset=utf-8
{
  "type": "https://azconfig.io/errors/invalid-argument",
  "title": "Unsupported API version",
  "name": "api-version",
  "detail": "The HTTP resource that matches the request URI '{request uri}' does not support the API version '{api-version}'.",
  "status": 400
}
```

## <a name="invalid-api-version"></a>Versão API inválida

Este erro ocorre quando um cliente faz um pedido com uma versão API, mas o valor é mal formado ou não pode ser analisado pelo servidor.

```http
HTTP/1.1 400 Bad Request
Content-Type: application/problem+json; charset=utf-8  
{
  "type": "https://azconfig.io/errors/invalid-argument",
  "title": "Invalid API version",
  "name": "api-version",
  "detail": "The HTTP resource that matches the request URI '{request uri}' does not support the API version '{api-version}'.",
  "status": 400
}
```

## <a name="ambiguous-api-version"></a>Versão API ambígua

Este erro ocorre quando um cliente solicita uma versão API que seja ambígua ao servidor (por exemplo, vários valores diferentes).

```http
HTTP/1.1 400 Bad Request
Content-Type: application/problem+json; charset=utf-8
{
  "type": "https://azconfig.io/errors/invalid-argument",
  "title": "Ambiguous API version",
  "name": "api-version",
  "detail": "The following API versions were requested: {comma separated api versions}. At most, only a single API version may be specified. Please update the intended API version and retry the request.",
  "status": 400
}
```
