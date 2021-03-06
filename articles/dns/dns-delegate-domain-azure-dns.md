---
title: 'Tutorial: Hospedar o seu domínio e subdomínio - Azure DNS'
description: Neste tutorial, você aprende a configurar O DNS do Azure para hospedar as suas zonas DNS.
services: dns
author: rohinkoul
ms.service: dns
ms.topic: tutorial
ms.date: 04/19/2021
ms.author: rohink
ms.openlocfilehash: c9c0568eb4d8a7403fc29f34a4c4e9f6e0fadecd
ms.sourcegitcommit: 425420fe14cf5265d3e7ff31d596be62542837fb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/20/2021
ms.locfileid: "107738869"
---
# <a name="tutorial-host-your-domain-in-azure-dns"></a>Tutorial: Alojar o seu domínio no DNS do Azure

Pode utilizar o DNS do Azure para alojar o seu domínio DNS e gerir os registos DNS. Ao alojar os seus domínios no Azure, pode gerir os recursos DNS com as mesmas credenciais, APIs, ferramentas e faturação dos seus outros serviços do Azure.

Suponha que compre o domínio `contoso.net` a partir de um registrador de nome de domínio e, em seguida, crie uma zona com o nome em `contoso.net` Azure DNS. Uma vez que é o proprietário do domínio, o seu registrador oferece-lhe a opção de configurar os registos do servidor de nomes (NS) para o seu domínio. O registrador armazena os registos NS na zona dos pais .NET. Os utilizadores de Internet em todo o mundo são então direcionados para o seu domínio na sua zona de DNS Azure quando tentam resolver os registos DNS em contoso.net.


Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar uma zona DE DNS.
> * Recupere uma lista de servidores de nomes.
> * Delegar o domínio.
> * Verifique se a delegação está a funcionar.


Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Deve ter um nome de domínio disponível para testar com o que pode hospedar no Azure DNS. Deve ter controlo total sobre este domínio. O controlo total inclui a capacidade de definir os registos do servidor de nomes (NS) do domínio.

Neste exemplo, vamos fazer referência ao domínio dos pais a `contoso.net` .

## <a name="create-a-dns-zone"></a>Criar uma zona DNS

