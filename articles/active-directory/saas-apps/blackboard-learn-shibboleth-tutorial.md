---
title: 教程：Azure Active Directory 与 Blackboard Learn - Shibboleth 的集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 Blackboard Learn - Shibboleth 之间配置单一登录。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/19/2021
ms.author: jeedes
ms.openlocfilehash: 0355c8dc682ed45865a65d59b8b4810a85d588fa
ms.sourcegitcommit: a0c1d0d0906585f5fdb2aaabe6f202acf2e22cfc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/21/2021
ms.locfileid: "98621192"
---
# <a name="tutorial-azure-active-directory-integration-with-blackboard-learn---shibboleth"></a>教程：Azure Active Directory 与 Blackboard Learn - Shibboleth 的集成

本教程介绍如何将 Blackboard Learn - Shibboleth 与 Azure Active Directory (Azure AD) 集成。 将 Blackboard Learn - Shibboleth 与 Azure AD 集成后，可以：

* 在 Azure AD 中控制谁有权访问 Blackboard Learn - Shibboleth。
* 让用户使用其 Azure AD 帐户自动登录到 Blackboard Learn - Shibboleth。
* 在一个中心位置（Azure 门户）管理帐户。

## <a name="prerequisites"></a>先决条件

若要开始操作，需备齐以下项目：
 
