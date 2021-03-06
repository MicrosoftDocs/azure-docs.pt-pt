---
title: Gerir a consola de gestão no local
description: Saiba mais sobre opções de consola de gestão no local, como backup e restauro, definindo o nome de anfitrião, e configurando um proxy para sensores.
ms.date: 1/12/2021
ms.topic: article
ms.openlocfilehash: 871c74eee4b74538a8a09188953916ff7376bc8d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2021
ms.locfileid: "104781726"
---
# <a name="manage-the-on-premises-management-console"></a>Gerir a consola de gestão no local

Este artigo abrange opções de consola de gestão no local, como backup e restauro, descarregamento de ficheiros de ativação de dispositivos da comissão, atualização de certificados e criação de um proxy para sensores.

Você a bordo da consola de gestão no local a partir do portal Azure.

## <a name="upload-an-activation-file"></a>Faça upload de um ficheiro de ativação

Quando iniciar a sua primeira sação, é descarregado um ficheiro de ativação para a consola de gestão no local. Este ficheiro contém os dispositivos comprometidos agregados que são definidos durante o processo de embarque. A lista inclui sensores associados a várias subscrições.

Após a ativação inicial, o número de dispositivos monitorizados pode exceder o número de dispositivos comprometidos definidos durante o embarque. Este evento pode acontecer, por exemplo, se ligar mais sensores à consola de gestão. Se houver uma discrepância entre o número de dispositivos monitorizados e o número de dispositivos comprometidos, aparece um aviso na consola de gestão. Se este evento ocorrer, deverá carregar um novo ficheiro de ativação.

Para carregar um ficheiro de ativação:

1. Vá ao Azure Defender para a página **de preços IoT.**
1. Selecione **o ficheiro de ativação Descarregue para o** separador consola de gestão. O ficheiro de ativação é descarregado.

   :::image type="content" source="media/how-to-manage-sensors-from-the-on-premises-management-console/cloud_download_opm_activation_file.png" alt-text="Descarregue o ficheiro de ativação.":::

1. Selecione **Definições** do Sistema a partir da consola de gestão.
1. **Selecione ativação**.
1. **Selecione Escolha um Ficheiro** e selecione o ficheiro que guardou.

## <a name="manage-certificates"></a>Gerir certificados

Após a instalação da consola de gestão no local, é gerado um certificado auto-assinado local e utilizado para aceder à aplicação web da consola de gestão. Quando os utilizadores de administrador insinuam-se pela primeira vez na consola de gestão, são solicitados a fornecer um certificado SSL/TLS. Para obter mais informações sobre a configuração da primeira vez, consulte [Ativar e configurar a sua consola de gestão no local.](how-to-activate-and-set-up-your-on-premises-management-console.md)

As secções seguintes fornecem informações sobre a atualização de certificados, trabalhando com comandos CLI certificados e certificados suportados e parâmetros de certificado.

### <a name="about-certificates"></a>Acerca de certificados

O Azure Defender for IoT utiliza certificados SSL e TLS para:

- Satisfaça os requisitos específicos de certificado e encriptação solicitados pela sua organização, fazendo o upload do certificado assinado pela AC.

- Permitir validação entre a consola de gestão e sensores conectados, e entre uma consola de gestão e uma consola de gestão de alta disponibilidade. A validação é avaliada com uma lista de revogação de certificados e a data de validade do certificado. *Se a validação falhar, a comunicação entre a consola de gestão e o sensor é interrompida e aparece um erro de validação na consola*. Esta opção é ativada por padrão após a instalação.

As regras de encaminhamento de terceiros não são validadas. Exemplos são informações de alerta enviadas para SYSLOG, Splunk ou ServiceNow; e comunicação com o Ative Directory.

#### <a name="ssl-certificates"></a>Certificados SSL