1. Vá ao [portal Azure](https://portal.azure.com/) para criar uma zona DE DNS. Procure e selecione **zonas DNS**.

   ![Zona DNS](./media/dns-delegate-domain-azure-dns/openzone650.png)

1. **Selecione Criar zona DNS**.

1. Na página da **zona Criar DNS,** insira os seguintes valores e, em seguida, selecione **Criar**. Por exemplo, `contoso.net`.

   > [!NOTE] 
   > Se a nova zona que está a criar for uma zona infantil (por exemplo, zona dos pais = `contoso.net` zona infantil `child.contoso.net` =), consulte a nossa [criação de um novo tutorial de zona de DNS infantil](./tutorial-public-dns-zones-child.md)

    | **Definição** | **Valor** | **Detalhes** |
    |--|--|--|
    | **Grupo de recursos**    | ContosoRG | Crie um grupo de recursos. O nome do grupo de recursos deve ser único dentro da subscrição que selecionou. A localização do grupo de recursos não tem qualquer impacto na zona DNS. A localização da zona do DNS é sempre "global", e não é mostrada. |
    | **Criança da zona**        | deixar descontrolado | Uma vez que esta zona **não** é uma [zona infantil,](./tutorial-public-dns-zones-child.md) você deve deixar isso descontrolado |
    | **Nome**              | `contoso.net` | Campo para o nome da zona dos seus pais      |
    | **Localização**          | E.U.A. Leste | Este campo baseia-se na localização selecionada como parte da criação do grupo de recursos  |
    

## <a name="retrieve-name-servers"></a>Obter servidores de nomes

Antes de pode delegar a zona DNS ao DNS do Azure, terá de conhecer os servidores de nomes da sua zona. O Azure DNS dá servidores de nomes de uma piscina cada vez que um fuso é criado.

1. Com a zona DNS criada, no painel **Favoritos** do portal do Azure, selecione **Todos os recursos**. Na página **Todos os recursos**, selecione a zona de DNS. Se a subscrição selecionada já tiver vários recursos, pode introduzir o seu nome de domínio na caixa de **nomes Filter para** aceder facilmente ao gateway da aplicação. 

1. Obtenha os servidores de nome na página da zona DNS. Neste exemplo, a zona `contoso.net` foi atribuída a servidores de `ns1-01.azure-dns.com` nomes, `ns2-01.azure-dns.net` * e `ns3-01.azure-dns.org` `ns4-01.azure-dns.info` :

   ![Lista dos servidores de nomes](./media/dns-delegate-domain-azure-dns/viewzonens500.png)

O DNS do Azure cria automaticamente registos NS autoritativos na sua zona para os servidores de nomes atribuídos.

## <a name="delegate-the-domain"></a>Delegar o domínio

Assim que a zona DNS for criada e tiver os servidores de nomes, terá de atualizar o domínio dos pais com os servidores de nomeS DNS do Azure. Cada entidade de registo tem as suas próprias ferramentas de gestão de DNS para alterar os registos do servidor de nomes de um domínio. 

1. Na página de gestão do DNS da entidade de registo, edite os registos NS e substitua-os pelos servidores de nomes do DNS do Azure.

1. Quando delegar um domínio para O Azure DNS, deve utilizar os servidores de nome que o Azure DNS fornece. Use todos os quatro servidores de nome, independentemente do nome do seu domínio. A delegação de domínio não requer que um servidor de nomes utilize o mesmo domínio de nível superior que o seu domínio.

> [!NOTE]
> Quando copiar cada endereço do servidor de nomes, certifique-se de que copia o ponto à direita no fim do endereço. O ponto à direita indica o fim de um nome de domínio completamente qualificado. Alguns registradores anexam o período se o nome NS não o tiver no final. Para estar em conformidade com o DNS RFC, inclua o período de trilha.

As delegações que usam servidores de nomes na sua própria zona, por vezes chamadas *de servidores de nomes de vaidade,* não são atualmente suportadas no Azure DNS.

## <a name="verify-the-delegation"></a>Verifique a delegação

Depois de completar a delegação, pode verificar se está a funcionar utilizando uma ferramenta como *a nslookup* para consultar o registo start of Authority (SOA) para a sua zona. O registo SOA é criado automaticamente quando a zona é criada. Pode ter de esperar pelo menos 10 minutos depois de completar a delegação, antes de poder verificar com sucesso se está a funcionar. Pode demorar algum tempo para as alterações serem propagadas em todo o sistema DNS.

Não é preciso especificar os servidores de nomeS DNS do Azure. Se a delegação estiver configurada corretamente, o processo de resolução DNS normal localiza os servidores de nomes automaticamente.

1. A partir de um pedido de comando, insira um comando nslookup semelhante ao seguinte exemplo:

   ```
   nslookup -type=SOA contoso.net
   ```

1. Verifique se a sua resposta se parece com a seguinte saída nslookup:

   ```
   Server: ns1-04.azure-dns.com
   Address: 208.76.47.4

   contoso.net
   primary name server = ns1-04.azure-dns.com
   responsible mail addr = msnhst.microsoft.com
   serial = 1
   refresh = 900 (15 mins)
   retry = 300 (5 mins)
   expire = 604800 (7 days)
   default TTL = 300 (5 mins)
   ```

## <a name="clean-up-resources"></a>Limpar os recursos

Pode manter o grupo de recursos **contosoRG** se pretende acompanhar o tutorial seguinte. Caso contrário, elimine o grupo de recursos **contosoRG** para eliminar os recursos criados neste tutorial.

Selecione o grupo de recursos **contosoRG** e, em seguida, **selecione Delete resource group**. 

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, criou uma zona DE DNS para o seu domínio e delegou-a para o Azure DNS. Para saber mais sobre o DNS do Azure e as aplicações Web, avance para o tutorial de aplicações Web.

> [!div class="nextstepaction"]
> [Criar registos DNS para uma aplicação Web num domínio personalizado](./dns-web-sites-custom-domain.md)
