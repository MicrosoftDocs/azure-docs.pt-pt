---
title: Azure HDInsight para Código de Estúdio Visual
description: Saiba como utilizar as Ferramentas de Colmeia Spark & (Azure HDInsight) para Código de Estúdio Visual. Utilize as ferramentas para criar e submeter consultas e scripts.
ms.service: hdinsight
ms.topic: how-to
ms.date: 10/20/2020
ms.custom: devx-track-python
ms.openlocfilehash: d7e2bd2987e7eaf7798093f3918f72b7e09a3cb0
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/13/2021
ms.locfileid: "107313485"
---
# <a name="use-spark--hive-tools-for-visual-studio-code"></a>Use ferramentas de colmeia spark & para código de estúdio visual

Aprenda a usar Apache Spark & Hive Tools para Código de Estúdio Visual. Use as ferramentas para criar e submeter trabalhos de lote apache Hive, consultas interativas de Hive e scripts PySpark para Apache Spark. Primeiro, descreveremos como instalar o Spark & Hive Tools no Código do Estúdio Visual. Depois vamos passar por como submeter empregos à Spark & Hive Tools.  

Spark & Hive Tools podem ser instalados em plataformas que são suportadas pelo Código do Estúdio Visual. Note os seguintes pré-requisitos para diferentes plataformas.

## <a name="prerequisites"></a>Pré-requisitos

São necessários os seguintes itens para completar as etapas deste artigo:

