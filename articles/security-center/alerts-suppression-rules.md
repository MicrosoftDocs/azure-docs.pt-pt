---
title: Usando regras de supressão de alertas para suprimir falsos positivos ou outros alertas de segurança indesejados no Centro de Segurança Azure.
description: Este artigo explica como usar as regras de supressão do Azure Security Center para esconder alertas de segurança indesejados
author: memildin
manager: rkarlin
services: security-center
ms.author: memildin
ms.date: 02/17/2021
ms.service: security-center
ms.topic: how-to
ms.openlocfilehash: 646495597565bbb033ac3adaa15f3754f33e8fd6
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "100634175"
---
# <a name="suppress-alerts-from-azure-defender"></a>Alertas de Supressão do Azure Defender

Esta página explica como pode usar regras de supressão de alertas para suprimir falsos positivos ou outros alertas de segurança indesejados do Azure Defender.

## <a name="availability"></a>Disponibilidade

|Aspeto|Detalhes|
|----|:----|
|Estado de libertação:|Disponibilidade Geral (GA)|
|Preços:|Gratuito<br>(A maioria dos alertas de segurança só estão disponíveis com o Azure Defender)|
|Funções e permissões necessárias:|**Administração de segurança** e **Proprietário** pode criar/eliminar regras.<br>**O leitor de segurança** e **o Reader** podem ver as regras.|
|Nuvens:|![Yes](./media/icons/yes-icon.png) Nuvens comerciais<br>![Yes](./media/icons/yes-icon.png) Nacional/Soberano (Gov dos EUA, China Gov, Outro Gov)|
|||


## <a name="what-are-suppression-rules"></a>O que são regras de supressão?

Os vários planos do Azure Defender detetam ameaças em qualquer área do seu ambiente e geram alertas de segurança.

Quando um único alerta não é interessante ou relevante, pode descartá-lo manualmente. Em alternativa, utilize as regras de supressão para descartar automaticamente alertas semelhantes no futuro. Normalmente, usaria uma regra de supressão para:

- Suprimir alertas que identificou como falsos positivos

- Alertas suprimidos que estão sendo desencadeados com demasiada frequência para serem úteis

As suas regras de supressão definem os critérios para os quais os alertas devem ser automaticamente dispensados.

> [!CAUTION]
> Suprimir alertas de segurança reduz a eficácia da proteção contra ameaças do Azure Defender. Deve verificar cuidadosamente o impacto potencial de qualquer regra de supressão e monitorizá-la ao longo do tempo.

:::image type="content" source="./media/alerts-suppression-rules/create-suppression-rule.gif" alt-text="Criar regra de supressão de alerta":::

## <a name="create-a-suppression-rule"></a>Criar uma regra de supressão

Existem algumas formas de criar regras para suprimir alertas de segurança indesejados:

- Para suprimir alertas ao nível do grupo de gestão, utilize o Azure Policy
- Para suprimir alertas ao nível da subscrição, pode utilizar o portal do Azure ou a API REST, conforme explicado abaixo

As regras de supressão só podem descartar alertas que já tenham sido desencadeados nas subscrições selecionadas.

Para criar uma regra diretamente no portal Azure:

1. Na página de alertas de segurança do Centro de Segurança:

    - Selecione o alerta específico que não quer ver mais e, a partir do painel de detalhes, **selecione Tome medidas**.

    - Ou, selecione o link das **regras** de supressão no topo da página e a partir da página de regras de supressão selecione **Criar nova regra de supressão**:

        ![Criar novo botão de regra de supressão**](media/alerts-suppression-rules/create-new-suppression-rule.png)

