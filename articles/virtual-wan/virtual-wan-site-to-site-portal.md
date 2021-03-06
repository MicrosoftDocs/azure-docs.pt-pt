---
title: 'Tutorial: Use Azure Virtual WAN para criar ligações site-to-site'
description: Neste tutorial, vai aprender a utilizar uma WAN Virtual do Azure para criar uma ligação VPN de site a site ao Azure.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: tutorial
ms.date: 03/05/2021
ms.author: cherylmc
ms.openlocfilehash: 365952beca96b91d312eab209c5332fca2d4842b
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/31/2021
ms.locfileid: "106061881"
---
# <a name="tutorial-create-a-site-to-site-connection-using-azure-virtual-wan"></a>Tutorial: Criar uma ligação site a site com a WAN Virtual do Azure

Este tutorial mostra-lhe como utilizar a WAN Virtual para se ligar aos seus recursos no Azure através de uma ligação VPN IPsec/IKE (IKEv1 e IKEv2). Este tipo de ligação requer um dispositivo VPN localizado no local que tenha um endereço IP público com acesso exterior atribuído ao mesmo. Para obter mais informações sobre a WAN Virtual, veja a [Descrição Geral da WAN Virtual](virtual-wan-about.md).

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar uma WAN Virtual
> * Criar um hub
> * Criar um site
> * Conecte um site a um centro
> * Ligue um site VPN a um hub
> * Ligar uma VNet a um hub
> * Descarregue um ficheiro de configuração
> * Configure o seu gateway VPN

> [!NOTE]
> Se tiver muitos sites, teria de utilizar, normalmente, um [parceiro de WAN Virtual](https://aka.ms/virtualwan) para criar esta configuração. Contudo, pode criá-la sozinho se se sentir à vontade com o trabalho em rede e souber configurar o seu próprio dispositivo VPN.
>

![Diagrama da WAN Virtual](./media/virtual-wan-about/virtualwan.png)

## <a name="prerequisites"></a>Pré-requisitos

Antes de iniciar a configuração, verifique se cumpre os seguintes critérios:

[!INCLUDE [Before you begin](../../includes/virtual-wan-before-include.md)]

## <a name="create-a-virtual-wan"></a><a name="openvwan"></a>Criar uma WAN Virtual

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-create-vwan-include.md)]

## <a name="create-a-hub"></a><a name="hub"></a>Criar um hub

Um hub é uma rede virtual que pode conter gateways para a funcionalidade site-to-site, ExpressRoute ou ponto a local. Depois de criar o hub, vai ser cobrado pelo hub, mesmo que não anexe quaisquer sites. Leva 30 minutos para criar a porta VPN local no centro virtual.

[!INCLUDE [Create a hub](../../includes/virtual-wan-tutorial-s2s-hub-include.md)]

## <a name="create-a-site"></a><a name="site"></a>Criar um site

Nesta secção, cria-se o site. Os sites correspondem às suas localizações físicas. Crie todos os sites que precisar. Por exemplo, se tiver uma sucursal em Nova Iorque, uma em Londres e outra em Lisboa, tem de criar três sites separados. Esses sites contêm os pontos finais dos seus dispositivos VPN no local. Você pode criar até 1000 sites por centro virtual em um WAN virtual. Se tivesse vários centros, poderia criar 1000 por cada um desses centros. Se tiver um dispositivo CPE parceiro Virtual WAN, consulte-os para saber mais sobre a sua automatização para a Azure. Tipicamente, a automatização implica uma simples experiência de clique para exportar informações de sucursais em larga escala para o Azure, e estabelecer conectividade do CPE para o gateway Azure Virtual WAN VPN. Para mais informações, consulte [a orientação da Automação da Azure para os parceiros CPE.](virtual-wan-configure-automation-providers.md)

[!INCLUDE [Create a site](../../includes/virtual-wan-tutorial-s2s-site-include.md)]

## <a name="connect-the-vpn-site-to-the-hub"></a><a name="connectsites"></a>Ligue o site VPN ao centro

Neste passo, ligue o seu site VPN ao centro.

[!INCLUDE [Connect VPN sites](../../includes/virtual-wan-tutorial-s2s-connect-vpn-site-include.md)]

## <a name="connect-the-vnet-to-the-hub"></a><a name="vnet"></a>Ligue o VNet ao hub

[!INCLUDE [Connect](../../includes/virtual-wan-connect-vnet-hub-include.md)]

## <a name="download-vpn-configuration"></a><a name="device"></a>Transferir a configuração da VPN

Utilize a configuração do dispositivo VPN para configurar o seu dispositivo VPN no local.

1. Na página da WAN virtual, clique em **Overview** (Descrição geral).
2. No topo da página **VPNSite hub ->,** clique em **Download VPN config**. O Azure cria uma conta de armazenamento no grupo de recursos 'microsoft-network-[location]', onde a localização é a localização do WAN. Depois de aplicar a configuração aos dispositivos VPN, pode eliminar esta conta de armazenamento.
3. Após a conclusão da criação do ficheiro, pode clicar na ligação para transferi-lo.
4. Aplique a configuração no seu dispositivo VPN no local.

### <a name="about-the-vpn-device-configuration-file"></a>Sobre o ficheiro de configuração do dispositivo VPN

O ficheiro de configuração do dispositivo contém as definições que vão ser utilizadas para configurar o dispositivo VPN no local. Quando vir este ficheiro, repare nas informações seguintes:

