---
title: 'Tutorial: Ligue um VNet a um circuito ExpressRoute - Portal Azure'
description: Este tutorial mostra-lhe como criar uma ligação para ligar uma rede virtual a um circuito Azure ExpressRoute utilizando o portal Azure.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: tutorial
ms.date: 10/15/2020
ms.author: duau
ms.custom: seodec18
ms.openlocfilehash: a9bfb4beea2c2bec1b819d228215cfff65e37fe4
ms.sourcegitcommit: 5fd1f72a96f4f343543072eadd7cdec52e86511e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/01/2021
ms.locfileid: "106110262"
---
# <a name="tutorial-connect-a-virtual-network-to-an-expressroute-circuit-using-the-portal"></a>Tutorial: Ligue uma rede virtual a um circuito ExpressRoute utilizando o portal

> [!div class="op_single_selector"]
> * [Portal do Azure](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-linkvnet-arm.md)
> * [CLI do Azure](howto-linkvnet-cli.md)
> * [Vídeo - Portal Azure](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (clássico)](expressroute-howto-linkvnet-classic.md)
> 

Este tutorial ajuda-o a criar uma ligação para ligar uma rede virtual a um circuito Azure ExpressRoute utilizando o portal Azure. As redes virtuais que liga ao circuito Azure ExpressRoute podem estar na mesma subscrição ou fazer parte de outra subscrição.

Neste tutorial, ficará a saber como:
> [!div class="checklist"]
> - Ligue uma rede virtual a um circuito na mesma subscrição.
> - Ligue uma rede virtual a um circuito numa subscrição diferente.
> - Elimine a ligação entre a rede virtual e o circuito ExpressRoute.

## <a name="prerequisites"></a>Pré-requisitos

* Reveja os [pré-requisitos,](expressroute-prerequisites.md) [requisitos de encaminhamento](expressroute-routing.md)e [fluxos de trabalho](expressroute-workflows.md) antes de iniciar a configuração.

* Deve ter um circuito ExpressRoute ativo.
  * Siga as instruções para [criar um circuito ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md) e tenha o circuito ativado pelo seu fornecedor de conectividade.
  * Certifique-se de que tem o Azure a espreitar para o seu circuito. Consulte o [Create e modifique o espreitamento de um artigo de circuito ExpressRoute](expressroute-howto-routing-portal-resource-manager.md) para obter instruções de observação e encaminhamento.
  * Certifique-se de que o espreitamento privado do Azure fica configurado e estabelece o Espreitamento de BGP entre a sua rede e a Microsoft para a conectividade de ponta a ponta.
  * Certifique-se de que tem uma rede virtual e um gateway de rede virtual criado e totalmente a provisionado. Siga as instruções para [criar uma porta de rede virtual para o ExpressRoute](expressroute-howto-add-gateway-resource-manager.md). Um gateway de rede virtual para ExpressRoute utiliza o GatewayType 'ExpressRoute', não VPN.

* Pode ligar até 10 redes virtuais a um circuito Standard ExpressRoute. Todas as redes virtuais devem estar na mesma região geopolítica quando utilizarem um circuito ExpressRoute padrão.

* Um único VNet pode ser ligado a até 16 circuitos ExpressRoute. Utilize o seguinte processo para criar um novo objeto de ligação para cada circuito ExpressRoute a que está a ligar. Os circuitos ExpressRoute podem estar na mesma subscrição, subscrições diferentes ou uma mistura de ambos.

* Se ativar o addon premium ExpressRoute, pode ligar redes virtuais fora da região geopolítica do circuito ExpressRoute. O addon premium também lhe permitirá ligar mais de 10 redes virtuais ao seu circuito ExpressRoute, dependendo da largura de banda escolhida. Consulte as [FAQ](expressroute-faqs.md) para obter mais detalhes sobre o complemento premium.

* Pode [ver um vídeo](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit) antes de começar a entender melhor os passos.

## <a name="connect-a-vnet-to-a-circuit---same-subscription"></a>Ligue um VNet a um circuito - a mesma subscrição

