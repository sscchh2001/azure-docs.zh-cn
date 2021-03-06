---
title: 权利管理中的委托和角色 - Azure AD
description: 了解如何将访问管理从 IT 管理员委托给部门经理和项目经理，使他们能够自行管理访问权限。
services: active-directory
documentationCenter: ''
author: ajburnle
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 12/23/2020
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 204d83b96e3cbe26759d678126d8826d0b2e492e
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/17/2021
ms.locfileid: "100577839"
---
# <a name="delegation-and-roles-in-azure-ad-entitlement-management"></a>Azure AD 权利管理中的委托和角色

默认情况下，全局管理员与用户管理员可以创建和管理 Azure AD 权利管理的各个方面。 但是，充当这些角色的用户不一定全面了解在哪些情况下需要访问包。 通常，它是用户在各自的部门、团队或项目中的用户，他们知道他们正在与谁合作，使用哪些资源，以及有多长时间。 您可以向用户授予执行作业所需的最少权限，并避免创建冲突或不适当的访问权限，而不是向非管理员授予不受限制的权限。

此视频概述了如何将访问管理从 IT 管理员委托给非管理员用户。

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE3Lq00]

## <a name="delegate-example"></a>委托示例

为了了解如何在权利管理中委托访问管理，让我们从一个示例着手。 假设你的组织中有以下管理员和经理。

![从 IT 管理员委托给经理](./media/entitlement-management-delegate/delegate-admin-dept-managers.png)

作为 IT 管理员，Hana 在每个部门中都有联系人-在市场营销、运营中 Mamta、在财务中进行了标记，以及负责其部门的资源和业务关键内容的 Joe。

使用权限管理，你可以将访问管理委派给这些非管理员，因为他们是知道哪些用户需要访问权限的用户，以及多长时间以及哪些资源。 委托给非管理员可确保适当的人员为其部门管理访问权限。

Hana 可以采用下面这种方式将访问管理委托给市场营销部、财务部和法务部。

1. Hana 创建新的 Azure AD 安全组，并将 Mamta、Mark 和 Joe 添加为组的成员。

1. Hana 将该组添加到目录创建者角色。

    Mamta、Mark 和 Joe 现在可以为自己的部门创建目录、添加自己的部门所需的资源，并在目录中执行进一步的委托。 它们看不到彼此的目录。

1. Mamta 创建“市场营销”目录，它是资源的容器。

1. Mamta 向此目录添加她的市场营销部拥有的资源。

1. Mamta 可以将其部门的其他人员添加为此目录的目录所有者，这有助于共享目录管理责任。

1. Mamta 可以进一步将“市场营销”目录中访问包的创建和管理委托给市场营销部的项目经理。 她可以通过向他们分配访问包管理者角色来完成此操作。 访问包管理者可以创建和管理访问包。 

下图显示了包含市场营销部、财务部和法务部的资源的目录。 项目经理可以使用这些目录为他们的团队或项目创建访问包。

![权利管理委托示例](./media/entitlement-management-delegate/elm-delegate.png)

委托后，市场营销部的角色可能与下表类似。

| 用户 | 工作角色 | Azure AD 角色 | 权利管理角色 |
| --- | --- | --- | --- |
| Hana | IT 管理员 | 全局管理员或用户管理员 |  |
| Mamta | 市场营销部经理 | 用户 | 目录创建者和目录所有者 |
| Bob | 市场营销部主管 | 用户 | 目录所有者 |
| Jessica | 市场营销部项目经理 | 用户 | 访问包管理者 |

## <a name="entitlement-management-roles"></a>权利管理角色

权利管理具有以下特定的角色。

| 权利管理角色 | 描述 |
| --- | --- |
| 目录创建者 | 创建和管理目录。 通常是指不是全局管理员的 IT 管理员，或是资源集合的资源所有者。 创建目录的人员将自动成为该目录的第一个目录所有者，并可以添加更多目录所有者。 目录创建者无法管理或查看他们不拥有的目录，也无法将他们不拥有的资源添加到目录中。 如果目录创建者需要管理其他目录或添加他们不拥有的资源，那么他们可以请求成为该目录或资源的共同所有者。 |
| 目录所有者 | 编辑和管理现有目录。 通常是 IT 管理员或资源所有者，或者是目录所有者选择的用户。 |
| 访问包管理者 | 编辑和管理目录中的所有现有访问包。 |
| 访问包分配管理人员 | 编辑和管理所有现有访问包的分配。 |

而且，所选的审批者和访问包的请求者拥有权限，但他们不是角色。

| Right | 描述 |
| --- | --- |
| 审批者 | 由策略授权，以批准或拒绝访问包的请求，但不能更改访问包定义。 |
| 请求者 | 由访问包的策略授权，可以请求该访问包。 |

下表列出了授权管理角色可以执行的任务。

