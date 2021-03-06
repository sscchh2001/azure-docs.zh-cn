---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 02/09/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: f26c7a4e9a571c317aa9cdfa1267778b607430be
ms.sourcegitcommit: 24f30b1e8bb797e1609b1c8300871d2391a59ac2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100100003"
---
|名称<br /><sub>（Azure 门户）</sub> |说明 |效果 |版本<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[机器人服务终结点应是有效的 HTTPS URI](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F6164527b-e1ee-4882-8673-572f425f5e0a) |传输过程中数据可能会被篡改。 存在一些协议提供加密，可解决滥用和篡改问题。 若要确保机器人仅通过加密通道进行通信，请将终结点设置为有效的 HTTPS URI。 这样可确保使用 HTTPS 协议对传输中的数据进行加密，并且这通常也是符合法规或行业标准的要求。 请访问：[https://docs.microsoft.com/azure/bot-service/bot-builder-security-guidelines](https://docs.microsoft.com/azure/bot-service/bot-builder-security-guidelines)。 |审核、拒绝、已禁用 |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Bot%20Service/BotService_ValidEndpoint_Audit.json) |
|[机器人服务应使用客户管理的密钥进行加密](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F51522a96-0869-4791-82f3-981000c2c67f) |Azure 机器人服务自动加密你的资源，以保护数据并实现组织安全性和合规性承诺。 默认使用 Microsoft 管理的加密密钥。 为了能够更灵活地管理密钥或控制对订阅的访问，请选择“客户管理的密钥”（亦称为“创建自己的密钥 (BYOK)”）。 详细了解 Azure 机器人服务加密：[https://docs.microsoft.com/azure/bot-service/bot-service-encryption](https://docs.microsoft.com/azure/bot-service/bot-service-encryption)。 |审核、拒绝、已禁用 |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Bot%20Service/BotService_CMKEnabled_Audit.json) |
