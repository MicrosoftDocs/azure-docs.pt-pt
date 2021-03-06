---
title: Criar e configurar um plano de proteção Azure DDoS utilizando o Azure CLI
description: Saiba como criar um Plano de Proteção DDoS utilizando o Azure CLI
services: ddos-protection
documentationcenter: na
author: aletheatoh
ms.service: ddos-protection
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/28/2020
ms.author: yitoh
ms.openlocfilehash: 8a8da50dc703d59dc16b5cb6253d39aeb33fd76d
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/20/2021
ms.locfileid: "107777640"
---
# <a name="quickstart-create-and-configure-azure-ddos-protection-standard-using-azure-cli"></a>Quickstart: Criar e configurar norma de proteção Azure DDoS usando Azure CLI

Começa com o Azure DDoS Protection Standard utilizando o Azure CLI. 

Um plano de proteção DDoS define um conjunto de redes virtuais que têm o padrão de proteção DDoS ativado, através de subscrições. Pode configurar um plano de proteção DDoS para a sua organização e ligar redes virtuais de várias subscrições ao mesmo plano. 

Neste arranque rápido, irá criar um plano de proteção DDoS e ligá-lo a uma rede virtual. 

## <a name="prerequisites"></a>Pré-requisitos

- Uma conta Azure com uma subscrição ativa. [Crie uma conta gratuita.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Azure CLI instalado localmente ou Azure Cloud Shell

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar o CLI localmente, este arranque rápido requer a versão 2.0.28 ou posterior do Azure CLI. Para localizar a versão, execute `az --version`. Se precisar de instalar ou atualizar, veja [Instalar a CLI do Azure]( /cli/azure/install-azure-cli).

## <a name="create-a-ddos-protection-plan"></a>Criar um plano de proteção DDoS

Em Azure, aloca recursos relacionados a um grupo de recursos. Pode utilizar um grupo de recursos existente ou criar um novo.

Para criar um grupo de recursos, utilize [o grupo AZ criar](/cli/azure/group#az_group_create). Neste exemplo, vamos nomear o nosso grupo de recursos _MyResourceGroup_ e usar a localização _dos EUA orientais:_

```azurecli-interactive
az group create \
    --name MyResourceGroup \
    --location eastus
```

Agora crie um plano de proteção DDoS chamado _MyDdosProtectionPlan:_

```azurecli-interactive
az network ddos-protection create \
    --resource-group MyResourceGroup \
    --name MyDdosProtectionPlan
```

## <a name="enable-ddos-protection-for-a-virtual-network"></a>Ativar a proteção DDoS para uma rede virtual

### <a name="enable-ddos-protection-for-a-new-virtual-network"></a>Ativar a proteção DDoS para uma nova rede virtual

Pode ativar a proteção DDoS ao criar uma rede virtual. Neste exemplo, vamos nomear a nossa rede virtual _MyVnet:_ 

```azurecli-interactive
az network vnet create \
    --resource-group MyResourceGroup \
    --name MyVnet \
    --location eastus \
    --ddos-protection true
    --ddos-protection-plan MyDdosProtectionPlan
```

Não é possível mover uma rede virtual para outro grupo de recursos ou subscrição quando o DDoS Standard estiver ativado para a rede virtual. Se precisar de mover uma rede virtual com o DDoS Standard ativado, desative primeiro a DDoS Standard, mova a rede virtual e, em seguida, ative a norma DDoS. Após a mudança, os limiares de política afinados automaticamente para todos os endereços IP públicos protegidos na rede virtual são reiniciados.

### <a name="enable-ddos-protection-for-an-existing-virtual-network"></a>Ativar a proteção DDoS para uma rede virtual existente

Ao [criar um plano de proteção DDoS,](#create-a-ddos-protection-plan)pode associar uma ou mais redes virtuais ao plano. Para adicionar mais do que uma rede virtual, basta listar os nomes ou IDs, separados pelo espaço. Neste exemplo, vamos adicionar _MyVnet:_

```azurecli-interactive
az group create \
    --name MyResourceGroup \
    --location eastus

az network ddos-protection create \
    --resource-group MyResourceGroup \
    --name MyDdosProtectionPlan
    --vnets MyVnet
```

Em alternativa, pode ativar a proteção DDoS para uma determinada rede virtual:

```azurecli-interactive
az network vnet update \
    --resource-group MyResourceGroup \
    --name MyVnet \
    --ddos-protection true
    --ddos-protection-plan MyDdosProtectionPlan
```

## <a name="validate-and-test"></a>Validar e testar

Primeiro, verifique os detalhes do seu plano de proteção DDoS:

```azurecli-interactive
az network ddos-protection show \
    --resource-group MyResourceGroup \
    --name MyDdosProtectionPlan
```

Verifique se o comando devolve os detalhes corretos do seu plano de proteção DDoS.

## <a name="clean-up-resources"></a>Limpar os recursos

Podes ficar com os teus recursos para o próximo tutorial. Se já não for necessário, elimine o grupo de recursos _MyResourceGroup._ Quando elimina o grupo de recursos, também elimina o plano de proteção DDoS e todos os seus recursos relacionados. 

Para eliminar o grupo de recursos utilize [o grupo Az eliminar:](/cli/azure/group#az_group_delete)

```azurecli-interactive
az group delete \
--name MyResourceGroup 
```

Atualizar uma determinada rede virtual para desativar a proteção DDoS:

```azurecli-interactive
az network vnet update \
    --resource-group MyResourceGroup \
    --name MyVnet \
    --ddos-protection false
    --ddos-protection-plan ""
```

Se pretender eliminar um plano de proteção DDoS, tem primeiro de dissociar todas as redes virtuais do mesmo. 

## <a name="next-steps"></a>Passos seguintes

Para aprender a visualizar e configurar a telemetria para o seu plano de proteção DDoS, continue para os tutoriais.

> [!div class="nextstepaction"]
> [Ver e configurar telemetria de proteção contra DDoS](telemetry.md)