* 一个 Azure AD 订阅。 如果没有订阅，可以获取一个[免费帐户](https://azure.microsoft.com/free/)。
* 启用了 Blackboard Learn - Shibboleth 单一登录 (SSO) 的订阅。

## <a name="scenario-description"></a>方案描述

本教程会在测试环境中配置和测试 Azure AD 单一登录。

* Blackboard Learn - Shibboleth 支持 **SP** 发起的 SSO

## <a name="add-blackboard-learn---shibboleth-from-the-gallery"></a>从库添加 Blackboard Learn - Shibboleth

要配置 Blackboard Learn - Shibboleth 到 Azure AD 的集成，需要从库将 Blackboard Learn - Shibboleth 添加到托管 SaaS 应用列表。

1. 使用工作或学校帐户或个人 Microsoft 帐户登录到 Azure 门户。
1. 在左侧导航窗格中，选择“Azure Active Directory”服务  。
1. 导航到“企业应用程序”，选择“所有应用程序”   。
1. 若要添加新的应用程序，请选择“新建应用程序”  。
1. 在“从库中添加”部分的搜索框中，键入“Blackboard Learn - Shibboleth” 。
1. 在结果面板中选择“Blackboard Learn - Shibboleth”，然后添加该应用。 在该应用添加到租户时等待几秒钟。

## <a name="configure-and-test-azure-ad-sso-for-blackboard-learn---shibboleth"></a>配置并测试 Blackboard Learn - Shibboleth 的 Azure AD SSO

使用名为 B.Simon 的测试用户配置并测试 Blackboard Learn - Shibboleth 的 Azure AD SSO。 若要使 SSO 正常工作，需要在 Azure AD 用户与 Blackboard Learn - Shibboleth 中的相关用户之间建立链接关系。

若要配置并测试 Blackboard Learn - Shibboleth 的 Azure AD SSO，请执行以下步骤：

1. **[配置 Azure AD SSO](#configure-azure-ad-sso)** - 使用户能够使用此功能。
    1. **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** - 使用 B. Simon 测试 Azure AD 单一登录。
    1. **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** - 使 B. Simon 能够使用 Azure AD 单一登录。
1. **[配置 Blackboard Learn - Shibboleth SSO](#configure-blackboard-learn---shibboleth-sso)** - 在应用程序端配置单一登录设置。
    1. **[创建 Blackboard Learn - Shibboleth 测试用户](#create-blackboard-learn---shibboleth-test-user)** - 在 Blackboard Learn - Shibboleth 中创建 B.Simon 的对应用户，并将其关联到其在 Azure AD 中的表示形式。
1. **[测试 SSO](#test-sso)** - 验证配置是否正常工作。

### <a name="configure-azure-ad-sso"></a>配置 Azure AD SSO

在本部分中，将在 Azure 门户中启用 Azure AD 单一登录。

若要配置 Blackboard Learn - Shibboleth 的 Azure AD 单一登录，请执行以下步骤：

1. 在 Azure 门户中的 Blackboard Learn - Shibboleth 应用程序集成页上，选择“单一登录” 。

2. 在 **选择单一登录方法** 对话框中，选择 **SAML/WS-Fed** 模式以启用单一登录。

3. 在“使用 SAML 设置单一登录”页面上，单击“铅笔”图标以打开“基本 SAML 配置”对话框 。

    ![编辑基本 SAML 配置](common/edit-urls.png)

4. 在“基本 SAML 配置”  部分中，按照以下步骤操作：

    a. 在“登录 URL”  文本框中，使用以下模式键入 URL：`https://<yourblackoardlearnserver>.blackboardlearn.com/Shibboleth.sso/Login`。

    b. 在“标识符”框中，使用以下模式键入 URL：`https://<yourblackoardlearnserver>.blackboardlearn.com/shibboleth-sp`

    c. 在“回复 URL”文本框中，使用以下模式键入 URL：`https://<yourblackoardlearnserver>.blackboardlearn.com/Shibboleth.sso/SAML2/POST`

    > [!NOTE]
    > 这些不是实际值。 请使用实际登录 URL、标识符和回复 URL 更新这些值。 若要获取这些值，请与 [Blackboard Learn - Shibboleth 客户端支持团队](https://www.blackboard.com/forms/contact-us_form.aspx)联系。 还可以参考 Azure 门户中的“基本 SAML 配置”  部分中显示的模式。

5. 在“使用 SAML 设置单一登录”页的“SAML 签名证书”部分，单击“下载”以根据要求下载从给定选项提供的“联合元数据 XML”并将其保存在计算机上     。

    ![证书下载链接](common/metadataxml.png)

6. 在“设置 Blackboard Learn - Shibboleth”部分，根据要求复制相应的 URL  。

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

在本部分，将通过授予 B.Simon 访问 Blackboard Learn - Shibboleth 的权限，使其能够使用 Azure 单一登录。

1. 在 Azure 门户中，依次选择“企业应用程序”、“所有应用程序”。  
1. 在应用程序列表中，选择“Blackboard Learn - Shibboleth”  。
1. 在应用的概述页中，找到“管理”部分，选择“用户和组”   。
1. 选择“添加用户”，然后在“添加分配”对话框中选择“用户和组”。
1. 在“用户和组”对话框中，从“用户”列表中选择“B.Simon”，然后单击屏幕底部的“选择”按钮。
1. 如果你希望将某角色分配给用户，可以从“选择角色”下拉列表中选择该角色。 如果尚未为此应用设置任何角色，你将看到选择了“默认访问权限”角色。
1. 在“添加分配”对话框中，单击“分配”按钮。

### <a name="configure-blackboard-learn---shibboleth-sso"></a>配置 Blackboard Learn - Shibboleth SSO

若要在 **Blackboard Learn - Shibboleth** 端配置单一登录，需要将下载的“联合元数据 XML”以及从 Azure 门户复制的相应 URL 发送给 [Blackboard Learn - Shibboleth 支持团队](https://www.blackboard.com/forms/contact-us_form.aspx)。 他们会对此进行设置，使两端的 SAML SSO 连接均正确设置。

### <a name="create-blackboard-learn---shibboleth-test-user"></a>创建 Blackboard Learn - Shibboleth 测试用户

在本部分中，在 Blackboard Learn - Shibboleth 中创建名为 Britta Simon 的用户。 与 [Blackboard Learn - Shibboleth 支持团队](https://www.blackboard.com/forms/contact-us_form.aspx)协作，在 Blackboard Learn - Shibboleth 平台中添加用户。 使用单一登录前，必须先创建并激活用户。

### <a name="test-sso"></a>测试 SSO

在本部分，你将使用以下选项测试 Azure AD 单一登录配置。 

* 在 Azure 门户中单击“测试此应用程序”。 这会重定向到 Blackboard Learn - Shibboleth 登录 URL，可在其中启动登录流。 

* 直接转到 Blackboard Learn - Shibboleth 登录 URL，并从那里启动登录流。

* 你可使用 Microsoft 的“我的应用”。 在“我的应用”中单击“Blackboard Learn - Shibboleth”磁贴时，应会自动登录到设置了 SSO 的 Blackboard Learn - Shibboleth。 有关“我的应用”的详细信息，请参阅[“我的应用”简介](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)。

## <a name="next-steps"></a>后续步骤

配置 Blackboard Learn - Shibboleth 后，可以强制实施会话控制，实时防止组织的敏感数据外泄和渗透。 会话控制从条件访问扩展而来。 [了解如何通过 Microsoft Cloud App Security 强制实施会话控制](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)。
