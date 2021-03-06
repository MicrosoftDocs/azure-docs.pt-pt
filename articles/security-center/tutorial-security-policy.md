---
title: Trabalhar com políticas de segurança | Microsoft Docs
description: Este artigo descreve como trabalhar com políticas de segurança no Azure Security Center.
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.date: 01/24/2021
ms.author: memildin
ms.openlocfilehash: 6ecedc20cf6924a82b6b4640d3caa75bc5958de0
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/20/2021
ms.locfileid: "102101329"
---
# <a name="manage-security-policies"></a>Manage security policies (Gerir políticas de segurança)

Este artigo explica como as políticas de segurança são configuradas e como vê-las no Centro de Segurança. 

## <a name="who-can-edit-security-policies"></a>Quem pode editar políticas de segurança?

Pode editar políticas de segurança através do portal Azure Policy, através da REST API ou utilizando o Windows PowerShell.

O Security Center utiliza o controlo de acesso baseado em funções Azure (Azure RBAC), que fornece funções incorporadas que pode atribuir aos utilizadores, grupos e serviços da Azure. Quando os utilizadores abrem o Centro de Segurança, vêem apenas informações relacionadas com os recursos a que podem aceder. O que significa que os utilizadores são atribuídos ao papel de *proprietário,* *contribuinte* ou *leitor* à subscrição do recurso. Há também duas funções específicas do Centro de Segurança:

- **Leitor de segurança**: Tem direito a visualizar itens do Centro de Segurança, tais como recomendações, alertas, política e saúde. Não posso fazer mudanças.
- **Administrador de segurança**: Tem os mesmos direitos de visão que *o leitor de segurança.* Também pode atualizar a política de segurança e dispensar alertas.

## <a name="manage-your-security-policies"></a>Gerir as suas políticas de segurança

Para ver as suas políticas de segurança no Centro de Segurança:

1. No painel do **Centro de Segurança,** selecione **a política de segurança.**

    :::image type="content" source="./media/security-center-policies/security-center-policy-mgt.png" alt-text="A página de gestão de políticas":::

   No ecrã de gestão de **políticas,** pode ver o número de grupos de gestão, subscrições e espaços de trabalho, bem como a estrutura do seu grupo de gestão.

1. Selecione o grupo de subscrição ou gestão cujas políticas pretende ver.

1. Aparece a página de política de segurança para esse grupo de subscrição ou gestão. Mostra as políticas disponíveis e atribuídas.

    :::image type="content" source="./media/tutorial-security-policy/security-policy-page.png" alt-text="Página de política de segurança do Centro de Segurança" lightbox="./media/tutorial-security-policy/security-policy-page.png":::

    > [!NOTE]
    > Se houver um rótulo "MG Herdado" ao lado da sua política de incumprimento, significa que a apólice foi atribuída a um grupo de gestão e herdada pela subscrição que está a ver.

1. Escolha entre as opções disponíveis nesta página:

    1. Para trabalhar com os padrões da indústria, **selecione Adicione mais padrões**. Para obter mais informações, consulte [Personalizar o conjunto de normas no seu painel de conformidade regulamentar](update-regulatory-compliance-packages.md).

    1. Para atribuir e gerir iniciativas personalizadas, **selecione Adicionar iniciativas personalizadas.** Para obter mais informações, consulte [utilizando iniciativas e políticas de segurança personalizadas.](custom-security-policies.md)

    1. Para visualizar e editar a iniciativa predefinitiva, **selecione Ver política eficaz** e proceda como descrito abaixo. 

        :::image type="content" source="./media/security-center-policies/policy-screen.png" alt-text="Tela de política eficaz":::

       Este ecrã **de política de segurança** reflete as ações tomadas pelas políticas atribuídas no grupo de subscrição ou gestão que selecionou.
       
       * Utilize os links no topo para abrir uma **atribuição** de política que se aplica no grupo de subscrição ou gestão. Estes links permitem-lhe aceder à atribuição e editar ou desativar a apólice. Por exemplo, se vir que uma determinada atribuição de políticas está efetivamente a negar a proteção do ponto final, use o link para editar ou desativar a política.
       
       * Na lista de políticas, pode ver a aplicação efetiva da política no seu grupo de subscrição ou gestão. São tomados em consideração as definições de cada política que se aplicam ao âmbito de aplicação e mostra-se o resultado cumulativo das ações tomadas pela política. Por exemplo, se numa atribuição da apólice for desativada, mas noutra está definida para AuditIfNotExist, então o efeito cumulativo aplica-se AuditIfNotExist. O efeito mais ativo tem sempre precedência.
       
       * O efeito das políticas pode ser: Apêndice, Auditoria, AuditIfNotExists, Deny, DeployIfNotExists, Desativado. Para obter mais informações sobre a forma como os efeitos são [aplicados,](../governance/policy/concepts/effects.md)consulte os efeitos da Política de Compreensão .

       > [!NOTE]
       > Quando vê as políticas atribuídas, pode ver várias atribuições e pode ver como cada atribuição é configurada por si só.


