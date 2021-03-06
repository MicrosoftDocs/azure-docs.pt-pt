---
title: Como implementar modelos para instâncias de contentores Azure
titleSuffix: Azure Machine Learning
description: Saiba como implementar os seus modelos de Machine Learning Azure como um serviço web utilizando instâncias de contentores Azure.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: how-to, deploy
ms.author: jordane
author: jpe316
ms.reviewer: larryfr
ms.date: 06/12/2020
ms.openlocfilehash: 845a146d9e3f920f3313a80f1bb8c845cb781f37
ms.sourcegitcommit: 2aeb2c41fd22a02552ff871479124b567fa4463c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/22/2021
ms.locfileid: "107875544"
---
# <a name="deploy-a-model-to-azure-container-instances"></a>Implementar um modelo no Azure Container Instances

Aprenda a usar a Azure Machine Learning para implementar um modelo como serviço web em Instâncias de Contentores Azure (ACI). Utilize instâncias do contentor Azure se uma das seguintes condições for verdadeira:

- É necessário implementar e validar rapidamente o seu modelo. Não é necessário criar recipientes ACI com antecedência. São criados como parte do processo de implantação.
- Estás a testar um modelo que está em desenvolvimento. 

Para obter informações sobre a disponibilidade de quotas e de região para o ACI, consulte quotas e disponibilidade de região para o artigo [instâncias de contentores Azure.](../container-instances/container-instances-quotas.md)

> [!IMPORTANT]
> É altamente aconselhável depurar localmente antes de implementar no serviço web, para mais informações ver [Debug Localmente](./how-to-troubleshoot-deployment-local.md)
>
> Também pode consultar o Azure Machine Learning – [Implementar no Bloco de Notas Local](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/deployment/deploy-to-local)

## <a name="prerequisites"></a>Pré-requisitos

- Uma área de trabalho do Azure Machine Learning. Para obter mais informações, consulte [Criar um espaço de trabalho para aprendizagem de máquinas Azure.](how-to-manage-workspace.md)

- Um modelo de machine learning registado no seu espaço de trabalho. Se não tiver um modelo registado, consulte [como e onde implementar modelos.](how-to-deploy-and-where.md)

- A [extensão Azure CLI para o serviço de aprendizagem automática](reference-azure-machine-learning-cli.md), [Azure Machine Learning Python SDK,](/python/api/overview/azure/ml/intro)ou a extensão do [Código do Estúdio Visual Azure Machine Learning.](tutorial-setup-vscode-extension.md)

- Os snippets de código __Python__ neste artigo assumem que as seguintes variáveis são definidas:

    * `ws` - Prepara-te para o teu espaço de trabalho.
    * `model` - Desa couso para o seu modelo registado.
    * `inference_config` - Definir para a configuração de inferência para o modelo.

    Para obter mais informações sobre a definição destas variáveis, consulte [como e onde implementar modelos.](how-to-deploy-and-where.md)

- Os cortes __do CLI__ neste artigo assumem que criaste um `inferenceconfig.json` documento. Para obter mais informações sobre a criação deste documento, consulte [como e onde implementar modelos.](how-to-deploy-and-where.md)

## <a name="limitations"></a>Limitações

* Ao utilizar as instâncias do Azure Container numa rede virtual, a rede virtual deve estar no mesmo grupo de recursos que o seu espaço de trabalho de Aprendizagem de Máquinas Azure.
* Ao utilizar as instâncias do contentor Azure dentro da rede virtual, o Registo de Contentores Azure (ACR) para o seu espaço de trabalho também não pode estar na rede virtual.

Para obter mais informações, consulte Como garantir o [inferencing com redes virtuais.](how-to-secure-inferencing-vnet.md#enable-azure-container-instances-aci)

## <a name="deploy-to-aci"></a>Implementar para ACI

Para implementar um modelo para Azure Container Instances, crie uma __configuração de implementação__ que descreva os recursos de computação necessários. Por exemplo, número de núcleos e memória. Também precisa de uma __configuração de inferência__, que descreve o ambiente necessário para hospedar o modelo e o serviço web. Para obter mais informações sobre a criação da configuração de inferência, consulte [como e onde implementar modelos.](how-to-deploy-and-where.md)

> [!NOTE]
> * O ACI é adequado apenas para pequenos modelos com menos de 1 GB de tamanho. 
> * Recomendamos a utilização de AKS de nó único para testar modelos maiores.
> * O número de modelos a implementar limita-se a 1.000 modelos por implantação (por contentor). 

### <a name="using-the-sdk"></a>Utilizar o SDK

```python
from azureml.core.webservice import AciWebservice, Webservice
from azureml.core.model import Model

deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)
service = Model.deploy(ws, "aciservice", [model], inference_config, deployment_config)
service.wait_for_deployment(show_output = True)
print(service.state)
```

Para obter mais informações sobre as classes, métodos e parâmetros utilizados neste exemplo, consulte os seguintes documentos de referência:

* [AciWebservice.deploy_configuration](/python/api/azureml-core/azureml.core.webservice.aciwebservice#deploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none--primary-key-none--secondary-key-none--collect-model-data-none--cmk-vault-base-url-none--cmk-key-name-none--cmk-key-version-none-)
* [Modelo.implementar](/python/api/azureml-core/azureml.core.model.model#deploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-)
* [Webservice.wait_for_deployment](/python/api/azureml-core/azureml.core.webservice%28class%29#wait-for-deployment-show-output-false-)

### <a name="using-the-azure-cli"></a>Com a CLI do Azure

Para utilizar o CLI, utilize o seguinte comando. `mymodel:1`Substitua-o pelo nome e versão do modelo registado. `myservice`Substitua-o pelo nome para prestar este serviço:

```azurecli-interactive
az ml model deploy -m mymodel:1 -n myservice -ic inferenceconfig.json -dc deploymentconfig.json
```

[!INCLUDE [deploymentconfig](../../includes/machine-learning-service-aci-deploy-config.md)]

Para obter mais informações, consulte a referência de implantação do [modelo az ml.](/cli/azure/ml/model#az_ml_model_deploy) 

## <a name="using-vs-code"></a>Utilizar o VS Code

Consulte [os seus modelos com o Código VS.](tutorial-train-deploy-image-classification-model-vscode.md#deploy-the-model)

> [!IMPORTANT]
> Não é preciso criar um recipiente ACI para testar com antecedência. Os contentores ACI são criados conforme necessário.

> [!IMPORTANT]
> Anexamos o espaço de trabalho a todos os recursos ACI subjacentes que são criados, todos os nomes ACI do mesmo espaço de trabalho terão o mesmo sufixo. O nome de serviço Azure Machine Learning continuaria a ser o mesmo cliente fornecido "service_name" e todos os utilizadores que enfrentam Azure Machine Learning SDK APIs não necessitam de qualquer alteração. Não damos quaisquer garantias sobre os nomes dos recursos subjacentes que estão a ser criados.

## <a name="next-steps"></a>Passos seguintes

* [Como implementar um modelo usando uma imagem personalizada do Docker](how-to-deploy-custom-docker-image.md)
* [Resolução de problemas de implantação](how-to-troubleshoot-deployment.md)
* [Atualizar o serviço web](how-to-deploy-update-web-service.md)
* [Utilizar o TLS para proteger um serviço Web através do Azure Machine Learning](how-to-secure-web-service.md)
* [Consumir um Modelo ML implantado como um serviço web](how-to-consume-web-service.md)
* [Monitorize os seus modelos de machine learning Azure com Insights de Aplicações](how-to-enable-app-insights.md)
* [Recolher dados para modelos em produção](how-to-enable-data-collection.md)