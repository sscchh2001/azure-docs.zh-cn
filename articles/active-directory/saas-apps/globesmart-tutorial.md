---
title: Tutorial:Azure Active Directory 与 GlobeSmart 的单一登录 (SSO) 集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 与 GlobeSmart 之间配置单一登录。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/05/2021
ms.author: jeedes
ms.openlocfilehash: 02128e4332191e325afa3a4f0d6f0471d5637ef5
ms.sourcegitcommit: 59cfed657839f41c36ccdf7dc2bee4535c920dd4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/06/2021
ms.locfileid: "99628087"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-globesmart"></a>Tutorial:Azure Active Directory 单一登录 (SSO) 与 GlobeSmart 的集成

本教程介绍如何将 GlobeSmart 与 Azure Active Directory (Azure AD) 集成。 将 GlobeSmart 与 Azure AD 集成后，你可以：

* 在 Azure AD 中控制谁有权访问 GlobeSmart。
* 让用户能够使用其 Azure AD 帐户自动登录到 GlobeSmart。
* 在一个中心位置（Azure 门户）管理帐户。

## <a name="prerequisites"></a>先决条件

若要开始操作，需备齐以下项目：

* 一个 Azure AD 订阅。 如果没有订阅，可以获取一个[免费帐户](https://azure.microsoft.com/free/)。
* 启用了单一登录 (SSO) 的 GlobeSmart 订阅。

## <a name="scenario-description"></a>方案描述

本教程在测试环境中配置并测试 Azure AD SSO。

* GlobeSmart 支持 SP 和 IDP 发起的 SSO
* GlobeSmart 支持实时用户预配

## <a name="adding-globesmart-from-the-gallery"></a>从库中添加 GlobeSmart

若要配置 GlobeSmart 与 Azure AD 的集成，需要从库中将 GlobeSmart 添加到托管 SaaS 应用列表。

1. 使用工作或学校帐户或个人 Microsoft 帐户登录到 Azure 门户。
1. 在左侧导航窗格中，选择“Azure Active Directory”服务  。
1. 导航到“企业应用程序”，选择“所有应用程序”   。
1. 若要添加新的应用程序，请选择“新建应用程序”  。
1. 在“从库中添加”部分的搜索框中，键入 **GlobeSmart**。
1. 从结果面板中选择“GlobeSmart”，然后添加该应用。 在该应用添加到租户时等待几秒钟。


## <a name="configure-and-test-azure-ad-sso-for-globesmart"></a>配置并测试 GlobeSmart 的 Azure AD SSO

使用名为 B.Simon 的测试用户配置并测试 GlobeSmart 的 Azure AD SSO。 若要使 SSO 正常工作，需要在 Azure AD 用户与 GlobeSmart 中的相关用户之间建立链接关系。

若要配置并测试 GlobeSmart 的 Azure AD SSO，请执行以下步骤：

1. **[配置 Azure AD SSO](#configure-azure-ad-sso)** - 使用户能够使用此功能。
    1. **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** - 使用 B. Simon 测试 Azure AD 单一登录。
    1. **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** - 使 B. Simon 能够使用 Azure AD 单一登录。
1. [配置 GlobeSmart SSO](#configure-globesmart-sso) - 在应用程序端配置单一登录设置。
    1. [创建 GlobeSmart 测试用户](#create-globesmart-test-user) - 在 GlobeSmart 中拥有 B. Simon 的对应用户，并将其链接到该用户的 Azure AD 表示形式。
1. **[测试 SSO](#test-sso)** - 验证配置是否正常工作。

## <a name="configure-azure-ad-sso"></a>配置 Azure AD SSO

按照下列步骤在 Azure 门户中启用 Azure AD SSO。

1. 在 **Azure 门户** 中的“GlobeSmart”应用程序集成页上，找到“管理”部分并选择“单一登录” 。
1. 在“选择单一登录方法”页上选择“SAML” 。
1. 在“设置 SAML 单一登录”页面上，单击“基本 SAML 配置”旁边的铅笔图标以编辑设置 。

   ![编辑基本 SAML 配置](common/edit-urls.png)

1. 如果要在“IDP”发起的模式下配置应用程序，请在“基本 SAML 配置”部分中输入以下字段的值 ：

    a. 在“标识符”框中，使用以下模式键入值：

    | 环境  | 代码 |
    |-------------|----|
    | 沙盒 | `urn:auth0:aperianglobal-staging:<INSTANCE_NAME>`|
    | 生产 | `urn:auth0:aperianglobal-production:<INSTANCE_NAME>`|
    | | |


    b. 在“回复 URL”文本框中，使用以下模式键入 URL： 

    | 环境  | 代码 |
    |-------------|----|
    | 沙盒 | ` https://aperianglobal-staging.auth0.com/login/callback?connection=<INSTANCE_NAME>`|
    | 生产 | `https://auth.aperianglobal.com/login/callback?connection=<INSTANCE_NAME>`|
    | | |

1. 如果要在 SP  发起的模式下配置应用程序，请单击“设置其他 URL”  ，并执行以下步骤：

    在“登录 URL”文本框中，使用以下模式键入 URL： 

    | 环境  | 代码 |
    |-------------|----|
    | 沙盒 | ` https://staging.aperianglobal.com?sp=<INSTANCE_NAME>`|
    | 生产 | `https://globesmart.aperianglobal.com?sp=<INSTANCE_NAME>`|
    | | |

    > [!NOTE]
    > 这些不是实际值。 请使用实际的“标识符”、“回复 URL”和“登录 URL”更新这些值。 请联系 [GlobeSmart 客户端支持团队](mailto:support@aperianglobal.com)获取这些值。 还可以参考 Azure 门户中的“基本 SAML 配置”部分中显示的模式。

1. GlobeSmart 应用程序需要特定格式的 SAML 断言，这要求你将自定义属性映射添加到 SAML 令牌属性配置。 以下屏幕截图显示了默认属性的列表。

    ![image](common/default-attributes.png)

1. 除了上述属性，GlobeSmart 应用程序还要求在 SAML 响应中传递回更多的属性，如下所示。 这些属性也是预先填充的，但可以根据要求查看它们。
    
    | 名称 |  源属性|
    | ---------------- | --------- |
    | firstName | user.givenname |
    | lastName | user.surname |
    | user_id | user.userprincipalname |
    | 电子邮件 | user.mail |

1. 在“使用 SAML 设置单一登录”页的“SAML 签名证书”部分中找到“联合元数据 XML”，选择“下载”以下载该证书并将其保存在计算机上     。

    ![证书下载链接](common/metadataxml.png)

1. 在“设置 GlobeSmart”部分中，根据你的需求复制相应的 URL。

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

在本部分中，你将通过授予 B. Simon 访问 GlobeSmart 的权限，允许其使用 Azure 单一登录。

1. 在 Azure 门户中，依次选择“企业应用程序”、“所有应用程序”。 
1. 在应用程序列表中，选择“GlobeSmart”。
1. 在应用的概述页中，找到“管理”部分，选择“用户和组” 。
1. 选择“添加用户”，然后在“添加分配”对话框中选择“用户和组”。
1. 在“用户和组”对话框中，从“用户”列表中选择“B.Simon”，然后单击屏幕底部的“选择”按钮。
1. 如果你希望将某角色分配给用户，可以从“选择角色”下拉列表中选择该角色。 如果尚未为此应用设置任何角色，你将看到选择了“默认访问权限”角色。
1. 在“添加分配”对话框中，单击“分配”按钮。

## <a name="configure-globesmart-sso"></a>配置 GlobeSmart SSO

若要在 GlobeSmart 端配置单一登录，需要将从 Azure 门户下载的“联合元数据 XML”以及从 Azure 门户复制的相应 URL 发送给 [Peoplecart 支持团队](mailto:support@aperianglobal.com)。 他们会对此进行设置，使两端的 SAML SSO 连接均正确设置。

### <a name="create-globesmart-test-user"></a>创建 GlobeSmart 测试用户

在本部分中，将在 GlobeSmart 中创建一个名为 Britta Simon 的用户。 GlobeSmart 支持实时用户预配（在默认情况下处于启用状态）。 此部分不存在任何操作项。 在进行身份验证后会在 GlobeSmart 中创建一个新用户（如果 GlobeSmart 中尚不存在该用户）。

## <a name="test-sso"></a>测试 SSO 

在本部分，你将使用以下选项测试 Azure AD 单一登录配置。 

#### <a name="sp-initiated"></a>SP 启动的：

* 在 Azure 门户中单击“测试此应用程序”。 这会重定向到可用于启动登录流的 GlobeSmart 登录 URL。  

* 直接访问 GlobeSmart 登录 URL，并从那里启动登录流。

#### <a name="idp-initiated"></a>IDP 启动的：

* 在 Azure 门户中单击“测试此应用程序”后，你将自动登录到为其设置了 SSO 的 GlobeSmart 

还可以使用 Microsoft“我的应用”在任何模式下测试此应用程序。 在“我的应用”中单击 GlobeSmart 磁贴时，如果是在 SP 模式下配置的，你会重定向到应用程序登录页来启动登录流；如果是在 IDP 模式下配置的，你将会自动登录到为其设置了 SSO 的 GlobeSmart。 有关“我的应用”的详细信息，请参阅[“我的应用”简介](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)。


## <a name="next-steps"></a>后续步骤

配置 GlobeSmart 后，可以强制实施会话控制，从而实时防止组织的敏感数据遭到外泄和渗透。 会话控制从条件访问扩展而来。 [了解如何通过 Microsoft Cloud App Security 强制实施会话控制](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)。


