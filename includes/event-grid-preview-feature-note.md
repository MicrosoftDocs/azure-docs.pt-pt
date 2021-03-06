---
title: incluir ficheiro
description: incluir ficheiro
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 11/06/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 644669ea27938e385e11f3b1911a23ab30829a95
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/25/2020
ms.locfileid: "96028558"
---
Esta funcionalidade está em pré-visualização. Para a utilizar, tem de instalar uma extensão de pré-visualização ou módulo.

### <a name="install-extension-for-azure-cli"></a>Extensão de instalação para Azure CLI

Para o Azure CLI, você precisa da [extensão da Grade de Eventos](/cli/azure/azure-cli-extensions-list).

Em [CloudShell:](../articles/cloud-shell/quickstart.md)

* Se já instalou a extensão anteriormente, atualize-a `az extension update -n eventgrid`
* Se ainda não instalou a extensão anteriormente, instale-a `az extension add -n eventgrid`

Para uma instalação local:

1. [Instale o Azure CLI](/cli/azure/install-azure-cli). Certifique-se de que tem a versão mais recente, verificando com `az --version` .
1. Desinstalar versões anteriores da extensão `az extension remove -n eventgrid`
1. Instale a `eventgrid` extensão com `az extension add -n eventgrid`

### <a name="install-module-for-powershell"></a>Módulo de instalação para PowerShell

Para o PowerShell, precisa do [módulo AzureRM.EventGrid](https://www.powershellgallery.com/packages/AzureRM.EventGrid/0.4.1-preview).

Em [CloudShell:](../articles/cloud-shell/quickstart-powershell.md)

* Instale o módulo `Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`

Para uma instalação local:

1. Consola Open PowerShell como administrador
1. Instale o módulo `Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`

Se o `-AllowPrerelease` parâmetro não estiver disponível, utilize os seguintes passos:

1. Executar `Install-Module PowerShellGet -Force`
1. Executar `Update-Module PowerShellGet`
1. Feche a consola PowerShell
1. Reiniciar PowerShell como administrador
1. Instale o módulo `Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`