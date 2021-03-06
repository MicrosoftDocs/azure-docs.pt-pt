---
title: Azure Relay - Migrar para autorização de assinatura de acesso partilhado
description: Descreve como migrar as aplicações do Azure Relay da utilização do Azure Ative Directory Access Control Service para a autorização de Assinatura de Acesso Partilhado.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: 3b793173270b0ddf25f0e971dbb2fed97cb10a55
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "87532871"
---
# <a name="azure-relay---migrate-from-azure-active-directory-access-control-service-to-shared-access-signature-authorization"></a>Azure Relay - Migrar do Serviço de Controlo de Acesso ao Diretório Ativo Azure para autorização de assinatura de acesso partilhado

As aplicações Azure Relay historicamente tiveram a opção de usar dois modelos de autorização diferentes: o modelo de token [Assinatura de Acesso Partilhado (SAS)](../service-bus-messaging/service-bus-sas.md) fornecido diretamente pelo serviço De Retransmissão, e um modelo federado onde a gestão das regras de autorização é gerida no interior pelo [Azure Ative Directory](../active-directory/index.yml) Access Control Service (ACS), e os tokens obtidos a partir de ACS são passados para Relay para autorizar o acesso às características desejadas.

O modelo de autorização ACS foi há muito substituído pela [autorização SAS](../service-bus-messaging/service-bus-authentication-and-authorization.md) como o modelo preferido, e toda a documentação, orientação e amostras utilizam exclusivamente SAS hoje em dia. Além disso, já não é possível criar novos espaços de nome relay que são emparelhados com ACS.

A SAS tem a vantagem de não depender imediatamente de outro serviço, mas pode ser utilizado diretamente de um cliente sem qualquer intermediário, dando ao cliente acesso à regra e chave de regras sas. O SAS também pode ser facilmente integrado com uma abordagem na qual um cliente tem primeiro de passar uma verificação de autorização com outro serviço e depois é emitido um token. Esta última abordagem é semelhante ao padrão de utilização do ACS, mas permite a emissão de fichas de acesso com base em condições específicas da aplicação que são difíceis de expressar em ACS.

Para todas as aplicações existentes que dependem da ACS, instamos os clientes a migrarem as suas aplicações para confiarem no SAS.

## <a name="migration-scenarios"></a>Cenários de migração

ACS e Relay são integrados através do conhecimento partilhado de uma chave de *assinatura.* A chave de assinatura é usada por um espaço de nome ACS para assinar fichas de autorização, e é usada pela Azure Relay para verificar se o token foi emitido pelo espaço de nome ACS emparelhado. O espaço de nomes ACS detém identidades de serviço e regras de autorização. As regras de autorização definem qual a identidade do serviço ou qual o símbolo emitido por um fornecedor de identidade externo que obtém o tipo de acesso a uma parte do gráfico de espaço de nome Relay, sob a forma de uma correspondência de prefixo mais longa.

Por exemplo, uma regra da ACS pode conceder a reclamação **enviar** o pedido de envio no prefixo do caminho para uma identidade de serviço, o `/` que significa que um símbolo emitido pela ACS com base nessa regra concede ao cliente o direito de enviar a todas as entidades no espaço de nome. Se o prefixo do caminho `/abc` for, a identidade é restrita ao envio a entidades nomeadas `abc` ou organizadas por baixo desse prefixo. Presume-se que os leitores desta orientação migratória já estão familiarizados com estes conceitos.

Os cenários de migração enquadram-se em três grandes categorias:

1.  **Incumprimentos inalterados**. Alguns clientes usam um objeto [SharedSecretTokenProvider,](/dotnet/api/microsoft.servicebus.sharedsecrettokenprovider) passando a identidade de serviço **do proprietário** gerada automaticamente e a sua chave secreta para o espaço de nomes ACS, emparelhado com o espaço de nome Relay, e não adicionam novas regras.

2.  **Identidades de serviço personalizadas com regras simples.** Alguns clientes adicionam novas identidades de serviço e concedem cada nova identidade de serviço **Enviar,** **Ouvir** e **Gerir** permissões para uma entidade específica.

3.  **Identidades de serviço personalizadas com regras complexas.** Muito poucos clientes têm conjuntos de regras complexos em que fichas emitidas externamente são mapeadas para direitos no Relay, ou onde uma única identidade de serviço é atribuída direitos diferenciados em vários caminhos do espaço de nome através de várias regras.

Para assistência com a migração de conjuntos de regras complexos, pode contactar [o suporte do Azure](https://azure.microsoft.com/support/options/). Os outros dois cenários permitem uma migração simples.

### <a name="unchanged-defaults"></a>Incumprimentos inalterados

Se a sua aplicação não tiver alterado os predefinidos ACS, pode substituir toda a utilização [do SharedSecretTokenProvider](/dotnet/api/microsoft.servicebus.sharedsecrettokenprovider) por um objeto [SharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) e utilizar o espaço de nome pré-configurado **RootManageSharedAccessKey** em vez da conta **do proprietário** acs. Note que mesmo com a conta **do proprietário** acs, esta configuração não foi (e ainda é) geralmente recomendada, porque esta conta/regra fornece autoridade de gestão completa sobre o espaço de nome, incluindo permissão para eliminar quaisquer entidades.

### <a name="simple-rules"></a>Regras simples

Se a aplicação utilizar identidades de serviço personalizadas com regras simples, a migração é simples no caso de uma identidade de serviço ACS ter sido criada para fornecer controlo de acesso num retransmissor específico. Este cenário acontece frequentemente em soluções ao estilo SaaS, onde cada relé é usado como ponte para um local de arrendamento ou filial, e a identidade de serviço é criada para esse site específico. Neste caso, a respetiva identidade de serviço pode ser migrada para uma regra de Assinatura de Acesso Partilhado, diretamente no relé. O nome de identidade do serviço pode tornar-se o nome da regra SAS e a chave de identidade de serviço pode tornar-se a chave de regra SAS. Os direitos da regra SAS são então configurados equivalentes à regra ACS aplicável respectivamente para a entidade.

Você pode fazer esta configuração nova e adicional de SAS no lugar em qualquer espaço de nome existente que é federado com ACS, e a migração para longe de ACS é subsequentemente realizada usando [SharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) em vez de [SharedSecretTokenProvider](/dotnet/api/microsoft.servicebus.sharedsecrettokenprovider). O espaço de nome não precisa de ser desvinculado do ACS.

### <a name="complex-rules"></a>Regras complexas

As regras da SAS não se destinam a ser contas, mas são nomeadas chaves de assinatura associadas a direitos. Como tal, os cenários em que a aplicação cria muitas identidades de serviço e lhes concede direitos de acesso a várias entidades ou a todo o espaço de nome ainda requerem um intermediário de emissão de fichas. Pode obter orientação para tal intermediário [contactando o suporte](https://azure.microsoft.com/support/options/).

## <a name="next-steps"></a>Passos seguintes

Para saber mais sobre a autenticação do Azure Relay, consulte os seguintes tópicos:

* [Autenticação e autorização do Relé Azure](relay-authentication-and-authorization.md)
* [Autenticação de ônibus de serviço com assinaturas de acesso compartilhado](../service-bus-messaging/service-bus-sas.md)
