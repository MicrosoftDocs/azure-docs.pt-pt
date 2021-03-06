---
title: Suporte de contentor
titleSuffix: Azure Cognitive Services
description: Saiba como criar um recurso de instância de recipiente Azure a partir do CLI Azure.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 32b00e031e3cf865093c267084117a8704b6e272
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/20/2021
ms.locfileid: "107800298"
---
## <a name="create-an-azure-container-instance-resource-from-the-azure-cli"></a>Criar um recurso de instância de contentores Azure a partir do CLI Azure

O YAML abaixo define o recurso Azure Container Instance. Copie e cole o conteúdo num novo ficheiro, nomeado `my-aci.yaml` e substitua os valores comentados pelos seus próprios. Consulte o [formato do modelo][template-format] para yaML válido. Consulte os [repositórios e imagens][repositories-and-images] dos recipientes para ver os nomes de imagem disponíveis e o respetivo repositório. Para obter mais informações sobre a referência YAML para instâncias de contentores, consulte [referência YAML: Instâncias do Contentor Azure][aci-yaml-ref].

```YAML
apiVersion: 2018-10-01
location: # < Valid location >
name: # < Container Group name >
properties:
  imageRegistryCredentials: # This is only required if you are pulling a non-public image that requires authentication to access. For example Text Analytics for health.
  - server: containerpreview.azurecr.io
    username: # < The username for the preview container registry >
    password: # < The password for the preview container registry >
  containers:
  - name: # < Container name >
    properties:
      image: # < Repository/Image name >
      environmentVariables: # These env vars are required
        - name: eula
          value: accept
        - name: billing
          value: # < Service specific Endpoint URL >
        - name: apikey
          value: # < Service specific API key >
      resources:
        requests:
          cpu: 4 # Always refer to recommended minimal resources
          memoryInGb: 8 # Always refer to recommended minimal resources
      ports:
        - port: 5000
  osType: Linux
  volumes: # This node, is only required for container instances that pull their model in at runtime, such as LUIS.
  - name: aci-file-share
    azureFile:
      shareName: # < File share name >
      storageAccountName: # < Storage account name>
      storageAccountKey: # < Storage account key >
  restartPolicy: OnFailure
  ipAddress:
    type: Public
    ports:
    - protocol: tcp
      port: 5000
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

> [!NOTE]
> Nem todos os locais têm a mesma disponibilidade de CPU e Memória. Consulte a tabela [de localização e recursos][location-to-resource] para a listagem dos recursos disponíveis para contentores por local e so.

Vamos confiar no ficheiro YAML que criámos para o [`az container create`][azure-container-create] comando. A partir do CLI Azure, execute o `az container create` comando substituindo o `<resource-group>` seu. Além disso, para assegurar valores dentro de uma implementação YAML consulte [valores seguros][secure-values].

```azurecli
az container create -g <resource-group> -f my-aci.yaml
```

A saída do comando é `Running...` se válida, após algum tempo a saída muda para uma cadeia JSON que representa o recurso ACI recém-criado. A imagem do recipiente é mais do que provável que não esteja disponível por um tempo, mas o recurso está agora implantado.

> [!TIP]
> Preste muita atenção às localizações das ofertas do Serviço Cognitivo Azure de pré-visualização pública, uma vez que o YAML terá de ser ajustado em conformidade para corresponder à localização.

[azure-container-create]: /cli/azure/container#az_container_create
[template-format]: /azure/templates/Microsoft.ContainerInstance/2018-10-01/containerGroups#template-format
[aci-yaml-ref]: ../../../container-instances/container-instances-reference-yaml.md
[repositories-and-images]: ../container-image-tags.md
[location-to-resource]: ../../../container-instances/container-instances-region-availability.md
[secure-values]: ../../../container-instances/container-instances-environment-variables.md#secure-values
