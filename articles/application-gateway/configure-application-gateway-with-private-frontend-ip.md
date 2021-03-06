---
title: Configure um ponto final interno do balançador de carga (ILB)
titleSuffix: Azure Application Gateway
description: Este artigo fornece informações sobre como configurar o Gateway de aplicação com um endereço IP de frontend privado
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: how-to
ms.date: 02/23/2021
ms.author: victorh
ms.openlocfilehash: 224cbe1e34e5915a7fa5fc1cf415c35f86c3abe4
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "101711659"
---
# <a name="configure-an-application-gateway-with-an-internal-load-balancer-ilb-endpoint"></a>Configure um gateway de aplicação com um ponto final interno de carregamento (ILB)

O Azure Application Gateway pode ser configurado com um VIP virado para a Internet ou com um ponto final interno que não esteja exposto à Internet. Um ponto final interno utiliza um endereço IP privado para o frontend, que também é conhecido como um *ponto final interno de balançador de carga (ILB).*

Configurar o gateway usando um endereço IP privado frontend é útil para aplicações internas de linha de negócio que não estão expostas à Internet. Também é útil para serviços e níveis dentro de uma aplicação multi-nível que estão em uma fronteira de segurança que não está exposta à Internet mas:

- ainda requerem distribuição de carga de robin redondo
- adesivo de sessão
- ou Terminação de Segurança da Camada de Transporte (TLS) (anteriormente conhecida como Camada de Tomadas Seguras (SSL)).

Este artigo guia-o através dos passos para configurar um gateway de aplicações com um endereço IP privado frontal utilizando o portal Azure.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no portal do Azure em <https://portal.azure.com>

## <a name="create-an-application-gateway"></a>Criar um gateway de aplicação

Para que o Azure comunique entre os recursos que cria, precisa de uma rede virtual. Ou cria uma nova rede virtual ou utiliza uma existente. 

Neste exemplo, cria-se uma nova rede virtual. Pode criar uma rede virtual ao mesmo tempo que cria o gateway de aplicação. As instâncias do Gateway de Aplicação são criadas em sub-redes separadas. Existem duas sub-redes neste exemplo: uma para o gateway de aplicações e outra para os servidores backend.

1. Expandir o menu do portal e selecionar **Criar um recurso.**
2. Selecione **Rede** e, em seguida, selecione **Gateway de Aplicação** na lista Destaques.
3. Insira *o myAppGateway* para o nome do gateway de aplicações e *do myResourceGroupAG* para o novo grupo de recursos.
4. Para **a Região**, selecione **Central US**.
5. Para **Tier**, selecione **Standard**.
6. Em **configurar a rede virtual** selecione Criar **novos**, e, em seguida, introduzir estes valores para a rede virtual:
   - *myVNet* - para o nome da rede virtual.
   - *10.0.0.0/16* - para o espaço de endereços de rede virtual.
   - *myAGSubnet* - para o nome da sub-rede.
   - *10.0.0.0/24* - para o espaço de endereço da sub-rede.
   - *myBackendSubnet* - para o nome da sub-rede backend.
   - *10.0.1.0/24* - para o espaço de endereço do sub-rede de reto.

    ![Criar a rede virtual](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-1.png)

6. Selecione **OK** para criar a rede virtual e sub-redes.
7. Selecione **Seguinte : Frontends**.
8. Para **o tipo de endereço IP frontend**, selecione **Private**.

   Por padrão, é uma atribuição dinâmica de endereço IP. O primeiro endereço disponível da sub-rede configurada é atribuído como endereço IP frontend.
   > [!NOTE]
   > Uma vez atribuído, o tipo de endereço IP (estático ou dinâmico) não pode ser alterado mais tarde.
9. Selecione **Seguinte:Backends**.
10. **Selecione Adicione uma piscina de backend**.
11. Para **nomear**, escreva *appGatewayBackendPool*.
12. Para **Adicionar pool de backend sem alvos**, selecione **Sim**. Vai adicionar os alvos mais tarde.
13. Selecione **Adicionar**.
14. Selecione **Seguinte:Configuração**.
15. De acordo com **as regras de encaminhamento**, selecione **Adicione uma regra de encaminhamento**.
16. Para **o nome da regra**, escreva *Rrule-01*.
17. Para **o nome do ouvinte,** *escreva Listener-01*.
18. Para **Frontend IP**, selecione **Private**.
19. Aceite as restantes predefinições e selecione o separador **alvos de Backend.**
20. Para **o tipo alvo**, selecione **backend pool** e, em seguida, selecione **appGatewayBackendPool**.
21. Para **a definição HTTP**, selecione Adicione **novo**.
22. Para **o nome de definição HTTP**, escreva *http-01*.
23. Para **o protocolo Backend**, selecione **HTTP**.
24. Para **porta Backend**, tipo *80*.
25. Aceite os predefinidos restantes e **selecione Adicionar**.
26. Na página De encaminhamento da regra **de encaminhamento,** selecione **Adicionar**.
27. Selecione **Seguinte: Tags**.
28. Selecione **Seguinte: Rever + criar**.
29. Reveja as definições na página do resumo e, em seguida, **selecione Criar** para criar os recursos de rede e o gateway de aplicação. A criação do gateway de aplicação pode demorar vários minutos. Aguarde até que a colocação termine com sucesso antes de passar para a secção seguinte.

