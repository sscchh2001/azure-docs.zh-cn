---
title: 教程：Azure Active Directory 与 Vtiger CRM (SAML) 的集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 Vtiger CRM (SAML) 之间配置单一登录。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 06/20/2019
ms.author: jeedes
ms.openlocfilehash: 93214eca847ba7233a92ea146d03c2966f24fa8b
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2020
ms.locfileid: "92519276"
---
# <a name="tutorial-integrate-vtiger-crm-saml-with-azure-active-directory"></a>教程：将 Vtiger CRM (SAML) 与 Azure Active Directory 集成

本教程介绍如何将 Vtiger CRM (SAML) 与 Azure Active Directory (Azure AD) 集成。 将 Vtiger CRM (SAML) 与 Azure AD 集成后，可以：

* 可以在 Azure AD 中控制谁有权访问 Vtiger CRM (SAML)。
* 可让用户使用其 Azure AD 帐户自动登录到 Vtiger CRM (SAML)。
* 在一个中心位置（Azure 门户）管理帐户。

若要了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要开始操作，需备齐以下项目：

* 一个 Azure AD 订阅。 如果没有订阅，可以从[此处](https://azure.microsoft.com/pricing/free-trial/)获取一个月免费试用版。
* 已启用 Vtiger CRM (SAML) 单一登录 (SSO) 的订阅

## <a name="scenario-description"></a>方案描述

本教程在测试环境中配置并测试 Azure AD SSO。 

* Vtiger CRM (SAML) 支持 **SP** 发起的 SSO
* Vtiger CRM (SAML) 支持 **实时** 用户预配

## <a name="adding-vtiger-crm-saml-from-the-gallery"></a>从库中添加 Vtiger CRM (SAML)

要配置 Vtiger CRM (SAML) 与 Azure AD 的集成，需要从库中将 Vtiger CRM (SAML) 添加到托管 SaaS 应用列表。

1. 使用工作或学校帐户或个人 Microsoft 帐户登录到 [Azure 门户](https://portal.azure.com)。
1. 在左侧导航窗格中，选择“Azure Active Directory”服务  。
1. 导航到“企业应用程序”，选择“所有应用程序”   。
1. 若要添加新的应用程序，请选择“新建应用程序”  。
1. 在“从库中添加”部分的搜索框中，键入“Vtiger CRM (SAML)”   。
1. 从结果面板中选择“Vtiger CRM (SAML)”，然后添加该应用  。 在该应用添加到租户时等待几秒钟。

## <a name="configure-and-test-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录

使用名为 B.Simon 的测试用户配置和测试 Vtiger CRM (SAML) 的 Azure AD SSO  。 若要运行 SSO，需要在 Azure AD 用户与 Vtiger CRM (SAML) 相关用户之间建立链接关系。

若要配置和测试 Vtiger CRM (SAML) 的 Azure AD SSO，请完成以下构建基块：

1. **[配置 Azure AD SSO](#configure-azure-ad-sso)** - 使用户能够使用此功能。
2. **[配置 Vtiger CRM (SAML) SSO](#configure-vtiger-crm-saml-sso)** - 在应用程序端配置单一登录设置。
3. **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
4. **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** - 使 Britta Simon 能够使用 Azure AD 单一登录。
5. **[创建 Vtiger CRM (SAML) 测试用户](#create-vtiger-crm-saml-test-user)** - 在 Vtiger CRM (SAML) 中创建 Britta Simon 的对应用户，并将其关联到其在 Azure AD 中的表示形式。
6. **[测试 SSO](#test-sso)** - 验证配置是否正常工作。

### <a name="configure-azure-ad-sso"></a>配置 Azure AD SSO

按照下列步骤在 Azure 门户中启用 Azure AD SSO。

1. 在 [Azure 门户](https://portal.azure.com/)的“Vtiger CRM (SAML)”应用程序集成页上，找到“管理”部分，选择“单一登录”    。
1. 在“选择单一登录方法”页上选择“SAML”   。
1. 在“设置 SAML 单一登录”页上，单击“基本 SAML 配置”的编辑/笔形图标以编辑设置   。

   ![编辑基本 SAML 配置](common/edit-urls.png)

1. 在“基本 SAML 配置”页上，输入以下字段的值  ：

    a. 在“登录 URL”文本框中，使用以下模式键入 URL  ：

   - `https://<customer_instance>.od1.vtiger.com`
   - `https://<customer_instance>.od2.vtiger.com`
   - `https://<customer_instance>.od1.vtiger.ws`

    b. 在“标识符(实体 ID)”文本框中，使用以下模式键入 URL：`https://<customer_instance>.od1.vtiger.com/sso/saml?acs` 

    > [!NOTE]
    > 这些不是实际值。 使用实际登录 URL 和标识符更新这些值。 请联系 [Vtiger CRM (SAML) 客户端支持团队](mailto:support@vtiger.com)获取这些值。 还可以参考 Azure 门户中的“基本 SAML 配置”  部分中显示的模式。

1. 在“设置 SAML 单一登录”页的“SAML 签名证书”部分中，找到“证书(Base64)”，选择“下载”以下载该证书并将其保存到计算机上     。

   ![证书下载链接](common/certificatebase64.png)

1. 在“设置 Vtiger CRM (SAML)”部分，根据要求复制相应的 URL  。

   ![复制配置 URL](common/copy-configuration-urls.png)

### <a name="configure-vtiger-crm-saml-sso"></a>配置 Vtiger CRM (SAML) SSO

若要在“Vtiger CRM (SAML)”端配置单一登录，需要将下载的“证书 (Base64)”以及从 Azure 门户复制的相应 URL 发送给 [Vtiger CRM (SAML) 支持团队](mailto:support@vtiger.com)   。 他们会对此进行设置，使两端的 SAML SSO 连接均正确设置。

### <a name="create-an-azure-ad-test-user"></a>创建 Azure AD 测试用户

在本部分，我们将在 Azure 门户中创建名为 B.Simon 的测试用户。

1. 在 Azure 门户的左侧窗格中，依次选择“Azure Active Directory”、“用户”和“所有用户”    。
1. 选择屏幕顶部的“新建用户”  。
1. 在“用户”属性中执行以下步骤  ：
   1. 在“名称”  字段中，输入 `B.Simon`。  
   1. 在“用户名”字段中输入 username@companydomain.extension  。 例如，`B.Simon@contoso.com` 。
   1. 选中“显示密码”复选框，然后记下“密码”框中显示的值。  
   1. 单击“创建”。 

### <a name="assign-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分，我们通过授予 B.Simon 访问 Vtiger CRM (SAML) 的权限，使其能够使用 Azure 单一登录。

1. 在 Azure 门户中，依次选择“企业应用程序”、“所有应用程序”。  
1. 在应用程序列表中，选择“Vtiger CRM (SAML)”  。
1. 在应用的概述页中，找到“管理”部分，选择“用户和组”   。

   ![“用户和组”链接](common/users-groups-blade.png)

1. 选择“添加用户”，然后在“添加分配”对话框中选择“用户和组”。   

    ![“添加用户”链接](common/add-assign-user.png)

1. 在“用户和组”对话框中，从“用户”列表中选择“B.Simon”，然后单击屏幕底部的“选择”按钮。   
1. 如果在 SAML 断言中需要任何角色值，请在“选择角色”对话框的列表中为用户选择合适的角色，然后单击屏幕底部的“选择”按钮。  
1. 在“添加分配”对话框中，单击“分配”按钮。  

### <a name="create-vtiger-crm-saml-test-user"></a>创建 Vtiger CRM (SAML) 测试用户

在本部分中，我们将在 Vtiger CRM (SAML) 中创建一个名为 Britta Simon 的用户。 Vtiger CRM (SAML) 支持默认启用的实时用户预配。 此部分不存在任何操作项。 如果 Vtiger CRM (SAML) 中尚不存在用户，身份验证后会创建一个新用户。

### <a name="test-sso"></a>测试 SSO

在访问面板中选择“Vtiger CRM (SAML)”磁贴时，应会自动登录到设置了 SSO 的 Vtiger CRM (SAML)。 有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](../user-help/my-apps-portal-end-user-access.md)（访问面板简介）。

## <a name="additional-resources"></a>其他资源

- [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](./tutorial-list.md)

- [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)

- [什么是 Azure Active Directory 中的条件访问？](../conditional-access/overview.md)