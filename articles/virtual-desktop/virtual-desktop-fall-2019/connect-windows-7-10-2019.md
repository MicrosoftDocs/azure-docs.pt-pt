---
title: Ligue ao Windows Virtual Desktop (clássico) Windows 10 ou 7 - Azure
description: Como ligar-se ao Windows Virtual Desktop (clássico) utilizando o cliente Windows Desktop.
author: Heidilohr
ms.topic: how-to
ms.date: 07/16/2020
ms.author: helohr
manager: femila
ms.openlocfilehash: 62686589b03f7187ec473dab8fba602eaf7176f2
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2021
ms.locfileid: "106445249"
---
# <a name="connect-with-the-windows-desktop-classic-client"></a>Conecte-se com o cliente do Windows Desktop (clássico)

> Aplica-se a: Windows 7, Windows 10 e Windows 10 IoT Enterprise

>[!IMPORTANT]
>Este conteúdo aplica-se ao Windows Virtual Desktop (clássico), que não suporta objetos de desktop virtual do Gestor de Recursos Azure Windows. Se estiver a tentar gerir os objetos de ambiente de trabalho virtuais do Azure Resource Manager Windows, consulte [este artigo](../connect-windows-7-10.md).

Pode aceder aos recursos de Desktop Virtual do Windows em dispositivos com Windows 7, Windows 10 e Windows 10 IoT Enterprise utilizando o cliente Windows Desktop. O cliente não suporta o Windows 8 ou o Windows 8.1.

>[!NOTE]
>O cliente Do Windows falha automaticamente no Windows Virtual Desktop (clássico). No entanto, se o cliente detetar que o utilizador também tem recursos Azure Resource Manager, adiciona automaticamente os recursos ou notifica o utilizador de que está disponível.

> [!IMPORTANT]
> O Windows Virtual Desktop não suporta o cliente RemoteApp e Desktop Connections (RADC) ou o cliente Remote Desktop Connection (MSTSC).

> [!IMPORTANT]
> O Windows Virtual Desktop não suporta atualmente o cliente remote desktop a partir da Windows Store.

## <a name="install-the-windows-desktop-client"></a>Instale o cliente windows desktop

Escolha o cliente que corresponda à sua versão do Windows:

- [Windows 64-bit](https://go.microsoft.com/fwlink/?linkid=2068602)
- [Windows 32-bit](https://go.microsoft.com/fwlink/?linkid=2098960)
- [Janelas ARM64](https://go.microsoft.com/fwlink/?linkid=2098961)

Pode instalar o cliente para o utilizador atual, que não requer direitos de administração, ou o seu administrador pode instalar e configurar o cliente para que todos os utilizadores do dispositivo possam aceder ao mesmo.

Uma vez instalado, o cliente pode ser lançado a partir do menu Iniciar, procurando por **Desktop Remoto**.

## <a name="subscribe-to-a-workspace"></a>Subscreva um espaço de trabalho

Há duas formas de subscrever um Espaço de Trabalho. O cliente pode tentar descobrir os recursos disponíveis a partir do seu trabalho ou conta escolar ou pode especificar diretamente o URL onde os seus recursos estão para casos em que o cliente não os consegue encontrar. Uma vez subscrito um Espaço de Trabalho, pode lançar recursos com um dos seguintes métodos:

- Vá ao Centro de Ligação e clique duas vezes num recurso para lançá-lo.
- Também pode ir ao menu Iniciar e procurar uma pasta com o nome Workspace ou introduzir o nome do recurso na barra de pesquisa.

### <a name="subscribe-with-a-user-account"></a>Inscreva-se com uma conta de utilizador

1. A partir da página principal do cliente, **selecione Subscrever**.
2. Inscreva-se na sua conta de utilizador quando solicitado.
3. Os recursos aparecerão no Centro de Conexão, e são agrupados por espaço de trabalho.

### <a name="subscribe-with-a-url"></a>Inscreva-se com um URL

1. A partir da página principal do cliente, **selecione Subscreva com URL**.
2. Insira o URL do Espaço de Trabalho ou o seu endereço de e-mail:
   - Se utilizar o **URL workspace,** use o que o seu administrador lhe deu. Se aceder a recursos a partir do Windows Virtual Desktop, pode utilizar um dos seguintes URLs:
     - Windows Virtual Desktop (clássico): `https://rdweb.wvd.microsoft.com/api/feeddiscovery/webfeeddiscovery.aspx`
     - Windows Virtual Desktop: `https://rdweb.wvd.microsoft.com/api/arm/feeddiscovery`
   - Se estiver a utilizar o campo **de e-mail,** insira o seu endereço de e-mail. Isto indica ao cliente para procurar um URL associado ao seu endereço de e-mail se o seu administrador tiver configurado a [descoberta de e-mail](/windows-server/remote/remote-desktop-services/rds-email-discovery).
3. Selecione **Seguinte**.
4. Inscreva-se na sua conta de utilizador quando solicitado.
5. Os recursos devem aparecer no Centro de Ligação, agrupado por espaço de trabalho.

## <a name="next-steps"></a>Passos seguintes

Para saber mais sobre como utilizar o cliente Windows Desktop, confira [Consulte o cliente do Windows Desktop.](/windows-server/remote/remote-desktop-services/clients/windowsdesktop/)
