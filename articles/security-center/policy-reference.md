---
title: Definições políticas incorporadas para o Azure Security Center
description: Lista definições de políticas incorporadas para o Azure Security Center. Estas definições políticas incorporadas fornecem abordagens comuns para gerir os seus recursos Azure.
ms.date: 04/21/2021
ms.topic: reference
author: memildin
ms.author: memildin
ms.service: security-center
ms.custom: subject-policy-reference
ms.openlocfilehash: 0fc5e2ed183020f32b59fec7f4436754467968e0
ms.sourcegitcommit: 2aeb2c41fd22a02552ff871479124b567fa4463c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/22/2021
ms.locfileid: "107872430"
---
# <a name="azure-policy-built-in-definitions-for-azure-security-center"></a>Definições incorporadas do Azure Policy para o Centro de Segurança do Azure

Esta página é um índice de definições políticas incorporadas da [Azure Policy](../governance/policy/overview.md) relacionadas com o Azure Security Center. Estão disponíveis os seguintes agrupamentos de definições políticas:

- O grupo [de iniciativas](#azure-security-center-initiatives) lista as definições de iniciativa da Política Azure na categoria "Centro de Segurança".
- O grupo [de iniciativa padrão](#azure-security-center-initiatives) lista todas as definições da Política Azure que fazem parte da iniciativa padrão do Security Center, [Azure Security Benchmark](https://docs.microsoft.com/security/benchmark/azure/introduction). Esta referência de autoria da Microsoft, amplamente respeitada, baseia-se nos controlos do [Center for Internet Security (CIS)](https://www.cisecurity.org/benchmark/azure/) e do National Institute of Standards and Technology [(NIST)](https://www.nist.gov/) com foco na segurança centrada na nuvem.
- O grupo [de categoria](#azure-security-center-category) lista todas as definições de Política Azure na categoria "Centro de Segurança".

Para obter mais informações sobre políticas de segurança, consulte [Trabalhar com políticas de segurança.](./tutorial-security-policy.md) Para obter mais incorporados em Azure Policy para outros serviços, consulte [definições incorporadas da Política Azure](../governance/policy/samples/built-in-policies.md).

O nome de cada definição de política incorporada liga-se à definição de política no portal Azure. Utilize o link na coluna **versão** para visualizar a fonte no [repo GitHub da Política Azure](https://github.com/Azure/azure-policy).

## <a name="azure-security-center-initiatives"></a>Iniciativas do Centro de Segurança Azure

Para conhecer as iniciativas incorporadas que são monitorizadas pelo Centro de Segurança, consulte a seguinte tabela:

[!INCLUDE [azure-policy-reference-policyset-security-center](../../includes/policy/reference/bycat/policysets-security-center.md)]

## <a name="security-centers-default-initiative-azure-security-benchmark"></a>Iniciativa padrão do Centro de Segurança (Azure Security Benchmark)

Para conhecer as políticas incorporadas que são monitorizadas pelo Centro de Segurança, consulte a seguinte tabela:

[!INCLUDE [azure-policy-reference-init-asc](../../includes/policy/reference/custom/init-asc.md)]

## <a name="azure-security-center-category"></a>Categoria Azure Security Center

[!INCLUDE [azure-policy-reference-category-securitycenter](../../includes/policy/reference/bycat/policies-security-center.md)]

## <a name="next-steps"></a>Passos seguintes

Neste artigo, você aprendeu sobre as definições de política de segurança da Azure Policy no Centro de Segurança. Para saber mais sobre iniciativas, políticas e como se relacionam com as recomendações do Centro de Segurança, veja [o que são políticas de segurança, iniciativas e recomendações?](security-policy-concept.md)
