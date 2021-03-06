---
title: 命名实体识别支持的类别
titleSuffix: Azure Cognitive Services
description: 了解文本分析 API 中支持的实体类别。
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 01/22/2021
ms.author: aahi
ms.openlocfilehash: 883c5a20612f4dab44c0d06776ee287b27174ab9
ms.sourcegitcommit: b4e6b2627842a1183fce78bce6c6c7e088d6157b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99097250"
---
# <a name="supported-entity-categories-in-the-text-analytics-api-v3"></a>文本分析 API v3 中支持的实体类别

使用本文查找[命名实体识别](how-tos/text-analytics-how-to-entity-linking.md) (NER) 可以返回的实体类别。 NER 运行预测模型来识别输入文档中的命名实体并对其分类。

用户也可预览 NER v3.1，其中包括检测个人 (`PII`) 信息和运行状况 (`PHI`) 信息的功能。 此外，单击“运行状况”选项卡可以查看运行状况文本分析中支持的类别的列表。 

可以在[迁移指南](migration-guide.md?tabs=named-entity-recognition)中找到版本2.1 返回的类型列表

## <a name="entity-categories"></a>实体类别

#### <a name="general"></a>[常规](#tab/general)

[!INCLUDE [supported entity types - general](./includes/entity-types/general-entities.md)]

#### <a name="pii"></a>[PII](#tab/personal)

[!INCLUDE [supported entity types - personally identifying information](./includes/entity-types/personal-information-entities.md)]

#### <a name="health"></a>[健康产业](#tab/health)

[!INCLUDE [biomedical entity types](./includes/entity-types/health-entities.md)]

**_

## <a name="next-steps"></a>后续步骤

[如何在文本分析中使用命名实体识别](how-tos/text-analytics-how-to-entity-linking.md)
