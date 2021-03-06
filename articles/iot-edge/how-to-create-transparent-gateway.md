---
title: Criar dispositivo de gateway transparente - Azure IoT Edge | Microsoft Docs
description: Use um dispositivo Azure IoT Edge como um gateway transparente que pode processar informações a partir de dispositivos a jusante
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 03/01/2021
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: f7f05fb84ff6cbe320e8f479912bdcdefdc41021
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2021
ms.locfileid: "103201645"
---
# <a name="configure-an-iot-edge-device-to-act-as-a-transparent-gateway"></a>Configurar um dispositivo IoT Edge para atuar como um gateway transparente

[!INCLUDE [iot-edge-version-201806-or-202011](../../includes/iot-edge-version-201806-or-202011.md)]

Este artigo fornece instruções detalhadas para configurar um dispositivo IoT Edge para funcionar como um gateway transparente para outros dispositivos comunicarem com o IoT Hub. Este artigo usa o termo *gateway IoT Edge* para se referir a um dispositivo IoT Edge configurado como um gateway transparente. Para obter mais informações, consulte [como um dispositivo IoT Edge pode ser usado como porta de entrada](./iot-edge-as-gateway.md).

<!-- 1.1 -->
::: moniker range="iotedge-2018-06"

>[!NOTE]
>Nas versões IoT Edge 1.1 ou mais antigas, um dispositivo IoT Edge não pode estar a jusante de um gateway IoT Edge.
>
>Os dispositivos a jusante não podem usar o upload de ficheiros.

::: moniker-end

<!-- 1.2.0 -->
::: moniker range=">=iotedge-2020-11"

>[!NOTE]
>Os dispositivos a jusante não podem usar o upload de ficheiros.

::: moniker-end

Existem três passos gerais para estabelecer uma ligação transparente de gateway bem sucedida. Este artigo abrange o primeiro passo:

1. **Configure o dispositivo gateway como um servidor para que os dispositivos a jusante possam ligar-se a ele de forma segura. Configurar o portal para receber mensagens de dispositivos a jusante e encaminhá-las para o destino adequado.**
2. Crie uma identidade do dispositivo para o dispositivo a jusante para que possa autenticar com o IoT Hub. Configure o dispositivo a jusante para enviar mensagens através do dispositivo gateway. Para esses passos, consulte [Autenticar um dispositivo a jusante para o Azure IoT Hub](how-to-authenticate-downstream-device.md).
3. Ligue o dispositivo a jusante ao dispositivo gateway e comece a enviar mensagens. Para esses passos, consulte [Ligar um dispositivo a jusante a um gateway Azure IoT Edge](how-to-connect-downstream-device.md).

Para que um dispositivo atue como um portal, tem de se ligar de forma segura aos seus dispositivos a jusante. O Azure IoT Edge permite-lhe utilizar uma infraestrutura de chaves públicas (PKI) para estabelecer ligações seguras entre dispositivos. Neste caso, estamos a permitir que um dispositivo a jusante se conecte a um dispositivo IoT Edge agindo como um gateway transparente. Para manter uma segurança razoável, o dispositivo a jusante deve confirmar a identidade do dispositivo gateway. Esta verificação de identidade impede que os seus dispositivos se conectem a gateways potencialmente maliciosos.

<!-- 1.1 -->
:::moniker range="iotedge-2018-06"
Um dispositivo a jusante pode ser qualquer aplicação ou plataforma que tenha uma identidade criada com o serviço de nuvem [Azure IoT Hub.](../iot-hub/index.yml) Estas aplicações utilizam frequentemente o [dispositivo Azure IoT SDK](../iot-hub/iot-hub-devguide-sdks.md). Um dispositivo a jusante pode até ser uma aplicação em execução no próprio dispositivo de gateway IoT Edge. No entanto, um dispositivo IoT Edge não pode ser a jusante de um gateway IoT Edge.
:::moniker-end
<!-- end 1.1 -->

