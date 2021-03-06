---
title: Instale a atualização 0.5 em StorSimple Virtual Array | Microsoft Docs
description: Descreve como utilizar o UI web StorSimple Virtual Array para aplicar o Update 0.5 utilizando o portal Azure e o método de correção em calor.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: how-to
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/10/2017
ms.author: alkohli
ms.openlocfilehash: 5723e8d9fc7b0a72393dda1b225ca073a6474a0a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "94534318"
---
# <a name="install-update-05-on-your-storsimple-virtual-array"></a>Instale atualização 0.5 no seu StorSimple Virtual Array

## <a name="overview"></a>Descrição Geral

Este artigo descreve os passos necessários para instalar o Update 0.5 no seu StorSimple Virtual Array através da UI web local e através do portal Azure. Tem de aplicar atualizações de software ou hotfixes para manter o seu StorSimple Virtual Array atualizado.

Antes de aplicar uma atualização, recomendamos que leve primeiro os volumes ou partilhas offline no anfitrião e, em seguida, no dispositivo. Isto minimiza qualquer possibilidade de danos em dados. Depois de os volumes ou ações estarem offline, também deverá fazer uma cópia de segurança manual do dispositivo.

> [!IMPORTANT]
> - A atualização 0.5 corresponde à versão de software **10.0.10290.0** no seu dispositivo. Para obter informações sobre as novidades desta atualização, aceda às [notas de lançamento para atualização 0.5](storsimple-virtual-array-update-05-release-notes.md).
>
> - Se estiver a executar a Atualização 0.2 ou posterior, recomendamos que instale as atualizações através do portal Azure. Se estiver a executar as versões de software Update 0.1 ou GA, tem de utilizar o método hotfix através da UI web local para instalar o Update 0.5.
>
> - Tenha em atenção que a instalação de uma atualização ou correção reinicia o dispositivo. Dado que o StorSimple Virtual Array é um único dispositivo de nó, qualquer I/S em andamento é interrompido e o seu dispositivo experimenta tempo de inatividade.

## <a name="use-the-azure-portal"></a>Utilizar o portal do Azure

Se executar a Atualização 0.2 e posteriormente, recomendamos que instale atualizações através do portal Azure. O procedimento do portal requer que o utilizador digitalize, descarregue e, em seguida, instale as atualizações. Este procedimento leva cerca de 7 minutos para ser concluído. Execute os seguintes passos para instalar a atualização ou hotfix.

[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-04.md)]

Depois de concluída a instalação, aceda ao serviço StorSimple Device Manager. Selecione **Dispositivos** e, em seguida, selecione e clique no dispositivo que acabou de atualizar. Vá a **Definições > Gerir > atualizações do dispositivo**. A versão de software visualizada deve ser **10.0.10290.0**.

## <a name="use-the-local-web-ui"></a>Use a uI web local

Existem dois passos ao utilizar a uI web local:

* Descarregue a atualização ou o hotfix
* Instale a atualização ou o hotfix

### <a name="download-the-update-or-the-hotfix"></a>Descarregue a atualização ou o hotfix

Execute os seguintes passos para transferir a atualização de software a partir do Catálogo Microsoft Update.

#### <a name="to-download-the-update-or-the-hotfix"></a>Para descarregar a atualização ou o hotfix

