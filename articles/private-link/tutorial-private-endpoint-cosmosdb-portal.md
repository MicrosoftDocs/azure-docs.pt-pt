---
title: 'Tutorial: Ligue-se a uma conta Azure Cosmos usando um ponto final Azure Private'
titleSuffix: Azure Private Link
description: Começa com este tutorial usando o Azure Private endpoint para ligar a uma conta Azure Cosmos em privado.
author: asudbring
ms.author: allensu
ms.service: private-link
ms.topic: tutorial
ms.date: 9/25/2020
ms.openlocfilehash: 3a7e75641f6bb84b490231fcd06e04c3cbad06d3
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "99063471"
---
# <a name="tutorial-connect-to-an-azure-cosmos-account-using-an-azure-private-endpoint"></a>Tutorial: Ligue-se a uma conta Azure Cosmos usando um Azure Private Endpoint

O ponto final privado Azure é o bloco de construção fundamental para private link em Azure. Permite que os recursos do Azure, como máquinas virtuais (VMs), comuniquem com os recursos de Private Link em privado.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Crie uma rede virtual e um hospedeiro de bastião.
> * Cria uma máquina virtual.
> * Crie uma conta Cosmos DB com um ponto final privado.
> * Teste conectividade à conta cosmos DB ponto final privado.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

