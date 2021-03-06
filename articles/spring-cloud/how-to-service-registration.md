---
title: Automatizar registo de serviço e descoberta
description: Saiba como automatizar a descoberta e o registo do serviço usando o Registo de Serviços de Nuvem de primavera
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 09/08/2020
ms.author: brendm
ms.custom: devx-track-java
zone_pivot_groups: programming-languages-spring-cloud
ms.openlocfilehash: e2ad6d8e3d81497eb7ebe612d01e60415790faea
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2021
ms.locfileid: "104878666"
---
# <a name="discover-and-register-your-spring-cloud-services"></a>Descubra e registe os seus serviços cloud de primavera

A Service Discovery é um requisito fundamental para uma arquitetura baseada em microservices.  Configurar cada cliente manualmente leva tempo e introduz a possibilidade de erro humano.  O Registo de Serviços de Nuvem de Azure Spring resolve este problema.  Uma vez configurado, um servidor de Registo de Serviço controlará o registo do serviço e a descoberta dos microserviços da sua aplicação. O servidor de Registo de Serviços mantém um registo dos microserviços implantados, permite o equilíbrio de carga do lado do cliente e dissocia os prestadores de serviços dos clientes sem depender de DNS.

::: zone pivot="programming-language-csharp"
Para obter informações sobre como configurar o registo de serviço para uma aplicação Steeltoe, consulte [preparar uma aplicação Java Spring para implantação em Azure Spring Cloud](how-to-prepare-app-deployment.md).
::: zone-end
::: zone pivot="programming-language-java"

## <a name="register-your-application-using-spring-cloud-service-registry"></a>Registe a sua aplicação usando o Registo de Serviços de Nuvem de primavera

Antes da sua aplicação poder gerir o registo e descoberta do serviço usando o Registo de Serviços da Nuvem de primavera, várias dependências devem ser incluídas no ficheiro *pom.xml* da aplicação.
Inclua dependências para *o* seu *cliente depom.xml* 

```xml
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-starter-azure-spring-cloud-client</artifactId>
        <version>2.1.0</version>
    </dependency>
```

## <a name="update-the-top-level-class"></a>Atualize a classe de nível superior

Finalmente, adicionamos uma anotação à classe de nível superior da sua aplicação

 ```java
    package foo.bar;

    import org.springframework.boot.SpringApplication;
    import org.springframework.cloud.client.SpringCloudApplication;

    @SpringCloudApplication
    public class DemoApplication {
        public static void main(String... args) {
            SpringApplication.run(DemoApplication.class, args);
        }
    }
 ```

O ponto final do servidor do serviço de cloud spring cloud será injetado como uma variável ambiental na sua aplicação.  Os microserviços poderão agora registar-se no servidor do Registo de Serviços e descobrir outros microserviços dependentes.

Note que pode levar alguns minutos para que as alterações se propaguem do servidor para todos os microserviços.
::: zone-end