<!-- 1.2 -->
:::moniker range=">=iotedge-2020-11"
Um dispositivo a jusante pode ser qualquer aplicação ou plataforma que tenha uma identidade criada com o serviço de nuvem [Azure IoT Hub.](../iot-hub/index.yml) Estas aplicações utilizam frequentemente o [dispositivo Azure IoT SDK](../iot-hub/iot-hub-devguide-sdks.md). Um dispositivo a jusante pode até ser uma aplicação em execução no próprio dispositivo de gateway IoT Edge.
:::moniker-end
<!-- end 1.2 -->

Pode criar qualquer infraestrutura de certificado que permita a confiança necessária para a sua topologia de porta de dispositivos. Neste artigo, assumimos a mesma configuração de certificado que você usaria para permitir a [segurança X.509 CA](../iot-hub/iot-hub-x509ca-overview.md) no IoT Hub, que envolve um certificado DE CA X.509 associado a um hub IoT específico (o hub IoT raiz CA), uma série de certificados assinados com este CA, e um CA para o dispositivo IoT Edge.

>[!NOTE]
>O *certificado de CA de raiz* utilizado ao longo destes artigos refere-se ao certificado público de autoridade mais alta da cadeia de certificados PKI, e não necessariamente à raiz do certificado de uma autoridade de certificados sindicado. Em muitos casos, trata-se, na verdade, de um certificado público intermédio da AC.

Os passos seguintes acompanham-no através do processo de criação dos certificados e da sua instalação nos locais certos no gateway. Pode utilizar qualquer máquina para gerar os certificados e copiá-los para o seu dispositivo IoT Edge.

## <a name="prerequisites"></a>Pré-requisitos

Um dispositivo Linux ou Windows com IoT Edge instalado.

Se não tiver um dispositivo pronto, pode criar um numa máquina virtual Azure. Siga os passos no [Implementar o seu primeiro módulo IoT Edge para um dispositivo Linux virtual](quickstart-linux.md) para criar um Hub IoT, criar uma máquina virtual e configurar o tempo de execução IoT Edge.

## <a name="set-up-the-device-ca-certificate"></a>Configurar o certificado ca do dispositivo

Todos os gateways IoT Edge precisam de um certificado de CA do dispositivo instalado neles. O daemon de segurança IoT Edge utiliza o certificado CA do dispositivo IoT Edge para assinar um certificado de CA de carga de trabalho, que por sua vez assina um certificado de servidor para o hub IoT Edge. O gateway apresenta o seu certificado de servidor ao dispositivo a jusante durante o início da ligação. O dispositivo a jusante verifica se o certificado do servidor faz parte de uma cadeia de certificados que chega ao certificado de CA raiz. Este processo permite que o dispositivo a jusante confirme que o gateway provém de uma fonte fidedigna. Para mais informações, consulte [como a Azure IoT Edge utiliza certificados.](iot-edge-certs.md)

![Configuração do certificado gateway](./media/how-to-create-transparent-gateway/gateway-setup.png)

O certificado de CA raiz e o certificado CA do dispositivo (com a sua chave privada) têm de estar presentes no dispositivo de gateway IoT Edge e configurados no ficheiro IoT Edge config. Lembre-se que, neste *caso, o certificado de CA raiz* significa a autoridade de certificado mais alta para este cenário IoT Edge. O certificado de CA do dispositivo de gateway e os certificados do dispositivo a jusante têm de chegar ao mesmo certificado de CA raiz.

>[!TIP]
>O processo de instalação do certificado de CA raiz e do certificado ca do dispositivo num dispositivo IoT Edge também é explicado mais detalhadamente em [certificados De gestão num dispositivo IoT Edge](how-to-manage-device-certificates.md).

Tenha os seguintes ficheiros prontos:

* Certificado root CA
* Certificado ca dispositivo
* Chave privada do dispositivo CA