## <a name="add-backend-pool"></a>Adicionar piscina de backend

O pool backend é usado para encaminhar pedidos para os servidores backend que servem o pedido. O backend pode ser composto por NICs, conjuntos de escala de máquinas virtuais, endereços IP públicos, endereços IP internos, nomes de domínio totalmente qualificados (FQDN) e back-ends multi-inquilinos como O Azure App Service. Neste exemplo, utiliza-se máquinas virtuais como o objetivo de backend. Pode utilizar máquinas virtuais existentes ou criar novas. Neste exemplo, cria-se duas máquinas virtuais que o Azure utiliza como servidores de backend para o gateway de aplicações.

Para fazer isto, tu:

1. Crie duas novas máquinas virtuais, *myVM* e *myVM2,* usadas como servidores backend.
2. Instale o IIS nas máquinas virtuais para verificar se o gateway de aplicações foi criado com sucesso.
3. Adicione os servidores backend à piscina de backend.

### <a name="create-a-virtual-machine"></a>Criar uma máquina virtual


1. Selecione **Criar um recurso**.
2. Selecione **Compute** e, em seguida, selecione **máquina Virtual**.
4. Introduza estes valores para a máquina virtual:
   - Selecione a sua subscrição.
   - Selecione *myResourceGroupAG* para **o grupo de recursos.**
   - Digite *myVM* para **nome de máquina virtual.**
   - Selecione **o Centro de Dados do Windows Server 2019** para **imagem**.
   - Digite um **nome de utilizador** válido.
   - Digite uma **senha** válida.
1. Aceite as restantes predefinições e selecione **Seguinte: Discos**.
1. Aceite as predefinições e selecione **Seguinte : Networking**.
1. Certifique-se de que o **myVNet** está selecionado para a rede virtual e a sub-rede é **myBackendSubnet**.
1. Aceite os restantes predefinidos e selecione **Seguinte : Gestão**.
1. **Selecione Desativar** para desativar os diagnósticos de arranque.
1. Selecione **Rever + criar**.
1. Reveja as definições na página do resumo e, em seguida, **selecione Criar**. Pode levar vários minutos para criar o VM. Aguarde até que a colocação termine com sucesso antes de passar para a secção seguinte.

### <a name="install-iis"></a>Instalar o IIS

1. Abra a Cloud Shell e certifique-se de que está definida para **PowerShell**.
    ![A screenshot mostra uma janela aberta da consola Azure Cloud Shell que utiliza o PowerShell.](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-3.png)
2. Execute o comando seguinte para instalar o IIS na máquina virtual:

   ```azurepowershell
   Set-AzVMExtension `
        -ResourceGroupName myResourceGroupAG `
        -ExtensionName IIS `
        -VMName myVM `
        -Publisher Microsoft.Compute `
        -ExtensionType CustomScriptExtension `
        -TypeHandlerVersion 1.4 `
        -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
         -Location CentralUS 

   ```

3. Crie uma segunda máquina virtual e instale o IIS com os passos que acabou de concluir. Utilize o myVM2 para o nome da máquina virtual e para `VMName` dentro `Set-AzVMExtension` .

### <a name="add-backend-servers-to-backend-pool"></a>Adicione servidores backend para backend pool

1. Selecione **Todos os recursos** e, em seguida, selecione **myAppGateway**.
2. Selecione **backend pools** e, em seguida, selecione **appGatewayBackendPool**.
3. No **tipo Alvo** selecione máquina **virtual**  e em **Target,** selecione o vNIC associado ao myVM.
4. Repita para adicionar MyVM2.
   ![Editar o painel de piscina backend com os tipos de destino e alvos em destaque.](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-4.png)
5. **Selecione Save.**

## <a name="create-a-client-virtual-machine"></a>Criar uma máquina virtual cliente

A máquina virtual do cliente é utilizada para ligar à piscina de backend gateway de aplicação.

- Crie uma terceira máquina virtual utilizando os passos anteriores. Utilize o myVM3 para o nome da máquina virtual.

## <a name="test-the-application-gateway"></a>Testar o gateway de aplicação

1. Na página myAppGateway, selecione **Configurações IP frontend** para observar o endereço IP privado frontend.
    ![Painel de configurações IP frontend com o tipo Privado realçado.](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-5.png)
2. Copie o endereço IP privado e, em seguida, cole-o na barra de endereços do navegador no myVM3 para aceder ao pool de backend do gateway da aplicação.

## <a name="next-steps"></a>Passos seguintes

Se quiser monitorizar a saúde da sua piscina de backend, consulte [registos de saúde e diagnóstico de back-end para o Gateway de aplicações](application-gateway-diagnostics.md).
