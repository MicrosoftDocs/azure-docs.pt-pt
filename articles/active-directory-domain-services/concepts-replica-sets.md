---
title: Réplica define conceitos para Azure AD Domain Services | Microsoft Docs
description: Saiba quais os conjuntos de réplicas nos Serviços de Domínio do Diretório Ativo do Azure e como fornecem redundância a aplicações que requerem serviços de identidade.
services: active-directory-ds
author: justinha
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 03/30/2021
ms.author: justinha
ms.openlocfilehash: 8bcd3ebef027ec72728be21b0fe1504236f553ba
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/31/2021
ms.locfileid: "106058175"
---
# <a name="replica-sets-concepts-and-features-for-azure-active-directory-domain-services"></a>Réplica define conceitos e funcionalidades para Azure Ative Directory Domain Services

Quando cria um domínio gerido por Azure Ative Directory Domain Services (Azure AD DS), define um espaço de nome único. Este espaço de nome é o nome de domínio, como *aaddscontoso.com*, e dois controladores de domínio (DCs) são então implantados na região de Azure selecionada. Esta implantação de DCs é conhecida como um conjunto de réplicas.

Você pode expandir um domínio gerido para ter mais de um conjunto de réplicas por inquilino AZure AD. Os conjuntos de réplicas podem ser adicionados a qualquer rede virtual esprevada em qualquer região do Azure que suporte Azure AD DS. Conjuntos de réplicas adicionais em diferentes regiões de Azure proporcionam recuperação geográfica de desastres para aplicações antigas se uma região de Azure ficar offline.

> [!NOTE]
> Os conjuntos de réplicas não permitem implantar vários domínios geridos únicos num único inquilino Azure. Cada conjunto de réplicas contém os mesmos dados.

## <a name="how-replica-sets-work"></a>Como os conjuntos de réplicas funcionam

Quando cria um domínio gerido, como *aaddscontoso.com,* é criado um conjunto de réplicas iniciais. Conjuntos de réplicas adicionais partilham o mesmo espaço e configuração de nome. As alterações ao Azure AD DS, incluindo a configuração, identidade do utilizador e credenciais, grupos, objetos de política de grupo, objetos de computador e outras alterações são aplicadas a todos os conjuntos de réplicas no domínio gerido utilizando a replicação de DS AD.

Cria-se cada conjunto de réplicas numa rede virtual. Cada rede virtual deve ser espreitada para todas as outras redes virtuais que hospedam um conjunto de réplicas de domínio gerido. Esta configuração cria uma topologia de rede de malha que suporta a replicação do diretório. Uma rede virtual pode suportar vários conjuntos de réplicas, desde que cada conjunto de réplicas esteja numa sub-rede virtual diferente.

Todos os conjuntos de réplicas são colocados no mesmo site do Ative Directory. Como resultado, todas as alterações são propagadas usando a replicação intrassita para uma rápida convergência.

> [!NOTE]
> Não é possível definir locais separados e definir definições de replicação entre conjuntos de réplicas.

O diagrama seguinte mostra um domínio gerido com dois conjuntos de réplicas. O primeiro conjunto de réplicas é criado com o espaço de nome de domínio. Um segundo conjunto de réplicas é criado depois disso:

![Diagrama de exemplo gerido domínio com dois conjuntos de réplica](./media/concepts-replica-sets/two-replica-set-example.png)

> [!NOTE]
> Os conjuntos de réplicas garantem a disponibilidade de serviços de autenticação em regiões onde um conjunto de réplicas é configurado. Para que uma aplicação tenha redundância geográfica se houver uma paralisação regional, a plataforma de aplicação que se baseia no domínio gerido também deve residir na outra região.
>
> A resiliência de outros serviços necessários para o funcionamento da aplicação, como os Azure VMs ou Azure App Services, não é fornecida por conjuntos de réplicas. O design de disponibilidade de outros componentes de aplicação precisa considerar funcionalidades de resiliência para serviços que compõem a aplicação.

O exemplo a seguir mostra um domínio gerido com três conjuntos de réplicas para fornecer mais resiliência e garantir a disponibilidade de serviços de autenticação. Em ambos os exemplos, existem cargas de trabalho de aplicação na mesma região que o conjunto de réplicas de domínio gerido:

![Diagrama de exemplo gerido domínio com três conjuntos de réplicas](./media/concepts-replica-sets/three-replica-set-example.png)

## <a name="deployment-considerations"></a>Considerações sobre implementação

O SKU padrão para um domínio gerido é o *Enterprise* SKU, que suporta vários conjuntos de réplicas. Para criar conjuntos de réplicas adicionais se tiver mudado para o *SKU Standard,* [atualize o domínio gerido](change-sku.md) para *Enterprise* ou *Premium*.

O número máximo suportado de conjuntos de réplicas é de cinco, incluindo a primeira réplica criada quando criou o domínio gerido.

A faturação de cada conjunto de réplicas baseia-se na configuração de domínio SKU. Por exemplo, se tiver um domínio gerido que utilize o *SKU da Enterprise* e tiver três conjuntos de réplicas, a sua subscrição é faturada por hora para cada um dos três conjuntos de réplicas.

## <a name="frequently-asked-questions"></a>Perguntas mais frequentes

### <a name="can-i-create-a-replica-set-in-subscription-different-from-my-managed-domain"></a>Posso criar uma réplica definida em subscrição diferente do meu domínio gerido?

N.º Os conjuntos de réplicas devem estar na mesma subscrição que o domínio gerido.

### <a name="how-many-replica-sets-can-i-create"></a>Quantos conjuntos de réplicas posso criar?

Pode criar um máximo de cinco conjuntos de réplicas — o conjunto de réplicas iniciais para o domínio gerido, mais quatro conjuntos de réplicas adicionais.

### <a name="how-does-user-and-group-information-get-synchronized-to-my-replica-sets"></a>Como é que as informações de utilizador e de grupo são sincronizadas com os meus conjuntos de réplicas?

Todos os conjuntos de réplicas estão ligados entre si usando uma rede virtual de malha que espreita. Um conjunto de réplicas recebe atualizações de utilizador e grupo a partir do Azure AD. Essas alterações são então replicadas para os outros conjuntos de réplicas utilizando a replicação intrassite AD DS através da rede vigiada.

Tal como acontece com o AD DS no local, um estado desligado prolongado pode causar perturbações na replicação. Como as redes virtuais não são transitivas, os requisitos de design para conjuntos de réplicas requerem uma topologia de rede totalmente malhada.

### <a name="how-do-i-make-changes-in-my-managed-domain-after-i-have-replica-sets"></a>Como faço alterações no meu domínio gerido depois de ter conjuntos de réplicas?

As alterações dentro do domínio gerido funcionam como antes. [Cria e utiliza um VM de gestão com as ferramentas RSAT que se unem ao domínio gerido.](tutorial-create-management-vm.md) Pode juntar-se a todos os VMs de gestão ao domínio gerido como desejar.

## <a name="next-steps"></a>Passos seguintes

Para começar com conjuntos de réplicas, [crie e configuure um domínio gerido AZure AD DS][tutorial-create-advanced]. Quando implantado, [crie e utilize conjuntos de réplicas adicionais][create-replica-set].

<!-- LINKS - INTERNAL -->
[tutorial-create-advanced]: tutorial-create-instance-advanced.md
[create-replica-set]: tutorial-create-replica-set.md
