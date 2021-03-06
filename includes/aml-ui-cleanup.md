---
title: incluir ficheiro
description: incluir ficheiro
services: machine-learning
ms.service: machine-learning
ms.custom: include file
ms.topic: include
author: sgilley
ms.author: sgilley
ms.date: 11/06/2019
ms.openlocfilehash: 754c9799ed4c2fd90cbcf1e9717b0be21edd54cb
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2021
ms.locfileid: "75659882"
---
>[!IMPORTANT]
>Pode utilizar os recursos que criou como pré-requisitos para outros tutoriais do Azure Machine Learning e artigos de procedimentos.

### <a name="delete-everything"></a>Apagar tudo

Se não planeia usar nada que tenha criado, elimine todo o grupo de recursos para não incorrer em quaisquer encargos.

1. No portal Azure, selecione **grupos de recursos** no lado esquerdo da janela.
 
   ![Eliminar grupo de recursos no portal do Azure](./media/aml-ui-cleanup/delete-resources.png)

1. Na lista, selecione o grupo de recursos que criou.

1. Selecione **Eliminar grupo de recursos**.

A eliminação do grupo de recursos também elimina todos os recursos que criou no designer. 

### <a name="delete-individual-assets"></a>Eliminar ativos individuais

No designer onde criou a sua experiência, elimine os ativos individuais selecionando-os e selecionando o botão **Eliminar.**

O alvo de cálculo que criou aqui *automaticamente escala automaticamente* para zero nós quando não está a ser usado. Esta ação é tomada para minimizar as cargas. Se quiser eliminar o alvo do cálculo, tome estes passos:

![Eliminar ativos](./media/aml-ui-cleanup/delete-asset.png)

Pode desagregar conjuntos de dados do seu espaço de trabalho selecionando cada conjunto de dados e selecionando **Unregister**.

![Conjunto de dados não registador](./media/aml-ui-cleanup/unregister-dataset1225.png)

Para eliminar um conjunto de dados, aceda à conta de armazenamento utilizando o portal Azure ou o Azure Storage Explorer e elimine manualmente esses ativos.