* Uma subscrição do Azure

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no [portal do Azure](https://portal.azure.com).

## <a name="create-a-virtual-network-and-bastion-host"></a>Criar uma rede virtual e hospedeiro de bastião

Nesta secção, você vai criar uma rede virtual, sub-rede e hospedeiro de bastião. 

O hospedeiro de bastião será utilizado para ligar de forma segura à máquina virtual para testar o ponto final privado.

1. No canto superior esquerdo do ecrã, selecione **Criar um recurso > Redes > Rede virtual** ou procure **Rede virtual** na caixa de pesquisa.

2. Na **Criação de rede virtual,** insira ou selecione esta informação no **separador Básicos:**

    | **Definição**          | **Valor**                                                           |
    |------------------|-----------------------------------------------------------------|
    | **Detalhes do projeto**  |                                                                 |
    | Subscrição     | selecione a subscrição do Azure                                  |
    | Grupo de Recursos   | Selecione **myResourceGroup** |
    | **Detalhes da instância** |                                                                 |
    | Name             | Insira **myVNet**                                    |
    | Region           | Selecione **E.U.A. Leste**. |

3. Selecione o separador **endereços IP** ou selecione o botão **Seguinte: Endereços IP** na parte inferior da página.

4. No separador **endereços IP,** introduza estas informações:

    | Definição            | Valor                      |
    |--------------------|----------------------------|
    | Espaço de endereço IPv4 | Insira **10.1.0.0/16** |

5. No **nome da sub-rede,** selecione a palavra **predefinição**.

6. Na **sub-rede Editar,** insira esta informação:

    | Definição            | Valor                      |
    |--------------------|----------------------------|
    | Nome da sub-rede | Insira **mySubnet** |
    | Intervalo de endereços da sub-rede | Insira **10.1.0.0/24** |

7. Selecione **Guardar**.

8. Selecione o separador **Segurança.**

9. Em **BastionHost**, selecione **Enable**. Insira esta informação:

    | Definição            | Valor                      |
    |--------------------|----------------------------|
    | Nome de bastião | Insira **myBastionHost** |
    | Espaço de endereço AzureBastionSubnet | Insira **10.1.1.0/24** |
    | Endereço IP Público | Selecione **Criar novo**. </br> Para **nome,** insira **myBastionIP**. </br> Selecione **OK**. |


8. Selecione o **separador 'Rever +' ou** selecionar o botão **'Rever +' criar.**

9. Selecione **Criar**.

## <a name="create-a-virtual-machine"></a>Criar uma máquina virtual

Nesta secção, irá criar uma máquina virtual que será usada para testar o ponto final privado.

1. No lado superior esquerdo do portal, selecione **Criar uma** máquina Virtual compute de recurso ou procurar por máquina  >    >   **Virtual** na caixa de pesquisa.
   
2. Na **Criação de uma máquina virtual,** escreva ou selecione os valores no **separador Básicos:**

    | Definição | Valor                                          |
    |-----------------------|----------------------------------|
    | **Detalhes do projeto** |  |
    | Subscrição | selecione a subscrição do Azure |
    | Grupo de Recursos | Selecione **myResourceGroup** |
    | **Detalhes da instância** |  |
    | Nome da máquina virtual | Insira **o myVM** |
    | Region | Selecione **E.U.A. Leste**. |
    | Opções de Disponibilidade | Selecione **Não é necessário um despedimento de infraestrutura** |
    | Imagem | Selecione **Windows Server 2019 Datacenter - Gen1** |
    | Instância do Azure Spot | Selecione **Não** |
    | Tamanho | Escolha o tamanho VM ou tome a definição predefinida |
    | **Conta de administrador** |  |
    | Nome de utilizador | Insira um nome de utilizador |
    | Palavra-passe | Introduza uma senha |
    | Confirmar palavra-passe | Reintroduza a palavra-passe |

3. Selecione o **separador 'Rede'** ou selecione **Seguinte: Discos** e, em seguida, **seguinte: Networking**.
  
4. No separador Networking, selecione ou introduza:

    | Definição | Valor |
    |-|-|
    | **Interface de rede** |  |
    | Rede virtual | **myVNet** |
    | Sub-rede | **mySubnet** |
    | IP público | Selecione **Nenhuma**. |
    | Grupo de segurança de rede NIC | **Básica**|
    | Portas de entrada públicas | Selecione **Nenhuma**. |
   
5. Selecione **Rever + criar**. 
  
6. Reveja as definições e, em seguida, **selecione Criar**.

## <a name="create-a-cosmos-db-account-with-a-private-endpoint"></a>Criar uma conta De Cosmos DB com um ponto final privado

Nesta secção, você vai criar uma conta Cosmos DB e configurar o ponto final privado.

1. No menu à esquerda, selecione Criar uma conta DB de base de **dados** de recursos  >    >  **cosmos,** ou procurar **a conta DB cosmos** na caixa de pesquisa.

2. No separador Básicos da **conta Create Cosmos DB** insira ou selecione as **seguintes** informações:

    | Definição | Valor                                          |
    |-----------------------|----------------------------------|
    | **Detalhes do projeto** |  |
    | Subscrição | Selecione a sua subscrição do Azure. |
    | Grupo de Recursos | Selecione **myResourceGroup**. |
    | **Detalhes da instância** |  |
    | Nome da conta | Insira **mycosmosdb**. Se o nome não estiver disponível, insira um nome único. |
    | API | Selecione **Núcleo (SQL)**. |
    | Localização | Selecione **East US**. |
    | Modo de capacidade | Deixe o **padrão Desatado De produção**. |
    | Aplicar Desconto de Escalão Gratuito | Deixe o predefinido **Não aplicar**. |
    | Redundância Geográfica | Deixe o **desativação predefinido**. |
    | Escritas de várias regiões | Deixe o **desativação predefinido**. |
   
3. Selecione o **separador 'Rede'** ou selecione o botão **Seguinte: Botão de rede.**

4. No **separador Networking,** introduza ou selecione as seguintes informações:

    | Definição | Valor |
    | ------- | ----- |
    | **Conectividade de rede** | |
    | Método de conectividade | Selecione **Ponto final privado**. |
    | **Configurar a Firewall** | |
    | Permitir o acesso a partir do portal Azure | Deixe o padrão **Permitir**. |
    | Permitir o acesso a partir do meu IP | Deixe o **Deny** padrão. |

5. No **ponto final privado,** selecione **+ Adicionar**.

6. Na **Criação de ponto final privado** insira ou selecione as seguintes informações:

    | Definição | Valor                                          |
    |-----------------------|----------------------------------|
    | Subscrição | selecione a subscrição do Azure |
    | Grupo de Recursos | Selecione **myResourceGroup** |
    | Localização | Selecione **E.U.A. Leste**. |
    | Name | Insira **o myPrivateEndpoint** |
    | Subresource-alvo | Deixe o núcleo predefinido **(SQL)** |
    | **Rede** |  |
    | Rede virtual | Selecione **myVNet** |
    | Sub-rede | Selecione **mySubnet** |
    | **Integração privada de DNS** |
    | Integrar com zona DNS privada | Deixe o padrão **Sim** |
    | Zona DNS Privada | Deixe o padrão (Novo) privatelink.documents.azure.com |

7. Selecione **OK**.

8. Selecione **Rever + criar**.

9. Selecione **Criar**.

### <a name="add-a-database-and-a-container"></a>Adicione uma base de dados e um recipiente

1. Selecione **Got to resource** or in the left-hand menu of the Azure portal, select All **Resources**  >  **mycosmosdb**.

2. No menu da esquerda, selecione **Data Explorer**.

3. Na janela **Data Explorer,** selecione **Novo Recipiente**.

4. No **Adicionar Recipiente,** insira ou selecione as seguintes informações:

    | Definição | Valor |
    | ------- | ----- |
    | ID da base de dados | Deixe o padrão de **Criar novo**. </br> **Insira o meudatabaseid** na caixa de texto. |
    | Produção (400 - 100.000 RU/s) | Deixe o padrão do **Manual**. </br> Insira **400** na caixa de texto. |
    | ID do Contentor | Insira **mycontainerid** |
    | Chave de partição | Entrar **/mykey** |

5. Selecione **OK**.

6. Na secção **Definições** da conta CosmosDB, selecione **Keys**.

7. Selecione a cópia na **CADEIA DE LIGAÇÃO PRIMÁRIA**.

## <a name="test-connectivity-to-private-endpoint"></a>Testar conectividade com o ponto final privado

Nesta secção, você usará a máquina virtual que criou no passo anterior para ligar à conta Cosmos DB através do ponto final privado.

1. Selecione **grupos** de recursos no painel de navegação à esquerda.

1. Selecione **myResourceGroup**.

1. Selecione **myVM**.

1. Na página geral do **myVM,** selecione **Connect** e, em **seguida, Bastion**.

1. Selecione o botão **azul Use Bastion.**

1. Introduza o nome de utilizador e a palavra-passe que introduziu durante a criação da máquina virtual.

1. Abra o Windows PowerShell no servidor depois de ligar.

1. Insira `nslookup <cosmosdb-account-name>.documents.azure.com` e valide a resolução de nomes. **\<cosmosdb-account-name>** Substitua-o pelo nome da conta Cosmos DB que criou nos passos anteriores. 

    ```powershell
    Server:  UnKnown
    Address:  168.63.129.16

    Non-authoritative answer:
    Name:    mycosmosdb8675.privatelink.documents.azure.com
    Address:  10.1.0.5
    Aliases:  mycosmosdb8675.documents.azure.com
    ```
    Um endereço IP privado de **10.1.0.5** é devolvido para o nome da conta Cosmos DB.  Este endereço encontra-se na sub-rede da rede virtual que criou anteriormente.
    
1. Obtenha a sua cadeia de ligação primária Azure Cosmos DB do portal. Uma cadeia de ligação válida está no formato:
   
   Para contas da API SQL: `https://<accountName>.documents.azure.com:443/;AccountKey=<accountKey>;` Para Azure Cosmos DB API para MongoDB: `mongodb://<accountName>:<accountKey>@cdbmongo36.mongo.cosmos.azure.com:10255/?ssl=true&replicaSet=globaldb&retrywrites=false`

1. Instale o [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md?tabs=windows&toc=%2fazure%2fstorage%2fblobs%2ftoc.json) na máquina virtual.

1. **Selecione Terminar** depois de instalado o **Microsoft Azure Storage Explorer.**  Deixe a caixa verificada para abrir o pedido.

1. No ecrã **de armazenamento 'Ligar a Azure',** selecione **Cancelar**.

1. No Storage Explorer, selecione o botão do rato certo nas **contas DB cosmos** e selecione **Connect to Cosmos DB**.

1. Deixe o padrão de **SQL** em **Select API**.

1. Na caixa em **Connection String,** cole a cadeia de ligação da conta Cosmos DB que copiou nos passos anteriores.

1. Selecione **Seguinte**.

1. Verifique se as definições estão corretas no **Resumo da Ligação**.  

1. Selecione **Ligar**.

1. Feche a ligação ao **myVM**.


## <a name="clean-up-resources"></a>Limpar os recursos

Se não continuar a utilizar esta aplicação, elimine a rede virtual, a máquina virtual e a conta DB cosmos com os seguintes passos:

1. A partir do menu da esquerda, selecione **Grupos de Recursos.**

2. Selecione **myResourceGroup**.

3. Selecione **Eliminar grupo de recursos**.

4. Introduza **o myResourceGroup** em **TIPO O NOME DO GRUPO DE RECURSOS**.

5. Selecione **Eliminar**.

## <a name="next-steps"></a>Próximos passos

Neste tutorial, criou um:

* Rede virtual e hospedeiro de bastião.
* Máquina virtual.
* Conta Cosmos DB.

Saiba como criar um serviço de Ligação Privada:
> [!div class="nextstepaction"]
> [Criar um serviço de Ligação Privada](create-private-link-service-portal.md)
