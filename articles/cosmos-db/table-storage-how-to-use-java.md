---
title: Use o armazenamento da mesa Azure ou a API de mesa DB da Azure Cosmos de Java
description: Armazenar dados estruturados na nuvem utilizando o armazenamento da Tabela Azure ou a API de Mesa DB AZure Cosmos de Java.
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.devlang: Java
ms.topic: sample
ms.date: 12/10/2020
author: ThomasWeiss
ms.author: thweiss
ms.custom: devx-track-java
ms.openlocfilehash: a5da5e1717f897d2236fd73f0fff525e157f7a0e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "97093694"
---
# <a name="how-to-use-azure-table-storage-or-azure-cosmos-db-table-api-from-java"></a>Como utilizar o Armazenamento de tabelas do Azure ou a API de Tabelas do Azure Cosmos DB a partir de Java

[!INCLUDE[appliesto-table-api](includes/appliesto-table-api.md)]

[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-applies-to-storagetable-and-cosmos](../../includes/storage-table-applies-to-storagetable-and-cosmos.md)]

Este artigo mostra-lhe como criar tabelas, armazenar os seus dados e realizar operações CRUD nos dados. Escolha o serviço Azure Table ou a API da Tabela DB AZure Cosmos. As amostras são escritas em Java e utilizam o [Azure Storage SDK v8 para Java][Azure Storage SDK for Java]. Os cenários abrangidos incluem a **criação**, a **listagem** e a **eliminação** de tabelas, bem como a **inserção**, a **consulta**, a **modificação** e a **eliminação** de entidades numa tabela. Para obter mais informações sobre as tabelas, veja a secção [Passos seguintes](#next-steps).

> [!IMPORTANT]
> A última versão do Azure Storage SDK suportando o armazenamento da mesa é [v8][Azure Storage SDK for Java]. Em breve, chegará uma nova versão do SDK de armazenamento de mesa para Java.

> [!NOTE]
> Está disponível um SDK para os programadores que utilizem o Armazenamento do Azure em dispositivos Android. Para obter mais informações, veja o [SDK do Armazenamento do Azure para Android][Azure Storage SDK for Android].
>

## <a name="create-an-azure-service-account"></a>Criar uma conta de serviço do Azure

[!INCLUDE [cosmos-db-create-azure-service-account](../../includes/cosmos-db-create-azure-service-account.md)]

**Criar uma conta de armazenamento Azure**

[!INCLUDE [cosmos-db-create-storage-account](../../includes/cosmos-db-create-storage-account.md)]

**Criar uma conta do Azure Cosmos DB**

[!INCLUDE [cosmos-db-create-tableapi-account](../../includes/cosmos-db-create-tableapi-account.md)]

## <a name="create-a-java-application"></a>Criar uma aplicação Java

Neste guia, vai utilizar funcionalidades de armazenamento que pode executar numa aplicação Java localmente ou em código que esteja a ser executado numa função da Web ou de trabalho no Azure.

Para utilizar os exemplos deste artigo, instale o Java Development Kit (JDK) e crie uma conta de armazenamento do Azure ou do Azure Cosmos DB na sua subscrição do Azure. Quando o tiver feito, verifique se o seu sistema de desenvolvimento cumpre os requisitos mínimos e as dependências indicadas no repositório [Azure Storage SDK for Java][Azure Storage SDK for Java] do GitHub. Se cumprir esses requisitos, pode seguir as instruções para transferir e instalar as Bibliotecas do Armazenamento do Azure para Java no seu sistema a partir daquele repositório. Depois de concluir essas tarefas, pode criar uma aplicação Java que utiliza os exemplos deste artigo.

## <a name="configure-your-application-to-access-table-storage"></a>Configurar a sua aplicação para aceder ao armazenamento de tabela

Adicione as seguintes declarações de importação à parte superior do ficheiro Java em que pretende utilizar as APIs de armazenamento do Azure ou a API de Tabela do Azure Cosmos DB para aceder às tabelas:

```java
// Include the following imports to use table APIs
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.table.*;
import com.microsoft.azure.storage.table.TableQuery.*;
```

## <a name="add-your-connection-string"></a>Adicione a sua cadeia de conexão

Pode ligar-se à conta de armazenamento Azure ou à conta AZure Cosmos DB Table API. Obtenha a cadeia de ligação com base no tipo de conta que está a utilizar.

### <a name="add-an-azure-storage-connection-string"></a>Adicionar uma cadeia de ligação do Armazenamento do Azure

Os clientes do Armazenamento do Azure utilizam uma cadeia de ligação de armazenamento para armazenar pontos finais e credenciais para aceder a serviços de gestão de dados. Se estiver a executar numa aplicação cliente, tem de fornecer a cadeia de ligação de armazenamento no formato seguinte, com o nome da conta de armazenamento e a Chave de acesso primária dessa conta indicada no [portal do Azure](https://portal.azure.com) nos valores **AccountName** e **AccountKey**. 

Este exemplo mostra como pode declarar um campo estático para conter a cadeia de ligação:

```java
// Define the connection-string with your values.
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```

### <a name="add-an-azure-cosmos-db-table-api-connection-string"></a>Adicionar uma cadeia de ligação da API de Tabela do Azure Cosmos DB

As contas do Azure Cosmos DB utilizam uma cadeia de ligação para armazenar o ponto final da tabela e as suas credenciais. Se estiver a executar numa aplicação cliente, tem de fornecer a cadeia de ligação do Azure Cosmos DB no formato seguinte, com o nome da conta do Azure Cosmos DB e a Chave de acesso primária dessa conta indicada no [portal do Azure](https://portal.azure.com) nos valores **AccountName** e **AccountKey**. 

Este exemplo mostra como pode declarar um campo estático para conter a cadeia de ligação do Azure Cosmos DB:

```java
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=https;" + 
    "AccountName=your_cosmosdb_account;" + 
    "AccountKey=your_account_key;" + 
    "TableEndpoint=https://your_endpoint;" ;
```

Numa aplicação que esteja a ser executada numa função no Azure, pode armazenar essa cadeia no ficheiro de configuração do serviço, *ServiceConfiguration.cscfg*, e pode aceder à mesma com uma chamada para o método **RoleEnvironment.getConfigurationSettings**. Eis um exemplo da obtenção da cadeia de ligação a partir de um elemento **Setting** (Definição) denominado *StorageConnectionString* no ficheiro de configuração do serviço:

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

Também pode armazenar a cadeia de ligação no ficheiro config.properties do seu projeto:

```java
StorageConnectionString = DefaultEndpointsProtocol=https;AccountName=your_account;AccountKey=your_account_key;TableEndpoint=https://your_table_endpoint/
```

Os exemplos seguintes partem do princípio de que utilizou um destes métodos para obter a cadeia de ligação de armazenamento.

## <a name="create-a-table"></a>Criar uma tabela

Um `CloudTableClient` objeto permite obter objetos de referência para tabelas e entidades. O código a seguir cria um `CloudTableClient` objeto e usa-o para criar um novo `CloudTable` objeto, que representa uma tabela chamada "pessoas". 

> [!NOTE]
> Existem outras formas de criar `CloudStorageAccount` objetos; para mais informações, consulte `CloudStorageAccount` na [Referência SDK do Cliente de Armazenamento Azure).]
>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create the table if it doesn't exist.
    String tableName = "people";
    CloudTable cloudTable = tableClient.getTableReference(tableName);
    cloudTable.createIfNotExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="list-the-tables"></a>Listar as tabelas

Para ver uma lista das tabelas, chame o método **CloudTableClient.listTables()** para obter uma lista iterável de nomes de tabelas.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Loop through the collection of table names.
    for (String table : tableClient.listTables())
    {
        // Output each table name.
        System.out.println(table);
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="add-an-entity-to-a-table"></a>Adicionar uma entidade a uma tabela

As entidades mapeiam para objetos Java usando uma implementação de classe `TableEntity` personalizada. Por conveniência, a `TableServiceEntity` classe implementa e usa a `TableEntity` reflexão para mapear propriedades para obter métodos de setter nomeados para as propriedades. Para adicionar uma entidade a uma tabela, crie primeiro uma classe que define as propriedades da sua entidade. O código seguinte define uma classe de entidade que utiliza o nome próprio do cliente como a chave da fila e o apelido como a chave de partição. Em conjunto, a chave da fila e a partição da entidade identificam de forma exclusiva a entidade na tabela. As entidades com a mesma chave de partição podem ser consultadas mais rapidamente do que as que têm chaves de partição diferentes.

```java
public class CustomerEntity extends TableServiceEntity {
    public CustomerEntity(String lastName, String firstName) {
        this.partitionKey = lastName;
        this.rowKey = firstName;
    }

    public CustomerEntity() { }

    String email;
    String phoneNumber;

    public String getEmail() {
        return this.email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public String getPhoneNumber() {
        return this.phoneNumber;
    }

    public void setPhoneNumber(String phoneNumber) {
        this.phoneNumber = phoneNumber;
    }
}
```

As operações de mesa envolvendo entidades requerem um `TableOperation` objeto. Este objeto define a operação a ser realizada numa entidade, que pode ser executada com um `CloudTable` objeto. O código a seguir cria uma nova instância da `CustomerEntity` classe com alguns dados do cliente a serem armazenados. O código em seguida chama `TableOperation` .insertOrReplace** para criar um `TableOperation` objeto para inserir uma entidade numa tabela, e associa o novo com `CustomerEntity` ele. Finalmente, o código chama o `execute` método do `CloudTable` objeto, especificando a tabela "pessoas" e a nova `TableOperation` , que depois envia um pedido ao serviço de armazenamento para inserir a nova entidade do cliente na tabela "pessoas" ou substituir a entidade se já existir.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.setEmail("Walter@contoso.com");
    customer1.setPhoneNumber("425-555-0101");

    // Create an operation to add the new customer to the people table.
    TableOperation insertCustomer1 = TableOperation.insertOrReplace(customer1);

    // Submit the operation to the table service.
    cloudTable.execute(insertCustomer1);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="insert-a-batch-of-entities"></a>Inserir um lote de entidades

Pode inserir um lote de entidades no serviço Tabela numa operação de escrita. O código a seguir cria um `TableBatchOperation` objeto e, em seguida, adiciona-lhe três operações de inserção. Cada operação de inserção é adicionada criando um novo objeto de entidade, definindo os seus valores e, em seguida, chamando `insert` o método no objeto para associar a entidade a uma nova `TableBatchOperation` operação de inserção. Em seguida, o código chama `execute` o `CloudTable` objeto, especificando a tabela "pessoas" e o `TableBatchOperation` objeto, que envia o lote de operações de mesa para o serviço de armazenamento num único pedido.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Define a batch operation.
    TableBatchOperation batchOperation = new TableBatchOperation();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a customer entity to add to the table.
    CustomerEntity customer = new CustomerEntity("Smith", "Jeff");
    customer.setEmail("Jeff@contoso.com");
    customer.setPhoneNumber("425-555-0104");
    batchOperation.insertOrReplace(customer);

    // Create another customer entity to add to the table.
    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.setEmail("Ben@contoso.com");
    customer2.setPhoneNumber("425-555-0102");
    batchOperation.insertOrReplace(customer2);

    // Create a third customer entity to add to the table.
    CustomerEntity customer3 = new CustomerEntity("Smith", "Denise");
    customer3.setEmail("Denise@contoso.com");
    customer3.setPhoneNumber("425-555-0103");
    batchOperation.insertOrReplace(customer3);

    // Execute the batch of operations on the "people" table.
    cloudTable.execute(batchOperation);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

Algumas coisas a salientar nas operações de lote:

* Pode realizar até cem operações de inserir, eliminar, unir, substituir, inserir ou intercalar e inserir ou substituir em qualquer combinação num lote individual.
* As operações em lote podem ter uma operação de obter se for a única operação no lote.
* Todas as entidades numa única operação em lote têm de ter a mesma chave de partição.
* Uma operação em lote está limitada a um payload de dados de 4 MB.

## <a name="retrieve-all-entities-in-a-partition"></a>Obter todas as entidades numa partição

Para consultar uma tabela para entidades numa divisória, pode utilizar um `TableQuery` . Ligue `TableQuery.from` para criar uma consulta numa tabela específica que devolva um tipo de resultado especificado. O código seguinte especifica um filtro para as entidades em que “Santos” é a chave de partição. `TableQuery.generateFilterCondition` é um método de ajuda para criar filtros para consultas. Solicite `where` a referência devolvida pelo método para aplicar o filtro à `TableQuery.from` consulta. Quando a consulta é executada com uma chamada no `execute` `CloudTable` objeto, retorna um `Iterator` com o tipo de resultado `CustomerEntity` especificado. Em seguida, pode utilizar o `Iterator` devolvido num loop "ForEach" para consumir os resultados. Este código imprime os campos de cada entidade nos resultados da consulta para a consola.

```java
try
{
    // Define constants for filters.
    final String PARTITION_KEY = "PartitionKey";
    final String ROW_KEY = "RowKey";
    final String TIMESTAMP = "Timestamp";

    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a filter condition where the partition key is "Smith".
    String partitionFilter = TableQuery.generateFilterCondition(
        PARTITION_KEY,
        QueryComparisons.EQUAL,
        "Smith");

    // Specify a partition query, using "Smith" as the partition key filter.
    TableQuery<CustomerEntity> partitionQuery =
        TableQuery.from(CustomerEntity.class)
        .where(partitionFilter);

    // Loop through the results, displaying information about the entity.
    for (CustomerEntity entity : cloudTable.execute(partitionQuery)) {
        System.out.println(entity.getPartitionKey() +
            " " + entity.getRowKey() +
            "\t" + entity.getEmail() +
            "\t" + entity.getPhoneNumber());
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Obter um intervalo de entidades numa partição

Se não pretender consultar todas as entidades de uma partição, pode utilizar operadores de comparação num filtro para especificar um intervalo. O código seguinte combina dois filtros para obter todas as entidades na partição “Santos”, em que a chave da fila (nome próprio) começa com uma letra anterior ao “E” do alfabeto. Depois, imprime os resultados da consulta. Se utilizar as entidades adicionadas à tabela na secção de inserção em lote deste guia, só são devolvidas duas entidades (Bernardo e Denise Santos); Jorge Santos não é incluído.

```java
try
{
    // Define constants for filters.
    final String PARTITION_KEY = "PartitionKey";
    final String ROW_KEY = "RowKey";
    final String TIMESTAMP = "Timestamp";

    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a filter condition where the partition key is "Smith".
    String partitionFilter = TableQuery.generateFilterCondition(
        PARTITION_KEY,
        QueryComparisons.EQUAL,
        "Smith");

    // Create a filter condition where the row key is less than the letter "E".
    String rowFilter = TableQuery.generateFilterCondition(
        ROW_KEY,
        QueryComparisons.LESS_THAN,
        "E");

    // Combine the two conditions into a filter expression.
    String combinedFilter = TableQuery.combineFilters(partitionFilter,
        Operators.AND, rowFilter);

    // Specify a range query, using "Smith" as the partition key,
    // with the row key being up to the letter "E".
    TableQuery<CustomerEntity> rangeQuery =
        TableQuery.from(CustomerEntity.class)
        .where(combinedFilter);

    // Loop through the results, displaying information about the entity
    for (CustomerEntity entity : cloudTable.execute(rangeQuery)) {
        System.out.println(entity.getPartitionKey() +
            " " + entity.getRowKey() +
            "\t" + entity.getEmail() +
            "\t" + entity.getPhoneNumber());
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="retrieve-a-single-entity"></a>Obter uma única entidade

Pode escrever uma consulta para obter uma entidade única e específica. O código seguinte chama `TableOperation.retrieve` com chave de partição e parâmetros chave de linha para especificar o cliente "Jeff Smith", em vez de criar um `Table Query` e usar filtros para fazer a mesma coisa. Quando é executada, a operação de obtenção devolve apenas uma entidade em vez de uma coleção. O `getResultAsType` método lança o resultado para o tipo de alvo de atribuição, um `CustomerEntity` objeto. Se este tipo não for compatível com o tipo especificado para a consulta, é emitida uma exceção. Se nenhuma entidade tiver uma correspondência exata entre partição e chave de linha, é devolvido um valor nulo. Especificar as chaves de partição e da fila numa consulta é a forma mais rápida de obter uma única entidade a partir do serviço Tabela.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Retrieve the entity with partition key of "Smith" and row key of "Jeff"
    TableOperation retrieveSmithJeff =
        TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

    // Submit the operation to the table service and get the specific entity.
    CustomerEntity specificEntity =
        cloudTable.execute(retrieveSmithJeff).getResultAsType();

    // Output the entity.
    if (specificEntity != null)
    {
        System.out.println(specificEntity.getPartitionKey() +
            " " + specificEntity.getRowKey() +
            "\t" + specificEntity.getEmail() +
            "\t" + specificEntity.getPhoneNumber());
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="modify-an-entity"></a>Modificar uma entidade

Para modificar uma entidade, obtenha-a a partir do serviço Tabela, faça alterações ao objeto da entidade e guarde-as novamente no serviço Tabela com uma operação de substituição ou intercalação. O código seguinte altera o número de telefone de um cliente existente. Em vez de chamar **TableOperation.insert**, como fizemos para inserir, este código chama **TableOperation.replace**. O método **CloudTable.execute** chama o serviço Tabela e a entidade é substituída, a não ser que tenha sido alterada por outra aplicação desde o momento em que esta aplicação foi obtida. Quando isso acontece, é emitida uma exceção e a entidade tem de ter obtida, modificada e guardada outra vez. Este padrão de repetição de simultaneidade otimista é comum nos sistemas de armazenamento distribuído.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
    TableOperation retrieveSmithJeff =
        TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

    // Submit the operation to the table service and get the specific entity.
    CustomerEntity specificEntity =
        cloudTable.execute(retrieveSmithJeff).getResultAsType();

    // Specify a new phone number.
    specificEntity.setPhoneNumber("425-555-0105");

    // Create an operation to replace the entity.
    TableOperation replaceEntity = TableOperation.replace(specificEntity);

    // Submit the operation to the table service.
    cloudTable.execute(replaceEntity);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="query-a-subset-of-entity-properties"></a>Consultar um subconjunto de propriedades de entidade

Uma consulta a uma tabela pode obter apenas algumas propriedades de uma entidade. Esta técnica, denominada projeção, reduz a largura de banda e pode melhorar o desempenho da consulta, especialmente para entidades grandes. A consulta no código que se segue utiliza o `select` método para devolver apenas os endereços de e-mail das entidades na tabela. Os resultados são projetados para uma coleção `String` de , que faz a conversão tipo nas `Entity Resolver` entidades devolvidas do servidor. Você pode aprender mais sobre a projeção em [Tabelas Azure: Introdução upsert e projeção de consulta][Tabelas Azure: Introdução upsert e projeção de consulta]. A projeção não é suportada no emulador de armazenamento local, pelo que este código funciona apenas quando se utiliza uma conta no serviço de tabela.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Define a projection query that retrieves only the Email property
    TableQuery<CustomerEntity> projectionQuery =
        TableQuery.from(CustomerEntity.class)
        .select(new String[] {"Email"});

    // Define an Entity resolver to project the entity to the Email value.
    EntityResolver<String> emailResolver = new EntityResolver<String>() {
        @Override
        public String resolve(String PartitionKey, String RowKey, Date timeStamp, HashMap<String, EntityProperty> properties, String etag) {
            return properties.get("Email").getValueAsString();
        }
    };

    // Loop through the results, displaying the Email values.
    for (String projectedString :
        cloudTable.execute(projectionQuery, emailResolver)) {
            System.out.println(projectedString);
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="insert-or-replace-an-entity"></a>Inserir ou Substituir uma entidade

Em muitos casos, poderá querer adicionar uma entidade a uma tabela sem saber se já existe nessa tabela. Uma operação de inserção ou substituição permite-lhe fazer um único pedido, que irá inserir a entidade se não existir ou substituir a existente se o fizer. Continuando com os exemplos anteriores, o seguinte código insere ou substitui a entidade para "Walter Harp". Depois de criar uma entidade nova, este código chama o método **TableOperation.insertOrReplace**. Em seguida, este código chama **a execução** no objeto **Cloud Table** com a tabela e a inserção ou substituição da operação da tabela como parâmetros. Para atualizar apenas uma parte de uma entidade, é possível, em alternativa, utilizar o método **TableOperation.insertOrMerge**. A inserção ou substituição não é suportada no emulador de armazenamento local, pelo que este código só funciona quando se utiliza uma conta no serviço de tabela. Pode aprender mais sobre inserir ou substituir e inserir ou fundir-se nesta [Tabelas Azure: Introdução upsert e projeção de consultas][Tabelas Azure: Introdução upsert e projeção de consultas].

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a new customer entity.
    CustomerEntity customer5 = new CustomerEntity("Harp", "Walter");
    customer5.setEmail("Walter@contoso.com");
    customer5.setPhoneNumber("425-555-0106");

    // Create an operation to add the new customer to the people table.
    TableOperation insertCustomer5 = TableOperation.insertOrReplace(customer5);

    // Submit the operation to the table service.
    cloudTable.execute(insertCustomer5);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="delete-an-entity"></a>Eliminar uma entidade

Pode facilmente eliminar uma entidade depois de a ter obtido. Depois de a entidade ser recuperada, ligue `TableOperation.delete` para a entidade para apagar. Então chame `execute` o `CloudTable` objeto. O código seguinte obtém e elimina uma entidade de cliente.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
    TableOperation retrieveSmithJeff = TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

    // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
    CustomerEntity entitySmithJeff =
        cloudTable.execute(retrieveSmithJeff).getResultAsType();

    // Create an operation to delete the entity.
    TableOperation deleteSmithJeff = TableOperation.delete(entitySmithJeff);

    // Submit the delete operation to the table service.
    cloudTable.execute(deleteSmithJeff);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="delete-a-table"></a>Eliminar uma tabela

Por fim, o código seguinte elimina uma tabela de uma conta de armazenamento. Cerca de 40 segundos depois de apagar uma mesa, não pode recriá-la. 

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Delete the table and all its data if it exists.
    CloudTable cloudTable = tableClient.getTableReference("people");
    cloudTable.deleteIfExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

[!INCLUDE [storage-check-out-samples-java](../../includes/storage-check-out-samples-java.md)]

## <a name="next-steps"></a>Passos seguintes

* [Getting Started with Azure Table Service in Java](https://github.com/Azure-Samples/storage-table-java-getting-started) (Introdução ao Serviço Tabela do Azure em Java)
* O [Explorador de Armazenamento do Microsoft Azure](../vs-azure-tools-storage-manage-with-storage-explorer.md) é uma aplicação autónoma e gratuita da Microsoft, que lhe permite trabalhar visualmente com dados do Armazenamento do Azure no Windows, macOS e Linux.
* [Azure Storage SDK for Java][Azure Storage SDK for Java] (SDK do Armazenamento do Azure para Java)
* [Azure Storage Client SDK Reference][Azure Storage Client SDK Reference] (Referência do SDK do Cliente do Armazenamento do Azure)
* [Azure Storage REST API][Azure Storage REST API]
* [Blog da equipa de armazenamento Azure][Azure Storage Team Blog]

Para obter mais informações, visite [Azure para programadores Java](/java/azure).

[Azure SDK for Java]: https://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/Azure/azure-storage-java/tree/v8.6.5
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
[Azure Storage Client SDK Reference]: https://azure.github.io/azure-storage-java/ (Referência do SDK do Cliente do Armazenamento do Azure)
[Azure Storage REST API]: /rest/api/storageservices/
[Azure Storage Team Blog]: https://blogs.msdn.microsoft.com/windowsazurestorage/