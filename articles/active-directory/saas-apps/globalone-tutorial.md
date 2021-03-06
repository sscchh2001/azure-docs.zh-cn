---
title: 教程：Azure Active Directory 与 EY GlobalOne 集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 与 EY GlobalOne 之间配置单一登录。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/07/2020
ms.author: jeedes
ms.openlocfilehash: a248718d12abf90abd80c9210b994e7d74ab111b
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2020
ms.locfileid: "92448701"
---
# <a name="tutorial-integrate-ey-globalone-with-azure-active-directory"></a>教程：将 EY GlobalOne 与 Azure Active Directory 集成

本教程介绍如何将 EY GlobalOne 与 Azure Active Directory (Azure AD) 集成。 将 EY GlobalOne 与 Azure AD 集成后，可以执行以下操作：

* 在 Azure AD 中控制谁有权访问 EY GlobalOne。
* 让用户可使用其 Azure AD 帐户自动登录到 EY GlobalOne。
* 在一个中心位置（Azure 门户）管理帐户。

若要了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要开始操作，需备齐以下项目：

* 一个 Azure AD 订阅。 如果没有订阅，可以获取一个[免费帐户](https://azure.microsoft.com/free/)。
* 已启用 EY GlobalOne 单一登录 (SSO) 的订阅。

## <a name="scenario-description"></a>方案描述

本教程在测试环境中配置并测试 Azure AD SSO。
* EY GlobalOne 支持 SP 和 IDP 发起的 SSO 
* EY GlobalOne 支持“实时”用户预配。
* 配置 EY GlobalOne 后，可以强制实施会话控制，实时防止组织的敏感数据外泄和渗透。 会话控制从条件访问扩展而来。 [了解如何通过 Microsoft Cloud App Security 强制实施会话控制](/cloud-app-security/proxy-deployment-aad)

## <a name="adding-ey-globalone-from-the-gallery"></a>从库中添加 EY GlobalOne

若要配置 EY GlobalOne 与 Azure AD 的集成，需要从库中将 EY GlobalOne 添加到托管 SaaS 应用列表。

1. 使用工作或学校帐户或个人 Microsoft 帐户登录到 [Azure 门户](https://portal.azure.com)。
1. 在左侧导航窗格中，选择“Azure Active Directory”服务。
1. 导航到“企业应用程序”，选择“所有应用程序” 。
1. 若要添加新的应用程序，请选择“新建应用程序”。
1. 在“从库中添加”部分的搜索框中，键入 EY GlobalOne 。
1. 从结果面板中选择“EY GlobalOne”，然后添加该应用。 在该应用添加到租户时等待几秒钟。

## <a name="configure-and-test-azure-ad-sso-for-ey-globalone"></a>配置并测试 EY GlobalOne 的 Azure AD SSO

使用名为 B. Simon 的测试用户配置和测试 EY GlobalOne 的 Azure AD SSO。 若要使 SSO 正常工作，需要在 Azure AD 用户与 EY GlobalOne 中的相关用户之间建立链接关系。

若要配置和测试 EY GlobalOne 的 Azure AD SSO，请完成以下构建基块：

1. **[配置 Azure AD SSO](#configure-azure-ad-sso)** ，使用户能够使用此功能。
    * **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** ，以使用 B. Simon 测试 Azure AD 单一登录。
    * **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** ，以使 B. Simon 能够使用 Azure AD 单一登录。
1. **[配置 EY GlobalOne](#configure-ey-globalone)** ，以在应用程序端配置 SSO 设置。
    * **[创建 EY GlobalOne 测试用户](#create-ey-globalone-test-user)** ，以便在 EY GlobalOne 中拥有 B. Simon 的对应用户，并将其链接到该用户的 Azure AD 表示形式。
1. **[测试 SSO](#test-sso)** ，验证配置是否正常工作。

### <a name="configure-azure-ad-sso"></a>配置 Azure AD SSO

按照下列步骤在 Azure 门户中启用 Azure AD SSO。

1. 在 [Azure 门户](https://portal.azure.com/)的“EY GlobalOne”应用程序集成页上，找到“管理”部分，然后选择“单一登录”  。
1. 在“选择单一登录方法”页上选择“SAML” 。
1. 在“设置 SAML 单一登录”页上，单击“基本 SAML 配置”的编辑/笔形图标以编辑设置 。

   ![编辑基本 SAML 配置](common/edit-urls.png)

1. 在基本 SAML 配置部分，应用程序进行了预配置，且已通过 Azure 预填充了必要的 URL。 用户需要单击“保存”按钮来保存配置。

1. EY GlobalOne 应用程序需要特定格式的 SAML 断言，这就需要你向 SAML 令牌属性配置中添加自定义属性映射。 以下屏幕截图显示了默认属性的列表。 单击“编辑”图标以打开“用户属性”对话框。

    ![显示“用户属性”部分的屏幕截图，其中已选择“编辑”图标。](common/edit-attribute.png)

1. 除了上述属性，EY GlobalOne 应用程序还需要在 SAML 响应中传递回更多的属性。 在“用户属性”对话框的“用户声明”部分执行以下步骤，以便添加 SAML 令牌属性，如下表所示：

    | 名称 | 源属性|
    | ---------------| --------------- |
    | FirstName | user.givenname |
    | LastName | user.surname |
    | 电子邮件 | user.mail |
    | 公司 | `<YOUR COMPANY NAME>` |

    a. 单击“添加新声明”以打开“管理用户声明”对话框。

    ![显示“用户声明”部分的屏幕截图，其中突出显示了“添加新声明”和“保存”操作。](common/new-save-attribute.png)

    ![图像](common/new-attribute-details.png)

    b. 在“名称”文本框中，键入为该行显示的属性名称。

    c. 将“命名空间”留空。

    d. 选择“源”作为“属性”。

    e. 在“源属性”  列表中，键入为该行显示的属性值。

    f. 单击“确定”

    g. 单击“ **保存** ”。

1. 在“设置 SAML 单一登录”页的“SAML 签名证书”部分中，找到“证书(原始)”，选择“下载”以下载该证书并将其保存到计算机上   。

   ![证书下载链接](common/certificateraw.png)

1. 在“设置 EY GlobalOne”部分中，根据要求复制相应的 URL。

   ![复制配置 URL](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>创建 Azure AD 测试用户

在本部分中，将在 Azure 门户中创建一个名为 B. Simon 的测试用户。

1. 在 Azure 门户的左侧窗格中，依次选择“Azure Active Directory”、“用户”和“所有用户”  。
1. 选择屏幕顶部的“新建用户”。
1. 在“用户”属性中执行以下步骤：
   1. 在“名称”字段中，输入 `B. Simon`。  
   1. 在“用户名”字段中输入 username@companydomain.extension。 例如，`B. Simon@contoso.com`。
   1. 选中“显示密码”复选框，然后记下“密码”框中显示的值。
   1. 单击“创建”。

### <a name="assign-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，你将通过授予 B. Simon 访问 EY GlobalOne 的权限，允许其使用 Azure 单一登录。

1. 在 Azure 门户中，依次选择“企业应用程序”、“所有应用程序”。 
1. 在应用程序列表中，选择“EY GlobalOne”。
1. 在应用的概述页中，找到“管理”部分，选择“用户和组” 。

   ![“用户和组”链接](common/users-groups-blade.png)

1. 选择“添加用户”，然后在“添加分配”对话框中选择“用户和组”。

    ![“添加用户”链接](common/add-assign-user.png)

1. 在“用户和组”对话框中，选择“用户”列表中的“B. Simon”，然后单击屏幕底部的“选择”按钮  。
1. 如果在 SAML 断言中需要任何角色值，请在“选择角色”对话框的列表中为用户选择合适的角色，然后单击屏幕底部的“选择”按钮。
1. 在“添加分配”对话框中，单击“分配”按钮。

## <a name="configure-ey-globalone"></a>配置 EY GlobalOne

若要在 EY GlobalOne 端配置单一登录，需要将已下载的“证书(原始)”以及从 Azure 门户复制的相应 URL 发送给 [EY GlobalOne 支持团队](mailto:globalone.support@ey.com) 。 他们会对此进行设置，使两端的 SAML SSO 连接均正确设置。

### <a name="create-ey-globalone-test-user"></a>创建 EY GlobalOne 测试用户

在本部分中，我们将在 EY GlobalOne 中创建一个名为 Britta Simon 的用户。 EY GlobalOne 支持实时用户预配，这项功在默认情况下处于启用状态。 此部分不存在任何操作项。 如果 EY GlobalOne 中尚不存在用户，那么在身份验证后会新建一个用户。

## <a name="test-sso"></a>测试 SSO

在访问面板中选择“EY GlobalOne”磁贴时，应当会自动登录到设置了 SSO 的 EY GlobalOne。 有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](../user-help/my-apps-portal-end-user-access.md)（访问面板简介）。

## <a name="additional-resources"></a>其他资源

- [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](./tutorial-list.md)

- [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)

- [什么是 Azure Active Directory 中的条件访问？](../conditional-access/overview.md)