Para cenários de produção, deve gerar estes ficheiros com a sua própria autoridade de certificados. Para cenários de desenvolvimento e teste, pode utilizar certificados de demonstração.

Se não tiver a sua própria autoridade de certificados e quiser utilizar certificados de demonstração, siga as instruções nos [certificados de demonstração create para testar as funcionalidades do dispositivo IoT Edge](how-to-create-test-certificates.md) para criar os seus ficheiros. Nessa página, é necessário dar os seguintes passos:

   1. Para começar, configurar os scripts para gerar certificados no seu dispositivo.
   2. Crie um certificado de CA de raiz. No final dessas instruções, terá um ficheiro de certificado de CA raiz:
      * `<path>/certs/azure-iot-test-only.root.ca.cert.pem`.
   3. Crie certificados CA do dispositivo IoT Edge. No final dessas instruções, terá um certificado de AC de dispositivo e a sua chave privada:
      * `<path>/certs/iot-edge-device-<cert name>-full-chain.cert.pem` e
      * `<path>/private/iot-edge-device-<cert name>.key.pem`

Se criou os certificados numa máquina diferente, copie-os para o seu dispositivo IoT Edge e, em seguida, proceda aos próximos passos.

<!-- 1.1 -->
:::moniker range="iotedge-2018-06"

1. No seu dispositivo IoT Edge, abra o ficheiro de segurança daemon config.

   * Windows: `C:\ProgramData\iotedge\config.yaml`
   * Linux: `/etc/iotedge/config.yaml`

1. Encontre a secção de **definições** de Certificado do ficheiro. Descomprometar as quatro linhas a começar pelos **certificados:** e fornecer os URIs de ficheiros aos seus três ficheiros como valores para as seguintes propriedades:
   * **device_ca_cert**: certificado ca do dispositivo
   * **device_ca_pk**: chave privada ca do dispositivo
   * **trusted_ca_certs**: certificado de CA raiz

   Certifique-se de que não existe um espaço branco anterior nos **certificados:** linha, e que as outras linhas são recortadas por dois espaços.

1. Guarde e feche o ficheiro.

1. Reiniciar ioT Edge.
   * Windows: `Restart-Service iotedge`
   * Linux: `sudo systemctl restart iotedge`
:::moniker-end
<!-- end 1.1 -->

<!--1.2 -->
:::moniker range=">=iotedge-2020-11"

1. No seu dispositivo IoT Edge, abra o ficheiro config: `/etc/aziot/config.toml`

   >[!TIP]
   >Se o ficheiro config ainda não existir no seu dispositivo, então use `/etc/aziot/config.toml.edge.template` como modelo para criar um.

1. Encontre o `trust_bundle_cert` parâmetro. Descompromete esta linha e forneça o ficheiro URI ao ficheiro de certificado de CA raiz no seu dispositivo.

1. Encontre a `[edge_ca]` secção do ficheiro. Descomprometar as três linhas desta secção e fornecer os URIs de ficheiros ao seu certificado e ficheiros-chave como valores para as seguintes propriedades:
   * **cert**: certificado ca dispositivo
   * **pk**: chave privada ca dispositivo

1. Guarde e feche o ficheiro.

1. Aplique as suas alterações.

   ```bash
   sudo iotedge config apply
   ```

:::moniker-end
<!-- end 1.2 -->

## <a name="deploy-edgehub-and-route-messages"></a>Implementar edgeHub e mensagens de rota

Os dispositivos a jusante enviam telemetria e mensagens para o dispositivo gateway, onde o módulo hub IoT Edge é responsável por encaminhar a informação para outros módulos ou para o IoT Hub. Para preparar o seu dispositivo de gateway para esta função, certifique-se de que:

* O módulo hub IoT Edge é implantado no dispositivo.

  Quando instala pela primeira vez o IoT Edge num dispositivo, apenas um módulo de sistema começa automaticamente: o agente IoT Edge. Uma vez que cria a primeira implementação para um dispositivo, o segundo módulo do sistema, o hub IoT Edge, também começa. Se o módulo **EdgeHub** não estiver a funcionar no seu dispositivo, crie uma implementação para o seu dispositivo.