> [!NOTE]
> As informações de configuração BGP não aparecerão se o fornecedor da camada 3 configurar os seus pares. Se o seu circuito estiver num estado a provisionado, deverá ser capaz de criar ligações.
>

### <a name="to-create-a-connection"></a>Para criar uma ligação

1. Certifique-se de que o seu circuito ExpressRoute e o seu perspero privado Azure foram configurados com sucesso. Siga as instruções em [Criar um circuito ExpressRoute](expressroute-howto-circuit-arm.md) e [criar e modificar o espreitamento para um circuito ExpressRoute](expressroute-howto-routing-arm.md). O seu circuito ExpressRoute deve parecer-se com a seguinte imagem:

    :::image type="content" source="./media/expressroute-howto-linkvnet-portal-resource-manager/express-route-circuit.png" alt-text="Screenshot do circuito ExpressRoute":::

1. Pode agora iniciar a disponibilização de uma ligação para ligar a sua porta de entrada de rede virtual ao circuito ExpressRoute. Selecione   >  **Ligação Adicione** para abrir a página **de ligação Adicionar.**

    :::image type="content" source="./media/expressroute-howto-linkvnet-portal-resource-manager/add-connection.png" alt-text="Adicionar screenshot de conexão":::

1. Introduza um nome para a ligação e, em seguida, selecione **Seguinte: Definições >**.

    :::image type="content" source="./media/expressroute-howto-linkvnet-portal-resource-manager/create-connection-basic.png" alt-text="Criar página básica de conexão":::

1. Selecione o gateway que pertence à rede virtual que pretende ligar ao circuito e selecione **Review + create**. Em seguida, **selecione Criar** após a validação concluída.

    :::image type="content" source="./media/expressroute-howto-linkvnet-portal-resource-manager/create-connection-settings.png" alt-text="Criar página de definições de ligação":::

1. Depois de a sua ligação ter sido configurada com sucesso, o seu objeto de ligação mostrará as informações para a ligação.

    :::image type="content" source="./media/expressroute-howto-linkvnet-portal-resource-manager/connection-object.png" alt-text="Screenshot do objeto de ligação":::

## <a name="connect-a-vnet-to-a-circuit---different-subscription"></a>Ligue um VNet a um circuito - subscrição diferente

Pode partilhar um circuito ExpressRoute através de várias subscrições. A figura a seguir mostra um simples esquema de como a partilha funciona para circuitos ExpressRoute em várias subscrições.

:::image type="content" source="./media/expressroute-howto-linkvnet-portal-resource-manager/cross-subscription.png" alt-text="Conectividade de subscrição cruzada":::

Cada uma das nuvens menores dentro da grande nuvem é usada para representar subscrições que pertencem a diferentes departamentos dentro de uma organização. Cada um dos departamentos da organização usa a sua própria subscrição para implantar os seus serviços, mas podem partilhar um único circuito ExpressRoute para ligar de volta à sua rede no local. Um único departamento (neste exemplo: IT) pode ser dono do circuito ExpressRoute. Outras subscrições dentro da organização podem utilizar o circuito ExpressRoute.

  > [!NOTE]
  > Os custos de conectividade e largura de banda para o circuito dedicado serão aplicados ao proprietário do circuito ExpressRoute. Todas as redes virtuais partilham a mesma largura de banda.
  >

### <a name="administration---about-circuit-owners-and-circuit-users"></a>Administração - Sobre proprietários de circuitos e utilizadores de circuitos

O 'proprietário do circuito' é um utilizador autorizado do recurso de energia do circuito ExpressRoute. O proprietário do circuito pode criar autorizações que podem ser resgatadas por "utilizadores de circuitos". Os utilizadores de circuitos são proprietários de gateways de rede virtuais que não estão dentro da mesma subscrição que o circuito ExpressRoute. Os utilizadores de circuitos podem resgatar autorizações (uma autorização por rede virtual).

O proprietário do circuito tem o poder de modificar e revogar autorizações a qualquer momento. A revogação de uma autorização resulta na suprição de todas as ligações de ligação da subscrição cujo acesso foi revogado.

