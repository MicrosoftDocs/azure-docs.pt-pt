---
title: Opções de redundância para discos geridos pela Azure
description: Saiba mais sobre o armazenamento redundante de zona e o armazenamento localmente redundante para discos geridos Azure.
author: roygara
ms.author: rogarana
ms.date: 03/02/2021
ms.topic: how-to
ms.service: virtual-machines
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: 0882efeccfc8dc83686d75ab39b8364219c3b5f1
ms.sourcegitcommit: 272351402a140422205ff50b59f80d3c6758f6f6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/17/2021
ms.locfileid: "107588093"
---
# <a name="redundancy-options-for-managed-disks"></a>Opções de redundância para discos geridos

Os discos geridos aZure oferecem duas opções de redundância de armazenamento, armazenamento redundante de zona (ZRS) como pré-visualização, e armazenamento localmente redundante. O ZRS oferece uma maior disponibilidade para discos geridos do que o armazenamento localmente redundante (LRS). No entanto, a latência de escrita para discos LRS é melhor do que os discos ZRS porque os discos LRS escrevem sincronizadamente dados a três cópias num único centro de dados.

## <a name="locally-redundant-storage-for-managed-disks"></a>Armazenamento localmente redundante para discos geridos

O armazenamento localmente redundante (LRS) replica os seus dados três vezes dentro de um único centro de dados na região selecionada. O LRS protege os seus dados contra as falhas de suporte do servidor e da unidade. 

Existem algumas formas de proteger a sua aplicação utilizando discos LRS de uma falha de zona inteira que pode ocorrer devido a desastres naturais ou problemas de hardware:
- Use uma aplicação como SQL Server AlwaysOn, que pode escrever sincronizadamente dados em duas zonas, e automaticamente falhar para outra zona durante um desastre.
- Faça cópias de segurança frequentes de discos LRS com fotografias ZRS.
- Permitir a recuperação de desastres em zonas cruzadas para discos LRS através da [Recuperação do Local de Azure](../site-recovery/azure-to-azure-how-to-enable-zone-to-zone-disaster-recovery.md). No entanto, a recuperação de desastres entre zonas não fornece zero Objetivo de Ponto de Recuperação (RPO).

Se o seu fluxo de trabalho não suporta escritas sincronizadas ao nível da aplicação em zonas, ou se a sua aplicação tiver de encontrar RPO zero, então os discos ZRS seriam ideais.

## <a name="zone-redundant-storage-for-managed-disks-preview"></a>Armazenamento redundante de zona para discos geridos (pré-visualização)

O armazenamento redundante de zona (ZRS) replica o seu disco gerido aZure sincronizadamente em três zonas de disponibilidade de Azure na região selecionada. Cada zona de disponibilidade é uma localização física separada com energia, refrigeração e rede independentes, 