1. Inicie o Internet Explorer e navegue para [https://catalog.update.microsoft.com](https://catalog.update.microsoft.com) .

2. Se esta for a primeira vez que utiliza o Catálogo Microsoft Update neste computador, clique em **Instalar** quando lhe for pedido para instalar o suplemento do Catálogo Microsoft Update.

3. Na caixa de pesquisa do Catálogo de Atualizações da Microsoft, insira o número da Base de Conhecimento (KB) do hotfix que pretende descarregar. Introduza **4021576** para atualização 0.5 e, em seguida, clique em **Procurar**.
   
    A listagem hotfix aparece, por exemplo, **StorSimple Virtual Array Update 0.5**.
   
    ![Catálogo de pesquisa](./media/storsimple-virtual-array-install-update-05/download1.png)

4. Clique em **Transferir**. 

5. Você deve ver dois ficheiros para descarregar, um *.msu* e um ficheiro *.cab.* Descarregue cada um desses ficheiros para uma pasta. A pasta também pode ser copiada para uma partilha de rede que é acessível a partir do dispositivo.

6. Abra a pasta onde os ficheiros estão localizados.
    ![Ficheiros no pacote](./media/storsimple-virtual-array-install-update-05/update05folder.png)

    Verá:
    -  Um ficheiro pacote autónomo da Microsoft Update `WindowsTH-KB3011067-x64` . Este ficheiro é utilizado para atualizar o software do dispositivo.
    - Um ficheiro do Pacote de Agente de Monitorização de `GenevaMonitoringAgentPackageInstaller` Genebra. Este ficheiro é utilizado para atualizar o agente do serviço de monitorização e diagnóstico (MDS). Clique duas vezes no ficheiro do táxi. É apresentado um .msi. Selecione o ficheiro, clique à direita e, em seguida, **Extraia** o ficheiro. Utilizará o ficheiro _.msi_ para atualizar o agente.

        ![Extrair ficheiro de atualização do agente MDS](./media/storsimple-virtual-array-install-update-05/extract-geneva-monitoring-agent-installer.png)
        
    

### <a name="install-the-update-or-the-hotfix"></a>Instale a atualização ou o hotfix

Antes da atualização ou instalação do hotfix, certifique-se de que tem a atualização ou o hotfix descarregado localmente no seu anfitrião ou acessível através de uma partilha de rede.

Utilize este método para instalar atualizações num dispositivo em execução de versões de software GA ou Update 0.1. Este procedimento leva menos de 2 minutos para ser concluído. Execute os seguintes passos para instalar a atualização ou hotfix.

#### <a name="to-install-the-update-or-the-hotfix"></a>Para instalar a atualização ou o hotfix

1. Na UI web local, aceda à  >  **Atualização de Software de** Manutenção .
   
    ![A screenshot mostra a atualização de Software selecionada a partir do menu Manutenção.](./media/storsimple-virtual-array-install-update-05/update1m.png)

2. Na **trajetória do ficheiro 'Actualizar',** insira o nome do ficheiro para a atualização ou o hotfix. Também pode navegar no ficheiro de instalação de atualização ou hotfix se for colocado numa partilha de rede. Clique em **Aplicar**.
   
    ![A screenshot mostra a caixa de texto do caminho do ficheiro Update na página de atualização do Software.](./media/storsimple-virtual-array-install-update-05/update2m.png)

3. É apresentado um aviso. Dado que se trata de um único dispositivo de nó, após a aplicação da atualização, o dispositivo reinicia e há tempo de inatividade. Clique no ícone de verificação.
   
   ![A screenshot mostra um aviso de caixa de diálogo de tempo de inatividade.](./media/storsimple-virtual-array-install-update-05/update3m.png)

4. A atualização começa. Depois de o dispositivo ser atualizado com sucesso, reinicia-o. A UI local não é acessível nesta duração.
   
    ![A screenshot mostra uma mensagem de sucesso para a atualização.](./media/storsimple-virtual-array-install-update-05/update5m.png)

5. Após o reinício, é levado para a página **Signo.** Para verificar se o software do dispositivo se atualizou, na UI web local, vá à   >  **Atualização de Software** de Manutenção . A versão do software visualizada deve ser **10.0.0.0.0.10290.0** para a atualização 0.5.
   
   > [!NOTE]
   > Reportamos as versões de software de uma forma ligeiramente diferente na UI web local e no portal Azure. Por exemplo, o web UI local reporta **10.0.0.0.0.10290** e o portal Azure reporta **10.0.10290.0** para a mesma versão.
   
    ![A screenshot mostra a página de atualização do Software com a versão atual do software.](./media/storsimple-virtual-array-install-update-05/update6m.png)

6. O próximo passo é atualizar o agente MDS. Na página **'Atualização** de Software', aceda ao caminho do **ficheiro 'Actualizar'** e navegue no `GenevaMonitoringAgentPackageInstaller.msi` ficheiro. Repita os passos 2-4. Após o reinício da matriz virtual, inscreva-se na UI web local.

A atualização está agora completa.

## <a name="next-steps"></a>Passos seguintes

Saiba mais sobre [a administração do seu StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).

