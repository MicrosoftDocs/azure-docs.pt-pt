---
title: Executar scripts num Azure Windows VM
description: Este tópico descreve como executar scripts dentro de uma máquina virtual do Windows
services: automation
ms.service: virtual-machines
ms.collection: windows
author: bobbytreed
ms.author: robreed
ms.date: 05/02/2018
ms.topic: how-to
manager: carmonm
ms.openlocfilehash: 7bf62eb2ab8d2ce82ce73e3e8ae26cf303b8ba67
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/20/2021
ms.locfileid: "107765886"
---
# <a name="run-scripts-in-your-windows-vm"></a>Execute scripts no seu Windows VM

Para automatizar tarefas ou problemas de resolução de problemas, poderá ter de executar comandos num VM. O seguinte artigo dá uma breve visão geral das funcionalidades que estão disponíveis para executar scripts e comandos dentro dos seus VMs.

## <a name="custom-script-extension"></a>Extensão de Script Personalizado

A [extensão de script personalizado](../extensions/custom-script-windows.md) é usada principalmente para a configuração de implementação de posts e instalação de software.

* Faça o download e execute scripts em máquinas virtuais Azure.
* Pode ser executado usando modelos de Gestor de Recursos Azure, Azure CLI, REST API, PowerShell ou portal Azure.
* Os ficheiros script podem ser descarregados a partir do armazenamento Azure ou GitHub, ou fornecidos a partir do seu PC quando executados a partir do portal Azure.
* Executar o script PowerShell em máquinas Windows e script Bash em máquinas Linux.
* Útil para configuração de implementação de posts, instalação de software e outras tarefas de configuração ou gestão.

## <a name="run-command"></a>Execute o comando 

A funcionalidade ['Comando de Execução'](run-command.md) permite a gestão de máquinas virtuais e aplicações e resolução de problemas utilizando scripts, e está disponível mesmo quando a máquina não está acessível, por exemplo, se a firewall do hóspede não tiver a porta RDP ou SSH aberta.

* Execute scripts em máquinas virtuais Azure.
* Pode ser executado usando [portal Azure](run-command.md), [REST API,](/rest/api/compute/virtual%20machines%20run%20commands/runcommand) [Azure CLI,](/cli/azure/vm/run-command#az_vm_run_command_invoke)ou [PowerShell](/powershell/module/az.compute/invoke-azvmruncommand)
* Execute rapidamente um script e veja a saída e repita conforme necessário no portal Azure.
* O script pode ser digitado diretamente ou pode executar um dos scripts incorporados.
* Executar o script PowerShell em máquinas Windows e script Bash em máquinas Linux.
* Útil para a gestão de máquinas e aplicações virtuais e para executar scripts em máquinas virtuais que são inacessíveis.

## <a name="hybrid-runbook-worker"></a>Função de Trabalho de Runbook Híbrida

O [Trabalhador De Runbook Híbrido](../../automation/automation-hybrid-runbook-worker.md) fornece máquina geral, aplicação e gestão do ambiente com scripts personalizados do utilizador armazenados numa conta Automation.

* Execute scripts em máquinas Azure e não-Azure.
* Pode ser executado usando o portal Azure, Azure CLI, REST API, PowerShell, webhook.
* Scripts armazenados e geridos numa Conta de Automação.
* Executar powerShell, powerShell Workflow, Python ou gráficos
* Sem limite de tempo no tempo de execução do script.
* Vários scripts podem ser executados simultaneamente.
* A saída completa do script é devolvida e armazenada.
* Histórico de emprego disponível por 90 dias.
* Os scripts podem ser executados como Sistema Local ou com credenciais fornecidas pelo utilizador.
* Requer [instalação manual](../../automation/automation-windows-hrw-install.md)

## <a name="serial-console"></a>Consola de série

A [consola Serial](/troubleshoot/azure/virtual-machines/serial-console-windows) proporciona acesso direto a um VM, semelhante ao facto de ter um teclado ligado ao VM.

* Executar comandos em máquinas virtuais Azure.
* Pode ser executada utilizando uma consola baseada em texto para a máquina no portal Azure.
* Faça login na máquina com uma conta de utilizador local.
* Útil quando o acesso à máquina virtual é necessário independentemente da rede ou do estado do sistema operativo da máquina.

## <a name="next-steps"></a>Passos seguintes

Saiba mais sobre as diferentes funcionalidades que estão disponíveis para executar scripts e comandos dentro dos seus VMs.

* [Extensão de Script Personalizado](../extensions/custom-script-windows.md)
* [Comando de Execução](run-command.md)
* [Função de Trabalho de Runbook Híbrida](../../automation/automation-hybrid-runbook-worker.md)
* [Consola em série](/troubleshoot/azure/virtual-machines/serial-console-windows)