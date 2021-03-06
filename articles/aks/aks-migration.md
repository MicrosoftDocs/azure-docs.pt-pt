---
title: Migrar para o Serviço Azure Kubernetes (AKS)
description: Migrar para o Serviço Azure Kubernetes (AKS).
services: container-service
ms.topic: article
ms.date: 03/25/2021
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: 123f4e5c2442b913a53288602d1c56f199b131a6
ms.sourcegitcommit: 2aeb2c41fd22a02552ff871479124b567fa4463c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/22/2021
ms.locfileid: "107872790"
---
# <a name="migrate-to-azure-kubernetes-service-aks"></a>Migrar para o Serviço Azure Kubernetes (AKS)

Para ajudá-lo a planear e executar uma migração bem sucedida para o Serviço Azure Kubernetes (AKS), este guia fornece detalhes para a configuração AKS recomendada atual. Embora este artigo não cubra todos os cenários, contém links para informações mais detalhadas para planear uma migração bem sucedida.

Este documento ajuda a suportar os seguintes cenários:

* Contentorizando certas aplicações e migrando-as para AKS usando [Azure Migrate](../migrate/migrate-services-overview.md).
* Migração de um cluster AKS apoiado por [conjuntos de disponibilidade](../virtual-machines/windows/tutorial-availability-sets.md) para [conjuntos de escala de máquina virtual](../virtual-machine-scale-sets/overview.md).
* Migrar um cluster AKS para utilizar um [balanceador de carga Standard SKU](./load-balancer-standard.md).
* Migrar do Serviço de [Contentores Azure (ACS) - reformando-se a 31 de janeiro de 2020](https://azure.microsoft.com/updates/azure-container-service-will-retire-on-january-31-2020/) para a AKS.
* Migrando do [motor AKS](/azure-stack/user/azure-stack-kubernetes-aks-engine-overview) para aKS.
* Migrando de aglomerados kubernetes não-Azure para AKS.
* Mover os recursos existentes para uma região diferente.

Ao migrar, certifique-se de que a versão target Kubernetes está dentro da janela suportada para AKS. As versões mais antigas podem não estar dentro da gama suportada e exigirão que uma atualização de versão seja suportada pela AKS. Para obter mais informações, consulte [as versões Kubernetes suportadas pela AKS.](./supported-kubernetes-versions.md)

Se está a migrar para uma versão mais recente de Kubernetes, reveja a [versão de Kubernetes e a versão distorça a política de suporte](https://kubernetes.io/docs/setup/release/version-skew-policy/#supported-versions).

Várias ferramentas de código aberto podem ajudar na sua migração, dependendo do seu cenário:

* [Velero](https://velero.io/) (Requer Kubernetes 1.7+)
* [Extensão CLI de Azure Kube](https://github.com/yaron2/azure-kube-cli)
* [ReShifter](https://github.com/mhausenblas/reshifter)

Neste artigo vamos resumir os detalhes da migração para:

> [!div class="checklist"]
> * Aplicações de contentorização através do Azure Migrate 
> * AKS com balanceador de carga padrão e conjuntos de balança de máquinas virtuais
> * Serviços Azure existentes anexados
> * Garantir quotas válidas
> * Alta Disponibilidade e continuidade de negócios
> * Considerações para aplicações apátridas
> * Considerações para aplicações estatais
> * Implementação da sua configuração de cluster

## <a name="use-azure-migrate-to-migrate-your-applications-to-aks"></a>Use a Azure Migrate para migrar as suas aplicações para AKS

A Azure Migrate oferece uma plataforma unificada para avaliar e migrar para a Azure nos servidores, infraestruturas, aplicações e dados. Para a AKS, pode utilizar a Azure Migrate para as seguintes tarefas:

* [Contentorizar aplicações de ASP.NET e migrar para AKS](../migrate/tutorial-containerize-aspnet-kubernetes.md)
* [Containerize aplicações web java e migra para AKS](../migrate/tutorial-containerize-java-kubernetes.md)

## <a name="aks-with-standard-load-balancer-and-virtual-machine-scale-sets"></a>AKS com balanceador de carga padrão e conjuntos de balança de máquinas virtuais

AKS é um serviço gerido que oferece capacidades únicas com menor carga de gestão. Uma vez que a AKS é um serviço gerido, você deve selecionar de um conjunto de [regiões](./quotas-skus-regions.md) que a AKS suporta. Poderá ser necessário modificar as aplicações existentes para as manter saudáveis no plano de controlo gerido pela AKS durante a transição do cluster existente para a AKS.

Recomendamos a utilização de clusters AKS apoiados por [conjuntos de balanças de máquinas virtuais](../virtual-machine-scale-sets/index.yml) e pelo [Balançador de Carga Padrão Azure](./load-balancer-standard.md) para garantir que obtém funcionalidades como:
* [Múltiplas piscinas de nó,](./use-multiple-node-pools.md)
* [Zonas de Disponibilidade,](../availability-zones/az-overview.md)
* [Gamas de IP autorizadas,](./api-server-authorized-ip-ranges.md)
* [Cluster Autoscaler,](./cluster-autoscaler.md)
* [Política Azure para AKS,](../governance/policy/concepts/policy-for-kubernetes.md)e
* Outras novidades à medida que são lançadas.

Os clusters AKS apoiados por [Conjuntos de Disponibilidade de Máquinas Virtuais](../virtual-machines/availability.md#availability-sets) carecem de suporte para muitas destas funcionalidades.

O exemplo a seguir cria um cluster AKS com um único conjunto de nós apoiado por um conjunto de escala de máquina virtual (VM). O aglomerado:
* Usa um equilibrador de carga padrão. 
* Permite o autoescalador do cluster na piscina de nós para o cluster.
* Define um mínimo de *1* e máximo de *3* nós.

```azurecli-interactive
# First create a resource group
az group create --name myResourceGroup --location eastus

# Now create the AKS cluster and enable the cluster autoscaler
az aks create \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --node-count 1 \
  --vm-set-type VirtualMachineScaleSets \
  --load-balancer-sku standard \
  --enable-cluster-autoscaler \
  --min-count 1 \
  --max-count 3
```

## <a name="existing-attached-azure-services"></a>Serviços Azure existentes anexados

Ao migrar aglomerados, pode ter ligado serviços externos da Azure. Embora os seguintes serviços não exijam recreação de recursos, eles exigirão atualização de ligações de anteriores a novos clusters para manter a funcionalidade.

* Registo de Contentores do Azure
* Log Analytics
* Application Insights
* Gestor de Tráfego
* Conta de Armazenamento
* Bases de Dados Externas

## <a name="ensure-valid-quotas"></a>Garantir quotas válidas

Uma vez que outros VMs serão implantados na sua subscrição durante a migração, deve verificar se as suas quotas e limites são suficientes para estes recursos. Se necessário, solicite um aumento da [quota vCPU](../azure-portal/supportability/per-vm-quota-requests.md).

Poderá ter de solicitar um aumento das [quotas da Rede](../azure-portal/supportability/networking-quota-requests.md) para garantir que não esgote os IPs. Para obter mais informações, consulte [as gamas de redes e IP para AKS](./configure-kubenet.md).

Para mais informações, consulte [os limites de subscrição e serviço da Azure.](../azure-resource-manager/management/azure-subscription-service-limits.md) Para verificar as suas quotas atuais, no portal Azure, vá à lâmina de [subscrições,](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)selecione a sua subscrição e, em seguida, selecione **Usage + quotas**.

## <a name="high-availability-and-business-continuity"></a>Alta Disponibilidade e Continuidade empresarial

Se a sua aplicação não conseguir lidar com o tempo de inatividade, terá de seguir as melhores práticas para cenários de migração de alta disponibilidade. Leia mais sobre [as melhores práticas para o planeamento complexo da continuidade do negócio, recuperação de desastres e maximização do tempo de funcionamento no Serviço Azure Kubernetes (AKS)](./operator-best-practices-multi-region.md).

Para aplicações complexas, você normalmente migrará ao longo do tempo e não ao mesmo tempo, o que significa que os ambientes antigos e novos podem precisar de comunicar através da rede. As aplicações anteriormente que utilizam `ClusterIP` serviços para comunicar podem ter de ser expostas como tipo `LoadBalancer` e ser protegidas adequadamente.

Para completar a migração, você vai querer apontar os clientes para os novos serviços que estão em execução na AKS. Recomendamos que redirecione o tráfego atualizando o DNS para apontar para o Balancer de Carga sentado em frente ao seu cluster AKS.

[O Azure Traffic Manager](../traffic-manager/index.yml) pode direcionar os clientes para o cluster e instância de aplicação de Kubernetes desejados. Traffic Manager é um equilibrador de carga de tráfego baseado em DNS que pode distribuir tráfego de rede em regiões. Para obter o melhor desempenho e redundância, direcione todo o tráfego de aplicações através do Traffic Manager antes de ir para o seu cluster AKS. 

Numa implementação multi-cluster, os clientes devem ligar-se a um nome DNS do Gestor de Tráfego que aponta para os serviços em cada cluster AKS. Defina estes serviços utilizando pontos finais do Traffic Manager. Cada ponto final é o *equilibrador de carga de serviço IP*. Utilize esta configuração para direcionar o tráfego de rede do ponto final do Gestor de Tráfego numa região até ao ponto final numa região diferente.

![AKS com gestor de tráfego](media/operator-best-practices-bc-dr/aks-azure-traffic-manager.png)

[O Serviço de Porta Frontal Azure](../frontdoor/front-door-overview.md) é outra opção para encaminhar o tráfego para clusters AKS. Com o Azure Front Door Service, pode definir, gerir e monitorizar o encaminhamento global para o seu tráfego web, otimizando para melhor desempenho e falha global instantânea para uma elevada disponibilidade. 

### <a name="considerations-for-stateless-applications"></a>Considerações para aplicações apátridas

A migração de aplicações apátridas é o caso mais simples:
1. Aplique as definições de recursos (YAML ou Helm) no novo cluster.
1. Garanta que tudo funcione como esperado.
1. Redirecione o tráfego para ativar o seu novo cluster.

### <a name="considerations-for-stateful-applications"></a>Considerações para aplicações estatais

Planeie cuidadosamente a sua migração de aplicações estatais para evitar a perda de dados ou tempo de inatividade inesperado.

* Se utilizar ficheiros Azure, pode montar a partilha de ficheiros como um volume no novo cluster. Consulte [os ficheiros de azure estático do monte como um volume](./azure-files-volume.md#mount-file-share-as-an-persistent-volume).
* Se utilizar Discos Geridos Azure, só pode montar o disco se não estiver ligado a qualquer VM. Consulte [o disco Azure Estático do Monte como um volume](./azure-disk-volume.md#mount-disk-as-volume).
* Se nenhuma dessas abordagens funcionar, pode utilizar uma reserva e restaurar opções. Ver [Velero em Azure.](https://github.com/vmware-tanzu/velero-plugin-for-microsoft-azure/blob/master/README.md)

#### <a name="azure-files"></a>Ficheiros do Azure

Ao contrário dos discos, os Ficheiros Azure podem ser montados em vários anfitriões simultaneamente. No seu cluster AKS, a Azure e a Kubernetes não o impedem de criar uma cápsula que o seu cluster AKS ainda utiliza. Para evitar a perda de dados e comportamentos inesperados, certifique-se de que os clusters não escrevem para os mesmos ficheiros simultaneamente.

Se a sua aplicação pode hospedar várias réplicas que apontam para a mesma partilha de ficheiros, siga os passos de migração apátridas e implemente as definições YAML para o seu novo cluster. 

Caso contrário, uma possível abordagem de migração envolve as seguintes etapas:

1. Validar a sua aplicação está a funcionar corretamente.
1. Aponte o seu tráfego ao vivo para o seu novo cluster AKS.
1. Desligue o velho aglomerado.

Se quiser começar com uma partilha vazia e fazer uma cópia dos dados de origem, pode utilizar os [`az storage file copy`](/cli/azure/storage/file/copy) comandos para migrar os seus dados.


#### <a name="migrating-persistent-volumes"></a>Volumes persistentes migratórios

Se estiver a migrar volumes persistentes existentes para a AKS, geralmente seguirá estes passos:

1. Quiesce escreve para o pedido. 
    * Este passo é opcional e requer tempo de inatividade.
1. Tira fotos dos discos.
1. Crie novos discos geridos a partir dos instantâneos.
1. Crie volumes persistentes em AKS.
1. Atualizar as especificações do casulo para [utilizar volumes existentes](./azure-disk-volume.md) em vez de PersistenteVolumeClaims (provisão estática).
1. Implemente a sua aplicação para AKS.
1. Validar a sua aplicação está a funcionar corretamente.
1. Aponte o seu tráfego ao vivo para o seu novo cluster AKS.

> [!IMPORTANT]
> Se optar por não escrever quiesce, terá de replicar dados para a nova implementação. Caso contrário, perderá os dados que foram escritos depois de tirar as fotos do disco.

Algumas ferramentas de código aberto podem ajudá-lo a criar discos geridos e a migrar volumes entre clusters Kubernetes:

* [Azure CLI Disk Copy](https://github.com/noelbundick/azure-cli-disk-copy-extension) copies e converte discos em grupos de recursos e regiões de Azure.
* [A extensão CLI de Azure Kube](https://github.com/yaron2/azure-kube-cli) enumera volumes ACS Kubernetes e migra-os para um cluster AKS.


### <a name="deployment-of-your-cluster-configuration"></a>Implementação da sua configuração de cluster

Recomendamos que utilize o seu pipeline de Integração Contínua (CI) e Entrega Contínua (CD) para implantar uma configuração de bem-estar conhecida para AKS. Pode utilizar a Azure Pipelines para [construir e implementar as suas aplicações na AKS](/azure/devops/pipelines/ecosystems/kubernetes/aks-template). Clone as suas tarefas de implantação existentes e certifique-se de que `kubeconfig` aponta para o novo cluster AKS.

Se isso não for possível, exporte definições de recursos do seu cluster Kubernetes existente e, em seguida, aplique-as em AKS. Pode usar `kubectl` para exportar objetos.

```console
kubectl get deployment -o=yaml --export > deployments.yaml
```

### <a name="moving-existing-resources-to-another-region"></a>Deslocação dos recursos existentes para outra região

Você pode querer mover o seu cluster AKS para uma [região diferente apoiada pela AKS][region-availability]. Recomendamos que crie um novo cluster na outra região, em seguida, implemente os seus recursos e aplicações para o seu novo cluster. 

Além disso, se tiver algum serviço como [o Azure Dev Spaces][azure-dev-spaces] em execução no seu cluster AKS, terá de instalar e configurar esses serviços no seu cluster na nova região.


Neste artigo, resumimos detalhes da migração para:

> [!div class="checklist"]
> * AKS com balanceador de carga padrão e conjuntos de balança de máquinas virtuais
> * Serviços Azure existentes anexados
> * Garantir quotas válidas
> * Alta Disponibilidade e continuidade de negócios
> * Considerações para aplicações apátridas
> * Considerações para aplicações estatais
> * Implementação da sua configuração de cluster


[region-availability]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
[azure-dev-spaces]: ../dev-spaces/index.yml
