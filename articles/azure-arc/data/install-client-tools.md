---
title: Instalar ferramentas de cliente
description: Instale azdata, kubectl, Azure CLI, psql, Azure Data Studio (Insiders) e a extensão arc para Azure Data Studio
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 6f42f712ecca77c00020304b63f5a1b0dbd77ad0
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/20/2021
ms.locfileid: "102172325"
---
# <a name="install-client-tools-for-deploying-and-managing-azure-arc-enabled-data-services"></a>Instalar as ferramentas de cliente para implementar e gerir serviços de dados preparados para o Azure Arc

> [!IMPORTANT]
> Se estiver a atualizar para um novo lançamento mensal, certifique-se de atualizar também para a versão mais recente do Azure Data Studio, a [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)] ferramenta e as [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)] extensões Azure Arc para Azure Data Studio.

Este documento percorre os passos para instalar o [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)] Azure Data Studio, Azure CLI (az) e a ferramenta Kubernetes CLI (kubectl) na sua máquina cliente.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="tools-for-creating-and-managing-azure-arc-enabled-data-services"></a>Ferramentas para criar e gerir serviços de dados aZure Arc 

A tabela que se segue lista as ferramentas comuns necessárias para criar e gerir os serviços de dados do Azure Arc e como instalar essas ferramentas:

| Ferramenta | Obrigatório | Descrição | Instalação |
|---|---|---|---|
| [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)] | Yes | Ferramenta de linha de comando para instalar e gerir um grande cluster de dados. [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)] também inclui um utilitário de linha de comando para ligar e consultar instâncias Azure SQL e SQL Server e servidores Postgres usando os comandos `azdata sql query` (executar uma única consulta a partir da linha de comando), `azdata sql shell` (uma concha interativa) `azdata postgres query` e `azdata postgres shell` . | [Instalar](/sql/azdata/install/deploy-install-azdata?toc=/azure/azure-arc/data/toc.json&bc=/azure/azure-arc/data/breadcrumb/toc.json) |
| Azure Data Studio | Yes | Ferramenta de experiência rica para conectar e consultar uma variedade de bases de dados, incluindo Azure SQL, SQL Server, PostrgreSQL e MySQL. As extensões ao Azure Data Studio proporcionam uma experiência de administração para serviços de dados habilitados a Azure Arc. | [Instalar](/sql/azure-data-studio/download-azure-data-studio) |
| [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)] extensão para Azure Data Studio | Yes | Extensão para Azure Data Studio que será instalada [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)] se ainda não o tiver.| Instale a partir da galeria de extensões no Azure Data Studio.|
| Extensão Azure Arc para Azure Data Studio | Yes | Extensão para Azure Data Studio que proporciona uma experiência de gestão para serviços de dados ativados a Azure Arc. Existe uma dependência da extensão para o [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)] Azure Data Studio. | Instale a partir da galeria de extensões no Azure Data Studio.|
| Extensão Pós-SQL no Azure Data Studio | No | Extensão PostgreSQL para Azure Data Studio que fornece capacidades de gestão para PostgreSQL. | <!--{need link} [Install](../azure-data-studio/data-virtualization-extension.md) --> Instale a partir da galeria de extensões no Azure Data Studio.|
| Azure CLI (az)<sup>1</sup> | Yes | Interface moderna de linha de comando para gerir serviços Azure. Usado com implementações AKS e para carregar Azure Arc permitiu o inventário de serviços de dados e dados de faturação para a Azure. (Mais[informações).](/cli/azure/) | [Instalar](/cli/azure/install-azure-cli) |
| Kubernetes CLI (kubectl)<sup>2</sup> | Yes | Ferramenta de linha de comando para gerir o cluster Kubernetes[(Mais informações).](https://kubernetes.io/docs/tasks/tools/install-kubectl/) | [Janelas](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-with-powershell-from-psgallery) \| [Linux](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-using-native-package-management) |
| caracóis <sup>3</sup> | Necessário para alguns scripts de amostra. | Ferramenta de linha de comando para transferência de dados com URLs. | [Janelas](https://curl.haxx.se/windows/) \| Linux: instalar pacote de caracóis |
| oc | Obrigatório para as implementações OpenShift e Azure Redhat OpenShift. |`oc` é a interface da linha de comando Open Shift (CLI). | [Instalação do CLI](https://docs.openshift.com/container-platform/4.4/cli_reference/openshift_cli/getting-started-cli.html#installing-the-cli)



<sup>1</sup> Deve utilizar a versão 2.0.4 ou posterior do Azure CLI. Corra `az --version` para encontrar a versão, se necessário.

<sup>2</sup> Deve utilizar a `kubectl` versão 1.13 ou posterior. Além disso, a versão de `kubectl` deve ser mais ou menos uma versão menor do seu cluster Kubernetes. Se pretender instalar uma versão específica no `kubectl` cliente, consulte [instalar binário via `kubectl` curl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-binary-using-curl) (no Windows 10, utilize cmd.exe e não o Windows PowerShell para executar o curl).

<sup>3</sup> Se estiver a utilizar o PowerShell, o caracol é um pseudónimo do Invoke-WebRequest cmdlet.

## <a name="next-steps"></a>Passos seguintes

[Criar o controlador de dados Azure Arc](create-data-controller.md)