O sensor Defender para IoT e a consola de gestão no local utilizam certificados SSL e TLS para as seguintes funções: 

 - Proteja as comunicações entre os utilizadores e a consola web do aparelho. 
 
 - Proteja as comunicações à API REST no sensor e na consola de gestão no local.
 
 - Comunicações seguras entre os sensores e uma consola de gestão no local. 

Uma vez instalado, o aparelho gera um certificado auto-assinado local para permitir o acesso preliminar à consola web. Os certificados Enterprise SSL e TLS podem ser instalados utilizando a [`cyberx-xsense-certificate-import`](#cli-commands) ferramenta de linha de comando. 

 > [!NOTE]
 > Para integrações e regras de encaminhamento em que o aparelho é o cliente e iniciador da sessão, são utilizados certificados específicos e não estão relacionados com os certificados do sistema.  
 >
 >Nestes casos, os certificados são normalmente recebidos do servidor, ou utilizam encriptação assimétrica onde será fornecido um certificado específico para configurar a integração. 

### <a name="update-certificates"></a>Certificados de atualização

Os utilizadores de administradores da consola de gestão no local podem atualizar certificados.

Para atualizar um certificado:  

1. Selecione **Definições do sistema**.

1. Selecione **Certificados SSL/TLS**.
1. Apague ou edite o certificado e adicione um novo.
   
   - Adicione um nome de certificado.
   
   - Faça o upload de um ficheiro CRT e do ficheiro chave e introduza uma palavra-passe.
   - Faça o upload de um ficheiro PEM, se necessário.

Para alterar a definição de validação:

1. Ligue ou desligue o **toggle de validação do certificado de ativação.**

1. Selecione **Guardar**.

Se a opção estiver ativada e a validação falhar, a comunicação entre a consola de gestão e o sensor é interrompida e aparece um erro de validação na consola.

### <a name="certificate-support"></a>Suporte a certificados

São suportados os seguintes certificados:

- Infraestrutura chave privada e empresarial (PKI privado)
 
- Infraestruturas de chaves públicas (PKI público) 

- Gerado localmente no aparelho (auto-assinado localmente) 

  > [!IMPORTANT]
  > Não recomendamos usar certificados auto-assinados. Este tipo de ligação não é seguro e deve ser utilizado apenas para ambientes de teste. Uma vez que o proprietário do certificado não pode ser validado, e a segurança do seu sistema não pode ser mantida, os certificados auto-assinados nunca devem ser usados para redes de produção.

### <a name="supported-ssl-certificates"></a>Certificados SSL suportados 

São suportados os seguintes parâmetros: 

**Certificado CRT**

- O ficheiro de certificado primário para o seu nome de domínio

- Algoritmo de assinatura = SHA256RSA
- Algoritmo de hash de assinatura = SHA256
- Válido a partir de = Data passada válida
- Válido Para = Data futura válida
- Chave Pública = RSA 2048 bits (mínimo) ou 4096 bits
- Ponto de Distribuição CRL = URL para ficheiro .crl
- Assunto CN = URL, pode ser um certificado wildcard; por exemplo, Sensor.contoso. <span> com,ou *.contoso. <span> com
- Assunto (C)ountry = definido, por exemplo, EUA
- Unidade org do sujeito (OU) = definida; por exemplo, Contoso Labs
- Objeto (O)rganização = definido; por exemplo, Contoso Inc

**Ficheiro chave**

- O ficheiro chave gerado quando criou a RSE

- RSA 2048 bits (mínimo) ou 4096 bits

 > [!Note]
 > Utilizando um comprimento de chave de 4096bits:
 > - O aperto de mão SSL no início de cada ligação será mais lento.  
 > - Há um aumento no uso do CPU durante os apertos de mão. 

**Cadeia de certificados**

- O ficheiro de certificado intermédio (se houver) que foi fornecido pela sua AC.

- O certificado de AC que emitiu o certificado do servidor deve ser o primeiro no ficheiro, seguido por quaisquer outros até à raiz. 
- A cadeia pode incluir atributos de saco.

**Frase-passe**

- Uma chave é suportada.

- Configurar quando estiver a importar o certificado.

Certificados com outros parâmetros podem funcionar, mas a Microsoft não os suporta.

#### <a name="encryption-key-artifacts"></a>Artefactos chave de encriptação

**.pem: arquivo de contentores de certificado**

Os ficheiros Privacy Enhanced Mail (PEM) eram o tipo geral de ficheiros usado para garantir e-mails. Hoje em dia, os ficheiros PEM são utilizados com certificados e utilizam as teclas X509 ASN1.  

O ficheiro do contentor é definido nos RFCs 1421 a 1424, um formato de contentor que pode incluir apenas o certificado público. Por exemplo, a Apache instala um certificado ca, ficheiros, ETC, SSL ou CERTS. Isto pode incluir toda uma cadeia de certificados, incluindo chaves públicas, chaves privadas e certificados de raiz.  

Também pode codificar um CSR como o formato PKCS10, que pode ser traduzido para PEM.

**.cert .cer .crt: arquivo de contentores de certificado**

Um `.pem` ficheiro , ou `.der` formatado com uma extensão diferente. O ficheiro é reconhecido pelo Windows Explorer como um certificado. O `.pem`   ficheiro não é reconhecido pelo Windows Explorer.

**.key: ficheiro chave privada**

Um ficheiro chave está no mesmo formato que um ficheiro PEM, mas tem uma extensão diferente. 

#### <a name="additional-commonly-available-key-artifacts"></a>Artefactos-chave comumente disponíveis

**.csr - pedido de assinatura de certificado.**  

Este ficheiro é utilizado para submissão às autoridades de certificados. O formato real é PKCS10, que é definido no RFC 2986, e pode incluir alguns, ou todos os detalhes-chave do certificado solicitado. Por exemplo, assunto, organização e estado. É a chave pública do certificado que é assinado pela AC, e recebe um certificado em troca.  

O certificado devolvido é o certificado público, que inclui a chave pública, mas não a chave privada. 

**.pkcs12 .pfx .p12 – cartão de senha**. 

Originalmente definida pela RSA nos Public-Key Cryptography Standards (PKCS), a variante de 12 foi originalmente melhorada pela Microsoft, e posteriormente submetida como RFC 7292.  

Este formato de contentor requer uma palavra-passe que contenha pares de certificados públicos e privados. Ao contrário dos `.pem`   ficheiros, este contentor está totalmente encriptado.  

Pode utilizar o OpenSSL para transformar o ficheiro num `.pem`   ficheiro com chaves públicas e privadas: `openssl pkcs12 -in file-to-convert.p12 -out converted-file.pem -nodes`  

**.der – PEM binário codificado.**

A forma de codificar a sintaxe ASN.1 no binário é através de um `.pem`   ficheiro, que é apenas um ficheiro codificado `.der` Base64. 

O OpenSSL pode converter estes ficheiros `.pem` para:  `openssl x509 -inform der -in to-convert.der -out converted.pem`  

O Windows reconhecerá estes ficheiros como ficheiros de certificado. Por predefinição, o Windows exportará certificados como `.der` ficheiros formatados com uma extensão diferente.

**.crl - lista de revogação de certificados**.  

As autoridades de certificados produzem-nos como forma de desutorizar os certificados antes da sua expiração. 

#### <a name="cli-commands"></a>Comandos da CLI

Utilize o `cyberx-xsense-certificate-import` comando CLI para importar certificados. Para utilizar esta ferramenta, é necessário enviar ficheiros de certificados para o dispositivo, utilizando ferramentas como WinSCP ou Wget.

O comando suporta as seguintes bandeiras de entrada:

- `-h`: Mostra a sintaxe de ajuda da linha de comando.

- `--crt`: Caminho para um ficheiro de certificado (extensão.crt).

- `--key`:  \* .key ficheiro. O comprimento da chave deve ser no mínimo 2.048 bits.

- `--chain`: Caminho para um ficheiro de cadeia de certificados (opcional).

- `--pass`: Frase de passe utilizada para encriptar o certificado (opcional).

- `--passphrase-set`: Predefinição = `False` , não é used. De definido `True` para utilizar a frase de passe anterior fornecida com o certificado anterior (opcional).

Quando estiver a utilizar o comando CLI:

- Verifique se os ficheiros do certificado são legíveis no aparelho.

- Verifique se o nome de domínio e o IP no certificado correspondem à configuração que o departamento de TI planeou.

### <a name="use-openssl-to-manage-certificates"></a>Utilize o OpenSSL para gerir certificados

Gerencie os seus certificados com os seguintes comandos:

| Description | Comando CLI |
|--|--|
| Gerar uma nova chave privada e pedido de assinatura de certificado | `openssl req -out CSR.csr -new -newkey rsa:2048 -nodes -keyout privateKey.key` |
| Gerar um certificado autoassinado | `openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout privateKey.key -out certificate.crt` |
| Gerar um pedido de assinatura de certificado (RSE) para uma chave privada existente | `openssl req -out CSR.csr -key privateKey.key -new` |
| Gerar um pedido de assinatura de certificado com base num certificado existente | `openssl x509 -x509toreq -in certificate.crt -out CSR.csr -signkey privateKey.key` |
| Remova uma palavra-passe de uma chave privada | `openssl rsa -in privateKey.pem -out newPrivateKey.pem` |

Se precisar de verificar as informações dentro de um Certificado, RSE ou Chave Privada, utilize estes comandos;

| Description | Comando CLI |
|--|--|
| Consulte um pedido de assinatura de certificado (CSR) | `openssl req -text -noout -verify -in CSR.csr` |
| Verifique uma chave privada | `openssl rsa -in privateKey.key -check` |
| Verifique um certificado | `openssl x509 -in certificate.crt -text -noout`  |

Se receber um erro que a tecla privada não corresponda ao certificado, ou que não seja confiável um certificado que instalou num site, utilize estes comandos para corrigir o erro;

| Description | Comando CLI |
|--|--|
| Verifique um hash MD5 da chave pública para garantir que corresponde ao que está numa rse ou chave privada | 1. `openssl x509 -noout -modulus -in certificate.crt | openssl md5` <br /> 2. `openssl rsa -noout -modulus -in privateKey.key | openssl md5` <br /> 3. `openssl req -noout -modulus -in CSR.csr | openssl md5 ` |

Para converter certificados e chaves em diferentes formatos para torná-los compatíveis com tipos específicos de servidores, ou software, utilize estes comandos;

| Description | Comando CLI |
|--|--|
| Converter um ficheiro DER (.crt .cer .der) para PEM  | `openssl x509 -inform der -in certificate.cer -out certificate.pem`  |
| Converter um ficheiro PEM para DER | `openssl x509 -outform der -in certificate.pem -out certificate.der`  |
| Converter um ficheiro PKCS#12 (.pfx .p12) contendo uma chave privada e certificados para PEM | `openssl pkcs12 -in keyStore.pfx -out keyStore.pem -nodes` <br />Pode adicionar `-nocerts` apenas à saída a chave privada, ou adicionar `-nokeys` apenas à saída os certificados. |
| Converter um ficheiro de certificado PEM e uma chave privada para PKCS#12 (.pfx .p12) | `openssl pkcs12 -export -out certificate.pfx -inkey privateKey.key -in certificate.crt -certfile CACert.crt` |

## <a name="define-backup-and-restore-settings"></a>Definir definições de backup e restauro

A cópia de segurança do sistema de consolas de gestão no local é executada automaticamente, diariamente. Os dados são guardados num disco diferente. A localização predefinida é `/var/cyberx/backups` . 

Pode transferir automaticamente este ficheiro para a rede interna. 

> [!NOTE]
> Pode efetuar o procedimento de cópia de segurança e restauro apenas na mesma versão. 

Para apoiar a máquina de consola de gestão no local: 

- Inscreva-se numa conta administrativa e `sudo cyberx-management-backup -full` entre.

Para restaurar o mais recente ficheiro de backup:

- Inscreva-se numa conta administrativa e `$ sudo cyberx-management-system-restore` entre.

Para guardar a cópia de segurança para um servidor SMB externo:

1. Crie uma pasta partilhada no servidor SMB externo.  

   Obtenha o caminho da pasta, nome de utilizador e senha necessária para aceder ao servidor SMB. 

2. No Defender for IoT, faça um diretório para os backups:

   - `sudo mkdir /<backup_folder_name_on_ server>` 

   - `sudo chmod 777 /<backup_folder_name_on_c_server>/` 

3. Editar fstab:  

   - `sudo nano /etc/fstab` 

   - `add - //<server_IP>/<folder_path> /<backup_folder_name_on_server> cifs rw,credentials=/etc/samba/user,vers=3.0,uid=cyberx,gid=cyberx,file_mode=0777,dir_mode=0777 0 0` 

4. Editar ou criar credenciais para o servidor SMB partilhar: 

   - `sudo nano /etc/samba/user` 

5. Adicionar: 

   - `username=<user name>`

   - `password=<password>` 

6. Monte o diretório: 

   - `sudo mount -a` 

7. Configure um diretório de reserva para a pasta partilhada na consola de gestão do Defender para IoT no local:  

   - `sudo nano /var/cyberx/properties/backup.properties` 

   - `set Backup.shared_location to <backup_folder_name_on_server>`

## <a name="edit-the-host-name"></a>Editar o nome do anfitrião

Para editar o nome de anfitrião da consola de gestão configurado no servidor organizacional DNS:

1. No painel esquerdo da consola de gestão, selecione **Definições do Sistema**.  

2. Na secção de networking da consola, selecione **Network**. 

3. Introduza o nome do anfitrião configurado no servidor ORGANIZACIONAL DNS. 

4. Selecione **Guardar**.

## <a name="define-vlan-names"></a>Definir nomes VLAN

Os nomes VLAN não são sincronizados entre o sensor e a consola de gestão. Deve definir nomes idênticos em componentes.

Na área de networking, selecione **VLAN** e adicione nomes aos IDs VLAN descobertos. Em seguida, selecione **Guardar**.

## <a name="define-a-proxy-to-sensors"></a>Definir um proxy para sensores

Melhore a segurança do sistema evitando a entrada do utilizador diretamente no sensor. Em vez disso, utilize um túnel de procuração para permitir que os utilizadores acedam ao sensor a partir da consola de gestão no local com uma única regra de firewall. Esta melhoria reduz a possibilidade de acesso não autorizado ao ambiente de rede para além do sensor.

Use um proxy em ambientes onde não há conectividade direta com os sensores.

:::image type="content" source="media/how-to-access-sensors-using-a-proxy/proxy-diagram.png" alt-text="O utilizador.":::

O seguinte procedimento liga um sensor à consola de gestão no local e permite fazer túneis nessa consola:

1. Inscreva-se no aparelho de consola de gestão no local CLI com credenciais administrativas.

2. Digite `sudo cyberx-management-tunnel-enable` e selecione **Enter**.

4. Digite `--port 10000` e selecione **Enter**.

## <a name="adjust-system-properties"></a>Ajustar as propriedades do sistema

As propriedades do sistema controlam várias operações e configurações na consola de gestão. Editá-las ou modificá-las pode danificar o funcionamento da consola de gestão. Consulte o [Microsoft Support](https://support.microsoft.com) antes de alterar as suas definições.

Para aceder às propriedades do sistema: 

1. Inscreva-se na consola de gestão no local ou no sensor.

2. Selecione **Definições do sistema**.

3. Selecione Propriedades do **Sistema** na secção **Geral.**

## <a name="change-the-name-of-the-on-premises-management-console"></a>Alterar o nome da consola de gestão no local

Pode alterar o nome da consola de gestão no local. Os novos nomes aparecem no navegador web da consola, em várias janelas de consolas e em registos de resolução de problemas. O nome predefinido é **consola de gestão.**

Para alterar o nome:

1. Na parte inferior do painel esquerdo, selecione o nome atual.

   :::image type="content" source="media/how-to-change-the-name-of-your-azure-consoles/console-name.png" alt-text="Screenshot da versão da consola de gestão no local.":::

2. Na caixa de diálogo de configuração da configuração da **consola de gestão editar,** insira o novo nome. O nome não pode ter mais de 25 caracteres.

   :::image type="content" source="media/how-to-change-the-name-of-your-azure-consoles/edit-management-console-configuration.png" alt-text="Screenshot de edição do Defender para configuração da plataforma IoT.":::

3. Selecione **Guardar**. O novo nome é aplicado.

   :::image type="content" source="media/how-to-change-the-name-of-your-azure-consoles/name-changed.png" alt-text="Screenshot que mostra o nome alterado da consola.":::

## <a name="password-recovery"></a>Recuperação de senha

A recuperação de palavras-passe para a sua consola de gestão no local está ligada à subscrição a que o dispositivo está ligado. Não é possível recuperar uma palavra-passe se não souber a que subscrição um dispositivo está ligado.

Para repor a sua palavra-passe:

1. Vá à página de inscrição da consola de gestão no local.
1. Selecione **recuperação de palavras-passe**.
1. Copie o identificador único.
1. Aceda à página Defender para **Sites e sensores** IoT e selecione o **separador Recuperar a minha palavra-passe.**
1. Introduza o identificador único e selecione **Recuperar**. O ficheiro de ativação é descarregado.
1. Vá à página **de Recuperação de Passwords** e faça o upload do ficheiro de ativação.
1. Selecione **Seguinte**.
 
   Foi-lhe dado agora o seu nome de utilizador e uma nova senha gerada pelo sistema.

> [!NOTE]
> O sensor está ligado à subscrição a que estava originalmente ligado. Só pode recuperar a palavra-passe utilizando a mesma subscrição a que está anexada.

## <a name="update-the-software-version"></a>Atualizar a versão do software

O procedimento a seguir descreve como atualizar a versão de software de consola de gestão de informação no local. O processo de atualização demora cerca de 30 minutos.

1. Aceda ao [Portal do Azure](https://portal.azure.com/).

1. Vá ao Defender para ioT.

1. Aceda à página **'Atualizações'.**

1. Selecione uma versão da secção de consola de gestão no local.

1. Selecione **Baixar** e guardar o ficheiro.

1. Inicie sessão na consola de gestão no local e selecione **Definições** do Sistema a partir do menu lateral.

1. No painel **'Atualização** da versão', selecione **Update**.

1. Selecione o ficheiro que descarregou na página Defender para **IoT Updates.**

## <a name="mail-server-settings"></a>Definições de servidor de correio

Defina as definições do servidor de correio SMTP para a consola de gestão no local.

Para definir:

1. Inscreva-se no CLI para a gestão no local com credenciais administrativas.
1. Escreva ```nano /var/cyberx/properties/remote-interfaces.properties```.
1. Selecione Enter. Aparecem as seguintes indicações.
```mail.smtp_server= ```
```mail.port=25 ```
```mail.sender=```
1. Introduza o nome do servidor SMTP e o remetente e selecione insira a introdução.

## <a name="see-also"></a>Ver também

[Gerir sensores a partir da consola de gestão](how-to-manage-sensors-from-the-on-premises-management-console.md)

[Gerir sensores individuais](how-to-manage-individual-sensors.md)