1. No novo painel de regras de supressão, insira os detalhes da sua nova regra.
    - A sua regra pode descartar o alerta de **todos os recursos** para que não receba alertas como este no futuro.     
    - A sua regra pode descartar o alerta **em critérios específicos** - quando se refere a um endereço IP específico, nome do processo, conta de utilizador, recurso Azure ou localização.

    > [!TIP]
    > Se abrir a nova página de regras a partir de um alerta específico, o alerta e a subscrição serão automaticamente configurados na sua nova regra. Se utilizar o novo link **de regra de supressão,** as subscrições selecionadas corresponderão ao filtro atual no portal.

    [![Painel de criação de regras de supressão](media/alerts-suppression-rules/new-suppression-rule-pane.png)](media/alerts-suppression-rules/new-suppression-rule-pane.png#lightbox)
1. Introduza os detalhes da regra:
    - **Nome** - Um nome para a regra. Os nomes das regras devem começar com uma letra ou um número, ter entre 2 e 50 carateres e não conter outros símbolos que não sejam traços (-) ou sublinhados (_). 
    - **Estado** - Ativado ou incapacitado.
    - **Razão** - Selecione uma das razões incorporadas ou 'outras' se não satisfazer as suas necessidades.
    - **Data de validade** - Data e hora de fim da regra. As regras podem ser executadas por um máximo de seis meses.
1. Opcionalmente, teste a regra usando o botão **Simulação** para ver quantos alertas teriam sido dispensados se esta regra estivesse ativa.
1. Salve a regra. 


## <a name="edit-a-suppression-rule"></a>Editar uma regra de supressão

Para editar uma regra que criou, use a página de regras de supressão.

1. A partir da página de alertas de segurança do Security Center, selecione o link das regras de **supressão** no topo da página.
1. A página de regras de supressão abre com todas as regras para as subscrições selecionadas.

    [![Lista de regras de supressão](media/alerts-suppression-rules/suppression-rules-page.png)](media/alerts-suppression-rules/suppression-rules-page.png#lightbox)

1. Para editar uma única regra, abra o menu de elipses (...) para a regra e **selecione Editar**.
1. Faça as alterações necessárias e **selecione Aplicar**. 

## <a name="delete-a-suppression-rule"></a>Eliminar uma regra de supressão

Para eliminar uma ou mais regras que criou, utilize a página de regras de supressão.

1. A partir da página de alertas de segurança do Security Center, selecione o link das regras de **supressão** no topo da página.
1. A página de regras de supressão abre com todas as regras para as subscrições selecionadas.
1. Para eliminar uma única regra, abra o menu de elipses (...) para a regra e selecione **Delete**.
1. Para eliminar várias regras, selecione as caixas de verificação para que as regras sejam eliminadas e selecione **Eliminar**.
    ![Excluir uma ou mais regras de supressão](media/alerts-suppression-rules/delete-multiple-alerts.png)

## <a name="create-and-manage-suppression-rules-with-the-api"></a>Criar e gerir regras de supressão com a API

Pode criar, visualizar ou eliminar regras de supressão de alerta através da API REST do Security Center. 

Os métodos HTTP relevantes para as regras de supressão na API REST são:

- **PUT:** Criar ou atualizar uma regra de supressão numa subscrição especificada.

- **GET**:

    - Para listar todas as regras configuradas para uma subscrição especificada. Este método devolve uma série de regras aplicáveis.

    - Para obter os detalhes de uma regra específica sobre uma subscrição especificada. Este método devolve uma regra de supressão.

    - Para simular o impacto de uma regra de supressão ainda na fase de conceção. Esta chamada identifica qual dos seus alertas existentes teria sido dispensado se a regra estivesse ativa.

- **DELETE**: Elimina uma regra existente (mas não altera o estado dos alertas já dispensados).

Para obter todos os detalhes e exemplos de utilização, consulte a documentação da [API.](/rest/api/securitycenter/) 


## <a name="next-steps"></a>Passos seguintes

Este artigo descreveu as regras de supressão no Azure Security Center que automaticamente descartam alertas indesejados.

Para obter mais informações sobre os alertas de segurança do Azure Defender, consulte as seguintes páginas:

- [Alertas de segurança e a cadeia de morte intencional](alerts-reference.md) - Um guia de referência para os alertas de segurança que poderá obter do Azure Defender.