---
title: Lista de verificação de pré-despreocupação para implantar o dispositivo Azure Stack Edge Mini R
description: Este artigo descreve as informações que podem ser recolhidas antes de implementar o seu dispositivo Azure Stack Edge Mini R.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 02/24/2021
ms.author: alkohli
ms.openlocfilehash: c6e0e6c18803d24f7aa41350d64de77f92c5c0c0
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/20/2021
ms.locfileid: "101730597"
---
# <a name="deployment-checklist-for-your-azure-stack-edge-mini-r-device"></a>Lista de verificação de implementação para o seu dispositivo Azure Stack Edge Mini R  

Este artigo descreve as informações que podem ser recolhidas antes da implantação real do seu dispositivo Azure Stack Edge Mini R. 

Utilize a seguinte lista de verificação para garantir que tem estas informações depois de ter feito um pedido para um dispositivo Azure Stack Edge Mini R e antes de receber o dispositivo. 

## <a name="deployment-checklist"></a>Lista de verificação de implementação 

| Fase                             | Parâmetro                                                                                                                                                                                                                           | Detalhes                                                                                                           |
|-----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| Gestão de dispositivos               | <li>Subscrição do Azure</li><li>Fornecedores de recursos registados</li><li>Conta de armazenamento do Azure</li>|<li>Ativado para acesso Azure Stack Edge Mini R/Data Box Gateway, proprietário ou colaborador.</li><li>No portal Azure, aceda a **Subscrições de > Domiciliários > os fornecedores de recursos > de subscrição da sua subscrição.** Procure `Microsoft.DataBoxEdge` e registe-se. Repita para `Microsoft.Devices` se colocar cargas de trabalho IoT.</li><li>Precisa de credenciais de acesso</li> |
| Instalação do dispositivo               | Cabos de alimentação na embalagem. <br>Para os EUA, é enviado um cabo SVE 18/3 com indicação de 125 V e 15 Amperes com um conector NEMA 5-15P para C13 (entrada para saída). | Para mais informações, consulte a lista de [cabos de alimentação suportados por país](azure-stack-edge-technical-specifications-power-cords-regional.md)  |
|                                   | <li>Pelo menos 1 x 1-GbE RJ-45 cabo de rede para a porta 1  </li><li> Pelo menos 1 x 25-GbE Cabo de cobre SFP+ para a Porta 3, Porta 4, Porta 5 ou Porto 6</li>| O cliente precisa de adquirir estes cabos.<br>Para obter uma lista completa de cabos de rede suportados, interruptores e transceptores para cartões de rede de dispositivos, consulte a [Matriz de Interoperabilidade da Série Cavium FastlinQ 41000](https://www.marvell.com/documents/xalflardzafh32cfvi0z/) e [os produtos compatíveis com o adaptador de rede de canais 25G ConnectX-4 da porta dupla Mellanox 25G ConnectX-4](https://docs.mellanox.com/display/ConnectX4LxFirmwarev14271016/Firmware+Compatible+Products).| 
| Ligação do dispositivo pela primeira vez      | <li>Portátil cujas definições IPv4 podem ser alteradas. Este portátil liga-se à Porta 1 através de um interruptor ou de um adaptador USB ao Ethernet.  </li><!--<li> A minimum of 1 GbE switch must be used for the device once the initial setup is complete. The local web UI will not be accessible if the connected switch is not at least 1 Gbe.</li>-->|   |
| Inscrição do dispositivo                      | Senha de administrador do dispositivo, entre 8 e 16 caracteres, incluindo três dos seguintes tipos de caracteres: maiúsculas, minúsculas, numéricas e caracteres especiais.                                            | A palavra-passe predefinida é *a Password1,* que expira no início da sposição.                                                     |
| Definições de rede                  | O dispositivo vem com portas de rede de 2 x 1-GbE, 4 x 25-GbE. <li>A porta 1 é utilizada apenas para configurar as definições de gestão. Uma ou mais portas de dados podem ser conectadas e configuradas. </li><li> Pelo menos uma interface de rede de dados entre o Porto 2 - Porto 6 precisa de ser ligada à Internet (com conectividade com a Azure).</li><li> Configuração DHCP e IPv4 estática suportada. | A configuração estática do IPv4 requer IP, servidor DNS e gateway predefinido.   |
| Definições de rede de cálculo     | <li>Requer 2 IPs gratuitos, estáticos e contíguos para nós Kubernetes e 1 IP estático para o serviço IoT Edge.</li><li>Requer 1 IP adicional para cada serviço ou módulo extra que irá implementar.</li>| Apenas a configuração estática do IPv4 é suportada.|
| (Opcional) Definições de procuração web     | <li>Servidor de procuração web IP/FQDN, porta </li><li>Nome de utilizador de procuração web, senha</li> |  |
| Firewall e definições de porta        | Se utilizar firewall, certifique-se de que os [padrões e portas dos URLs listados](azure-stack-edge-system-requirements.md#networking-port-requirements) são permitidos para os IPs do dispositivo. |  |
| (Recomendado) Definições de tempo       | Configure o fuso horário, o servidor NTP primário, o servidor NTP secundário. | Configure o servidor NTP primário e secundário na rede local.<br>Se o servidor local não estiver disponível, os servidores NTP públicos podem ser configurados.                                                    |
| (Opcional) Atualizar as definições do servidor | <li>Requerer o endereço IP do servidor de atualização na rede local, caminho para o servidor WSUS. </li> | Por predefinição, o servidor de atualização do Windows público é utilizado.|
| Definições do dispositivo | <li>Nome de domínio totalmente qualificado do dispositivo (FQDN) </li><li>Domínio DNS</li> | |
| (Opcional) Certificados  | Para testar cargas de trabalho não produtivos, utilize [a opção de certificados Gerar](azure-stack-edge-gpu-deploy-configure-certificates.md#generate-device-certificates) <br><br> Se trouxer os seus próprios certificados, incluindo a(s) cadeia de assinaturas, [adicione certificados](azure-stack-edge-gpu-deploy-configure-certificates.md#bring-your-own-certificates) em formato apropriado.| Configure certificados apenas se alterar o nome do dispositivo e/ou o domínio DNS. |
| Ativação  | Requerer a chave de ativação a partir do recurso Azure Stack Edge.    | Uma vez gerada, a chave expira em 3 dias. |

<!--
| (Optional) MAC Address            | If MAC address needs to be approved, get the address of the connected port from local UI of the device. |                                                                                                                   |
| (Optional) Network switch port    | Device hosts Hyper-V VMs for compute. Some network switch port configurations don’t accommodate these setups by default.                                                                                                        |                                                                                                                   |-->


## <a name="next-steps"></a>Passos seguintes

Prepare-se para implantar o [seu dispositivo Azure Stack Edge Mini R](azure-stack-edge-gpu-deploy-prep.md).
