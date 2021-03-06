---
title: Use o servidor DNS personalizado
titleSuffix: Azure Machine Learning
description: Como configurar um servidor DNS personalizado para trabalhar com um espaço de trabalho de Aprendizagem de Máquinas Azure e ponto final privado.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.reviewer: larryfr
ms.author: jhirono
author: jhirono
ms.date: 04/01/2021
ms.topic: conceptual
ms.custom: how-to, contperf-fy21q3
ms.openlocfilehash: 9021c3f70c9fc053998d1b31271a1ca3b0124b4d
ms.sourcegitcommit: d23602c57d797fb89a470288fcf94c63546b1314
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/01/2021
ms.locfileid: "106169543"
---
# <a name="how-to-use-your-workspace-with-a-custom-dns-server"></a>Como utilizar a área de trabalho com um servidor DNS personalizado

Ao utilizar um espaço de trabalho de aprendizagem automática Azure com um ponto final privado, [existem várias formas de lidar com a resolução do nome DNS](../private-link/private-endpoint-dns.md). Por predefinição, o Azure lida automaticamente com a resolução de nomes para o seu espaço de trabalho e ponto final privado. Se, em vez disso, __utilizar o seu próprio servidor DNS personalizado,__ deve criar manualmente entradas DE DNS ou utilizar reencaminhadores condicional para o espaço de trabalho.

> [!IMPORTANT]
> Este artigo abrange apenas como encontrar os endereços de domínio totalmente qualificados (FQDN) e IP para estas entradas, não fornece informações sobre a configuração dos registos DNS para estes itens. Consulte a documentação do seu software DNS para obter informações sobre como adicionar registos.

## <a name="prerequisites"></a>Pré-requisitos

- Uma Rede Virtual Azure que utiliza [o seu próprio servidor DNS](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).

- Um espaço de trabalho de aprendizagem automática Azure com um ponto final privado. Para obter mais informações, consulte [Criar um espaço de trabalho para aprendizagem de máquinas Azure.](how-to-manage-workspace.md)

- Familiaridade com a utilização [do isolamento da rede durante o treino & inferência](./how-to-network-security-overview.md).

- Familiaridade com [configuração de zona DE DNS de endpoint privado Azure](../private-link/private-endpoint-dns.md)

- Opcionalmente, [Azure CLI](/cli/azure/install-azure-cli) ou [Azure PowerShell](/powershell/azure/install-az-ps).

## <a name="public-regions"></a>Regiões públicas

A lista que se segue contém os nomes de domínio totalmente qualificados (FQDN) utilizados pelo seu espaço de trabalho se estiver numa região pública::

* `<workspace-GUID>.workspace.<region>.cert.api.azureml.ms`
* `<workspace-GUID>.workspace.<region>.api.azureml.ms`
* `ml-<workspace-name, truncated>-<region>-<workspace-guid>.notebooks.azure.net`

    > [!NOTE]
    > O nome do espaço de trabalho para este FQDN pode ser truncado. A truncação é feita para manter `ml-<workspace-name, truncated>-<region>-<workspace-guid>` 63 caracteres.
* `<instance-name>.<region>.instances.azureml.ms`

    > [!NOTE]
    > * As instâncias computacional só podem ser acedidas a partir da rede virtual.
    > * O endereço IP para este FQDN **não** é o IP da instância computacional. Em vez disso, utilize o endereço IP privado do ponto final privado do espaço de trabalho (o IP das `*.api.azureml.ms` entradas.)

## <a name="azure-china-21vianet-regions"></a>Regiões Azure China 21Vianet

As seguintes FQDNs são para as regiões Azure China 21Vianet:

* `<workspace-GUID>.workspace.<region>.cert.api.ml.azure.cn`
* `<workspace-GUID>.workspace.<region>.api.ml.azure.cn`
* `ml-<workspace-name, truncated>-<region>-<workspace-guid>.notebooks.chinacloudapi.cn`

    > [!NOTE]
    > O nome do espaço de trabalho para este FQDN pode ser truncado. A truncação é feita para manter `ml-<workspace-name, truncated>-<region>-<workspace-guid>` 63 caracteres.
