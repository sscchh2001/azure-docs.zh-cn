---
title: 教程：Azure Active Directory 与 Greenhouse 的集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 Greenhouse 之间配置单一登录。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 11/25/2020
ms.author: jeedes
ms.openlocfilehash: 77f72d6c63231f0854b58470f86c65ffc81c9775
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/23/2021
ms.locfileid: "98731914"
---
# <a name="tutorial-azure-active-directory-integration-with-greenhouse"></a>教程：Azure Active Directory 与 Greenhouse 集成

本教程将介绍如何将 Greenhouse 与 Azure Active Directory (Azure AD) 集成。 将 Greenhouse 与 Azure AD 集成后，你可以：

* 在 Azure AD 中控制谁有权访问 Greenhouse。
* 让用户使用其 Azure AD 帐户自动登录到 Greenhouse。
* 在一个中心位置（Azure 门户）管理帐户。

## <a name="prerequisites"></a>先决条件

若要开始操作，需备齐以下项目：

* 一个 Azure AD 订阅。 如果没有订阅，可以获取一个[免费帐户](https://azure.microsoft.com/free/)。
* 已启用 Greenhouse 单一登录 (SSO) 的订阅。

> [!NOTE] 
> 此集成也可以通过 Azure AD 美国国家云环境使用。 你可以在“Azure AD 美国国家云应用程序库”中找到此应用程序，并以与在公有云中相同的方式对其进行配置。 

## <a name="scenario-description"></a>方案描述

本教程在测试环境中配置并测试 Azure AD SSO。

* Greenhouse 支持 SP  发起的 SSO

## <a name="adding-greenhouse-from-the-gallery"></a>从库中添加 Greenhouse

若要配置 Greenhouse 与 Azure AD 的集成，需要从库中将 Greenhouse 添加到托管 SaaS 应用列表。

1. 使用工作或学校帐户或个人 Microsoft 帐户登录到 Azure 门户。
1. 在左侧导航窗格中，选择“Azure Active Directory”服务  。
1. 导航到“企业应用程序”，选择“所有应用程序”   。
1. 若要添加新的应用程序，请选择“新建应用程序”  。
1. 在“从库中添加”部分的搜索框中键入“Greenhouse” 。
1. 从结果面板中选择“Greenhouse”，然后添加该应用。 在该应用添加到租户时等待几秒钟。


## <a name="configure-and-test-azure-ad-sso-for-greenhouse"></a>配置并测试 Greenhouse 的 Azure AD SSO

使用名为 B.Simon 的测试用户配置并测试 Greenhouse 的 Azure AD SSO。 若要使 SSO 正常运行，需要在 Azure AD 用户与 Greenhouse 中的相关用户之间建立链接关系。

若要配置并测试 Greenhouse 的 Azure AD SSO，请执行以下步骤：

1. **[配置 Azure AD SSO](#configure-azure-ad-sso)** - 使用户能够使用此功能。
    1. **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
    1. **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** - 使 Britta Simon 能够使用 Azure AD 单一登录。
2. **[配置 Greenhouse SSO](#configure-greenhouse-sso)** - 在应用程序端配置单一登录设置。
    1. [创建 Greenhouse 测试用户](#create-greenhouse-test-user) - 在 Greenhouse 中创建 Britta Simon 的对应用户，并将其关联到用户的 Azure AD 表示形式。
3. **[测试 SSO](#test-sso)** - 验证配置是否正常工作。

## <a name="configure-azure-ad-sso"></a>配置 Azure AD SSO

按照下列步骤在 Azure 门户中启用 Azure AD SSO。

1. 在 Azure 门户中的 Greenhouse 应用程序集成页上，找到“管理”部分，并选择“单一登录”  。
1. 在“选择单一登录方法”页上选择“SAML” 。
1. 在“使用 SAML 设置单一登录”页上，单击“基本 SAML 配置”的编辑/笔形图标以编辑设置 。

    ![编辑基本 SAML 配置](common/edit-urls.png)

4. 在“基本 SAML 配置”  部分中，按照以下步骤操作：

    a. 在“登录 URL”文本框中，使用以下模式键入 URL：`https://<companyname>.greenhouse.io` 

    b. 在“标识符(实体 ID)”文本框中，使用以下模式键入 URL：`https://<companyname>.greenhouse.io`

    > [!NOTE]
    > 这些不是实际值。 使用实际登录 URL 和标识符更新这些值。 请联系 [Greenhouse 客户端支持团队](https://www.greenhouse.io/contact)获取这些值。 还可以参考 Azure 门户中的“基本 SAML 配置”部分中显示的模式。

4. 在“使用 SAML 设置单一登录”页的“SAML 签名证书”部分，单击“下载”以根据要求下载从给定选项提供的“联合元数据 XML”并将其保存在计算机上     。

    ![证书下载链接](common/metadataxml.png)

6. 在“设置 Greenhouse”部分中，根据要求复制相应的 URL。

    ![复制配置 URL](common/copy-configuration-urls.png)


### <a name="create-an-azure-ad-test-user"></a>创建 Azure AD 测试用户

在本部分，我们将在 Azure 门户中创建名为 B.Simon 的测试用户。

1. 在 Azure 门户的左侧窗格中，依次选择“Azure Active Directory”、“用户”和“所有用户”  。
1. 选择屏幕顶部的“新建用户”。
1. 在“用户”属性中执行以下步骤：
   1. 在“名称”字段中，输入 `B.Simon`。  
   1. 在“用户名”字段中输入 username@companydomain.extension。 例如，`B.Simon@contoso.com` 。
   1. 选中“显示密码”复选框，然后记下“密码”框中显示的值。
   1. 单击“创建”。

### <a name="assign-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分，你将授予 B.Simon 访问 Greenhouse 的权限，使其能够使用 Azure 单一登录。

1. 在 Azure 门户中，依次选择“企业应用程序”、“所有应用程序”。 
1. 在应用程序列表中，选择“Greenhouse”。
1. 在应用的概述页中，找到“管理”部分，选择“用户和组” 。
1. 选择“添加用户”，然后在“添加分配”对话框中选择“用户和组”。
1. 在“用户和组”对话框中，从“用户”列表中选择“B.Simon”，然后单击屏幕底部的“选择”按钮。
1. 如果你希望将某角色分配给用户，可以从“选择角色”下拉列表中选择该角色。 如果尚未为此应用设置任何角色，你将看到选择了“默认访问权限”角色。
1. 在“添加分配”对话框中，单击“分配”按钮。

## <a name="configure-greenhouse-sso"></a>配置 Greenhouse SSO

1. 在另一个 Web 浏览器窗口中，以管理员身份登录到 Greenhouse 网站。

1. 请转到“配置”>“开发人员中心”>“单一登录”。

    ![SSO 页面的屏幕截图](./media/greenhouse-tutorial/configure.png)

1. 在“单一登录”页面上执行以下步骤。

    ![SSO 配置页面的屏幕截图](./media/greenhouse-tutorial/sso-page.png)

    a. 复制“SSO 断言使用者 URL”值，并将它粘贴到 Azure 门户的“基本 SAML 配置”部分的“回复 URL”文本框中  。

    b. 在“实体 ID/颁发者”文本框中，粘贴从 Azure 门户复制的“Azure AD 标识符”值 。

    c. 在“单一登录 URL”文本框中，粘贴从 Azure 门户复制的“登录 URL”值 。

    d. 在记事本中打开从 Azure 门户下载的联合元数据 XML，并将内容粘贴到“IdP 证书指纹”文本框中 。

    e. 从下拉列表中选择“名称标识符格式”值。

    f. 单击“开始测试”。

    >[!NOTE]
    >或者，也可以单击“选择文件”选项上传“联合元数据 XML”文件。  

### <a name="create-greenhouse-test-user"></a>创建一个 Greenhouse 测试用户

要使 Azure AD 用户能够登录 Greenhouse，必须将其预配到 Greenhouse。 对于 Greenhouse，需要手动执行预配。

>[!NOTE]
>可以使用 Greenhouse 提供的任何其他 Greenhouse 用户帐户创建工具或 API 来预配 Azure AD 用户帐户。 

**若要预配用户帐户，请执行以下步骤：**

1. 以管理员身份登录到你的 **Greenhouse** 公司站点。

2. 请转到“配置”>“用户”>“新用户”
   
    ![用户](./media/greenhouse-tutorial/create-user-1.png "用户")

4. 在 **“添加新用户”** 部分中，执行以下步骤：
   
    ![添加新用户](./media/greenhouse-tutorial/create-user-2.png "添加新用户")

    a. 在 **“输入用户电子邮件”** 文本框中，输入要设置的有效 Azure Active Directory 帐户的电子邮件地址。

    b. 单击“ **保存**”。    
   
      >[!NOTE]
      >Azure Active Directory 帐户持有者将收到一封电子邮件，其中包含用于在激活帐户前确认帐户的链接。

### <a name="test-sso"></a>测试 SSO 

在本部分，你将使用以下选项测试 Azure AD 单一登录配置。 

* 在 Azure 门户中单击“测试此应用程序”。 这会重定向到 Greenhouse 登录 URL，可在其中启动登录流。 

* 直接转到 Greenhouse 登录 URL，并从那里启动登录流。

* 你可使用 Microsoft 的“我的应用”。 单击“我的应用”中的 Greenhouse 磁贴时，会重定向到 Greenhouse 登录 URL。 有关“我的应用”的详细信息，请参阅[“我的应用”简介](../user-help/my-apps-portal-end-user-access.md)。


## <a name="next-steps"></a>后续步骤

配置 Greenhouse 后，可以强制实施会话控制，实时防止组织的敏感数据发生外泄和渗透。 会话控制从条件访问扩展而来。 [了解如何通过 Microsoft Cloud App Security 强制实施会话控制](/cloud-app-security/proxy-deployment-any-app)。