* O módulo hub IoT Edge tem rotas configuradas para lidar com mensagens recebidas de dispositivos a jusante.

  O dispositivo gateway deve ter uma rota no lugar para lidar com mensagens de dispositivos a jusante ou então essas mensagens não serão processadas. Pode enviar as mensagens para módulos no dispositivo gateway ou diretamente para o IoT Hub.

Para implantar o módulo hub IoT Edge e configurar-o com rotas para lidar com mensagens recebidas de dispositivos a jusante, siga estes passos:

1. No portal do Azure, navegue para o seu hub IoT.

2. Vá ao **IoT Edge** e selecione o seu dispositivo IoT Edge que pretende utilizar como porta de entrada.

3. Selecione **módulos de conjunto**.

4. Na página **Módulos,** pode adicionar todos os módulos que pretende implementar no dispositivo gateway. Para efeitos deste artigo estamos focados em configurar e implementar o módulo edgeHub, que não precisa de ser explicitamente definido nesta página.

5. Selecione **Seguinte: Rotas**.

6. Na página **Rotas,** certifique-se de que existe uma rota para lidar com mensagens provenientes de dispositivos a jusante. Por exemplo:

   * Uma rota que envia todas as mensagens, seja de um módulo ou de um dispositivo a jusante, para o IoT Hub:
       * **Nome:**`allMessagesToHub`
       * **Valor:**`FROM /messages/* INTO $upstream`

   * Uma rota que envia todas as mensagens de todos os dispositivos a jusante para o IoT Hub:
      * **Nome:**`allDownstreamToHub`
      * **Valor:**`FROM /messages/* WHERE NOT IS_DEFINED ($connectionModuleId) INTO $upstream`

      Esta rota funciona porque, ao contrário das mensagens dos módulos IoT Edge, as mensagens de dispositivos a jusante não têm um ID de módulo associado a eles. A utilização da cláusula **WHERE** da rota permite-nos filtrar quaisquer mensagens com essa propriedade do sistema.

      Para obter mais informações sobre o encaminhamento de mensagens, consulte [os módulos de implantação e estabeleça rotas.](./module-composition.md#declare-routes)

7. Assim que a sua rota ou rotas forem criadas, selecione **Review + create**.

8. Na página **'Rever + criar',** selecione **Criar.**

## <a name="open-ports-on-gateway-device"></a>Portas abertas no dispositivo gateway

Os dispositivos Standard IoT Edge não precisam de qualquer conectividade de entrada para funcionar, porque toda a comunicação com o IoT Hub é feita através de ligações de saída. Os dispositivos gateway são diferentes porque precisam de receber mensagens dos seus dispositivos a jusante. Se uma firewall estiver entre os dispositivos a jusante e o dispositivo gateway, então a comunicação também tem de ser possível através da firewall.

Para que um cenário de gateway funcione, pelo menos um dos protocolos suportados do hub IoT Edge deve estar aberto para o tráfego de entrada a partir de dispositivos a jusante. Os protocolos suportados são MQTT, AMQP, HTTPS, MQTT sobre WebSockets e AMQP sobre WebSockets.

| Porta | Protocolo |
| ---- | -------- |
| 8883 | MQTT |
| 5671 | AMQP |
| 443 | HTTPS <br> MQTT+WS <br> AMQP+WS |

## <a name="next-steps"></a>Passos seguintes

Agora que tem um dispositivo IoT Edge configurado como um gateway transparente, precisa de configurar os seus dispositivos a jusante para confiar no gateway e enviar-lhe mensagens. Continue a [autenticar um dispositivo a jusante até ao Azure IoT Hub](how-to-authenticate-downstream-device.md) para os próximos passos na configuração do seu cenário transparente de gateway.