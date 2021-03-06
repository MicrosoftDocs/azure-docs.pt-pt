---
title: Tutorial:Construa uma Node.js web app com Azure Cosmos DB JavaScript SDK para gerir dados da API SQL
description: Este Node.js tutorial explora como usar o Microsoft Azure Cosmos DB para armazenar e aceder a dados de uma aplicação web Node.js Express hospedada na funcionalidade de Aplicações Web do Microsoft Azure App Service.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 11/05/2019
ms.author: sngun
ms.custom: devx-track-js
ms.openlocfilehash: 49cf54bda985f7d97b2db6a3ada7859aee829cff
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "97359545"
---
# <a name="tutorial-build-a-nodejs-web-app-using-the-javascript-sdk-to-manage-a-sql-api-account-in-azure-cosmos-db"></a>Tutorial: Construa uma Node.js aplicação web utilizando o JavaScript SDK para gerir uma conta API SQL em Azure Cosmos DB 
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

> [!div class="op_single_selector"]
> * [.NET](sql-api-dotnet-application.md)
> * [Java](sql-api-java-application.md)
> * [Node.js](sql-api-nodejs-application.md)
> * [Python](./create-sql-api-python.md)
> * [Xamarin](mobile-apps-with-xamarin.md)
> 

Como desenvolvedor, pode ter aplicações que utilizam dados de documentos NoSQL. Pode utilizar uma conta API SQL no Azure Cosmos DB para armazenar e aceder a estes dados de documentos. Este tutorial Node.js mostra como armazenar e aceder a dados a partir de uma conta API SQL em Azure Cosmos DB utilizando uma aplicação Node.js Express que está hospedada na funcionalidade de Aplicações Web do Microsoft Azure App Service. Neste tutorial, irá construir uma aplicação baseada na Web (toda a app) que lhe permite criar, recuperar e completar tarefas. As tarefas são armazenadas como documentos JSON no Azure Cosmos DB. 

Este tutorial demonstra como criar uma conta API SQL em Azure Cosmos DB utilizando o portal Azure. Em seguida, você constrói e executar uma aplicação web que é construída no Node.js SDK para criar uma base de dados e recipiente, e adicionar itens ao recipiente. Este tutorial utiliza a versão 3.0 do JavaScript SDK.

Este tutorial abrange as seguintes tarefas:

> [!div class="checklist"]
> * Criar uma conta do Azure Cosmos DB
> * Criar uma nova aplicação Node.js
> * Ligar a aplicação ao Azure Cosmos DB
> * Executar e implementar a aplicação no Azure

## <a name="prerequisites"></a><a name="_Toc395783176"></a>Pré-requisitos

Antes de seguir as instruções deste artigo, certifique-se de que tem os seguintes recursos:

* Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar. 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [Node.js][Node.js] versão 6.10 ou superior.
* [Express generator](https://www.expressjs.com/starter/generator.html) (pode instalar o Express através de `npm install express-generator -g`)
* Instale o [Git][Git] na estação de trabalho local.

## <a name="create-an-azure-cosmos-db-account"></a><a name="_Toc395637761"></a>Criar uma conta do Azure Cosmos DB
Comecemos por criar uma conta do Azure Cosmos DB. Se já tiver uma conta ou se estiver a utilizar o Emulador do Azure Cosmos DB para este tutorial, pode avançar para o [Passo 2: Criar uma aplicação Node.js](#_Toc395783178).

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [cosmos-db-keys](../../includes/cosmos-db-keys.md)]

## <a name="create-a-new-nodejs-application"></a><a name="_Toc395783178"></a>Criar uma nova aplicação Node.js
Agora, vamos aprender a criar um projeto básico Olá, Mundo Node.js com a arquitetura Express.

1. Abra o seu terminal favorito, como a linha de comandos Node.js.

1. Navegue para o diretório no qual pretende armazenar a nova aplicação.

1. Utilizar o Express generator para gerar uma nova aplicação designada **todo**.

   ```bash
   express todo
   ```

1. Abra o diretório **todo** e instale as dependências.

   ```bash
   cd todo
   npm install
   ```

1. Execute a nova aplicação.

   ```bash
   npm start
   ```

1. Pode ver a sua aplicação nova ao navegar para `http://localhost:3000`.
   
   :::image type="content" source="./media/sql-api-nodejs-application/cosmos-db-node-js-express.png" alt-text="Saiba Node.js - Captura de ecrã da aplicação Olá, Mundo numa janela do browser":::

   Pare a aplicação utilizando CTRL+C na janela do terminal e selecione **y** para terminar a função de lote.

## <a name="install-the-required-modules"></a><a name="_Toc395783179"></a>Instale os módulos necessários

O ficheiro **package.json** é um dos ficheiros criados na raiz do projeto. Este ficheiro contém uma lista de módulos adicionais que são necessários para a sua aplicação Node.js. Quando implementar esta aplicação no Azure, este ficheiro servirá para determinar quais os módulos que devem estar instalados no Azure para suportar a sua aplicação. Instale mais dois pacotes para este tutorial.

1. Instale o módulo **\@ azul/cosmos** através da npm. 

   ```bash
   npm install @azure/cosmos
   ```

## <a name="connect-the-nodejs-application-to-azure-cosmos-db"></a><a name="_Toc395783180"></a>Ligue a aplicação Node.js à Azure Cosmos DB
Agora que já concluiu a configuração inicial, vai escrever o código de que a aplicação Todo precisa para comunicar com o Azure Cosmos DB.

### <a name="create-the-model"></a>Criar o modelo
1. Na raiz do seu diretório de projeto, crie um novo diretório chamado **modelos.**  

2. No diretório **modelos**, crie um novo ficheiro designado **taskDao.js**. Este ficheiro contém código necessário para criar a base de dados e o recipiente. Também define métodos para ler, atualizar, criar e encontrar tarefas em Azure Cosmos DB. 

3. Copie o seguinte código no ficheiro **taskDao.js:**

   ```javascript
    // @ts-check
    const CosmosClient = require('@azure/cosmos').CosmosClient
    const debug = require('debug')('todo:taskDao')

    // For simplicity we'll set a constant partition key
    const partitionKey = undefined
    class TaskDao {
      /**
       * Manages reading, adding, and updating Tasks in Cosmos DB
       * @param {CosmosClient} cosmosClient
       * @param {string} databaseId
       * @param {string} containerId
       */
      constructor(cosmosClient, databaseId, containerId) {
        this.client = cosmosClient
        this.databaseId = databaseId
        this.collectionId = containerId

        this.database = null
        this.container = null
      }

      async init() {
        debug('Setting up the database...')
        const dbResponse = await this.client.databases.createIfNotExists({
          id: this.databaseId
        })
        this.database = dbResponse.database
        debug('Setting up the database...done!')
        debug('Setting up the container...')
        const coResponse = await this.database.containers.createIfNotExists({
          id: this.collectionId
        })
        this.container = coResponse.container
        debug('Setting up the container...done!')
      }

      async find(querySpec) {
        debug('Querying for items from the database')
        if (!this.container) {
          throw new Error('Collection is not initialized.')
        }
        const { resources } = await this.container.items.query(querySpec).fetchAll()
        return resources
      }

      async addItem(item) {
        debug('Adding an item to the database')
        item.date = Date.now()
        item.completed = false
        const { resource: doc } = await this.container.items.create(item)
        return doc
      }

      async updateItem(itemId) {
        debug('Update an item in the database')
        const doc = await this.getItem(itemId)
        doc.completed = true

        const { resource: replaced } = await this.container
          .item(itemId, partitionKey)
          .replace(doc)
        return replaced
      }

      async getItem(itemId) {
        debug('Getting an item from the database')
        const { resource } = await this.container.item(itemId, partitionKey).read()
        return resource
      }
    }

    module.exports = TaskDao
   ```
4. Guarde e feche o ficheiro **taskDao.js**.  

### <a name="create-the-controller"></a>Criar o controlador

1. No diretório **rotas** do seu projeto, crie um novo ficheiro designado **tasklist.js**.  

2. Adicione o seguinte código ao **tasklist.js**. Este código carrega os módulos CosmosClient e async, que são utilizados por **tasklist.js**. Este código também define a classe **TaskList**, que é transmitida como uma instância do objeto **TaskDao** que definimos anteriormente:
   
   ```javascript
    const TaskDao = require("../models/TaskDao");
    
    class TaskList {
      /**
       * Handles the various APIs for displaying and managing tasks
       * @param {TaskDao} taskDao
       */
      constructor(taskDao) {
        this.taskDao = taskDao;
      }
      async showTasks(req, res) {
        const querySpec = {
          query: "SELECT * FROM root r WHERE r.completed=@completed",
          parameters: [
            {
              name: "@completed",
              value: false
            }
          ]
        };

        const items = await this.taskDao.find(querySpec);
        res.render("index", {
          title: "My ToDo List ",
          tasks: items
        });
      }

      async addTask(req, res) {
        const item = req.body;

        await this.taskDao.addItem(item);
        res.redirect("/");
      }

      async completeTask(req, res) {
        const completedTasks = Object.keys(req.body);
        const tasks = [];

        completedTasks.forEach(task => {
          tasks.push(this.taskDao.updateItem(task));
        });

        await Promise.all(tasks);

        res.redirect("/");
      }
    }

    module.exports = TaskList;
   ```

3. Guarde e feche o ficheiro **tasklist.js**.

### <a name="add-configjs"></a>Adicionar config.js

1. Na raiz do diretório do projeto, crie um ficheiro novo designado **config.js**. 

2. Adicione o seguinte código ao ficheiro **config.js**. Este código define os parâmetros e os valores da configuração necessários para a nossa aplicação.
   
   ```javascript
   const config = {};

   config.host = process.env.HOST || "[the endpoint URI of your Azure Cosmos DB account]";
   config.authKey =
     process.env.AUTH_KEY || "[the PRIMARY KEY value of your Azure Cosmos DB account";
   config.databaseId = "ToDoList";
   config.containerId = "Items";

   if (config.host.includes("https://localhost:")) {
     console.log("Local environment detected");
     console.log("WARNING: Disabled checking of self-signed certs. Do not have this code in production.");
     process.env.NODE_TLS_REJECT_UNAUTHORIZED = "0";
     console.log(`Go to http://localhost:${process.env.PORT || '3000'} to try the sample.`);
   }

   module.exports = config;
   ```

3. No ficheiro **config.js,** atualize os valores do HOST e AUTH_KEY utilizando os valores encontrados na página Chaves da sua conta DB Azure Cosmos no [portal Azure](https://portal.azure.com). 

4. Guarde e feche o ficheiro **config.js**.

### <a name="modify-appjs"></a>Modificar app.js

1. No diretório do projeto, abra o ficheiro **app.js**. Este ficheiro foi criado anteriormente, aquando da criação da aplicação Web Express.  

2. Adicione o seguinte código ao ficheiro **app.js**. Este código define o ficheiro de configuração a utilizar e carrega os valores para algumas variáveis que utilizará nas próximas secções. 
   
   ```javascript
    const CosmosClient = require('@azure/cosmos').CosmosClient
    const config = require('./config')
    const TaskList = require('./routes/tasklist')
    const TaskDao = require('./models/taskDao')

    const express = require('express')
    const path = require('path')
    const logger = require('morgan')
    const cookieParser = require('cookie-parser')
    const bodyParser = require('body-parser')

    const app = express()

    // view engine setup
    app.set('views', path.join(__dirname, 'views'))
    app.set('view engine', 'jade')

    // uncomment after placing your favicon in /public
    //app.use(favicon(path.join(__dirname, 'public', 'favicon.ico')));
    app.use(logger('dev'))
    app.use(bodyParser.json())
    app.use(bodyParser.urlencoded({ extended: false }))
    app.use(cookieParser())
    app.use(express.static(path.join(__dirname, 'public')))

    //Todo App:
    const cosmosClient = new CosmosClient({
      endpoint: config.host,
      key: config.authKey
    })
    const taskDao = new TaskDao(cosmosClient, config.databaseId, config.containerId)
    const taskList = new TaskList(taskDao)
    taskDao
      .init(err => {
        console.error(err)
      })
      .catch(err => {
        console.error(err)
        console.error(
          'Shutting down because there was an error settinig up the database.'
        )
        process.exit(1)
      })

    app.get('/', (req, res, next) => taskList.showTasks(req, res).catch(next))
    app.post('/addtask', (req, res, next) => taskList.addTask(req, res).catch(next))
    app.post('/completetask', (req, res, next) =>
      taskList.completeTask(req, res).catch(next)
    )
    app.set('view engine', 'jade')

    // catch 404 and forward to error handler
    app.use(function(req, res, next) {
      const err = new Error('Not Found')
      err.status = 404
      next(err)
    })

    // error handler
    app.use(function(err, req, res, next) {
      // set locals, only providing error in development
      res.locals.message = err.message
      res.locals.error = req.app.get('env') === 'development' ? err : {}

      // render the error page
      res.status(err.status || 500)
      res.render('error')
    })

    module.exports = app
   ```

3. Por fim, guarde e feche o ficheiro **app.js**.

## <a name="build-a-user-interface"></a><a name="_Toc395783181"></a>Construir uma interface de utilizador

Agora vamos construir a interface do utilizador para que um utilizador possa interagir com a aplicação. A aplicação Express que criámos nas secções anteriores utiliza o **Jade** como motor de vista.

1. O ficheiro **layout.jade** no diretório **vistas** é utilizado como um modelo global para outros ficheiros **.jade**. Neste passo, irá modificá-lo para utilizar o Twitter Bootstrap, que é um conjunto de ferramentas utilizado para criar um site.  

2. Abra o ficheiro **layout.jade** encontrado na pasta **views** e substitua o conteúdo pelo seguinte código:

   ```html
   doctype html
   html
     head
       title= title
       link(rel='stylesheet', href='//ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css')
       link(rel='stylesheet', href='/stylesheets/style.css')
     body
       nav.navbar.navbar-inverse.navbar-fixed-top
         div.navbar-header
           a.navbar-brand(href='#') My Tasks
       block content
       script(src='//ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js')
       script(src='//ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js')
   ```

    Este código diz ao motor **Jade** para renderizar algum HTML para a nossa aplicação, e cria um **bloco** chamado **conteúdo** onde podemos fornecer o layout para as nossas páginas de conteúdo. Guarde e feche o ficheiro **layout.jade**.

3. Agora, abra o ficheiro **index.jade**, a vista que será utilizada pela nossa aplicação, e substitua o conteúdo do ficheiro pelo seguinte código:

   ```html
   extends layout
   block content
        h1 #{title}
        br
        
        form(action="/completetask", method="post")
         table.table.table-striped.table-bordered
            tr
              td Name
              td Category
              td Date
              td Complete
            if (typeof tasks === "undefined")
              tr
                td
            else
              each task in tasks
                tr
                  td #{task.name}
                  td #{task.category}
                  - var date  = new Date(task.date);
                  - var day   = date.getDate();
                  - var month = date.getMonth() + 1;
                  - var year  = date.getFullYear();
                  td #{month + "/" + day + "/" + year}
                  td
                   if(task.completed) 
                    input(type="checkbox", name="#{task.id}", value="#{!task.completed}", checked=task.completed)
                   else
                    input(type="checkbox", name="#{task.id}", value="#{!task.completed}", checked=task.completed)
          button.btn.btn-primary(type="submit") Update tasks
        hr
        form.well(action="/addtask", method="post")
          label Item Name:
          input(name="name", type="textbox")
          label Item Category:
          input(name="category", type="textbox")
          br
          button.btn(type="submit") Add item
   ```

Este código expande o modelo e fornece o conteúdo para o marcador de posição **conteúdo** que vimos anteriormente no ficheiro **layout.jade**. Neste esquema, criámos dois formulários HTML.

O primeiro formulário contém uma tabela para os dados e um botão que nos permite atualizar os itens mediante a publicação no método **/completeTask** do controlador.
    
O segundo formulário contém dois campos de entrada e um botão que permite criar um novo item ao publicar no método **/addtask** do controlador. É tudo o que precisamos para que a aplicação funcione.

## <a name="run-your-application-locally"></a><a name="_Toc395783181"></a>Executar a sua aplicação localmente

Agora que construiu a aplicação, pode executá-la localmente usando os seguintes passos:  

1. Para testar a aplicação na sua máquina local, corra `npm start` no terminal para iniciar a sua aplicação e, em seguida, refresque a página do `http://localhost:3000` navegador. A página deve ser agora apresentada conforme mostrado na captura de ecrã seguinte:
   
    :::image type="content" source="./media/sql-api-nodejs-application/cosmos-db-node-js-localhost.png" alt-text="Captura de ecrã da aplicação MyTodo List numa janela do browser":::

    > [!TIP]
    > Se receber um erro sobre o travessão no ficheiro layout.jade ou no ficheiro index.jade, certifique-se de que as duas primeiras linhas em ambos os ficheiros são justificadas à esquerda, sem espaços. Se houver espaços antes das duas primeiras linhas, remova-as, guarde ambos os ficheiros e, em seguida, refresque a janela do seu navegador. 

