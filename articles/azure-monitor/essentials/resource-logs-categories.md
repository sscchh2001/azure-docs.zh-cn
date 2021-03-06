---
title: Azure Monitor 资源日志支持的服务和类别
description: Azure Monitor 参考：了解 Azure 资源日志支持的服务和事件架构。
ms.subservice: logs
ms.topic: reference
ms.date: 01/29/2021
ms.openlocfilehash: 39ff78cd97682096fb284e137868c246dfdd7f14
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/17/2021
ms.locfileid: "100607840"
---
# <a name="supported-categories-for-azure-resource-logs"></a>Azure 资源日志支持的类别

> [!NOTE]
> 资源日志以前称为诊断日志。 此名称在 2019 年 10 月发生了更改，因为 Azure Monitor 收集的日志类型已转变，不仅仅包括 Azure 资源。

[Azure Monitor 资源日志](../essentials/platform-logs-overview.md)是 Azure 服务发出的日志，用于描述这些服务或资源的操作。 通过 Azure Monitor 提供的所有资源日志共享公共顶级架构，且每个服务都能灵活地为其事件发出唯一属性。

资源类型（为 `resourceId` 属性时可用）和 `category` 的组合唯一标识架构。 所有资源日志都有一个通用架构，其中包含特定于服务的字段，这些字段随后会针对不同的日志类别进行添加。 有关详细信息，请参阅 [Azure 资源日志的通用架构和特定于服务的架构]()


## <a name="costs"></a>成本

将任何数据发送和存储到 Log Analytics、Azure 存储和/或事件中心都会产生相关成本。 你可能需要为将数据获取到这些位置以及将数据保存在那里支付费用。  资源日志是可以发送到这些位置的数据类型之一。 

