---
title: Crie regras de análise personalizadas para detetar ameaças com Azure Sentinel| Microsoft Docs
description: Use este tutorial para aprender a criar regras de análise personalizadas para detetar ameaças de segurança com o Azure Sentinel. Aproveite o agrupamento de eventos, o agrupamento de alertas e o enriquecimento de alerta e compreenda o AUTO DISABLED.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/21/2021
ms.author: yelevin
ms.openlocfilehash: 180a5edd00b6085ffd91568471ca763f5e4e9711
ms.sourcegitcommit: 260a2541e5e0e7327a445e1ee1be3ad20122b37e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/21/2021
ms.locfileid: "107814861"
---
# <a name="tutorial-create-custom-analytics-rules-to-detect-threats"></a>Tutorial: Criar regras de análise personalizadas para detetar ameaças

Agora que [ligou as suas fontes](quickstart-onboard.md) de dados ao Azure Sentinel, pode criar regras de análise personalizadas para o ajudar a descobrir ameaças e comportamentos anómalos que estão presentes no seu ambiente. Estas regras procuram eventos ou conjuntos específicos de eventos em todo o seu ambiente, alertam-no quando determinados limiares ou condições de evento são atingidos, geram incidentes para o seu SOC triagem e investigação, e responder a ameaças com processos automatizados de rastreio e remediação. 

Este tutorial ajuda-o a criar regras personalizadas para detetar ameaças com o Azure Sentinel.

Ao completar este tutorial, poderá fazer o seguinte:
> [!div class="checklist"]
> * Criar regras de análise
> * Definir como os eventos e alertas são processados
> * Definir como os alertas e incidentes são gerados
> * Escolha respostas automáticas de ameaças para as suas regras

## <a name="create-a-custom-analytics-rule-with-a-scheduled-query"></a>Crie uma regra de análise personalizada com uma consulta programada

1. A partir do menu de navegação Azure Sentinel, **selecione Analytics**.

1. Na barra de ação no topo, selecione **+Criar** e selecione **a regra de consulta agendada**. Isto abre o **assistente de regras de Analytics**.

    :::image type="content" source="media/tutorial-detect-threats-custom/create-scheduled-query-small.png" alt-text="Criar consulta agendada" lightbox="media/tutorial-detect-threats-custom/create-scheduled-query-full.png":::

### <a name="analytics-rule-wizard---general-tab"></a>Assistente de regras de analítica - separador geral

- Forneça um **nome** único e uma **descrição.** 