2. Utilize os campos Item, Item e Categoria para introduzir uma nova tarefa e, em seguida, selecione **Adicionar Item**. Cria um documento no Azure Cosmos DB com essas propriedades. 

3. A página deverá ser atualizada para mostrar o item criado recentemente na ToDo List.
   
    :::image type="content" source="./media/sql-api-nodejs-application/cosmos-db-node-js-added-task.png" alt-text="Captura de ecrã da aplicação com um novo item na ToDo List":::

4. Para completar uma tarefa, selecione a caixa de verificação na coluna Complete e, em seguida, selecione **Tarefas de Atualização**. Este procedimento atualiza o documento já criado e retira-o da vista.

5. Para parar a aplicação, prima CTRL+C na janela do terminal e, em seguida, selecione **Y** para terminar a tarefa de lote.

## <a name="deploy-your-application-to-web-apps"></a><a name="_Toc395783182"></a>Implemente a sua aplicação para Aplicações Web

Depois de a sua aplicação ter sucesso localmente, pode implantá-la para Azure utilizando os seguintes passos:

1. Se ainda não o fez, ative um repositório de git para a sua aplicação de Web Apps.

2. Adicione a sua aplicação De Aplicações Web como um comando git.
   
   ```bash
   git remote add azure https://username@your-azure-website.scm.azurewebsites.net:443/your-azure-website.git
   ```

3. Implemente a aplicação ao enviá-la para a ligação remota.
   
   ```bash
   git push azure main
   ```

4. Em alguns segundos, a aplicação Web é publicada e iniciada num browser.

## <a name="clean-up-resources"></a>Limpar os recursos

Quando estes recursos já não forem necessários, pode eliminar o grupo de recursos, a conta DB da Azure Cosmos e todos os recursos conexos. Para tal, selecione o grupo de recursos que utilizou para a conta DB Azure Cosmos, selecione **Delete** e, em seguida, confirme o nome do grupo de recursos para eliminar.

## <a name="next-steps"></a><a name="_Toc395637775"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Criar aplicações móveis com o Xamarin e o Azure Cosmos DB](mobile-apps-with-xamarin.md)


[Node.js]: https://nodejs.org/
[Git]: https://git-scm.com/
[GitHub]: https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-todo-app