* **vpnSiteConfiguration -** esta secção mostra os detalhes do dispositivo configurados como site que se vai ligar à WAN virtual. Inclui o nome e o endereço IP público do dispositivo da sucursal.
* **VPNSiteConnections -** Esta secção fornece informações sobre as seguintes definições:

    * **Espaço** de endereço do(s) hub virtual(s) VNet.<br>Exemplo:
 
        ```
        "AddressSpace":"10.1.0.0/24"
        ```
    * **Espaço** de endereço dos VNets que estão ligados ao hub.<br>Exemplo:

         ```
        "ConnectedSubnets":["10.2.0.0/16","10.3.0.0/16"]
         ```
    * **Endereços IP** do vpngateway do hub virtual. Como cada ligação da via é composta por dois túneis em configuração ativa, verá ambos os endereços IP listados neste ficheiro. Neste exemplo, vê "Instance0" e "Instance1" para cada site.<br>Exemplo:

        ``` 
        "Instance0":"104.45.18.186"
        "Instance1":"104.45.13.195"
        ```
    * **Detalhes de configuração de ligação vpngateway,** tais como BGP, chave pré-partilhada, etc. O PSK é a chave pré-partilhada que é gerada automaticamente para si. Pode sempre editar a ligação na página Overview (Descrição geral) de um PSK personalizado.
  
### <a name="example-device-configuration-file"></a>Exemplo de ficheiro de configuração de dispositivo

  ```
  { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"r403583d-9c82-4cb8-8570-1cbbcd9983b5"
      },
      "vpnSiteConfiguration":{ 
         "Name":"testsite1",
         "IPAddress":"73.239.3.208"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe",
               "ConnectedSubnets":[ 
                  "10.2.0.0/16",
                  "10.3.0.0/16"
               ]
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.186",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"bkOWe5dPPqkx0DfFE3tyuP7y3oYqAEbI",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   },
   { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"1f33f891-e1ab-42b8-8d8c-c024d337bcac"
      },
      "vpnSiteConfiguration":{ 
         "Name":" testsite2",
         "IPAddress":"66.193.205.122"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe"
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.187",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"XzODPyAYQqFs4ai9WzrJour0qLzeg7Qg",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   },
   { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"cd1e4a23-96bd-43a9-93b5-b51c2a945c7"
      },
      "vpnSiteConfiguration":{ 
         "Name":" testsite3",
         "IPAddress":"182.71.123.228"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe"
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.187",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"YLkSdSYd4wjjEThR3aIxaXaqNdxUwSo9",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   }
  ```

### <a name="configuring-your-vpn-device"></a>Configurar o dispositivo VPN

>[!NOTE]
> Se está a trabalhar com uma solução de parceiros do WAN Virtual, a configuração do dispositivo VPN acontece automaticamente. O controlador de dispositivo obtém o ficheiro de configuração do Azure e aplica o dispositivo para configurar a ligação ao Azure. Isto significa que não é preciso saber como configurar o dispositivo VPN manualmente.
>

Se precisar de instruções para configurar o dispositivo, pode utilizar as instruções na [página VPN device configuration scripts](~/articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#configscripts) (Scripts de configuração de dispositivos VPN), tendo em conta os seguintes avisos:

* As instruções a página de dispositivos VPN não foram escritas para a WAN Virtual, mas pode utilizar os valores desta a partir do ficheiro de configuração para configurar o seu dispositivo VPN manualmente. 
* Os scripts de configuração do dispositivo transferíveis que se destinam ao Gateway de VPN não funcionam para a WAN Virtual, uma vez que a configuração é diferente.
* Um novo WAN virtual pode suportar tanto o IKEv1 como o IKEv2.
* O WAN virtual pode utilizar dispositivos VPN baseados em políticas e por rotas e instruções do dispositivo.

## <a name="configure-your-vpn-gateway"></a><a name="gateway-config"></a>Configure o seu gateway VPN

Pode visualizar e configurar as definições de gateway VPN a qualquer momento selecionando **Ver/Configurar**.

:::image type="content" source="media/virtual-wan-site-to-site-portal/view-configuration-1.png" alt-text="Screenshot que mostra a página 'VPN (Site-to-site)' com uma seta que aponta para a ação 'Ver/Configurar'." lightbox="media/virtual-wan-site-to-site-portal/view-configuration-1-expand.png":::

Na página **Editar VPN Gateway,** pode ver as seguintes definições:

* Endereço IP público VPN Gateway (atribuído por Azure)
* Endereço IP privado VPN Gateway (atribuído por Azure)
* Endereço IP BGP padrão VPN Gateway (atribuído por Azure)
* Opção de configuração para Endereço IP BGP personalizado: Este campo está reservado para APIPA (Endereço IP Privado Automático). A Azure suporta o BGP IP nos intervalos 169.254.21.* e 169.254.22.*. O Azure aceita ligações BGP nestas gamas, mas irá marcar a ligação com o IP BGP predefinido.

   :::image type="content" source="media/virtual-wan-site-to-site-portal/view-configuration-2.png" alt-text="Ver configuração" lightbox="media/virtual-wan-site-to-site-portal/view-configuration-2-expand.png":::

## <a name="clean-up-resources"></a><a name="cleanup"></a>Limpar os recursos

Quando já não precisar dos recursos que criou, elimine-os. Alguns dos recursos DE WAN virtuais devem ser eliminados numa determinada ordem devido a dependências. Apagar pode levar cerca de 30 minutos para ser concluído.

[!INCLUDE [Delete resources](../../includes/virtual-wan-resource-cleanup.md)]

## <a name="next-steps"></a>Passos seguintes

Em seguida, para saber mais sobre O WAN Virtual, consulte:

> [!div class="nextstepaction"]
> * [FAQ da WAN Virtual](virtual-wan-faq.md)
