---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 02/11/2021
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 708baa83ca919adcc374be36c229ce3ff30da384
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/19/2021
ms.locfileid: "100362939"
---
1. Na página do portal para o seu wan virtual, na secção **Conectividade,** selecione **sites VPN** para abrir a página de sites VPN.
1. Na página **Sites de VPN**, clique em **+Criar site**.
1. Na página **do Site Create VPN,** no separador **Básicos,** complete os seguintes campos:

   :::image type="content" source="./media/virtual-wan-tutorial-site-include/site-basics.png" alt-text="Separador básico" lightbox="./media/virtual-wan-tutorial-site-include/site-basics.png":::

    * **Região** - Anteriormente referida como localização. Esta é a localização em que pretende criar este recurso do site.
    * **Nome** - O nome pelo qual pretende consultar o seu site no local.
    * **Fornecedor de dispositivos** - O nome do fornecedor de dispositivos VPN (por exemplo: Citrix, Cisco, Barracuda). Adicionar o fornecedor de dispositivos pode ajudar a Equipa Azure a compreender melhor o seu ambiente de forma a adicionar possibilidades de otimização adicionais no futuro, ou para ajudá-lo a resolver problemas.
    * **Espaço de endereço privado** - O espaço de endereço IP localizado no seu site no local. O tráfego destinado a este espaço de endereços é encaminhado para o site local. Isto é necessário quando o BGP não está ativado para o site.
    
      >[!NOTE]
      >Se editar o espaço do endereço após a criação do site (por exemplo, adicionar um espaço de endereço adicional) pode demorar 8-10 minutos a atualizar as rotas eficazes enquanto os componentes são recriados.
      >
1. Selecione **Links** para adicionar informações sobre as ligações físicas no ramo. Se tiver um dispositivo CPE parceiro Virtual WAN, consulte com eles para ver se esta informação é trocada com o Azure como parte do upload de informações da sucursal configurados a partir dos seus sistemas.

   :::image type="content" source="./media/virtual-wan-tutorial-site-include/site-links.png" alt-text="Separador de links" lightbox="./media/virtual-wan-tutorial-site-include/site-links.png":::

   * **Nome do link** - Um nome que pretende fornecer para o link físico no Site VPN. Exemplo: mylink1.
   * **Velocidade de ligação** - Esta é a velocidade do dispositivo VPN na localização do ramo. Exemplo: 50, o que significa 50 Mbps é a velocidade do dispositivo VPN no local do ramo.
   * **Nome do fornecedor** de ligação - O nome do link físico no Site VPN. Exemplo: ATT, Verizon.
   * **Link endereço IP/FQDN** - Endereço IP público do dispositivo no local utilizando este link. Opcionalmente, pode fornecer o endereço IP privado do seu dispositivo VPN no local que está por trás do ExpressRoute. Também pode incluir um nome de domínio totalmente qualificado. Por exemplo, *something.contoso.com.* O FQDN deve ser resolúvel a partir do portal VPN. Isto é possível se o servidor DNS que hospeda este FQDN for acessível através da internet. O endereço IP tem precedência quando tanto o endereço IP como o FQDN são especificados.

     >[!NOTE]
     >
     >* Suporta um endereço IPv4 por FQDN. Se o FQDN for resolvido para vários endereços IP, então o gateway VPN capta o primeiro endereço IP4 da lista. Os endereços IPv6 não são suportados neste momento.
     >
     >* O gateway VPN mantém uma cache DNS que é refrescada a cada 5 minutos. O portal tenta resolver fQDNs apenas para túneis desligados. Uma alteração de reset ou configuração de gateway também pode desencadear a resolução FQDN.
     >
   * **Link Border Gateway Protocol** - Configurar o BGP numa ligação WAN virtual equivale a configurar o BGP numa VPN de gateway de rede virtual Azure. O seu endereço de pares BGP no local não deve ser o mesmo que o endereço IP público do seu VPN para dispositivo ou o espaço de endereço VNet do site VPN. Utilize um endereço IP diferente no dispositivo VPN para o seu IP de pares BGP. Pode ser um endereço atribuído à interface de loopback no dispositivo. Especifique este endereço no site VPN correspondente que representa a localização.  Para pré-requisitos BGP, consulte [Sobre o BGP com o Azure VPN Gateway](../articles/vpn-gateway/vpn-gateway-bgp-overview.md). Pode sempre editar uma ligação de ligação VPN para atualizar os seus parâmetros BGP (Peering IP no link e no AS #).
1. Pode adicionar ou eliminar mais links. Quatro links por VPN Site são suportados. Por exemplo, se tiver quatro ISPs (fornecedor de serviços de Internet) na localização da sucursal, pode criar quatro links, um por cada ISP, e fornecer as informações para cada link.
1. Uma vez terminado o preenchimento dos campos, selecione **Review + create** para verificar e criar o site.
1. Navegue para o centro virtual que deseja e desmarca a **associação Hub** para ligar o seu site VPN ao centro.

   :::image type="content" source="./media/virtual-wan-tutorial-site-include/connect.png" alt-text="Ligue-se a este centro" lightbox="./media/virtual-wan-tutorial-site-include/connect.png":::