### <a name="circuit-owner-operations"></a>Operações do proprietário do circuito

**Para criar uma autorização de ligação**

O proprietário do circuito cria uma autorização, que cria uma chave de autorização para ser usada por um utilizador de circuito para ligar os seus gateways de rede virtuais ao circuito ExpressRoute. Uma autorização é válida para apenas uma ligação.

> [!NOTE]
> Cada ligação requer uma autorização separada.
>

1. Na página ExpressRoute, selecione **Autorizações** e, em seguida, escreva um **nome** para a autorização e selecione **Guardar**.

    :::image type="content" source="./media/expressroute-howto-linkvnet-portal-resource-manager/authorization.png" alt-text="Autorizações":::

2. Uma vez guardada a configuração, copie o **ID do recurso** e a **chave de autorização**.

    :::image type="content" source="./media/expressroute-howto-linkvnet-portal-resource-manager/authorization-key.png" alt-text="Chave de autorização":::

**Para eliminar uma autorização de ligação**

Pode eliminar uma ligação selecionando o ícone **Eliminar** para a chave de autorização para a sua ligação.

:::image type="content" source="./media/expressroute-howto-linkvnet-portal-resource-manager/delete-authorization-key.png" alt-text="Eliminar chave de autorização":::

Se pretender eliminar a ligação mas reter a chave de autorização, pode eliminar a ligação da página de ligação do circuito.

:::image type="content" source="./media/expressroute-howto-linkvnet-portal-resource-manager/delete-connection-owning-circuit.png" alt-text="Eliminar circuito de propriedade de conexão":::

### <a name="circuit-user-operations"></a>Operações de utilizador de circuitos

O utilizador do circuito precisa do ID do recurso e de uma chave de autorização do proprietário do circuito.

**Para resgatar uma autorização de ligação**

1. Selecione o + Criar um botão **de recurso.** Procure por **Conexão** e selecione **Criar**.

    :::image type="content" source="./media/expressroute-howto-linkvnet-portal-resource-manager/create-new-resources.png" alt-text="Criar novos recursos":::

1. Certifique-se de que o *tipo de Ligação* está definido para **ExpressRoute**. Selecione o *grupo de Recursos* e *localização* e, em seguida, selecione **OK** na página Basics.

    > [!NOTE]
    > A localização *deve* coincidir com a localização virtual do gateway de rede para a qual está a criar a ligação.

    :::image type="content" source="./media/expressroute-howto-linkvnet-portal-resource-manager/connection-basics.png" alt-text="Página básica":::

1. Na página **Definições,** selecione o *gateway de rede Virtual* e verifique a caixa de verificação de autorização **Redentor.** Introduza a *tecla de Autorização* e o *circuito peer URI* e dê à ligação um nome. Selecione **OK**. 
 
    > [!NOTE]
    > O *Circuito Par URI* é o ID de recursos do circuito ExpressRoute (que pode encontrar sob o painel de definição de propriedades do Circuito ExpressRoute).

    :::image type="content" source="./media/expressroute-howto-linkvnet-portal-resource-manager/connection-settings.png" alt-text="Página de definições":::

1. Reveja as informações na página **Resumo** e selecione **OK**.

    :::image type="content" source="./media/expressroute-howto-linkvnet-portal-resource-manager/connection-summary.png" alt-text="Página de resumo":::

## <a name="clean-up-resources"></a>Limpar os recursos

Pode eliminar uma ligação e desvincular o seu VNet a um circuito ExpressRoute selecionando o ícone **Eliminar** na página para a sua ligação.

:::image type="content" source="./media/expressroute-howto-linkvnet-portal-resource-manager/delete-connection.png" alt-text="Eliminar ligação":::

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, aprendeu a ligar uma rede virtual a um circuito na mesma subscrição e a uma subscrição diferente. Para mais informações sobre o gateway ExpressRoute, consulte: 

> [!div class="nextstepaction"]
> [Sobre os gateways de rede virtual ExpressRoute](expressroute-about-virtual-network-gateways.md)