- Um cluster do Azure HDInsight. Para criar um cluster, consulte [Começar com HDInsight](hadoop/apache-hadoop-linux-create-cluster-get-started-portal.md). Ou use um cluster spark and Hive que suporte um ponto final Apache Livy.
- [Visual Studio Code](https://code.visualstudio.com/).
- [Mono.](https://www.mono-project.com/docs/getting-started/install/) Mono é necessário apenas para Linux e macOS.
- [Um ambiente interativo PySpark para Código de Estúdio Visual.](set-up-pyspark-interactive-environment.md)
- Um diretório local. Este artigo utiliza  **C:\HD\HDexample**.

## <a name="install-spark--hive-tools"></a>Instalar ferramentas de colmeia & faísca

Depois de conhecer os pré-requisitos, pode instalar Spark & Hive Tools for Visual Studio Code seguindo estes passos:

1. Abra o Visual Studio Code.

2. A partir da barra de menu, navegue para **ver**  >  **extensões.**

3. Na caixa de pesquisa, **insira Spark & Hive**.

4. Selecione **Spark & Hive Tools** a partir dos resultados da pesquisa e, em seguida, selecione **Instalar**:

   :::image type="content" source="./media/hdinsight-for-vscode/install-hdInsight-plugin.png" alt-text="Spark & Hive para instalação visual Studio Code Python":::

5. Selecione **Recarregar** quando necessário.

## <a name="open-a-work-folder"></a>Abrir uma pasta de trabalho

Para abrir uma pasta de trabalho e criar um ficheiro no Código do Estúdio Visual, siga estes passos:

1. A partir da barra de menu, navegue para **a** pasta  >  **'Arquivo' ...**  >  **C:\HD\HDexample**, e, em seguida, selecione o botão **Select Folder.** A pasta aparece na vista **Explorer** à esquerda.

2. Na visualização **do Explorer,** selecione a pasta **HDexample** e, em seguida, selecione o ícone **Novo Ficheiro** ao lado da pasta de trabalho:

   :::image type="content" source="./media/hdinsight-for-vscode/visual-studio-code-new-file.png" alt-text="ícone de arquivo novo código de estúdio visual":::

3. Nomeie o novo ficheiro utilizando a extensão do `.hql` ficheiro (Hive consultas) ou a extensão do `.py` ficheiro (Script Spark). Este exemplo utiliza **HelloWorld.hql**.

## <a name="set-the-azure-environment"></a>Definir o ambiente Azure

Para um utilizador nacional de nuvem, siga estes passos para definir primeiro o ambiente Azure e, em seguida, use o **Azure: Iniciar sê-lo** no comando para iniciar sação no Azure:

1. Navegar **para**  >  **definições de preferências**  >  **de ficheiros**.
2. Pesse na seguinte cadeia: **Azure: Cloud**.
3. Selecione a nuvem nacional da lista:

   :::image type="content" source="./media/hdinsight-for-vscode/set-default-login-entry-configuration.png" alt-text="Definir configuração de entrada de login predefinido":::

## <a name="connect-to-an-azure-account"></a>Ligar a uma conta do Azure

Antes de poder submeter scripts aos seus clusters a partir do Código do Estúdio Visual, o utilizador pode iniciar sposição na subscrição do Azure ou [ligar um cluster HDInsight](#link-a-cluster). Utilize o nome de utilizador/palavra-passe ambari ou a credencial de domínio para o cluster ESP ligar ao seu cluster HDInsight. Siga estes passos para ligar a Azure:

1. A partir da barra de menu, navegue para **ver**  >  **paleta de comando...** e insira **Azure: Iniciar Sedura**:

   :::image type="content" source="./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-login.png" alt-text="Ferramentas de hive spark & para login visual studio code":::

2. Siga as instruções de inscrição para iniciar seduca. Depois de conectado, o nome da sua conta Azure aparece na barra de estado na parte inferior da janela Visual Studio Code.  

## <a name="link-a-cluster"></a>Ligue um cluster

### <a name="link-azure-hdinsight"></a>Link: Azure HDInsight

Pode ligar um cluster normal utilizando um nome de utilizador gerido por [Apache Ambari,](https://ambari.apache.org/)ou pode ligar um cluster Hadoop seguro do Pacote de Segurança Empresarial utilizando um nome de utilizador de domínio (como: `user1@contoso.com` ).

1. A partir da barra de menu, navegue para **ver**  >  **paleta de comando...** e insira **Faísca/ Colmeia: Link a Cluster**.

   :::image type="content" source="./media/hdinsight-for-vscode/link-cluster-command.png" alt-text="Comando de cluster de ligação de paleta de comando":::

2. Selecione o tipo de cluster ligado **Azure HDInsight**.

3. Introduza o URL do cluster HDInsight.

4. Insira o seu nome de utilizador Ambari; o padrão é **administrador.**

5. Insira a sua senha Ambari.

6. Selecione o tipo de cluster.

7. Desa quando se pode definir o nome de visualização do cluster (opcional).

8. Rever a vista **DE SAÍDA** para verificação.

   > [!NOTE]  
   > O nome de utilizador e a palavra-passe ligados são utilizados se o cluster iniciar sessão na subscrição do Azure e ligar um cluster.  

### <a name="link-generic-livy-endpoint"></a>Link: Genérico Livy endpoint

1. A partir da barra de menu, navegue para **ver**  >  **paleta de comando...** e insira **Faísca/ Colmeia: Link a Cluster**.

2. Selecione o tipo de cluster **ligado Genérico Livy Endpoint**.

3. Insira o ponto final genérico da Livy. Por exemplo: http \: //10.172.41.42:18080.

4. Selecione o tipo de autorização **Básico** ou **Nenhum**.  Se selecionar **Basic**:  
   
   1. Insira o seu nome de utilizador Ambari; o padrão é **administrador.**  

   2. Insira a sua senha Ambari.

5. Rever a vista **DE SAÍDA** para verificação.

## <a name="list-clusters"></a>Agrupamentos de listas

1. A partir da barra de menu, navegue para **ver**  >  **paleta de comando...** e insira **Faísca/ Colmeia: Cluster de Listas**.

2. Selecione a subscrição que deseja.

3. Reveja a vista **OUTPUT.** Esta vista mostra o seu cluster (ou clusters) ligados e todos os clusters sob a sua assinatura Azure:

   :::image type="content" source="./media/hdinsight-for-vscode/list-cluster-result1.png" alt-text="Desafine uma configuração de cluster padrão":::

## <a name="set-the-default-cluster"></a>Desafine o cluster predefinido

1. Reabra a pasta **HDexample** que foi discutida [anteriormente,](#open-a-work-folder)se fechada.  

2. Selecione o ficheiro **HelloWorld.hql** que foi criado [anteriormente](#open-a-work-folder). Abre no editor de guiões.

3. Clique com o botão direito no editor de script e, em seguida, selecione **Spark / Hive: set Default Cluster**.  

4. [Ligue-se](#connect-to-an-azure-account) à sua conta Azure ou ligue um cluster se ainda não o fez.

5. Selecione um cluster como o cluster predefinido para o ficheiro de script atual. As ferramentas atualizam automaticamente o **.VSCode\settings.jsno** ficheiro de configuração:

   :::image type="content" source="./media/hdinsight-for-vscode/set-default-cluster-configuration.png" alt-text="Definir configuração de cluster padrão":::

## <a name="submit-interactive-hive-queries-and-hive-batch-scripts"></a>Submeta consultas interativas de Colmeia e scripts de lote de Colmeia

Com spark & Hive Tools for Visual Studio Code, pode submeter consultas interativas de Hive e scripts de lote de Colmeia aos seus clusters.

1. Reabra a pasta **HDexample** que foi discutida [anteriormente,](#open-a-work-folder)se fechada.  

2. Selecione o ficheiro **HelloWorld.hql** que foi criado [anteriormente](#open-a-work-folder). Abre no editor de guiões.

3. Copie e cole o seguinte código no seu ficheiro Hive e, em seguida, guarde-o:

   ```hiveql
   SELECT * FROM hivesampletable;
   ```

4. [Ligue-se](#connect-to-an-azure-account) à sua conta Azure ou ligue um cluster se ainda não o fez.

5. Clique com o direito no editor de scripts e **selecione Hive: Interactive** para submeter a consulta, ou use o atalho de teclado Ctrl+Alt+I.  **Selecione Hive: Lote** para submeter o script, ou use o atalho do teclado Ctrl+Alt+H.  

6. Se não especificou um cluster predefinido, selecione um cluster. As ferramentas também permitem submeter um bloco de código em vez de todo o ficheiro de script, utilizando o menu de contexto. Após alguns momentos, os resultados da consulta aparecem num novo separador:

   :::image type="content" source="./media/hdinsight-for-vscode/interactive-hive-result.png" alt-text="Resultado interativo da consulta da Colmeia Apache":::

   - **Painel de resultados:** Pode guardar todo o resultado como ficheiro CSV, JSON ou Excel para um caminho local ou apenas selecionar várias linhas.

   - **Painel DE MENSAGEÇÕES:** Quando seleciona um número **de Linha,** salta para a primeira linha do script de execução.

## <a name="submit-interactive-pyspark-queries"></a>Submeter consultas interativas pySpark

Os utilizadores podem realizar a interativa PySpark das seguintes formas:

### <a name="using-the-pyspark-interactive-command-in-py-file"></a>Usando o comando interativo PySpark em ficheiro PY
Utilizando o comando interativo PySpark para submeter as consultas, siga estes passos:

1. Reabra a pasta **HDexample** que foi discutida [anteriormente,](#open-a-work-folder)se fechada.  

2. Crie um novo ficheiro **HelloWorld.py,** seguindo os passos [anteriores.](#open-a-work-folder)

3. Copiar e colar o seguinte código no ficheiro do script:

   ```python
   from operator import add
   lines = spark.read.text("/HdiSamples/HdiSamples/FoodInspectionData/README").rdd.map(lambda r: r[0])
   counters = lines.flatMap(lambda x: x.split(' ')) \
                .map(lambda x: (x, 1)) \
                .reduceByKey(add)

   coll = counters.collect()
   sortedCollection = sorted(coll, key = lambda r: r[1], reverse = True)

   for i in range(0, 5):
        print(sortedCollection[i])
   ```

4. O pedido de instalação do núcleo PySpark/Synapse Pyspark é apresentado no canto inferior direito da janela. Pode clicar no botão **Instalar** para proceder às instalações PySpark/Synapse Pyspark; ou clique no botão **Saltar** para saltar este passo.

   :::image type="content" source="./media/hdinsight-for-vscode/install-the-pyspark-kernel.png" alt-text="A screenshot mostra uma opção para saltar a instalação PySpark.":::

5. Se precisar de o instalar mais tarde, pode navegar para  >  Definições **de Preferência**  >  de Ficheiros , em seguida,**desmarque** **a configuração HDInsight: Ative a instalação Skip Pyspark** nas definições. 
    
    :::image type="content" source="./media/hdinsight-for-vscode/enable-skip-pyspark-installation.png" alt-text="A screenshot mostra a opção de Ativar a Instalação Skip Pyspark.":::

6. Se a instalação for bem sucedida no passo 4, a caixa de mensagens "PySpark instalada com sucesso" é exibida no canto inferior direito da janela. Clique no botão **Recarregar** para recarregar a janela.

   :::image type="content" source="./media/hdinsight-for-vscode/pyspark-kernel-installed-successfully.png" alt-text="pyspark instalado com sucesso":::


7. A partir da barra de menu, navegue para **ver**  >  **paleta de comando...** ou use o atalho de teclado **Shift + Ctrl + P** e **introduza Python: Selecione Intérprete para iniciar o Jupyter Server**.

   :::image type="content" source="./media/hdinsight-for-vscode/select-interpreter-to-start-jupyter-server.png" alt-text="selecione intérprete para iniciar o servidor jupyter":::

8. Selecione a opção python abaixo.

   :::image type="content" source="./media/hdinsight-for-vscode/choose-the-below-option.png" alt-text="escolher a opção abaixo":::
    
9. A partir da barra de menu, navegue para **ver**  >  **paleta de comando...** ou use o atalho de teclado **Shift + Ctrl + P** e **introduza Developer: Reload Window**.

    :::image type="content" source="./media/hdinsight-for-vscode/reload-window.png" alt-text="janela de recarga":::

10. [Ligue-se](#connect-to-an-azure-account) à sua conta Azure ou ligue um cluster se ainda não o fez.

11. Selecione todo o código, clique com o direito no editor de scripts e selecione **Spark: PySpark Interactive / Synapse: Pyspark Interactive** para submeter a consulta. 

    :::image type="content" source="./media/hdinsight-for-vscode/pyspark-interactive-right-click.png" alt-text="menu de contexto interativo pyspark":::

12. Selecione o cluster, se não tiver especificado um cluster predefinido. Após alguns momentos, os resultados da **Python Interactive** aparecem num novo separador. Clique no PySpark para mudar o núcleo para **PySpark / Synapse Pyspark**, e o código será executado com sucesso. Se quiser mudar para o kernel Synapse Pyspark, é encorajado a desativação das definições automáticas no portal Azure. Caso contrário, pode demorar muito tempo a acordar o cluster e a definir o núcleo de sinapse pela primeira vez. Se as ferramentas também permitirem submeter um bloco de código em vez de todo o ficheiro de script, utilizando o menu de contexto:

    :::image type="content" source="./media/hdinsight-for-vscode/pyspark-interactive-python-interactive-window.png" alt-text="pyspark janela interativa python":::

13. Introduza **%%info** e, em seguida, pressione Shift+Enter para ver as informações de trabalho (opcional):

    :::image type="content" source="./media/hdinsight-for-vscode/pyspark-interactive-view-job-information.png" alt-text="pyspark interactive view informações de trabalho":::

A ferramenta também suporta a consulta **Spark SQL:**

  :::image type="content" source="./media/hdinsight-for-vscode/pyspark-ineteractive-select-result.png" alt-text="pyspark resultado da visão interativa":::


### <a name="perform-interactive-query-in-py-file-using-a--comment"></a>Realize consulta interativa em ficheiro PY usando um comentário #%%

1. Adicione **#%%** antes do código Py para obter experiência de caderno.

   :::image type="content" source="./media/hdinsight-for-vscode/run-cell.png" alt-text="adicionar #%%":::

2. Clique em **Run Cell**. Após alguns momentos, os resultados da Python Interactive aparecem num novo separador. Clique no PySpark para mudar o núcleo para PySpark/Synapse PySpark, em seguida, clique novamente em **Run Cell** e o código será executado com sucesso.

   :::image type="content" source="./media/hdinsight-for-vscode/run-cell-get-results.png" alt-text="executar resultados celulares":::

## <a name="leverage-ipynb-support-from-python-extension"></a>Alavancagem suporte IPYNB da extensão Python

1. Pode criar um Bloco de Notas Jupyter por comando a partir da Paleta de Comando ou criando um novo ficheiro .ipynb no seu espaço de trabalho. Para mais informações, consulte [Trabalhar com cadernos Jupyter no Código do Estúdio Visual](https://code.visualstudio.com/docs/python/jupyter-support)

2. Clique no botão **de célula Executar,** siga as instruções para **definir a piscina de faíscas predefinitivamente** (encoraje fortemente a definir o cluster/piscina predefinido sempre antes de abrir um portátil) e, em seguida, **recarregar** a janela.

   :::image type="content" source="./media/hdinsight-for-vscode/set-the-default-spark-pool-and-reload.png" alt-text="definir a piscina de faísca padrão e recarregar":::

3. Clique no PySpark para mudar o kernel para **PySpark / Synapse Pyspark**, e, em seguida, clique em **Run Cell**, depois de um tempo, o resultado será exibido.

   :::image type="content" source="./media/hdinsight-for-vscode/run-ipynb-file-results.png" alt-text="executar resultados ipynb":::


> [!NOTE]
>
> [ms-toolsai.jupyter >versão 2021.3.684299474 não é suportado nesta extensão](#known-issues) é um problema conhecido. Por favor, utilize o kernel synapse mantendo-se fiel ao Microsoft Jupyter 2021.3.684299474.

## <a name="submit-pyspark-batch-job"></a>Submeter trabalho de lote PySpark

1. Reabra a pasta **HDexample** que discutiu [anteriormente,](#open-a-work-folder)se estiver fechada.  

2. Crie um novo ficheiro **BatchFile.py** seguindo os passos [anteriores.](#open-a-work-folder)

3. Copiar e colar o seguinte código no ficheiro do script:

   ```python
   from __future__ import print_function
   import sys
   from operator import add
   from pyspark.sql import SparkSession
   if __name__ == "__main__":
       spark = SparkSession\
           .builder\
           .appName("PythonWordCount")\
           .getOrCreate()
   
       lines = spark.read.text('/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv').rdd.map(lambda r: r[0])
       counts = lines.flatMap(lambda x: x.split(' '))\
                  .map(lambda x: (x, 1))\
                   .reduceByKey(add)
       output = counts.collect()
       for (word, count) in output:
           print("%s: %i" % (word, count))
       spark.stop()
   ```

4. [Ligue-se](#connect-to-an-azure-account) à sua conta Azure ou ligue um cluster se ainda não o fez.

5. Clique com o botão direito no editor de script e, em seguida, selecione **Spark: PySpark Batch**, ou **Synapse: PySpark Batch****.

6. Selecione um cluster/piscina de faíscas para submeter o seu trabalho PySpark para:

   :::image type="content" source="./media/hdinsight-for-vscode/submit-pythonjob-result.png" alt-text="Submeter saída de resultado de trabalho python":::

Depois de submeter um trabalho python, os registos de submissão aparecem na janela **OUTPUT** no Código do Estúdio Visual. O URL spark UI e o URL de UI de fio também são mostrados. Se submeter o trabalho de lote a uma piscina Apache Spark, o URL UI da história da faísca e o URL UI da Aplicação de Emprego de Faísca também são mostrados. Pode abrir o URL num navegador web para rastrear o estado do trabalho.

## <a name="integrate-with-hdinsight-identity-broker-hib"></a>Integre-se com o HdInsight Identity Broker (HIB)

### <a name="connect-to-your-hdinsight-esp-cluster-with-id-broker-hib"></a>Conecte-se ao seu cluster HDInsight ESP com o ID Broker (HIB)

Pode seguir os passos normais para iniciar seducação na subscrição do Azure para ligar ao seu cluster HDInsight ESP com o ID Broker (HIB). Após o sinstrução, verá a lista de clusters no Azure Explorer. Para obter mais instruções, consulte [Connect to your HDInsight cluster](#connect-to-an-azure-account).

### <a name="run-a-hivepyspark-job-on-an-hdinsight-esp-cluster-with-id-broker-hib"></a>Executar um trabalho de Hive/PySpark num cluster HDInsight ESP com ID Broker (HIB)

Para executar um trabalho de colmeia, pode seguir os passos normais para submeter o trabalho ao cluster HDInsight ESP com ID Broker (HIB). Consulte [as consultas interativas de Colmeia e os scripts do lote de Colmeia](#submit-interactive-hive-queries-and-hive-batch-scripts) para obter mais instruções.

Para executar um trabalho pySpark interativo, você pode seguir os passos normais para submeter o trabalho ao cluster HDInsight ESP com ID Broker (HIB). Consulte [as consultas interativas do PySpark](#submit-interactive-pyspark-queries) para obter mais instruções.

Para executar uma função de lote PySpark, pode seguir os passos normais para submeter o trabalho ao cluster HDInsight ESP com ID Broker (HIB). Consulte o [trabalho de lote PySpark](#submit-pyspark-batch-job) para obter mais instruções.

## <a name="apache-livy-configuration"></a>Configuração Apache Livy

A configuração [Apache Livy](https://livy.incubator.apache.org/) é suportada. Pode configurá-lo no **.VSCode\settings.jsarquivado na** pasta do espaço de trabalho. Atualmente, a configuração livy apenas suporta o script Python. Para mais informações, consulte [Livy README.](https://github.com/cloudera/livy/blob/master/README.rst )

<a id="triggerlivyconf"></a>**Como desencadear a configuração da Livy**

### <a name="method-1"></a>Método 1  

1. A partir da barra de menu, navegue para **definições**  >  **de preferências**  >  **de ficheiros**.
2. Na caixa **de definições de pesquisa,** insira **HDInsight Job Submission: Livy Conf**.  
3. Selecione **Editar em settings.js** para obter o resultado de pesquisa relevante.

### <a name="method-2"></a>Método 2

Envie um ficheiro e note que a `.vscode` pasta é automaticamente adicionada à pasta de trabalho. Pode ver a configuração Livy selecionando **.vscode\settings.jsem**.

- As definições do projeto:

  :::image type="content" source="./media/hdinsight-for-vscode/hdi-apache-livy-config.png" alt-text="Configuração HDInsight Apache Livy":::

  >[!NOTE]
  >Para o **controlador Configurações demória** e **executorMessor,** defina o valor e a unidade. Por exemplo: 1g ou 1024m.

- Configurações livy suportadas:

  **POST /lotes**
  
  **Corpo de pedido**

  | name | descrição | tipo |
  | --- | --- | --- |
  | file | Ficheiro que contém o pedido de execução | Caminho (obrigatório) |
  | proxyUser | Utilizador a personificar ao executar o trabalho | String |
  | nome de classeName | Aplicação Java/Spark classe principal | String |
  | args | Argumentos de linha de comando para a aplicação | Lista de cordas |
  | frascos | Frascos para serem usados nesta sessão | Lista de cordas | 
  | pyFiles | Ficheiros Python a serem usados nesta sessão | Lista de cordas |
  | ficheiros | Ficheiros a serem usados nesta sessão | Lista de cordas |
  | motoristaMemory | Quantidade de memória para usar para o processo do condutor | String |
  | driverCores | Número de núcleos para utilizar para o processo do condutor | int |
  | executorMemory | Quantidade de memória para utilizar por processo executor | String |
  | executorCores | Número de núcleos para utilizar para cada executor | int |
  | numExecutors | Número de executores para lançar para esta sessão | int |
  | arquivos | Arquivos a serem usados nesta sessão | Lista de cordas |
  | fila | Nome da fila YARN a submeter| String |
  | name | Nome desta sessão | String |
  | conf | Propriedades de configuração de faíscas | Mapa de key=val |

  **Corpo de resposta** O objeto lote criado.

  | name | descrição | tipo |
  | --- | ---| --- |
  | ID | ID de sessão | int |
  | appId | ID de aplicação desta sessão | String |
  | appInfo | Informação detalhada da aplicação | Mapa de key=val |
  | log | Linhas de registo | Lista de cordas |
  | state |Estado do lote | String |

  > [!NOTE]
  > O livy config atribuído é apresentado no painel de saída quando submete o script.

## <a name="integrate-with-azure-hdinsight-from-explorer"></a>Integre com Azure HDInsight do Explorer

Pode visualizar a Tabela de Colmeia nos seus clusters diretamente através do explorador **Azure HDInsight:**

1. [Ligue-se](#connect-to-an-azure-account) à sua conta Azure se ainda não o fez.

2. Selecione o ícone **Azure** da coluna mais à esquerda.

3. A partir do painel esquerdo, expanda **o AZURE: HDINSIGHT**. As subscrições e clusters disponíveis estão listados.

4. Expanda o cluster para ver a base de dados de metadados da Hive e o esquema de tabela.

5. Clique à direita na mesa da Colmeia. Por exemplo: **hivesmpletable**. Selecione **Pré-visualização**.

   :::image type="content" source="./media/hdinsight-for-vscode/hdinsight-for-vscode-preview-hive-table.png" alt-text="Spark & Colmeia para a mesa de pré-visualização do Código do Estúdio Visual":::

6. A janela **de pré-visualização resultados** abre::

   :::image type="content" source="./media/hdinsight-for-vscode/hdinsight-for-vscode-preview-results-window.png" alt-text="Spark & Hive para janela de pré-visualização do Código do Estúdio Visual":::

- Painel de resultados

   Pode guardar todo o resultado como um ficheiro CSV, JSON ou Excel para um caminho local, ou apenas selecionar várias linhas.

- Painel de mensagens

  1. Quando o número de filas na tabela é superior a 100, vê-se a seguinte mensagem: "As primeiras 100 linhas são exibidas para a mesa da Colmeia."
  2. Quando o número de filas na tabela é inferior ou igual a 100, vê-se a seguinte mensagem: "São exibidas 60 linhas para a mesa da Colmeia".
  3. Quando não há conteúdo na mesa, vê-se a seguinte mensagem: `0 rows are displayed for Hive table.` "

     >[!NOTE]
     >
     >No Linux, instale xclip para ativar os dados da tabela de cópias.
     >
     >:::image type="content" source="./media/hdinsight-for-vscode/hdinsight-for-vscode-preview-linux-install-xclip.png" alt-text="Spark & Hive para código de estúdio visual em Linux":::

## <a name="additional-features"></a>Características adicionais

Spark & Hive for Visual Studio Code também suporta as seguintes funcionalidades:

- **IntelliSense autocomplete**. Surgem sugestões para palavras-chave, métodos, variáveis e outros elementos de programação. Diferentes ícones representam diferentes tipos de objetos:

    :::image type="content" source="./media/hdinsight-for-vscode/hdinsight-for-vscode-auto-complete-objects.png" alt-text="Ferramentas de colmeia spark & para objetos do código do estúdio visual IntelliSense":::

- **Marcador de erro IntelliSense**. O serviço de idiomas sublinha erros de edição no script da Hive.     
- **Destaques de sintaxe**. O serviço de idiomas utiliza cores diferentes para diferenciar variáveis, palavras-chave, tipo de dados, funções e outros elementos de programação:

    :::image type="content" source="./media/hdinsight-for-vscode/hdinsight-for-vscode-syntax-highlights.png" alt-text="Ferramentas de hive spark & para destaques de sintaxe do Código do Estúdio Visual":::

## <a name="reader-only-role"></a>Papel apenas para o leitor

Os utilizadores que recebem o papel apenas para o leitor para o cluster não podem submeter empregos ao cluster HDInsight, nem ver a base de dados da Hive. Contacte o administrador de cluster para atualizar a sua função para [**Operador de Cluster HDInsight**](./hdinsight-migrate-granular-access-cluster-configurations.md#add-the-hdinsight-cluster-operator-role-assignment-to-a-user) no [portal Azure](https://portal.azure.com/). Se tiver credenciais Ambari válidas, pode ligar manualmente o cluster utilizando as seguintes orientações.

### <a name="browse-the-hdinsight-cluster"></a>Navegue no cluster HDInsight  

Quando seleciona o explorador Azure HDInsight para expandir um cluster HDInsight, é solicitado que ligue o cluster se tiver o papel apenas para o leitor para o cluster. Utilize o seguinte método para ligar ao cluster utilizando as suas credenciais Ambari.

### <a name="submit-the-job-to-the-hdinsight-cluster"></a>Submeta o trabalho para o cluster HDInsight

Ao submeter o trabalho a um cluster HDInsight, é solicitado a ligar o cluster se estiver no papel apenas para o leitor para o cluster. Utilize os seguintes passos para ligar ao cluster utilizando credenciais Ambari.

### <a name="link-to-the-cluster"></a>Ligação ao cluster

1. Insira um nome de utilizador Ambari válido.
2. Introduza uma senha válida.

   :::image type="content" source="./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-username.png" alt-text="Spark & Ferramentas de Colmeia para nome de utilizador do código do estúdio visual":::

   :::image type="content" source="./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-password.png" alt-text="Ferramentas de colmeia de & faísca para senha de código do estúdio visual":::

   > [!NOTE]
   >
   >Pode utilizar `Spark / Hive: List Cluster` para verificar o cluster ligado:
   >
   >:::image type="content" source="./media/hdinsight-for-vscode/list-cluster-result1.png" alt-text="Ferramentas de hive & para leitor de código de estúdio visual ligado":::

## <a name="azure-data-lake-storage-gen2"></a>Armazenamento do Azure Data Lake Ger2

### <a name="browse-a-data-lake-storage-gen2-account"></a>Navegue numa conta Gen2 de armazenamento de dados

Selecione o explorador Azure HDInsight para expandir uma conta Gen2 de armazenamento de data lake. É-lhe pedido que introduza a chave de acesso ao armazenamento se a sua conta Azure não tiver acesso ao armazenamento da Gen2. Após a validação da chave de acesso, a conta Desasissoramento Gen2 da Data Lake é expandida automaticamente.

### <a name="submit-jobs-to-an-hdinsight-cluster-with-data-lake-storage-gen2"></a>Submeta empregos a um cluster HDInsight com data lake storage gen2

Submeta um trabalho a um cluster HDInsight usando data lake storage gen2. É-lhe pedido que introduza a chave de acesso ao armazenamento se a sua conta Azure não tiver acesso por escrito ao armazenamento da Gen2. Após a validação da chave de acesso, o trabalho será submetido com sucesso.

:::image type="content" source="./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-accesskey.png" alt-text="Ferramentas de hive & para o código do estúdio visual AccessKey":::

> [!NOTE]
>
> Pode obter a chave de acesso para a conta de armazenamento a partir do portal Azure. Para obter mais informações, consulte [gerir as teclas de acesso à conta de armazenamento.](../storage/common/storage-account-keys-manage.md)

## <a name="unlink-cluster"></a>Desvincular o aglomerado

1. A partir da barra de menu, vá à **Paleta de**  >  **Comando,** e depois **introduza Spark/ Hive: Desvincular um Cluster**.  

2. Selecione um cluster para desvincular.  

3. Consulte a vista **OUTPUT** para verificação.  

## <a name="sign-out"></a>Terminar sessão  

A partir da barra de menu, vá à **Paleta de** Comando  >  **Ver,** e depois insira **Azure: Assine**.

## <a name="known-issues"></a>Problemas Conhecidos 

 ms-toolsai.jupyter >versão 2021.3.684299474 não é suportada nesta extensão, por favor, utilize o kernel synapse mantendo-se fiel ao Microsoft Jupyter 2021.3.684299474.

 1. Desative a extensão de atualização automática.

      ![desativar a extensão de atualização automática](./media/hdinsight-for-vscode/disable-auto-updating-extension.png)

2. Instale uma versão selecionada do Microsoft Jupyter.

      ![versão selecionada do microsoft jupyter](./media/hdinsight-for-vscode/selected-version-of-microsoft-jupyter.png)

3. Instalar a versão Microsoft Jupyter 2021.3.684299474


## <a name="next-steps"></a>Passos seguintes

Para um vídeo que demonstre a utilização de Spark & Hive para Visual Studio Code, consulte [Spark & Hive for Visual Studio Code](https://go.microsoft.com/fwlink/?linkid=858706).