将某些类别的资源日志导出到这些位置需要额外的费用。 下表列出了具有导出成本的日志。 有关此定价的详细信息，请参阅 " [Azure Monitor 定价" 页](https://azure.microsoft.com/pricing/details/monitor/)中的 "平台日志" 部分。

## <a name="supported-log-categories-per-resource-type"></a>每种资源类型支持的日志类别

下面列出了可用于每种资源类型的日志类型。 

某些类别可能只适用于特定类型的资源。 如果你觉得缺少资源，请参阅特定于资源的文档。 例如，Microsoft.Sql/servers/databases 类别并非适用于所有类型的数据库。 有关详细信息，请参阅[有关 SQL 数据库诊断日志记录的信息](../../azure-sql/database/metrics-diagnostic-telemetry-logging-streaming-export-configure.md)。 

如果你认为缺少某些内容，你可以在本文底部打开 GitHub 注释。
## <a name="microsoftaaddomainservices"></a>Microsoft.AAD/domainServices

|Category|类别显示名称|导出成本|
|---|---|---|
|AccountLogon|AccountLogon|否|
|AccountManagement|AccountManagement|否|
|DetailTracking|DetailTracking|否|
|DirectoryServiceAccess|DirectoryServiceAccess|否|
|LogonLogoff|LogonLogoff|否|
|ObjectAccess|ObjectAccess|否|
|PolicyChange|PolicyChange|否|
|PrivilegeUse|PrivilegeUse|否|
|SystemSecurity|SystemSecurity|否|


## <a name="microsoftanalysisservicesservers"></a>Microsoft.AnalysisServices/servers

|Category|类别显示名称|导出成本|
|---|---|---|
|引擎|引擎|否|
|服务|服务|否|


## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

|Category|类别显示名称|导出成本|
|---|---|---|
|GatewayLogs|ApiManagement 网关的相关日志|否|


## <a name="microsoftappconfigurationconfigurationstores"></a>Microsoft.AppConfiguration/configurationStores

|Category|类别显示名称|导出成本|
|---|---|---|
|HttpRequest|HTTP 请求|是|


## <a name="microsoftappplatformspring"></a>Microsoft.AppPlatform/Spring

|Category|类别显示名称|导出成本|
|---|---|---|
|ApplicationConsole|应用程序控制台|否|
|SystemLogs|系统日志|否|


## <a name="microsoftattestationattestationproviders"></a>Microsoft.Attestation/attestationProviders

|Category|类别显示名称|导出成本|
|---|---|---|
|AuditEvent|AuditEvent 消息日志类别。|否|
|ERR|错误消息日志类别。|否|
|INF|信息性消息日志类别。|否|
|警告|警告消息日志类别。|否|


## <a name="microsoftautomationautomationaccounts"></a>Microsoft.Automation/automationAccounts

|Category|类别显示名称|导出成本|
|---|---|---|
|DscNodeStatus|Dsc 节点状态|否|
|JobLogs|作业日志|否|
|JobStreams|作业流|否|


## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Category|类别显示名称|导出成本|
|---|---|---|
|ServiceLog|服务日志|否|


## <a name="microsoftbatchaiworkspaces"></a>Microsoft.BatchAI/workspaces

|Category|类别显示名称|导出成本|
|---|---|---|
|BaiClusterEvent|BaiClusterEvent|否|
|BaiClusterNodeEvent|BaiClusterNodeEvent|否|
|BaiJobEvent|BaiJobEvent|否|


## <a name="microsoftblockchainblockchainmembers"></a>Microsoft.Blockchain/blockchainMembers

|Category|类别显示名称|导出成本|
|---|---|---|
|BlockchainApplication|区块链应用程序|否|
|FabricOrderer|Fabric Orderer|否|
|FabricPeer|Fabric Peer|否|
|代理|代理|否|


## <a name="microsoftblockchaincordamembers"></a>Microsoft.Blockchain/cordaMembers

|Category|类别显示名称|导出成本|
|---|---|---|
|BlockchainApplication|区块链应用程序|否|


## <a name="microsoftbotservicebotservices"></a>microsoft.botservice/botservices

|Category|类别显示名称|导出成本|
|---|---|---|
|BotRequest|从通道到机器人的请求|否|
|DependencyRequest|对依赖项的请求|否|


## <a name="microsoftcdncdnwebapplicationfirewallpolicies"></a>Microsoft.Cdn/cdnwebapplicationfirewallpolicies

|Category|类别显示名称|导出成本|
|---|---|---|
|WebApplicationFirewallLogs|Web 应用程序防火墙日志|否|


## <a name="microsoftcdnprofiles"></a>Microsoft.Cdn/profiles

|Category|类别显示名称|导出成本|
|---|---|---|
|AzureCdnAccessLog|Azure CDN 访问日志|否|
|FrontDoorAccessLog|FrontDoor 访问日志|是|
|FrontDoorHealthProbeLog|FrontDoor 运行状况探测日志|是|
|FrontDoorWebApplicationFirewallLog|FrontDoor WebApplicationFirewall 日志|是|


## <a name="microsoftcdnprofilesendpoints"></a>Microsoft.Cdn/profiles/endpoints

|Category|类别显示名称|导出成本|
|---|---|---|
|CoreAnalytics|获取终结点的指标，例如带宽、流出量等。|否|


## <a name="microsoftclassicnetworknetworksecuritygroups"></a>Microsoft.ClassicNetwork/networksecuritygroups

|Category|类别显示名称|导出成本|
|---|---|---|
|网络安全组规则流事件|网络安全组规则流事件|否|


## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Category|类别显示名称|导出成本|
|---|---|---|
|审核|审核日志|否|
|RequestResponse|请求和响应日志|否|
|跟踪|跟踪日志|否|


## <a name="microsoftcommunicationcommunicationservices"></a>Microsoft Communication/CommunicationServices

|Category|类别显示名称|导出成本|
|---|---|---|
|ChatOperational|操作聊天日志|否|
|SMSOperational|操作短信日志|否|
|使用情况|使用情况记录|否|


## <a name="microsoftcontainerregistryregistries"></a>Microsoft.ContainerRegistry/registries

|Category|类别显示名称|导出成本|
|---|---|---|
|ContainerRegistryLoginEvents|登录事件|否|
|ContainerRegistryRepositoryEvents|RepositoryEvent 日志|否|


## <a name="microsoftcontainerservicemanagedclusters"></a>Microsoft.ContainerService/managedClusters

|Category|类别显示名称|导出成本|
|---|---|---|
|cluster-autoscaler|Kubernetes 群集自动缩放程序|否|
|防护|防护|否|
|kube-apiserver|Kubernetes API 服务器|否|
|kube-audit|Kubernetes 审核|否|
|kube-审核-管理员|Kubernetes 审核管理日志|否|
|kube-controller-manager|Kubernetes 控制器管理器|否|
|kube-scheduler|Kubernetes 计划程序|否|


## <a name="microsoftcustomprovidersresourceproviders"></a>Microsoft.CustomProviders/resourceproviders

|Category|类别显示名称|导出成本|
|---|---|---|
|AuditLogs|MiniRP 调用的审核日志|否|


## <a name="microsoftd365customerinsightsinstances"></a>D365CustomerInsights/实例

|Category|类别显示名称|导出成本|
|---|---|---|
|审核|审核事件|否|
|可运行|操作事件|否|


## <a name="microsoftdatabricksworkspaces"></a>Microsoft.Databricks/workspaces

|Category|类别显示名称|导出成本|
|---|---|---|
|accounts|Databricks 帐户|否|
|clusters|Databricks 群集|否|
|dbfs|Databricks 文件系统|否|
|instancePools|实例池|否|
|jobs|Databricks 作业|否|
|笔记本|Databricks Notebook|否|
|secrets|Databricks 机密|否|
|sqlPermissions|Databricks SQLPermissions|否|
|ssh|Databricks SSH|否|
|工作区|Databricks 工作区|否|


## <a name="microsoftdatacollaborationworkspaces"></a>DataCollaboration/工作区

|Category|类别显示名称|导出成本|
|---|---|---|
|CollaborationAudit|协作审核|是|
|DataAssets|数据资产|否|
|管道|管道|否|
|建议|建议|否|
|脚本|脚本|否|


## <a name="microsoftdatafactoryfactories"></a>Microsoft.DataFactory/factories

|Category|类别显示名称|导出成本|
|---|---|---|
|ActivityRuns|管道活动运行日志|否|
|PipelineRuns|管道运行日志|否|
|SSISIntegrationRuntimeLogs|SSIS 集成运行时日志|否|
|SSISPackageEventMessageContext|SSIS 包事件消息上下文|否|
|SSISPackageEventMessages|SSIS 包事件消息|否|
|SSISPackageExecutableStatistics|SSIS 包可执行文件统计|否|
|SSISPackageExecutionComponentPhases|SSIS 包执行组件阶段|否|
|SSISPackageExecutionDataStatistics|SSIS 包执行数据统计信息|否|
|TriggerRuns|触发器运行日志|否|


## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.DataLakeAnalytics/accounts

|Category|类别显示名称|导出成本|
|---|---|---|
|审核|审核日志|否|
|请求|请求日志|否|


## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.DataLakeStore/accounts

|Category|类别显示名称|导出成本|
|---|---|---|
|审核|审核日志|否|
|请求|请求日志|否|


## <a name="microsoftdatashareaccounts"></a>Microsoft.DataShare/accounts

|Category|类别显示名称|导出成本|
|---|---|---|
|ReceivedShareSnapshots|已收到共享快照|否|
|SentShareSnapshots|已发送共享快照|否|
|共享|共享|否|
|ShareSubscriptions|共享订阅|否|


## <a name="microsoftdbformariadbservers"></a>Microsoft.DBforMariaDB/servers

|Category|类别显示名称|导出成本|
|---|---|---|
|MySqlAuditLogs|MariaDB 审核日志|否|
|MySqlSlowLogs|MariaDB 服务器日志|否|


## <a name="microsoftdbformysqlflexibleservers"></a>Microsoft.DBforMySQL/flexibleServers

|Category|类别显示名称|导出成本|
|---|---|---|
|MySqlAuditLogs|MySQL 审核日志|否|
|MySqlSlowLogs|MySQL 慢速日志|否|


## <a name="microsoftdbformysqlservers"></a>Microsoft.DBforMySQL/servers

|Category|类别显示名称|导出成本|
|---|---|---|
|MySqlAuditLogs|MySQL 审核日志|否|
|MySqlSlowLogs|MySQL 服务器日志|否|


## <a name="microsoftdbforpostgresqlflexibleservers"></a>Microsoft.DBforPostgreSQL/flexibleServers

|Category|类别显示名称|导出成本|
|---|---|---|
|PostgreSQLLogs|PostgreSQL 服务器日志|否|


## <a name="microsoftdbforpostgresqlservers"></a>Microsoft.DBforPostgreSQL/servers

|Category|类别显示名称|导出成本|
|---|---|---|
|PostgreSQLLogs|PostgreSQL 服务器日志|否|
|QueryStoreRuntimeStatistics|PostgreSQL 查询存储运行时统计信息|否|
|QueryStoreWaitStatistics|PostgreSQL 查询存储等待统计信息|否|


## <a name="microsoftdbforpostgresqlserversv2"></a>Microsoft.DBforPostgreSQL/serversv2

|Category|类别显示名称|导出成本|
|---|---|---|
|PostgreSQLLogs|PostgreSQL 服务器日志|否|


## <a name="microsoftdesktopvirtualizationapplicationgroups"></a>Microsoft.DesktopVirtualization/applicationgroups

|Category|类别显示名称|导出成本|
|---|---|---|
|检查点|检查点|否|
|错误|错误|否|
|管理|管理|否|


## <a name="microsoftdesktopvirtualizationhostpools"></a>Microsoft.DesktopVirtualization/hostpools

|Category|类别显示名称|导出成本|
|---|---|---|
|AgentHealthStatus|AgentHealthStatus|否|
|检查点|检查点|否|
|连接|连接|否|
|错误|错误|否|
|HostRegistration|HostRegistration|否|
|管理|管理|否|


## <a name="microsoftdesktopvirtualizationworkspaces"></a>Microsoft.DesktopVirtualization/workspaces

|Category|类别显示名称|导出成本|
|---|---|---|
|检查点|检查点|否|
|错误|错误|否|
|源|源|否|
|管理|管理|否|


## <a name="microsoftdeviceselasticpoolsiothubtenants"></a>Microsoft.Devices/ElasticPools/IotHubTenants

|Category|类别显示名称|导出成本|
|---|---|---|
|C2DCommands|C2D 命令|否|
|C2DTwinOperations|C2D 孪生操作|否|
|配置|配置|否|
|连接|连接|否|
|D2C 孪生操作|D2C 孪生操作|否|
|DeviceIdentityOperations|设备标识操作|否|
|DeviceStreams|设备流（预览版）|否|
|DeviceTelemetry|设备遥测|否|
|DirectMethods|直接方法|否|
|DistributedTracing|分布式跟踪（预览版）|否|
|FileUploadOperations|文件上传操作|否|
|JobsOperations|作业操作|否|
|路由|路由|否|
|TwinQueries|孪生查询|否|


## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs

|Category|类别显示名称|导出成本|
|---|---|---|
|C2DCommands|C2D 命令|否|
|C2DTwinOperations|C2D 孪生操作|否|
|配置|配置|否|
|连接|连接|否|
|D2C 孪生操作|D2C 孪生操作|否|
|DeviceIdentityOperations|设备标识操作|否|
|DeviceStreams|设备流（预览版）|否|
|DeviceTelemetry|设备遥测|否|
|DirectMethods|直接方法|否|
|DistributedTracing|分布式跟踪（预览版）|否|
|FileUploadOperations|文件上传操作|否|
|JobsOperations|作业操作|否|
|路由|路由|否|
|TwinQueries|孪生查询|否|


## <a name="microsoftdevicesprovisioningservices"></a>Microsoft.Devices/provisioningServices

|Category|类别显示名称|导出成本|
|---|---|---|
|DeviceOperations|设备操作|否|
|ServiceOperations|服务操作|否|


## <a name="microsoftdigitaltwinsdigitaltwinsinstances"></a>Microsoft.DigitalTwins/digitalTwinsInstances

|Category|类别显示名称|导出成本|
|---|---|---|
|DigitalTwinsOperation|DigitalTwinsOperation|否|
|EventRoutesOperation|EventRoutesOperation|否|
|ModelsOperation|ModelsOperation|否|
|QueryOperation|QueryOperation|否|


## <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.DocumentDB/databaseAccounts

|Category|类别显示名称|导出成本|
|---|---|---|
|CassandraRequests|CassandraRequests|否|
|ControlPlaneRequests|ControlPlaneRequests|否|
|DataPlaneRequests|DataPlaneRequests|否|
|GremlinRequests|GremlinRequests|否|
|MongoRequests|MongoRequests|否|
|PartitionKeyRUConsumption|PartitionKeyRUConsumption|否|
|PartitionKeyStatistics|PartitionKeyStatistics|否|
|QueryRuntimeStatistics|QueryRuntimeStatistics|否|


## <a name="microsofteventgriddomains"></a>Microsoft.EventGrid/domains

|Category|类别显示名称|导出成本|
|---|---|---|
|DeliveryFailures|传递失败日志|否|
|PublishFailures|发布失败日志|否|


## <a name="microsofteventgridpartnernamespaces"></a>EventGrid/partnerNamespaces

|Category|类别显示名称|导出成本|
|---|---|---|
|DeliveryFailures|传递失败日志|否|
|PublishFailures|发布失败日志|否|


## <a name="microsofteventgridpartnertopics"></a>EventGrid/partnerTopics

|Category|类别显示名称|导出成本|
|---|---|---|
|DeliveryFailures|传递失败日志|否|


## <a name="microsofteventgridsystemtopics"></a>Microsoft.EventGrid/systemTopics

|Category|类别显示名称|导出成本|
|---|---|---|
|DeliveryFailures|传递失败日志|否|


## <a name="microsofteventgridtopics"></a>Microsoft.EventGrid/topics

|Category|类别显示名称|导出成本|
|---|---|---|
|DeliveryFailures|传递失败日志|否|
|PublishFailures|发布失败日志|否|


## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Category|类别显示名称|导出成本|
|---|---|---|
|ArchiveLogs|存档日志|否|
|AutoScaleLogs|自动缩放日志|否|
|CustomerManagedKeyUserLogs|客户管理的密钥日志|否|
|EventHubVNetConnectionEvent|VNet/IP 筛选连接日志|否|
|KafkaCoordinatorLogs|Kafka 协调器日志|否|
|KafkaUserErrorLogs|Kafka 用户错误日志|否|
|OperationalLogs|操作日志|否|


## <a name="microsoftexperimentationexperimentworkspaces"></a>microsoft 试验/experimentWorkspaces

|Category|类别显示名称|导出成本|
|---|---|---|
|请求|请求|否|


## <a name="microsofthealthcareapisservices"></a>Microsoft.HealthcareApis/services

|Category|类别显示名称|导出成本|
|---|---|---|
|AuditLogs|审核日志|否|


## <a name="microsoftinsightsautoscalesettings"></a>microsoft.insights/autoscalesettings

|Category|类别显示名称|导出成本|
|---|---|---|
|AutoscaleEvaluations|自动缩放评估|否|
|AutoscaleScaleActions|自动缩放缩放操作|否|


## <a name="microsoftinsightscomponents"></a>Microsoft.Insights/Components

|Category|类别显示名称|导出成本|
|---|---|---|
|AppAvailabilityResults|可用性结果|否|
|AppBrowserTimings|浏览器计时|否|
|AppDependencies|依赖项|否|
|AppEvents|事件|否|
|AppExceptions|异常|否|
|AppMetrics|指标|否|
|AppPageViews|页面视图|否|
|AppPerformanceCounters|性能计数器|否|
|AppRequests|请求|否|
|AppSystemEvents|系统事件|否|
|AppTraces|跟踪|否|


## <a name="microsoftiotspacesgraph"></a>Microsoft.IoTSpaces/Graph

|Category|类别显示名称|导出成本|
|---|---|---|
|审核|审核|否|
|流出量|流出量|否|
|流入量|流入量|否|
|可运行|可运行|否|
|跟踪|跟踪|否|
|UserDefinedFunction|UserDefinedFunction|否|


## <a name="microsoftkeyvaultmanagedhsms"></a>microsoft.keyvault/managedhsms

|Category|类别显示名称|导出成本|
|---|---|---|
|AuditEvent|审核事件|否|


## <a name="microsoftkeyvaultvaults"></a>Microsoft.KeyVault/vaults

|Category|类别显示名称|导出成本|
|---|---|---|
|AuditEvent|审核日志|否|


## <a name="microsoftkustoclusters"></a>Microsoft.Kusto/Clusters

|Category|类别显示名称|导出成本|
|---|---|---|
|命令|命令|否|
|FailedIngestion|引入操作失败|否|
|IngestionBatching|引入批处理|否|
|查询|查询|否|
|SucceededIngestion|成功引入操作|否|
|TableDetails|表详细信息|否|
|TableUsageStatistics|表使用情况统计信息|否|


## <a name="microsoftlogicintegrationaccounts"></a>Microsoft.Logic/integrationAccounts

|Category|类别显示名称|导出成本|
|---|---|---|
|IntegrationAccountTrackingEvents|集成帐户跟踪事件|否|


## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Category|类别显示名称|导出成本|
|---|---|---|
|WorkflowRuntime|工作流运行时诊断事件|否|


## <a name="microsoftmachinelearningservicesworkspaces"></a>Microsoft.MachineLearningServices/workspaces

|Category|类别显示名称|导出成本|
|---|---|---|
|AmlComputeClusterEvent|AmlComputeClusterEvent|否|
|AmlComputeClusterNodeEvent|AmlComputeClusterNodeEvent|否|
|AmlComputeCpuGpuUtilization|AmlComputeCpuGpuUtilization|否|
|AmlComputeJobEvent|AmlComputeJobEvent|否|
|AmlRunStatusChangedEvent|AmlRunStatusChangedEvent|否|


## <a name="microsoftmediamediaservices"></a>Microsoft.Media/mediaservices

|Category|类别显示名称|导出成本|
|---|---|---|
|KeyDeliveryRequests|密钥传递请求|否|


## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Category|类别显示名称|导出成本|
|---|---|---|
|ApplicationGatewayAccessLog|应用程序网关访问日志|否|
|ApplicationGatewayFirewallLog|应用程序网关防火墙日志|否|
|ApplicationGatewayPerformanceLog|应用程序网关性能日志|否|


## <a name="microsoftnetworkazurefirewalls"></a>Microsoft.Network/azurefirewalls

|Category|类别显示名称|导出成本|
|---|---|---|
|AzureFirewallApplicationRule|Azure 防火墙应用程序规则|否|
|AzureFirewallDnsProxy|Azure 防火墙 DNS 代理|否|
|AzureFirewallNetworkRule|Azure 防火墙网络规则|否|


## <a name="microsoftnetworkbastionhosts"></a>Microsoft.Network/bastionHosts

|Category|类别显示名称|导出成本|
|---|---|---|
|BastionAuditLogs|堡垒审核日志|否|


## <a name="microsoftnetworkexpressroutecircuits"></a>Microsoft.Network/expressRouteCircuits

|Category|类别显示名称|导出成本|
|---|---|---|
|PeeringRouteLog|对等互连路由表日志|否|


## <a name="microsoftnetworkfrontdoors"></a>Microsoft.Network/frontdoors

|Category|类别显示名称|导出成本|
|---|---|---|
|FrontdoorAccessLog|Frontdoor 访问日志|否|
|FrontdoorWebApplicationFirewallLog|Frontdoor Web 应用程序防火墙日志|否|


## <a name="microsoftnetworkloadbalancers"></a>Microsoft.Network/loadBalancers

|Category|类别显示名称|导出成本|
|---|---|---|
|LoadBalancerAlertEvent|负载均衡器警报事件|否|
|LoadBalancerProbeHealthStatus|负载均衡器探测运行状况|否|


## <a name="microsoftnetworknetworksecuritygroups"></a>Microsoft.Network/networksecuritygroups

|Category|类别显示名称|导出成本|
|---|---|---|
|NetworkSecurityGroupEvent|网络安全组事件|否|
|NetworkSecurityGroupFlowEvent|网络安全组规则流事件|否|
|NetworkSecurityGroupRuleCounter|网络安全组规则计数器|否|


## <a name="microsoftnetworkp2svpngateways"></a>Microsoft.Network/p2sVpnGateways

|Category|类别显示名称|导出成本|
|---|---|---|
|GatewayDiagnosticLog|网关诊断日志|否|
|IKEDiagnosticLog|IKE 诊断日志|否|
|P2SDiagnosticLog|P2S 诊断日志|否|


## <a name="microsoftnetworkpublicipaddresses"></a>Microsoft.Network/publicIPAddresses

|Category|类别显示名称|导出成本|
|---|---|---|
|DDoSMitigationFlowLogs|DDoS 缓解决策的流日志|否|
|DDoSMitigationReports|DDoS 缓解措施报告|否|
|DDoSProtectionNotifications|DDoS 保护通知|否|


## <a name="microsoftnetworktrafficmanagerprofiles"></a>Microsoft.Network/trafficManagerProfiles

|Category|类别显示名称|导出成本|
|---|---|---|
|ProbeHealthStatusEvents|流量管理器探测运行状况结果事件|否|


## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft.Network/virtualNetworkGateways

|Category|类别显示名称|导出成本|
|---|---|---|
|GatewayDiagnosticLog|网关诊断日志|否|
|IKEDiagnosticLog|IKE 诊断日志|否|
|P2SDiagnosticLog|P2S 诊断日志|否|
|RouteDiagnosticLog|路由诊断日志|否|
|TunnelDiagnosticLog|隧道诊断日志|否|


## <a name="microsoftnetworkvirtualnetworks"></a>Microsoft.Network/virtualNetworks

|Category|类别显示名称|导出成本|
|---|---|---|
|VMProtectionAlerts|VM 保护警报|否|


## <a name="microsoftnetworkvpngateways"></a>Microsoft.Network/vpnGateways

|Category|类别显示名称|导出成本|
|---|---|---|
|GatewayDiagnosticLog|网关诊断日志|否|
|IKEDiagnosticLog|IKE 诊断日志|否|
|RouteDiagnosticLog|路由诊断日志|否|
|TunnelDiagnosticLog|隧道诊断日志|否|


## <a name="microsoftnotificationhubsnamespaces"></a>Microsoft.NotificationHubs/namespaces

|Category|类别显示名称|导出成本|
|---|---|---|
|OperationalLogs|操作日志|否|


## <a name="microsoftoperationalinsightsworkspaces"></a>Microsoft.OperationalInsights/workspaces

|Category|类别显示名称|导出成本|
|---|---|---|
|审核|审核日志|否|


## <a name="microsoftpowerbitenants"></a>Microsoft PowerBI/租户

|Category|类别显示名称|导出成本|
|---|---|---|
|引擎|引擎|否|


## <a name="microsoftpowerbitenantsworkspaces"></a>Microsoft PowerBI/租户/工作区

|Category|类别显示名称|导出成本|
|---|---|---|
|引擎|引擎|否|


## <a name="microsoftpowerbidedicatedcapacities"></a>Microsoft.PowerBIDedicated/capacities

|Category|类别显示名称|导出成本|
|---|---|---|
|引擎|引擎|否|


## <a name="microsoftprojectbabylonaccounts"></a>Microsoft.ProjectBabylon/accounts

|Category|类别显示名称|导出成本|
|---|---|---|
|ScanStatusLogEvent|ScanStatus|否|


## <a name="microsoftpurviewaccounts"></a>microsoft.purview/accounts

|Category|类别显示名称|导出成本|
|---|---|---|
|ScanStatusLogEvent|ScanStatus|否|


## <a name="microsoftrecoveryservicesvaults"></a>Microsoft.RecoveryServices/Vaults

|Category|类别显示名称|导出成本|
|---|---|---|
|AddonAzureBackupAlerts|附加 Azure 备份警报数据|否|
|AddonAzureBackupJobs|附加 Azure 备份作业数据|否|
|AddonAzureBackupPolicy|附加 Azure 备份策略数据|否|
|AddonAzureBackupProtectedInstance|附加 Azure 备份受保护实例数据|否|
|AddonAzureBackupStorage|附加 Azure 备份存储数据|否|
|AzureBackupReport|Azure 备份报告数据|否|
|AzureSiteRecoveryEvents|Azure Site Recovery 事件|否|
|AzureSiteRecoveryJobs|Azure Site Recovery 作业|否|
|AzureSiteRecoveryProtectedDiskDataChurn|Azure Site Recovery 受保护的磁盘数据改动|否|
|AzureSiteRecoveryRecoveryPoints|Azure Site Recovery 恢复点|否|
|AzureSiteRecoveryReplicatedItems|Azure Site Recovery 复制项|否|
|AzureSiteRecoveryReplicationDataUploadRate|Azure Site Recovery 复制数据上传速度|否|
|AzureSiteRecoveryReplicationStats|Azure Site Recovery 复制统计信息|否|
|CoreAzureBackup|核心 Azure 备份数据|否|


## <a name="microsoftrelaynamespaces"></a>Microsoft.Relay/namespaces

|Category|类别显示名称|导出成本|
|---|---|---|
|HybridConnectionsEvent|HybridConnections 事件|否|
|HybridConnectionsLogs|HybridConnectionsLogs|否|


## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Category|类别显示名称|导出成本|
|---|---|---|
|OperationLogs|操作日志|否|


## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Category|类别显示名称|导出成本|
|---|---|---|
|OperationalLogs|操作日志|否|


## <a name="microsoftsignalrservicesignalr"></a>Microsoft.SignalRService/SignalR

|Category|类别显示名称|导出成本|
|---|---|---|
|AllLogs|Azure SignalR 服务日志。|否|


## <a name="microsoftsqlmanagedinstances"></a>Microsoft.Sql/managedInstances

|Category|类别显示名称|导出成本|
|---|---|---|
|DevOpsOperationsAudit|Devops 操作审核日志|否|
|ResourceUsageStats|资源使用情况统计信息|否|
|SQLSecurityAuditEvents|SQL 安全审核事件|否|


## <a name="microsoftsqlmanagedinstancesdatabases"></a>Microsoft.Sql/managedInstances/databases

|Category|类别显示名称|导出成本|
|---|---|---|
|错误|错误|否|
|QueryStoreRuntimeStatistics|查询存储运行时统计信息|否|
|QueryStoreWaitStatistics|查询存储等待统计信息|否|
|SQLInsights|SQL Insights|否|


## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Category|类别显示名称|导出成本|
|---|---|---|
|AutomaticTuning|自动优化|否|
|块|块|否|
|DatabaseWaitStatistics|数据库等待统计信息|否|
|死锁数|死锁数|否|
|DevOpsOperationsAudit|Devops 操作审核日志|否|
|DmsWorkers|Dms 辅助角色|否|
|错误|错误|否|
|ExecRequests|Exec 请求|否|
|QueryStoreRuntimeStatistics|查询存储运行时统计信息|否|
|QueryStoreWaitStatistics|查询存储等待统计信息|否|
|RequestSteps|请求步骤|否|
|SQLInsights|SQL Insights|否|
|SqlRequests|Sql 请求|否|
|SQLSecurityAuditEvents|SQL 安全审核事件|否|
|超时|超时|否|
|等待|等待|否|


## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft.Storage/storageAccounts/blobServices

|Category|类别显示名称|导出成本|
|---|---|---|
|StorageDelete|StorageDelete|是|
|StorageRead|StorageRead|是|
|StorageWrite|StorageWrite|是|


## <a name="microsoftstoragestorageaccountsfileservices"></a>Microsoft.Storage/storageAccounts/fileServices

|Category|类别显示名称|导出成本|
|---|---|---|
|StorageDelete|StorageDelete|是|
|StorageRead|StorageRead|是|
|StorageWrite|StorageWrite|是|


## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft.Storage/storageAccounts/queueServices

|Category|类别显示名称|导出成本|
|---|---|---|
|StorageDelete|StorageDelete|是|
|StorageRead|StorageRead|是|
|StorageWrite|StorageWrite|是|


## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft.Storage/storageAccounts/tableServices

|Category|类别显示名称|导出成本|
|---|---|---|
|StorageDelete|StorageDelete|是|
|StorageRead|StorageRead|是|
|StorageWrite|StorageWrite|是|


## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Category|类别显示名称|导出成本|
|---|---|---|
|创作|创作|否|
|执行|执行|否|


## <a name="microsoftsynapseworkspaces"></a>Microsoft.Synapse/workspaces

|Category|类别显示名称|导出成本|
|---|---|---|
|BuiltinSqlReqsEnded|已结束的内置 Sql 池请求|否|
|GatewayApiRequests|Synapse 网关 API 请求|否|
|SQLSecurityAuditEvents|SQL 安全审核事件|否|
|SynapseRbacOperations|Synapse RBAC 操作|否|


## <a name="microsoftsynapseworkspacesbigdatapools"></a>Microsoft.Synapse/workspaces/bigDataPools

|Category|类别显示名称|导出成本|
|---|---|---|
|BigDataPoolAppsEnded|已结束的大数据池应用程序|否|


## <a name="microsoftsynapseworkspacessqlpools"></a>Microsoft.Synapse/workspaces/sqlPools

|Category|类别显示名称|导出成本|
|---|---|---|
|DmsWorkers|Dms 辅助角色|否|
|ExecRequests|Exec 请求|否|
|RequestSteps|请求步骤|否|
|SqlRequests|Sql 请求|否|
|SQLSecurityAuditEvents|Sql 安全审核事件|否|
|等待|等待|否|


## <a name="microsofttimeseriesinsightsenvironments"></a>Microsoft.TimeSeriesInsights/environments

|Category|类别显示名称|导出成本|
|---|---|---|
|流入量|流入量|否|
|管理|管理|否|


## <a name="microsofttimeseriesinsightsenvironmentseventsources"></a>Microsoft.TimeSeriesInsights/environments/eventsources

|Category|类别显示名称|导出成本|
|---|---|---|
|流入量|流入量|否|
|管理|管理|否|


## <a name="microsoftwebhostingenvironments"></a>microsoft.web/hostingenvironments

|Category|类别显示名称|导出成本|
|---|---|---|
|AppServiceEnvironmentPlatformLogs|应用服务环境平台日志|否|


## <a name="microsoftwebsites"></a>microsoft.web/sites

|Category|类别显示名称|导出成本|
|---|---|---|
|AppServiceAntivirusScanAuditLogs|报告防病毒审核日志|否|
|AppServiceAppLogs|应用服务应用程序日志|否|
|AppServiceAuditLogs|访问审核日志|否|
|AppServiceConsoleLogs|应用服务控制台日志|否|
|AppServiceFileAuditLogs|站点内容更改审核日志|否|
|AppServiceHTTPLogs|HTTP 日志|否|
|AppServiceIPSecAuditLogs|IPSecurity 审核日志|否|
|AppServicePlatformLogs|应用服务平台日志|否|
|FunctionAppLogs|函数应用程序日志|否|


## <a name="microsoftwebsitesslots"></a>microsoft.web/sites/slots

|Category|类别显示名称|导出成本|
|---|---|---|
|AppServiceAntivirusScanAuditLogs|报告防病毒审核日志|否|
|AppServiceAppLogs|应用服务应用程序日志|否|
|AppServiceAuditLogs|访问审核日志|否|
|AppServiceConsoleLogs|应用服务控制台日志|否|
|AppServiceFileAuditLogs|站点内容更改审核日志|否|
|AppServiceHTTPLogs|HTTP 日志|否|
|AppServiceIPSecAuditLogs|IPSecurity 审核日志|否|
|AppServicePlatformLogs|应用服务平台日志|否|
|FunctionAppLogs|函数应用程序日志|否|



## <a name="next-steps"></a>后续步骤

* [详细了解资源日志](../essentials/platform-logs-overview.md)
* [将资源日志流式传输到 **事件中心**](./resource-logs.md#send-to-azure-event-hubs)
* [使用 Azure Monitor REST API 更改资源日志诊断设置](/rest/api/monitor/diagnosticsettings)
* [使用 Log Analytics 分析 Azure 存储中的日志](./resource-logs.md#send-to-log-analytics-workspace)