- No campo **tático,** pode escolher entre categorias de ataques para classificar a regra. Estes são baseados nas táticas do [MITRE ATT&quadro CK.](https://attack.mitre.org/)

- Desa um alerta **de Severidade** conforme apropriado. 

- Quando cria a regra, o seu **Estado** é **Ativado** por padrão, o que significa que será executado imediatamente após terminar de criá-la. Se não quiser que seja executado imediatamente, selecione **Desativado**, e a regra será adicionada ao separador **regras Ative** e pode ativá-la a partir daí quando precisar.

   :::image type="content" source="media/tutorial-detect-threats-custom/general-tab.png" alt-text="Comece a criar uma regra de análise personalizada":::

## <a name="define-the-rule-query-logic-and-configure-settings"></a>Defina a lógica de consulta de regras e configurar as definições

No separador lógica de **regras Definida,** pode escrever uma consulta diretamente no campo **de consulta regra,** ou criar a consulta no Log Analytics e, em seguida, copiá-la e colá-la aqui.

- As consultas são escritas na Linguagem de Consulta Kusto (KQL). Saiba mais sobre [conceitos](/azure/data-explorer/kusto/concepts/) e [consultas KQL,](/azure/data-explorer/kusto/query/)e consulte este guia de referência [rápido](/azure/data-explorer/kql-quick-reference)e útil.

- O exemplo mostrado nesta imagem consulta a tabela *SecurityEvent* para exibir um tipo de eventos de [início de série falhados do Windows](/windows/security/threat-protection/auditing/event-4625).

   :::image type="content" source="media/tutorial-detect-threats-custom/set-rule-logic-tab-1-new.png" alt-text="Lógica e configuração da regra de consulta de configuração" lightbox="media/tutorial-detect-threats-custom/set-rule-logic-tab-all-1-new.png":::

- Aqui está outra consulta de amostra, que o alertaria quando um número anómalo de recursos é criado na [Atividade Azure](../azure-monitor/essentials/activity-log.md).

    ```kusto
    AzureActivity
    | where OperationName == "Create or Update Virtual Machine" or OperationName =="Create Deployment"
    | where ActivityStatus == "Succeeded"
    | make-series dcount(ResourceId)  default=0 on EventSubmissionTimestamp in range(ago(7d), now(), 1d) by Caller
    ```

    > [!NOTE]
    > #### <a name="rule-query-best-practices"></a>Consultas de regras melhores práticas
    > - O comprimento da consulta deve ter entre 1 e 10.000 caracteres e não pode conter `search *` " ou " `union *` Pode utilizar [funções definidas pelo utilizador](/azure/data-explorer/kusto/query/functions/user-defined-functions) para ultrapassar a limitação do comprimento da consulta.
    >
    > - A utilização de funções ADX para criar consultas do Azure Data Explorer dentro da janela de consulta Log Analytics **não é suportada**.
    >
    > - Ao utilizar a **`bag_unpack`** função numa consulta, se projetar as colunas como campos usando `project field1` " " e a coluna não existir, a consulta falhará. Para evitar que isto aconteça, deve projetar a coluna da seguinte forma:
    >   - `project field1 = column_ifexists("field1","")`

### <a name="alert-enrichment"></a>Alerta de enriquecimento

> [!IMPORTANT]
> As funcionalidades de enriquecimento de alerta estão atualmente em **PREVIEW**. Consulte os [Termos Complementares de Utilização para o Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) para termos legais adicionais aplicáveis às funcionalidades do Azure que estejam em versão beta, pré-visualização ou ainda não lançadas em disponibilidade geral.
    
- Utilize a secção de configuração **de mapeamento** da Entidade para mapear parâmetros dos resultados da sua consulta a entidades reconhecidas pelo Azure Sentinel. As entidades enriquecem a produção das regras (alertas e incidentes) com informação essencial que serve de bloco de construção de quaisquer processos de investigação e ações corretivas que se seguem. São também os critérios pelos quais pode agrupar alertas em incidentes no separador **De Definições de Incidentes.**

    Saiba mais sobre [as entidades em Azure Sentinel.](entities-in-azure-sentinel.md)

    Consulte [os campos de dados do Mapa às entidades do Azure Sentinel](map-data-fields-to-entities.md) para obter instruções completas de mapeamento de entidades, juntamente com informações importantes sobre [compatibilidade retrógrada.](map-data-fields-to-entities.md#notes-on-the-new-version)

- Utilize a secção de configuração **de detalhes personalizados** para extrair itens de dados de eventos da sua consulta e austá-los nos alertas produzidos por esta regra, dando-lhe visibilidade imediata do conteúdo do evento nos seus alertas e incidentes.

    Saiba mais sobre a divulgação de detalhes personalizados em alertas e consulte as [instruções completas](surface-custom-details-in-alerts.md).

### <a name="query-scheduling-and-alert-threshold"></a>Agendamento de consultas e limiar de alerta

- Na secção **de agendamento de consultas,** definir os seguintes parâmetros:

   :::image type="content" source="media/tutorial-detect-threats-custom/set-rule-logic-tab-2.png" alt-text="Definir horário de consulta e agrupamento de eventos" lightbox="media/tutorial-detect-threats-custom/set-rule-logic-tab-all-2-new.png":::

    - **Desacomo se questione** cada uma para controlar a frequência com que a consulta é executada - com a frequência de cada 5 minutos ou com frequência como uma vez a cada 14 dias.

    - Defina **os dados do Lookup do último** para determinar o período de tempo dos dados abrangidos pela consulta - por exemplo, pode consultar os últimos 10 minutos de dados, ou as últimas 6 horas de dados. O máximo é de 14 dias.

        > [!NOTE]
        > **Intervalos de consulta e período de retrocesso**
        >
        >  Estas duas configurações são independentes uma da outra, até um ponto. Pode executar uma consulta num curto intervalo que cubra um período de tempo mais longo do que o intervalo (na verdade, tendo consultas sobrepostas), mas não pode executar uma consulta num intervalo superior ao período de cobertura, caso contrário terá lacunas na cobertura geral da consulta.
        >
        > **Atraso na ingestão**
        >
        > Para ter em conta **a latência** que pode ocorrer entre a geração de um evento na fonte e a sua ingestão no Azure Sentinel, e para garantir uma cobertura completa sem duplicação de dados, o Azure Sentinel executa regras de análise programadas com um **atraso de cinco minutos** em função da hora programada.
        >
        > Para uma explicação técnica detalhada do porquê deste atraso ser necessário e como resolve este problema, consulte a excelente publicação de Ron Marsiano no blog sobre o assunto, " Lidar com o[atraso de ingestão em Azure Sentinel regras de alerta programadas](https://techcommunity.microsoft.com/t5/azure-sentinel/handling-ingestion-delay-in-azure-sentinel-scheduled-alert-rules/ba-p/2052851)".

- Utilize a secção **de limiar de alerta** para definir o nível de sensibilidade da regra. Por exemplo, **desacordo O alerta de Geração quando** o número de resultados de consulta é maior do **que** e introduza o número 1000 se quiser que a regra gere um alerta apenas se a consulta devolver mais de 1000 resultados cada vez que for executado. Este é um campo obrigatório, por isso, se não quiser definir um limiar – ou seja, se quiser que o seu alerta registe todos os eventos – introduza 0 no campo de números.
    
### <a name="results-simulation"></a>Simulação de resultados

Na área de simulação de **Resultados,** no lado direito do assistente, selecione **Teste com dados atuais** e Azure Sentinel irá mostrar-lhe um gráfico dos resultados (eventos de registo) que a consulta teria gerado ao longo das últimas 50 vezes que teria executado, de acordo com o calendário atualmente definido. Se modificar a consulta, selecione **Teste com os dados atuais** novamente para atualizar o gráfico. O gráfico mostra o número de resultados durante o período de tempo definido, que é determinado pelas definições na secção **de agendamento de consultas.**
  
Aqui está o que a simulação de resultados pode parecer para a consulta na imagem acima. O lado esquerdo é a vista padrão, e o lado direito é o que se vê quando se paira sobre um ponto no tempo no gráfico.

:::image type="content" source="media/tutorial-detect-threats-custom/results-simulation.png" alt-text="Imagens de simulação de resultados":::

Se vir que a sua consulta desencadearia alertas demasiado ou demasiado frequentes, pode experimentar as definições nas secções de **agendamento** de consultas e **de alerta** e selecionar o Teste com os [dados](#query-scheduling-and-alert-threshold) **atuais** novamente.

### <a name="event-grouping-and-rule-suppression"></a>Agrupamento de eventos e supressão de regras

> [!IMPORTANT]
> O agrupamento de eventos está atualmente em **PREVIEW**. Consulte os [Termos Complementares de Utilização para o Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) para termos legais adicionais aplicáveis às funcionalidades do Azure que estejam em versão beta, pré-visualização ou ainda não lançadas em disponibilidade geral.
    
- No **âmbito do agrupamento event,** escolha uma de duas formas de lidar com o agrupamento de **eventos** em **alertas:** 

    - **Agrupe todos os eventos num único alerta** (a definição predefinida). A regra gera um único alerta cada vez que executa, desde que a consulta retorne mais resultados do que o limiar de **alerta** especificado acima. O alerta inclui um resumo de todos os eventos devolvidos nos resultados. 

    - **Acionar um alerta para cada evento**. A regra gera um alerta único para cada evento devolvido pela consulta. Isto é útil se quiser que os eventos sejam exibidos individualmente, ou se pretender agrupar os mesmos por determinados parâmetros - por utilizador, nome de anfitrião ou outra coisa qualquer. Pode definir estes parâmetros na consulta.
    
        Atualmente, o número de alertas que uma regra pode gerar está limitado a 20. Se, numa regra específica, **o agrupamento de eventos** estiver definido **para desencadear um alerta para cada evento**, e a consulta da regra retorna mais de 20 eventos, cada um dos primeiros 19 eventos gerará um alerta único, e o 20º alerta irá resumir todo o conjunto de eventos devolvidos. Por outras palavras, o 20º alerta é o que teria sido gerado no âmbito do **Grupo todos os eventos numa única** opção de alerta.

    > [!NOTE]
    > Qual é a diferença entre **eventos** e **alertas?**
    >
    > - Um **evento** é uma descrição de uma única ocorrência de uma ação. Por exemplo, uma única entrada num ficheiro de registo pode contar como um evento. Neste contexto, um evento refere-se a um único resultado devolvido por uma consulta numa regra de análise.
    >
    > - Um **alerta** é uma coleção de eventos que, juntos, são significativos do ponto de vista da segurança. Um alerta poderia conter um único evento se o evento tivesse implicações significativas em segurança - um login administrativo de um país estrangeiro fora do horário de expediente, por exemplo.
    >
    > - A propósito, o que são **incidentes?** A lógica interna de Azure Sentinel cria **incidentes** a partir de **alertas** ou grupos de alertas. A fila dos incidentes é o ponto focal do trabalho dos analistas da SOC - triagem, investigação e reparação.
    > 
    > O Azure Sentinel ingere eventos crus de algumas fontes de dados, e já processados alertas de outras pessoas. É importante notar com qual está a lidar a qualquer momento.

- Na secção **Supressão,** pode rodar a **consulta stop running após o alerta for gerada** definição **Se,** uma vez que recebe um alerta, pretender suspender o funcionamento desta regra por um período de tempo superior ao intervalo de consulta. Se ligar isto, deve definir **parar de fazer a consulta durante** o tempo que a consulta deve parar de funcionar, até 24 horas.

## <a name="configure-the-incident-creation-settings"></a>Configurar as definições de criação de incidentes

No separador **Definições de Incidentes,** pode escolher se e como o Azure Sentinel transforma alertas em incidentes acionáveis. Se este separador for deixado em paz, o Azure Sentinel criará um único incidente separado de cada alerta. Pode optar por não ter incidentes criados, ou agrupar vários alertas num único incidente, alterando as definições neste separador.

> [!IMPORTANT]
> O separador de definições de incidentes encontra-se atualmente em **PREVIEW**. Consulte os [Termos Complementares de Utilização para o Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) para termos legais adicionais aplicáveis às funcionalidades do Azure que estejam em versão beta, pré-visualização ou ainda não lançadas em disponibilidade geral.

:::image type="content" source="media/tutorial-detect-threats-custom/incident-settings-tab.png" alt-text="Defina as definições de agrupamento de incidentes e alerta":::

### <a name="incident-settings"></a>Definições de incidentes

Na secção **de definições de incidentes,** **crie incidentes a partir de alertas desencadeados por esta regra de análise** é definido por padrão para **Enabled**, o que significa que O Azure Sentinel criará um único incidente separado de cada alerta desencadeado pela regra.
    
- Se não quiser que esta regra resulte na criação de incidentes (por exemplo, se esta regra for apenas para recolher informações para análise posterior), desabro quanto a **Deficientes**.

- Se quiser que um único incidente seja criado a partir de um grupo de alertas, em vez de um para cada alerta, consulte a secção seguinte.

### <a name="alert-grouping"></a>Agrupamento de alerta

Na secção **de agrupamento de alertas,** se pretender que um único incidente seja gerado a partir de um grupo de até 150 alertas semelhantes ou recorrentes (ver nota), detete **os alertas relacionados com o Grupo, desencadeados por esta regra de análise, em incidentes** a **Enabled,** e definir os seguintes parâmetros.

- **Limitar o grupo aos alertas criados dentro do prazo selecionado**: Determine o prazo dentro do qual os alertas semelhantes ou recorrentes serão agrupados. Todos os alertas correspondentes dentro deste prazo gerarão colectivamente um incidente ou um conjunto de incidentes (dependendo das definições de agrupamento abaixo). Alertas fora deste período de tempo gerarão um incidente separado ou conjunto de incidentes.

- **Alertas de grupo desencadeados por esta regra de análise num único incidente:** Escolha a base em que os alertas serão agrupados em conjunto:

    - **O grupo alerta para um único incidente se todas as entidades corresponderem:** Os alertas são agrupados se partilharem valores idênticos para cada uma das entidades mapeadas (definidas no separador lógica de regra definida acima). Esta é a definição recomendada.

    - **Grupo todos os alertas desencadeados por esta regra num único incidente**: Todos os alertas gerados por esta regra são agrupados mesmo que não partilhem valores idênticos.

    - **O grupo alerta para um único incidente se as entidades selecionadas corresponderem:** Os alertas são agrupados se partilharem valores idênticos para algumas das entidades mapeadas (que pode selecionar a partir da lista de suspensos). É melhor utilizar esta definição se, por exemplo, pretender criar incidentes separados com base nos endereços IP de origem ou alvo.

- **Reaberta incidentes de correspondência fechados**: Se um incidente tiver sido resolvido e fechado, e mais tarde em outro alerta for gerado que deve pertencer a esse incidente, defina esta definição para **Enabled** se quiser que o incidente fechado seja reaberto, e deixe como **Desativado** se quiser que o alerta crie um novo incidente.
    
    > [!NOTE]
    > **Até 150 alertas** podem ser agrupados num único incidente. Se mais de 150 alertas forem gerados por uma regra que os agrupará num único incidente, um novo incidente será gerado com os mesmos detalhes do incidente que o original, e os alertas em excesso serão agrupados no novo incidente.

## <a name="set-automated-responses-and-create-the-rule"></a>Desateia respostas automatizadas e criaa a regra

1. No separador **respostas automatizadas,** selecione quaisquer livros de reprodução que pretenda executar automaticamente quando um alerta é gerado pela regra personalizada. Para obter mais informações sobre a criação e automatização de livros, consulte [Responder às ameaças](tutorial-respond-threats-playbook.md).

    :::image type="content" source="media/tutorial-detect-threats-custom/automated-response-tab.png" alt-text="Definir as definições de resposta automatizada":::

1. Selecione **Rever e crie** para rever todas as definições da sua nova regra de alerta. Quando aparecer a mensagem "Validação passada", selecione **Criar** para rubricar a sua regra de alerta.

    :::image type="content" source="media/tutorial-detect-threats-custom/review-and-create-tab.png" alt-text="Reveja todas as definições e crie a regra":::

## <a name="view-the-rule-and-its-output"></a>Ver a regra e a sua saída
  
- Pode encontrar a sua regra personalizada recém-criada (do tipo "Agendado") na tabela sob o separador **regras Ative** no ecrã principal de **Analytics.** A partir desta lista pode ativar, desativar ou eliminar cada regra.

- Para ver os resultados das regras de alerta que cria, vá à página **Incidentes,** onde pode triagem, investigação de [incidentes](tutorial-investigate-cases.md)e remediar as ameaças.

> [!NOTE]
> Os alertas gerados no Azure Sentinel estão disponíveis através da [Microsoft Graph Security](/graph/security-concept-overview). Para obter mais informações, consulte a documentação de [alertas de segurança do gráfico da Microsoft](/graph/api/resources/security-api-overview).

## <a name="troubleshooting"></a>Resolução de problemas

### <a name="issue-no-events-appear-in-query-results"></a>Problema: Não aparecem eventos nos resultados da consulta

Se **o agrupamento de eventos** estiver definido para desencadear um alerta para cada **evento**, em certos cenários, ao visualizar os resultados da consulta posteriormente (como quando voltar a alertar de um incidente), é possível que não apareçam resultados de consulta. Isto porque a ligação do evento ao alerta é realizada pelo hashing da informação do evento em particular, e pela inclusão do haxixe na consulta. Se os resultados da consulta tiverem mudado desde que o alerta foi gerado, o haxixe deixará de ser válido e não serão apresentados resultados. 

Para ver os eventos, retire manualmente a linha com o haxixe da consulta da regra e execute a consulta.

### <a name="issue-a-scheduled-rule-failed-to-execute-or-appears-with-auto-disabled-added-to-the-name"></a>Problema: Uma regra programada não executou ou aparece com AUTO DISABLED adicionado ao nome

É uma ocorrência rara que uma regra de consulta programada não funciona, mas pode acontecer. O Azure Sentinel classifica as falhas na frente como transitórias ou permanentes, com base no tipo específico da falha e nas circunstâncias que o levaram.

#### <a name="transient-failure"></a>Falha transitória

Uma falha transitória ocorre devido a uma circunstância que é temporária e em breve voltará ao normal, altura em que a execução da regra será bem sucedida. Alguns exemplos de falhas que Azure Sentinel classifica como transitórios são:

- Uma consulta de regras demora muito tempo a esgotar-se e a esgotar-se.
- Problemas de conectividade entre fontes de dados e Log Analytics, ou entre Log Analytics e Azure Sentinel.
- Qualquer outra falha nova e desconhecida é considerada transitória.

Em caso de falha transitória, Azure Sentinel continua a tentar executar a regra novamente após intervalos pré-determinados e cada vez maiores, até um ponto. Depois disso, a regra só voltará a ser executada na sua próxima hora programada. Uma regra nunca será desativada automaticamente devido a uma falha transitória.

#### <a name="permanent-failure---rule-auto-disabled"></a>Falha permanente - regra auto-desativada

Uma falha permanente ocorre devido a uma alteração das condições que permitem a execução da regra, que sem intervenção humana não voltará ao seu estatuto anterior. Seguem-se alguns exemplos de falhas que são classificadas como permanentes:

- O espaço de trabalho alvo (sobre o qual a consulta de regras funcionou) foi eliminado.
- A tabela-alvo (sobre a qual a consulta de regras funcionou) foi suprimida.
- Azure Sentinel tinha sido removido do espaço de trabalho do alvo.
- Uma função utilizada pela consulta de regras já não é válida; foi modificado ou removido.
- As permissões a uma das fontes de dados da consulta de regras foram alteradas.
- Uma das fontes de dados da consulta de regras foi eliminada ou desligada.

**Em caso de um número pré-determinado de falhas permanentes consecutivas, do mesmo tipo e da mesma regra,** Azure Sentinel para de tentar executar a regra, e também toma os seguintes passos:

- Desativa a regra.
- Adiciona as palavras **"AUTO DISABLED"** ao início do nome da regra.
- Acrescenta a razão da falha (e da desativação) à descrição da regra.

Pode determinar facilmente a presença de quaisquer regras de desativadas automáticas, classificando a lista de regras pelo nome. As regras de desativação automática estarão no topo da lista.

Os gestores da SOC devem verificar regularmente a lista de regras para a presença de regras de desativação automática.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, aprendeu a detetar ameaças usando Azure Sentinel.

- Saiba como [investigar incidentes em Azure Sentinel.](tutorial-investigate-cases.md)
- Conheça as [entidades em Azure Sentinel.](entities-in-azure-sentinel.md)
- Saiba como [configurar respostas automáticas de ameaças em Azure Sentinel](tutorial-respond-threats-playbook.md).