| 任务 | 管理员 | 目录创建者 | 目录所有者 | 访问包管理者 | 访问包分配管理人员 |
| --- | :---: | :---: | :---: | :---: | :---: |
| [委托给目录创建者](entitlement-management-delegate-catalog.md) | :heavy_check_mark: |  |  |  |  |
| [添加连接的组织](entitlement-management-organization.md) | :heavy_check_mark: |  |  |  |  |
| [“创建新目录”](entitlement-management-catalog-create.md) | :heavy_check_mark: | :heavy_check_mark: |  |  |  |
| [将资源添加到目录](entitlement-management-catalog-create.md#add-resources-to-a-catalog) | :heavy_check_mark: |  | :heavy_check_mark: |  |  |
| [添加目录所有者](entitlement-management-catalog-create.md#add-additional-catalog-owners) | :heavy_check_mark: |  | :heavy_check_mark: |  |  |
| [编辑目录](entitlement-management-catalog-create.md#edit-a-catalog) | :heavy_check_mark: |  | :heavy_check_mark: |  |  |
| [删除目录](entitlement-management-catalog-create.md#delete-a-catalog) | :heavy_check_mark: |  | :heavy_check_mark: |  |  |
| [委托给访问包管理者](entitlement-management-delegate-managers.md) | :heavy_check_mark: |  | :heavy_check_mark: |  |  |
| [删除访问包管理者](entitlement-management-delegate-managers.md#remove-an-access-package-manager) | :heavy_check_mark: |  | :heavy_check_mark: |  |  |
| [在目录中创建新的访问包](entitlement-management-access-package-create.md) | :heavy_check_mark: |  | :heavy_check_mark:  | :heavy_check_mark:  |  |
| [更改访问包中的资源角色](entitlement-management-access-package-resources.md) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [创建和编辑策略](entitlement-management-access-package-request-policy.md) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [直接将用户分配到访问包](entitlement-management-access-package-assignments.md#directly-assign-a-user) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  :heavy_check_mark: |
| [直接从访问包中删除用户](entitlement-management-access-package-assignments.md#remove-an-assignment) | :heavy_check_mark:  |  | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [查看谁有访问包的分配](entitlement-management-access-package-assignments.md#view-who-has-an-assignment) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  :heavy_check_mark: |
| [查看访问包的请求](entitlement-management-access-package-requests.md#view-requests) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:  |
| [查看请求的传递错误](entitlement-management-troubleshoot.md#view-a-requests-delivery-errors) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  :heavy_check_mark: |
| [重新处理请求](entitlement-management-troubleshoot.md#reprocess-a-request) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:  |
| [取消挂起的请求](entitlement-management-troubleshoot.md#cancel-a-pending-request) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [隐藏访问包](entitlement-management-access-package-edit.md#change-the-hidden-setting) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [删除访问包](entitlement-management-access-package-edit.md#delete-an-access-package) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |

## <a name="required-roles-to-add-resources-to-a-catalog"></a>将资源添加到目录所需的角色

全局管理员可以在目录中添加或删除任何组（云创建的安全组，或云创建的Microsoft 365 组）、应用程序或 SharePoint Online 站点。 用户管理员可以添加或删除目录中的任何组或应用程序，但配置为可分配给目录角色的组除外。 请注意，用户管理员可以管理目录中的访问包，其中包括配置为可分配给目录角色的组。  有关可分配角色的组的详细信息，请参阅 [在 Azure Active Directory 中创建可分配角色的组](../roles/groups-create-eligible.md)。

对于不是全局管理员或用户管理员的用户，若要将组、应用程序或 SharePoint Online 站点添加到目录，该用户必须 *同时* 拥有所需的 Azure AD 目录角色和目录所有者授权管理角色。 下表列出了将资源添加到目录所需的角色组合。 若要从目录中删除资源，必须具有相同的角色。

| Azure AD 目录角色 | 权利管理角色 | 可以添加安全组 | 可以添加 Microsoft 365 组 | 可以添加应用 | 可以添加 SharePoint Online 站点 |
| --- | :---: | :---: | :---: | :---: | :---: |
| [全局管理员](../roles/permissions-reference.md) | 不适用 |  :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [用户管理员](../roles/permissions-reference.md) | 不适用 |  :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Intune 管理员](../roles/permissions-reference.md) | 目录所有者 | :heavy_check_mark: | :heavy_check_mark: |  |  |
| [Exchange 管理员](../roles/permissions-reference.md) | 目录所有者 |  | :heavy_check_mark: |  |  |
| [Teams 服务管理员](../roles/permissions-reference.md) | 目录所有者 |  | :heavy_check_mark: |  |  |
| [SharePoint 管理员](../roles/permissions-reference.md) | 目录所有者 |  | :heavy_check_mark: |  | :heavy_check_mark: |
| [应用程序管理员](../roles/permissions-reference.md) | 目录所有者 |  |  | :heavy_check_mark: |  |
| [云应用程序管理员](../roles/permissions-reference.md) | 目录所有者 |  |  | :heavy_check_mark: |  |
| 用户 | 目录所有者 | 仅限组所有者 | 仅限组所有者 | 仅限应用所有者 |  |

若要确定任务的最小特权角色，还可以参考[按 Azure Active Directory 中的管理员任务划分的管理员角色](../roles/delegate-by-task.md#entitlement-management)。

## <a name="next-steps"></a>后续步骤

- [将访问管理委托给目录创建者](entitlement-management-delegate-catalog.md)
- [创建和管理资源的目录](entitlement-management-catalog-create.md)
