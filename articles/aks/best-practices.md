---
title: Boas práticas para o Serviço Azure Kubernetes (AKS)
description: Recolha do operador de cluster e das melhores práticas do promotor para construir e gerir aplicações no Serviço Azure Kubernetes (AKS)
services: container-service
ms.topic: article
ms.date: 03/09/2021
ms.openlocfilehash: f4bd109ce6827de26b1649380b6f6a2ba7c10e17
ms.sourcegitcommit: 5f482220a6d994c33c7920f4e4d67d2a450f7f08
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/08/2021
ms.locfileid: "107105906"
---
# <a name="cluster-operator-and-developer-best-practices-to-build-and-manage-applications-on-azure-kubernetes-service-aks"></a>Operador de cluster e desenvolvedor de boas práticas para construir e gerir aplicações no Serviço Azure Kubernetes (AKS)

A construção e execução de aplicações com sucesso no Serviço Azure Kubernetes (AKS) requerem compreensão e implementação de algumas considerações fundamentais, incluindo:
* Recursos multi-arrendamento e agendamento.
* Cluster e segurança de casulo.
* Continuidade de negócios e recuperação de desastres. 


O grupo de produtos AKS, equipas de engenharia e equipas de campo (incluindo os cintos pretos globais [GBBs]) contribuíram para, escreveu, e agrupou as seguintes boas práticas e artigos conceptuais. O seu objetivo é ajudar os operadores de cluster e os desenvolvedores a compreender as considerações acima e a implementar as características apropriadas.


## <a name="cluster-operator-best-practices"></a>As melhores práticas do operador do cluster

Como operador de cluster, trabalhe em conjunto com os proprietários de aplicações e desenvolvedores para compreender as suas necessidades. Em seguida, pode utilizar as seguintes boas práticas para configurar os seus clusters AKS conforme necessário.

**Arquitetura multi-inquilino**

* [Melhores práticas de isolamento de cluster](operator-best-practices-cluster-isolation.md)
    * Inclui componentes do núcleo de vários arrendamentos e isolamento lógico com espaços de nome.
* [Melhores práticas de funcionalidades básicas do Scheduler](operator-best-practices-scheduler.md)
    * Inclui a utilização de quotas de recursos e orçamentos de disrupção de cápsulas.
* [Melhores práticas de funcionalidades avançadas do Scheduler](operator-best-practices-advanced-scheduler.md)
    * Inclui a utilização de manchas e tolerâncias, seletores de nó e afinidade, e afinidade inter-pod e anti-afinidade.
* [Melhores práticas de autorização e autenticação](operator-best-practices-identity.md)
    * Inclui integração com o Azure Ative Directory, utilizando o controlo de acesso baseado em funções de Kubernetes (Kubernetes RBAC), utilizando a Azure RBAC, e identidades de pod.

**Segurança**

* [Melhores práticas para atualizações e segurança do cluster](operator-best-practices-cluster-security.md)
    * Inclui garantir o acesso ao servidor API, limitar o acesso ao contentor e gerir upgrades e reboots de nó.
* [Melhores práticas para gestão e segurança de imagem de contentores](operator-best-practices-container-image-management.md)
    * Inclui a segurança da imagem e dos tempos de execução e as construções automatizadas nas atualizações de imagem base.
* [Melhores práticas para a segurança do pod](developer-best-practices-pod-security.md)
    * Inclui garantir o acesso aos recursos, limitar a exposição credencial e usar identidades de vagem e cofres digitais.

**Rede e armazenamento**

* [Melhores práticas para a conectividade da rede](operator-best-practices-network.md)
    * Inclui diferentes modelos de rede, utilizando firewalls de entrada e aplicações web (WAF), e segurança de acesso SSH ao nó.
* [Melhores práticas para armazenamento e cópias de segurança](operator-best-practices-storage.md)
    * Inclui a escolha do tipo de armazenamento e tamanho adequados do nó, volumes de fornecimento dinâmico e backups de dados.

**Funcionando cargas de trabalho prontas para a empresa**

* [Melhores práticas para a continuidade do negócio e recuperação de desastres](operator-best-practices-multi-region.md)
    * Inclui o uso de pares de regiões, múltiplos clusters com Azure Traffic Manager, e geo-replicação de imagens de contentores.

## <a name="developer-best-practices"></a>Melhores práticas do desenvolvedor

Como desenvolvedor ou proprietário de aplicações, pode simplificar a sua experiência de desenvolvimento e definir necessidades de desempenho de aplicação necessárias.

* [Melhores práticas para os programadores de aplicações gerirem os recursos](developer-best-practices-resource-management.md)
    * Inclui a definição de pedidos e limites de recursos de pod, configurar ferramentas de desenvolvimento e verificar se existem problemas de aplicação.
* [Melhores práticas para a segurança do pod](developer-best-practices-pod-security.md)
    * Inclui garantir o acesso aos recursos, limitar a exposição credencial e usar identidades de vagem e cofres digitais.

## <a name="kubernetes--aks-concepts"></a>Conceitos Kubernetes / AKS

Para ajudar a compreender algumas das funcionalidades e componentes destas melhores práticas, você também pode ver os seguintes artigos conceptuais para clusters no Serviço Azure Kubernetes (AKS):

* [Conceitos fundamentais de Kubernetes](concepts-clusters-workloads.md)
* [Acesso e identidade](concepts-identity.md)
* [Conceitos de segurança](concepts-security.md)
* [Conceitos de rede](concepts-network.md)
* [Opções de armazenamento](concepts-storage.md)
* [Opções de escala](concepts-scale.md)

## <a name="next-steps"></a>Passos seguintes

Se precisar de começar com a AKS, siga um dos quickstarts para implantar um cluster Azure Kubernetes Service (AKS) utilizando o portal [Azure CLI](kubernetes-walkthrough.md) ou [Azure](kubernetes-walkthrough-portal.md).