## <a name="disable-security-policies-and-disable-recommendations"></a>Desativar as políticas de segurança e desativar recomendações

Quando a sua iniciativa de segurança desencadear uma recomendação que é irrelevante para o seu ambiente, pode impedir que essa recomendação volte a aparecer. Para desativar uma recomendação, desative a política específica que gera a recomendação.

A recomendação que pretende desativar continuará a aparecer se for necessária para um padrão regulamentar que tenha aplicado com as ferramentas de conformidade regulamentar do Centro de Segurança. Mesmo que tenha desativado uma política na iniciativa incorporada, uma política por iniciativa do padrão regulamentar ainda irá desencadear a recomendação se for necessária para o cumprimento. Não se pode desativar as políticas de iniciativas normais regulamentares.

Para obter mais informações sobre recomendações, consulte [recomendações de segurança de gestão.](security-center-recommendations.md)

1. No Centro de Segurança, a partir da secção **de Conformidade & Política,** selecione **a política de Segurança**.

    :::image type="content" source="./media/tutorial-security-policy/policy-management.png" alt-text="Iniciar o processo de gestão de políticas no Azure Security Center":::

2. Selecione o grupo de subscrição ou gestão para o qual pretende desativar a recomendação.

   > [!NOTE]
   > Lembre-se de que um grupo de gestão aplica as suas políticas às suas subscrições. Portanto, se desativar a política de uma subscrição e a subscrição pertencer a um grupo de gestão que ainda utiliza a mesma política, irá continuar a receber as recomendações de política. A política ainda será aplicada a partir do nível de gestão e as recomendações ainda serão geradas.

1. **Selecione Ver política eficaz**.

    :::image type="content" source="./media/tutorial-security-policy/view-effective-policy.png" alt-text="Como abrir a política eficaz atribuída à sua subscrição":::

1. Selecione a política atribuída.

   ![selecionar política](./media/tutorial-security-policy/security-policy.png)

1. Na secção **PARÂMETROS,** procure a política que invoca a recomendação que pretende desativar, e da lista de abandono, selecione **Disabled**

   ![política de desativação](./media/tutorial-security-policy/disable-policy.png)

1. Selecione **Guardar**.

   > [!NOTE]
   > As alterações à política de desativação podem demorar até 12 horas a produzir efeitos.

## <a name="next-steps"></a>Passos seguintes
Esta página explicou as políticas de segurança. Para obter informações relacionadas, consulte as seguintes páginas:

- [Saiba como definir políticas usando o PowerShell](../governance/policy/assign-policy-powershell.md)
- [Saiba como editar uma política de segurança na Política Azure](../governance/policy/tutorials/create-and-manage.md)
- [Saiba como definir uma política entre subscrições ou grupos de Gestão usando a Política Azure](../governance/policy/overview.md)
- [Saiba como ativar o Centro de Segurança em todas as subscrições de um grupo de gestão](onboard-management-group.md)