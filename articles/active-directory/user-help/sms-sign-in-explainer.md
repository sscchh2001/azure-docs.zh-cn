---
title: 电话号码的 SMS 登录用户体验-Azure AD
description: 详细了解使用新的或现有的电话号码进行短信登录的用户体验
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: end-user-help
ms.date: 01/21/2021
ms.author: curtand
ms.reviewer: kasimpso
ms.custom: user-help, seo-update-azuread-jan
ms.openlocfilehash: 1a50f2032a978a552205d1bba602249f34f0478a
ms.sourcegitcommit: 52e3d220565c4059176742fcacc17e857c9cdd02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/21/2021
ms.locfileid: "98661580"
---
# <a name="use-your-phone-number-as-a-user-name"></a>使用你的电话号码作为用户名

注册设备后，你可以通过手机访问你的组织的服务，而你的组织无法访问你的手机。 如果你是管理员，可以在[为用户配置和启用基于短信的身份验证](../authentication/howto-authentication-sms-signin.md)中找到详细信息。

如果你的组织尚不提供短信登录，在帐户中注册手机时，你将不会看到此选项。  

## <a name="when-you-have-a-new-phone-number"></a>如果你有新的电话号码

如果你购买了新的手机或者申请了新的电话号码，在提供短信登录功能的组织中注册此号码时，需要完成一般的手机注册过程：

1. 选择“添加方法”  。
1. 选择“手机”  。
1. 输入电话号码，然后选择“以短信形式向我发送验证码”  。
1. 输入验证码后，选择“下一步”  。
1. 随后会看到提示“已验证短信。 你的手机已成功注册。”

> [!Important]
> 由于已知问题，在短时间内添加电话号码将不会注册 SMS 登录号码。 必须使用添加的号码登录，然后按照提示注册该号码，用于短信登录。

### <a name="when-the-phone-number-is-in-use"></a>当电话号码已被使用时

如果你尝试使用的电话号码已被组织中的其他人使用，将会出现以下消息：

![电话号码已被使用时出现的错误消息](media/sms-sign-in-explainer/sms-sign-in-error.png)

请联系管理员解决此问题。

## <a name="when-you-have-an-existing-number"></a>如果你有现有的号码

如果你已在组织中使用了某个电话号码，并且可将该电话号码用作用户名，则可通过以下步骤登录。

1. 允许通过短信登录时，系统会显示一条横幅，询问你是否允许使用该电话号码进行短信登录：

    :::image type="content" source="media/sms-sign-in-explainer/sms-sign-in-banner.png" alt-text="屏幕截图，显示标题，以便为电话号码启用 SMS 登录，并选中 &quot;启用&quot; 操作。" lightbox="media/sms-sign-in-explainer/sms-sign-in-banner.png":::

1. 此外，如果在手机方法磁贴中选择脱字号，则会显示“启用”按钮  ：

    [![启用电话号码进行短信登录的横幅。](media/sms-sign-in-explainer/sms-sign-in-phone-method.png)](media/sms-sign-in-explainer/sms-sign-in-phone-method.png#lightbox)

1. 若要启用该方法，请选择“启用”  。 系统会提示确认该操作：

    ![启用电话号码进行短信登录的确认对话框](media/sms-sign-in-explainer/sms-sign-in-confirmation.png)

1. 选择“启用”  。

## <a name="when-you-remove-your-phone-number"></a>删除电话号码时

1. 若要删除电话号码，请选择短信登录手机方法磁贴中的删除按钮。

    [![删除用于短信登录的电话号码的横幅。](media/sms-sign-in-explainer/sms-sign-in-delete-method.png)](media/sms-sign-in-explainer/sms-sign-in-delete-method.png#lightbox)

2. 当系统提示确认该操作时，请选择“确定”  。

无法删除已用作默认登录方法的电话号码。 若要删除该电话号码，必须更改默认登录方法，然后再次删除该电话号码。
