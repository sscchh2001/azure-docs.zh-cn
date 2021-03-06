---
title: 向用户授予特定实验室策略的权限 |Microsoft Docs
description: 了解如何根据各个客户的需求在开发测试实验室中向用户授予特定实验室策略的权限
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 976862476d25e4e9a4933d8a5319eec9d77ca39b
ms.sourcegitcommit: 03713bf705301e7f567010714beb236e7c8cee6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/21/2020
ms.locfileid: "92328464"
---
# <a name="grant-user-permissions-to-specific-lab-policies"></a>向用户授予特定实验室策略的权限
## <a name="overview"></a>概述
本文说明了如何使用 PowerShell 向用户授予某个特定实验室策略的权限。 这样一来，可基于每个用户的需求应用权限。 例如，你可能想向某个特定用户授予更改 VM 策略设置的权限，但不想授予成本策略权限。

## <a name="policies-as-resources"></a>将策略作为资源
如 azure RBAC) 一文中的 [azure 基于角色的访问控制 (](../role-based-access-control/role-assignments-portal.md) 中所述，azure rbac 可为 azure 的资源提供精细的访问管理。 使用 Azure RBAC，可以在 DevOps 团队中对职责进行隔离，只向用户授予执行作业所需的访问权限。

在开发测试实验室中，策略是一种可启用 Azure RBAC 操作 **microsoft.devtestlab/实验室/policySets**/policy/的资源类型。 每个实验室策略都是策略资源类型中的资源，可以作为作用域分配给 Azure 角色。

例如，若要授予用户对 **允许的 VM 大小** 策略的读/写权限，你需要创建一个可与 **microsoft.devtestlab/实验室/policySets/policy/** 操作一起工作的自定义角色，然后将相应的用户分配到 **microsoft.devtestlab/实验室/policySets/policy/AllowedVmSizesInLab**范围内的此自定义角色。

若要详细了解 Azure RBAC 中的自定义角色，请参阅 [azure 自定义角色](../role-based-access-control/custom-roles.md)。

## <a name="creating-a-lab-custom-role-using-powershell"></a>使用 PowerShell 创建实验室自定义角色
若要开始，需要 [安装 Azure PowerShell](/powershell/azure/install-az-ps)。 

一旦设置了 Azure PowerShell cmdlet，即可执行以下任务：

* 列出资源提供程序的所有操作
* 列出特定角色中的操作：
* 创建自定义角色

下方 PowerShell 脚本举例说明如何了执行这些任务：

```azurepowershell
# List all the operations/actions for a resource provider.
Get-AzProviderOperation -OperationSearchString "Microsoft.DevTestLab/*"

# List actions in a particular role.
(Get-AzRoleDefinition "DevTest Labs User").Actions

# Create custom role.
$policyRoleDef = (Get-AzRoleDefinition "DevTest Labs User")
$policyRoleDef.Id = $null
$policyRoleDef.Name = "Policy Contributor"
$policyRoleDef.IsCustom = $true
$policyRoleDef.AssignableScopes.Clear()
$policyRoleDef.AssignableScopes.Add("/subscriptions/<SubscriptionID> ")
$policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/policySets/policies/*")
$policyRoleDef = (New-AzRoleDefinition -Role $policyRoleDef)
```

## <a name="assigning-permissions-to-a-user-for-a-specific-policy-using-custom-roles"></a>使用自定义角色将某特定策略的权限分配给用户
一旦定义了自定义角色，即可将其分配给用户。 为了将自定义角色分配给用户，首先必须获取代表用户的 **ObjectId**。 为此，请使用 **AzADUser** cmdlet。

在下方示例中，*SomeUser* 用户的 **ObjectId** 为 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3。

```azurepowershell
PS C:\>Get-AzADUser -SearchString "SomeUser"

DisplayName                    Type                           ObjectId
-----------                    ----                           --------
someuser@hotmail.com                                          05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3
```

获得用户的 **ObjectId** 和自定义角色名称后，可以使用 **AzRoleAssignment** cmdlet 将该角色分配给用户：

```azurepowershell
PS C:\>New-AzRoleAssignment -ObjectId 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 -RoleDefinitionName "Policy Contributor" -Scope /subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.DevTestLab/labs/<LabName>/policySets/default/policies/AllowedVmSizesInLab
```

在上一示例中，使用了 **AllowedVmSizesInLab** 策略。 可以使用下方任意策略：

* MaxVmsAllowedPerUser
* MaxVmsAllowedPerLab
* AllowedVmSizesInLab
* LabVmsShutdown

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>后续步骤
向用户分配特定实验室策略的权限后，需要考虑执行后续步骤：

* [保护对实验室的访问](devtest-lab-add-devtest-user.md)
* [设置实验室策略](devtest-lab-set-lab-policy.md)
* [创建实验室模板](devtest-lab-create-template.md)
* [为 Vm 创建自定义项目](devtest-lab-artifact-author.md)
* [将 VM 添加到实验室](devtest-lab-add-vm.md)
