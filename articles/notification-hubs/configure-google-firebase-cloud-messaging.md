---
title: Configure mensagens em nuvem de fogo do Google em Azure Notification Hubs | Microsoft Docs
description: Saiba como configurar um centro de notificação Azure com as definições de Mensagens Cloud do Google Firebase.
services: notification-hubs
author: sethmanheim
manager: femila
ms.service: notification-hubs
ms.workload: mobile
ms.topic: article
ms.date: 08/04/2020
ms.author: sethm
ms.reviewer: thsomasu
ms.lastreviewed: 03/25/2019
ms.openlocfilehash: 0da103c11e2412108535ca322917632f5d95559d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "96003570"
---
# <a name="configure-google-firebase-settings-for-a-notification-hub-in-the-azure-portal"></a>Configurar as configurações da Base de Fogo do Google para um centro de notificação no portal Azure

Este artigo mostra-lhe como configurar as definições de Mensagens Cloud (FCM) do Google Firebase para um centro de notificação Azure utilizando o portal Azure.  

## <a name="prerequisites"></a>Pré-requisitos

Se ainda não criou um centro de notificação, crie um agora. Para mais informações, consulte [Criar um centro de notificação Azure no portal Azure](create-notification-hub-portal.md).

## <a name="configure-google-firebase-cloud-messaging-fcm"></a>Configure mensagens cloud do Google Firebase (FCM)

O procedimento a seguir descreve os passos para configurar as definições de Mensagens Cloud (FCM) do Google Firebase para um centro de notificação:

1. No portal Azure, na página **'Centro de Notificação',** selecione **Google (GCM/FCM)** no menu esquerdo.
2. Cole a **Chave API** para o projeto FCM que guardou anteriormente.
3. Selecione **Guardar**.

   ![Screenshot que mostra como configurar os Centros de Notificação para o Google FCM](./media/notification-hubs-android-push-notification-google-fcm-get-started/fcm-server-key.png)

## <a name="next-steps"></a>Passos seguintes

Para obter um tutorial com instruções passo a passo para o envio de notificações para dispositivos Android utilizando os Hubs de Notificação do Azure e mensagens cloud do Google Firebase, consulte [Enviar notificações push para dispositivos Android utilizando Os Centros de Notificação e Google FCM](notification-hubs-android-push-notification-google-fcm-get-started.md).
