---
title: 注册表身份验证选项
description: 专用 Azure 容器注册表的身份验证选项，包括使用 Azure Active Directory 标识、使用服务主体以及使用可选的管理凭据进行登录。
ms.topic: article
ms.date: 01/30/2020
ms.openlocfilehash: 5315c11e0f1e2c859384e3783ae4be5d709adb42
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92148558"
---
# <a name="authenticate-with-an-azure-container-registry"></a>使用 Azure 容器注册表进行身份验证

可通过几种方法使用 Azure 容器注册表进行身份验证，并且每种方法适用于一种或多种注册表使用方案。

建议的方法包括通过[个人登录](#individual-login-with-azure-ad)来直接向注册表进行身份验证，或者应用程序和容器业务流程协调程序可以通过使用 Azure Active Directory (Azure AD) [服务主体](#service-principal)执行无人参与或“无外设”身份验证。

## <a name="authentication-options"></a>身份验证选项

下表列出了可用的身份验证方法和典型方案。 有关详细信息，请参阅链接的内容。

| 方法                               | 如何进行身份验证                                           | 方案                                                            | Azure 基于角色的访问控制 (Azure RBAC)                             | 限制                                |
|---------------------------------------|-------------------------------------------------------|---------------------------------------------------------------------|----------------------------------|--------------------------------------------|
| [单个 AD 标识](#individual-login-with-azure-ad)                | Azure CLI 中的 `az acr login`                              | 开发人员、测试人员的交互式推送/拉取                                    | 是                              | AD 令牌必须每隔 3 小时续订一次     |
| [AD 服务主体](#service-principal)                  | `docker login`<br/><br/>Azure CLI 中的 `az acr login`<br/><br/> API 或工具中的注册表登录设置<br/><br/> [Kubernetes 拉取机密](container-registry-auth-kubernetes.md)                                           | 从 CI/CD 管道进行的无人参与推送<br/><br/> 到 Azure 或外部服务的无人参与拉取  | 是                              | 存储过程密码默认有效期为 1 年       |                                                           
| [与 AKS 集成](../aks/cluster-container-registry-integration.md?toc=/azure/container-registry/toc.json&bc=/azure/container-registry/breadcrumb/toc.json)                    | 创建或更新 AKS 群集时附加注册表  | 到 AKS 群集的无人参与拉取                                                  | 否，仅限拉取访问             | 仅适用于 AKS 群集            |
| [Azure 资源的托管标识](container-registry-authentication-managed-identity.md)  | `docker login`<br/><br/>Azure CLI 中的  `az acr login`                                        | 从 Azure CI/CD 管道进行的无人参与推送<br/><br/> 到 Azure 服务的无人参与拉取<br/><br/>   | 是                              | 仅从[支持 Azure 资源托管标识](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-managed-identities-for-azure-resources)的 Azure 服务使用              |
| [管理员用户](#admin-account)                            | `docker login`                                          | 各个开发人员或测试人员的交互式推送/拉取<br/><br/>从注册表到 Azure 应用服务或 Azure 容器实例的映像门户部署                      | 否，始终仅限拉取和推送访问  | 每个注册表一个帐户，不建议用于多个用户         |
| [存储库范围的访问令牌](container-registry-repository-scoped-permissions.md)               | `docker login`<br/><br/>Azure CLI 中的 `az acr login`   | 各个开发人员或测试人员对存储库的交互式推送/拉取<br/><br/> 各个系统或外部设备对存储库的无人参与推送/拉取                  | 是                              | 当前未与 AD 标识集成  |

## <a name="individual-login-with-azure-ad"></a>使用 Azure AD 进行单次登录

直接使用注册表时（例如向开发工作站拉取映像和从中将映像推送到创建的注册表），可使用单独的 Azure 标识进行身份验证。 用[az login](/cli/azure/reference-index#az-login)登录到[Azure CLI](/cli/azure/install-azure-cli) ，然后运行[az acr login](/cli/azure/acr#az-acr-login)命令：

```azurecli
az login
az acr login --name <acrName>
```

使用 `az acr login` 登录时，CLI 将使用执行 `az login` 时创建的令牌和注册表对会话进行无缝身份验证。 若要完成身份验证流，你的环境中必须已安装且正在运行 Docker CLI 和 Docker 守护程序。 `az acr login` 使用 Docker 客户端在 `docker.config` 文件中设置 Azure Active Directory 令牌。 以这种方式登录后，系统会缓存凭据，并且会话中的后续 `docker` 命令将不再需要用户名或密码。

> [!TIP]
> 当希望将 Docker 映像以外的项目（例如 [OCI 项目](container-registry-oci-artifacts.md)）推送或拉取到注册表时，还可以使用 `az acr login` 来对单个标识进行身份验证。  

对于注册表访问，`az acr login` 使用的令牌有效期为 3 小时  ，因此，建议在运行 `docker` 命令之前始终登录到注册表。 如果令牌过期，可以通过再次使用 `az acr login` 命令重新进行身份验证来刷新令牌。 

配合使用 `az acr login` 和 Azure 标识可提供 [Azure 基于角色的访问控制 (Azure RBAC)](../role-based-access-control/role-assignments-portal.md)。 在某些情况下，你可能要在 Azure AD 中使用自己的单个标识登录注册表，或使用特定 [Azure 角色和权限](container-registry-roles.md)配置其他 Azure 用户。 对于跨服务方案，或者若要在不想管理个人访问的情况下满足工作组或部署工作流的需求，还可以使用 [Azure 资源的托管标识](container-registry-authentication-managed-identity.md)进行登录。

### <a name="az-acr-login-with---expose-token"></a>使用 --expose-token 运行 az acr login

在某些情况下，当 Docker 守护程序未在环境中运行时，可能需要使用 `az acr login` 进行身份验证。 例如，你可能需要在 Azure Cloud Shell 中运行的 `az acr login` 脚本，该脚本提供 DOCKER CLI 但不运行 docker 后台程序。

对于此方案，请先使用 `--expose-token` 参数运行 `az acr login`。 此选项公开访问令牌，而不是通过 Docker CLI 登录。

```azurecli
az acr login --name <acrName> --expose-token
```

输出显示访问令牌（此处为缩写）：

```console
{
  "accessToken": "eyJhbGciOiJSUzI1NiIs[...]24V7wA",
  "loginServer": "myregistry.azurecr.io"
}
``` 

然后，运行 `docker login`，将 `00000000-0000-0000-0000-000000000000` 作为用户名进行传递并使用访问令牌作为密码：

```console
docker login myregistry.azurecr.io --username 00000000-0000-0000-0000-000000000000 --password eyJhbGciOiJSUzI1NiIs[...]24V7wA
```

## <a name="service-principal"></a>服务主体

如果为注册表分配了[服务主体](../active-directory/develop/app-objects-and-service-principals.md)，则应用程序或服务可以将其用于无外设身份验证。 服务主体允许对注册表进行 [Azure 基于角色的访问控制 (Azure RBAC)](../role-based-access-control/role-assignments-portal.md)，并且可以为注册表分配多个服务主体。 如果拥有多个服务主体，则可为不同应用程序定义不同的访问权限。

容器注册表的可用角色包括：

* **AcrPull**：拉取

* **AcrPush**：拉取和推送

* **所有者**：拉取、推送和为其他用户分配角色

有关完整的角色列表，请参阅 [Azure 容器注册表角色和权限](container-registry-roles.md)。

有关创建服务主体以使用 Azure 容器注册表进行身份验证的 CLI 脚本，以及更多指导，请参阅[使用服务主体进行 Azure 容器注册表身份验证](container-registry-auth-service-principal.md)。

## <a name="admin-account"></a>管理员帐户

每个容器注册表包含一个管理员用户帐户，此帐户默认禁用。 可以在 Azure 门户中或通过使用 Azure CLI 或其他 Azure 工具启用管理员用户并管理其凭据。 管理员帐户对注册表具有完全权限。

当前对于某些方案，需要管理员帐户才能将映像从容器注册表部署到特定 Azure 服务。 例如，在门户中将容器映像从注册表直接部署到 [Azure 容器实例](../container-instances/container-instances-using-azure-container-registry.md#deploy-with-azure-portal)或[用于容器的 Azure Web 应用](container-registry-tutorial-deploy-app.md)时，需要管理员帐户。

> [!IMPORTANT]
> 管理员帐户专门用于单个用户访问注册表，主要用于测试目的。 建议不要在多个用户之间共享管理帐户凭据。 对于使用管理员帐户进行身份验证的所有用户，他们都将显示为对注册表具有推送和拉取访问权限的单个用户。 更改或禁用此帐户会禁用使用凭据的所有用户的注册表访问权限。 建议用户和服务主体在无外设方案中使用单个标识。
>

管理员帐户有两个密码，这两个密码都可以再生成。 使用这两个密码，可以在再生成一个密码时使用另一个密码保持与注册表的连接。 如果管理员帐户已启用，可以在系统提示时将用户名和/或密码传递到 `docker login` 命令，以对注册表进行基本身份验证。 例如：

```
docker login myregistry.azurecr.io 
```

有关管理登录凭据的最佳做法，请参阅 [docker login](https://docs.docker.com/engine/reference/commandline/login/) 命令参考。

若要启用现有注册表的管理员用户，可以在 Azure CLI 中使用 [az acr update](/cli/azure/acr#az-acr-update) 命令的 `--admin-enabled` 参数：

```azurecli
az acr update -n <acrName> --admin-enabled true
```

可以在 Azure 门户中启用管理员用户，操作方法如下：导航你的注册表，在“设置”下选择“访问密钥”，然后在“管理员用户”下选择“启用”     。

![在 Azure 门户中启用管理员用户的用户界面][auth-portal-01]

## <a name="next-steps"></a>后续步骤

* [使用 Azure CLI 推送第一个映像](container-registry-get-started-azure-cli.md)

<!-- IMAGES -->
[auth-portal-01]: ./media/container-registry-authentication/auth-portal-01.png