* `<instance-name>.<region>.instances.ml.azure.cn`
## <a name="find-the-ip-addresses"></a>Encontre os endereços IP

Para encontrar os endereços IP internos para as FQDNs no VNet, utilize um dos seguintes métodos:

> [!NOTE]
> Os nomes de domínio e endereços IP totalmente qualificados serão diferentes com base na sua configuração. Por exemplo, o valor GUID no nome de domínio será específico para o seu espaço de trabalho.

# <a name="azure-cli"></a>[CLI do Azure](#tab/azure-cli)

```azurecli
az network private-endpoint show --endpoint-name <endpoint> --resource-group <resource-group> --query 'customDnsConfigs[*].{FQDN: fqdn, IPAddress: ipAddresses[0]}' --output table
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell
$workspaceDns=Get-AzPrivateEndpoint -Name <endpoint> -resourcegroupname <resource-group>
$workspaceDns.CustomDnsConfigs | format-table
```

# <a name="azure-portal"></a>[Portal do Azure](#tab/azure-portal)

1. No [portal Azure,](https://portal.azure.com)selecione o seu __espaço de trabalho__ Azure Machine Learning .
1. Na secção __Definições,__ selecione __ligações de ponto final privado .__
1. Selecione o link na coluna __de ponto final privado__ que é visualizada.
1. Uma lista dos nomes de domínio totalmente qualificados (FQDN) e endereços IP para o ponto final privado do espaço de trabalho estão na parte inferior da página.

:::image type="content" source="./media/how-to-custom-dns/private-endpoint-custom-dns.png" alt-text="Lista de FQDNs no portal":::

---

A informação devolvida de todos os métodos é a mesma; uma lista do FQDN e endereço IP privado para os recursos. Segue-se o seguinte exemplo de uma região global de Azure:

| FQDN | Endereço IP |
| ----- | ----- |
| `fb7e20a0-8891-458b-b969-55ddb3382f51.workspace.eastus.api.azureml.ms` | `10.1.0.5` |
| `ml-myworkspace-eastus-fb7e20a0-8891-458b-b969-55ddb3382f51.notebooks.azure.net` | `10.1.0.6` |

> [!IMPORTANT]
> Algumas FQDNs não são mostradas na lista pelo ponto final privado, mas são exigidas pelo espaço de trabalho em Eastus, Southcentralus e Westus2. Estas FQDNs estão listadas na tabela seguinte, e também devem ser adicionadas ao seu servidor DNS e/ou a uma Zona DNS Privada Azure:
>
> * `<workspace-GUID>.workspace.<region>.cert.api.azureml.ms`
> * `<workspace-GUID>.workspace.<region>.experiments.azureml.net`
> * `<workspace-GUID>.workspace.<region>.modelmanagement.azureml.net`
> * `<workspace-GUID>.workspace.<region>.aether.ms`
> * Se tiver uma instância computacional, `<instance-name>.<region>.instances.azureml.ms` use, onde `<instance-name>` está o nome da sua instância de computação. Utilize o endereço IP privado do ponto final privado do espaço de trabalho. A instância computacional só pode ser acedida a partir da rede virtual.
>
> Para todos estes endereços IP, utilize o mesmo endereço que as `*.api.azureml.ms` entradas devolvidas das etapas anteriores.

O quadro que se segue mostra o exemplo dos IPs das regiões Azure China 21Vianet:

| FQDN | Endereço IP |
| ----- | ----- |
| `52882c08-ead2-44aa-af65-08a75cf094bd.workspace.chinaeast2.api.ml.azure.cn` | `10.1.0.5` |
| `ml-mype-pltest-chinaeast2-52882c08-ead2-44aa-af65-08a75cf094bd.notebooks.chinacloudapi.cn` | `10.1.0.6` |
## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre a utilização do Azure Machine Learning com uma rede virtual, consulte a visão geral da [rede virtual.](how-to-network-security-overview.md)

Para obter mais informações sobre a integração de Pontos Finais Privados na sua configuração DNS, consulte [a configuração do DNS do Ponto Final Privado Azure](../private-link/private-endpoint-dns.md).