Os discos ZRS permitem-lhe recuperar de falhas nas zonas de disponibilidade. Se uma zona inteira caiu, um disco ZRS pode ser ligado a um VM numa zona diferente. Também pode utilizar discos ZRS como disco partilhado para fornecer uma melhor disponibilidade para aplicações agrupadas ou distribuídas como SQL FCI, SAP ASCS/SCS ou GFS2. Pode anexar um disco ZRS partilhado a VMs primários e secundários em diferentes zonas para tirar partido das zonas ZRS e [Availability.](../availability-zones/az-overview.md) Se a sua zona primária falhar, pode falhar rapidamente no VM secundário utilizando [a reserva persistente SCSI](disks-shared-enable.md#supported-scsi-pr-commands).

### <a name="limitations"></a>Limitações

Durante a pré-visualização, o ZRS para discos geridos tem as seguintes restrições:

- Suportado apenas com unidades de estado sólido premium (SSD) e SSDs standard.
- Atualmente disponível apenas na região lesteUS2EUAP.
- Os discos ZRS só podem ser criados com modelos do Gestor de Recursos Azure utilizando a `2020-12-01` API.

Inscreva-se para a pré-visualização [aqui](https://aka.ms/ZRSDisksPreviewSignUp).

### <a name="billing-implications"></a>Implicações de faturação

Para mais detalhes consulte a [página de preços do Azure](https://azure.microsoft.com/pricing/details/managed-disks/).

### <a name="comparison-with-other-disk-types"></a>Comparação com outros tipos de discos

Com exceção de mais latência de escrita, os discos que usam ZRS são idênticos aos discos que usam LRS. Têm os mesmos alvos de desempenho. Recomendamos que realize [benchmarking em disco](disks-benchmarks.md) para simular a carga de trabalho da sua aplicação para comparar a latência entre os discos LRS e ZRS. 

### <a name="create-zrs-managed-disks"></a>Criar discos geridos ZRS

Utilize a `2020-12-01` API com o seu modelo Azure Resource Manager para criar um disco ZRS.

#### <a name="create-a-vm-with-zrs-disks"></a>Criar um VM com discos ZRS

```
$vmName = "yourVMName" 
$adminUsername = "yourAdminUsername"
$adminPassword = ConvertTo-SecureString "yourAdminPassword" -AsPlainText -Force
$osDiskType = "StandardSSD_ZRS"
$dataDiskType = "Premium_ZRS"
$region = "eastus2euap"
$resourceGroupName = "yourResourceGroupName"

New-AzResourceGroup -Name $resourceGroupName -Location $region
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName `
-TemplateUri "https://raw.githubusercontent.com/Azure-Samples/managed-disks-powershell-getting-started/master/ZRSDisks/CreateVMWithZRSDataDisks.json" `
-resourceName $vmName `
-adminUsername $adminUsername `
-adminPassword $adminPassword `
-region $region `
-osDiskType $osDiskType `
-dataDiskType $dataDiskType
```

#### <a name="create-vms-with-a-shared-zrs-disk-attached-to-the-vms-in-different-zones"></a>Criar VMs com um disco ZRS partilhado ligado aos VMs em diferentes zonas

```
$vmNamePrefix = "yourVMNamePrefix"
$adminUsername = "yourAdminUserName"
$adminPassword = ConvertTo-SecureString "yourAdminPassword" -AsPlainText -Force
$osDiskType = "StandardSSD_LRS"
$sharedDataDiskType = "Premium_ZRS"
$region = "eastus2euap"
$resourceGroupName = "zrstesting1"

New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName `
-TemplateUri "https://raw.githubusercontent.com/Azure-Samples/managed-disks-powershell-getting-started/master/ZRSDisks/CreateVMsWithASharedDisk.json" `
-vmNamePrefix $vmNamePrefix `
-adminUsername $adminUsername `
-adminPassword $adminPassword `
-region $region `
-osDiskType $osDiskType `
-dataDiskType $sharedDataDiskType
```

#### <a name="create-a-virtual-machine-scale-set-with-zrs-disks"></a>Criar um conjunto de escala de máquina virtual com discos ZRS

```
$vmssName="yourVMSSName"
$adminUsername="yourAdminName"
$adminPassword=ConvertTo-SecureString "yourAdminPassword" -AsPlainText -Force
$region="eastus2euap"
$osDiskType="StandardSSD_LRS"
$dataDiskType="Premium_ZRS"

New-AzResourceGroupDeployment -ResourceGroupName zrstesting `
-TemplateUri "https://raw.githubusercontent.com/Azure-Samples/managed-disks-powershell-getting-started/master/ZRSDisks/CreateVMSSWithZRSDisks.json" `
-vmssName "yourVMSSName" `
-adminUsername "yourAdminName" `
-adminPassword $password `
-region "eastus2euap" `
-osDiskType "StandardSSD_LRS" `
-dataDiskType "Premium_ZRS" `
```

## <a name="next-steps"></a>Passos seguintes

- Utilize estes [modelos de gestor de recursos Azure para criar um VM com discos ZRS](https://github.com/Azure-Samples/managed-disks-powershell-getting-started/tree/master/ZRSDisks).
