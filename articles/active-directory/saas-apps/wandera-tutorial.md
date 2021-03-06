---
title: 教程：Azure Active Directory 与 Wandera RADAR Admin 的集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 Wandera RADAR Admin 之间配置单一登录。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/27/2020
ms.author: jeedes
ms.openlocfilehash: d13619b818e18c64d9882f9e3181824173403859
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96181384"
---
# <a name="tutorial-integrate-wandera-radar-admin-with-azure-active-directory"></a>教程：将 Wandera RADAR Admin 与 Azure Active Directory 集成

本教程介绍如何将 Wandera RADAR Admin 与 Azure Active Directory (Azure AD) 集成。 将 Wandera RADAR Admin 与 Azure AD 集成后，可以：

* 在 Azure AD 中控制谁有权访问 Wandera RADAR Admin。
* 让用户使用其 Azure AD 帐户自动登录到 Wandera RADAR Admin。
* 在一个中心位置（Azure 门户）管理帐户。

若要了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要开始操作，需备齐以下项目：

* 一个 Azure AD 订阅。 如果没有订阅，可以获取一个[免费帐户](https://azure.microsoft.com/free/)。
* 启用了 Wandera RADAR Admin 单一登录 (SSO) 的订阅。

## <a name="scenario-description"></a>方案描述

本教程在测试环境中配置并测试 Azure AD SSO。

* Wandera RADAR Admin 支持 IDP 发起的 SSO
* 配置 Wandera RADAR Admin 后，可以强制实施会话控制，实时防止组织的敏感数据外泄和渗透。 会话控制从条件访问扩展而来。 [了解如何通过 Microsoft Cloud App Security 强制实施会话控制](/cloud-app-security/proxy-deployment-any-app)。

## <a name="adding-wandera-radar-admin-from-the-gallery"></a>从库中添加 Wandera RADAR Admin

若要配置 Wandera RADAR Admin 与 Azure AD 的集成，需要从库中将 Wandera RADAR Admin 添加到托管 SaaS 应用列表。

1. 使用工作或学校帐户或个人 Microsoft 帐户登录到 [Azure 门户](https://portal.azure.com)。
1. 在左侧导航窗格中，选择“Azure Active Directory”服务  。
1. 导航到“企业应用程序”，选择“所有应用程序”   。
1. 若要添加新的应用程序，请选择“新建应用程序”  。
1. 在“从库中添加”部分的搜索框中，键入“Wandera RADAR Admin”。
1. 从结果面板中选择“Wandera RADAR Admin”，然后添加该应用。 在该应用添加到租户时等待几秒钟。


## <a name="configure-and-test-azure-ad-sso"></a>配置和测试 Azure AD SSO

使用名为 B.Simon 的测试用户配置并测试 Wandera RADAR Admin 的 Azure AD SSO。 若要使 SSO 有效，需要在 Azure AD 用户与 Wandera RADAR Admin 中的相关用户之间建立关联。

若要配置并测试 Wandera RADAR Admin 的 Azure AD SSO，请完成以下构建基块：

1. **[配置 Azure AD SSO](#configure-azure-ad-sso)** - 使用户能够使用此功能。
   * **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** - 使用 B. Simon 测试 Azure AD 单一登录。
   * **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** - 使 B. Simon 能够使用 Azure AD 单一登录。
1. **[配置 Wandera RADAR Admin SSO](#configure-wandera-radar-admin-sso)** - 在应用程序端配置单一登录设置。
   * **[创建 Wandera RADAR Admin 测试用户](#create-wandera-radar-admin-test-user)** - 在 Wandera RADAR Admin 中创建 B.Simon 的对应用户，该用户与 Azure AD 中表示 B.Simon 的用户相关联。
1. **[测试 SSO](#test-sso)** - 验证配置是否正常工作。

### <a name="configure-azure-ad-sso"></a>配置 Azure AD SSO

按照下列步骤在 Azure 门户中启用 Azure AD SSO。

1. 在 [Azure 门户](https://portal.azure.com/)的“Wandera RADAR Admin”应用程序集成页上，找到“管理”部分并选择“单一登录”  。
1. 在“选择单一登录方法”页上选择“SAML” 。
1. 在“设置 SAML 单一登录”页上，单击“基本 SAML 配置”的编辑/笔形图标以编辑设置 。

   ![编辑基本 SAML 配置](common/edit-urls.png)

1. 在“基本 SAML 配置”部分，输入以下字段的值：

    在“回复 URL”文本框中，使用以下模式键入 URL：`https://radar.wandera.com/saml/acs/<tenant id>`

    > [!NOTE]
    > 此值不是真实值。 请使用实际回复 URL 更新此值。 请联系 [Wandera RADAR Admin客户端支持团队](https://www.wandera.com/about-wandera/contact/#supportsection)来获取此值。 还可以参考 Azure 门户中的“基本 SAML 配置”部分中显示的模式。

1. 在“使用 SAML 设置单一登录”页的“SAML 签名证书”部分中找到“联合元数据 XML”，选择“下载”以下载该证书并将其保存在计算机上     。

    ![证书下载链接](common/metadataxml.png)

1. 在“设置 SAML 单一登录”页上，单击与“SAML 签名证书”对应的编辑/笔形图标以编辑设置。

    ![签名选项](common/signing-option.png)

    1. 选择“签名选项”作为“签名 SAML 响应和断言”。

    1. 将“SHA-256”选为“签名算法”。

1. 在“设置 Wandera RADAR Admin”部分中，根据要求复制相应的 URL。

    ![复制配置 URL](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>创建 Azure AD 测试用户

在本部分，我们将在 Azure 门户中创建名为 B.Simon 的测试用户。

1. 在 Azure 门户的左侧窗格中，依次选择“Azure Active Directory”、“用户”和“所有用户”  。
1. 选择屏幕顶部的“新建用户”。
1. 在“用户”属性中执行以下步骤：
   1. 在“名称”字段中，输入 `B.Simon`。  
   1. 在“用户名”字段中输入 username@companydomain.extension。 例如，`B.Simon@contoso.com`。
   1. 选中“显示密码”复选框，然后记下“密码”框中显示的值。
   1. 单击“创建”。

### <a name="assign-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，将通过授予 B.Simon 访问 Wandera RADAR Admin 的权限，允许其使用 Azure 单一登录。

1. 在 Azure 门户中，依次选择“企业应用程序”、“所有应用程序”。 
1. 在应用程序列表中，选择“Wandera RADAR Admin”。
1. 在应用的概述页中，找到“管理”部分，选择“用户和组” 。

   ![“用户和组”链接](common/users-groups-blade.png)

1. 选择“添加用户”，然后在“添加分配”对话框中选择“用户和组”。

    ![“添加用户”链接](common/add-assign-user.png)

1. 在“用户和组”对话框中，从“用户”列表中选择“B.Simon”，然后单击屏幕底部的“选择”按钮。
1. 如果在 SAML 断言中需要任何角色值，请在“选择角色”对话框的列表中为用户选择合适的角色，然后单击屏幕底部的“选择”按钮。
1. 在“添加分配”对话框中，单击“分配”按钮。

## <a name="configure-wandera-radar-admin-sso"></a>配置 Wandera RADAR Admin SSO

1. 若要在 Wandera RADAR Admin 中自动执行配置，需要通过单击“安装扩展”来安装“我的应用安全登录浏览器扩展”。

    ![我的应用扩展](common/install-myappssecure-extension.png)

2. 将扩展添加到浏览器后，单击“设置 Wandera RADAR Admin”会定向到 Wandera RADAR Admin 应用程序。 在此处，请提供管理员凭据以登录到 Wandera RADAR Admin。浏览器扩展会自动配置应用程序，并自动执行步骤 3-4。

    ![设置配置](common/setup-sso.png)

3. 若要手动设置 Wandera RADAR Admin，请打开新的 Web 浏览器窗口，以管理员身份登录 Wandera RADAR Admin 公司站点，并执行以下步骤：

4. 在页面的右上角，单击“设置” > “管理” > “单一登录”，然后选中“启用 SAML 2.0”选项以执行以下步骤。

    ![Wandera RADAR Admin 配置](./media/wandera-tutorial/config01.png)

    a. 单击 **或手动输入必填字段**。

    b. 在“IdP 实体 ID”文本框中，粘贴从 Azure 门户复制的“Azure AD 标识符”值。

    c. 在记事本中打开“联合元数据 XML”，复制其内容并将其粘贴到“IdP 公共 X.509 证书”文本框中。

    d. 单击“ **保存**”。

### <a name="create-wandera-radar-admin-test-user"></a>创建 Wandera RADAR Admin 测试用户

在本部分中，你将在 Wandera RADAR Admin 中创建名为 B.Simon 的用户。请与 [Wandera RADAR Admin 支持团队](https://www.wandera.com/about-wandera/contact/#supportsection)协作，将用户添加到 Wandera RADAR Admin 平台。 使用单一登录前，必须先创建并激活用户。

## <a name="test-sso"></a>测试 SSO

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

单击访问面板中的 Wandera RADAR Admin 磁贴时，应当会自动登录到设置了 SSO 的 Wandera RADAR Admin。 有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](../user-help/my-apps-portal-end-user-access.md)（访问面板简介）。

## <a name="additional-resources"></a>其他资源

- [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](./tutorial-list.md)

- [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)

- [什么是 Azure Active Directory 中的条件访问？](../conditional-access/overview.md)