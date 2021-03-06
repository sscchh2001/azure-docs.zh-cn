---
title: 只读副本 - Azure Database for MySQL
description: 了解 Azure Database for MySQL 中的只读副本：选择区域、创建副本、连接副本、监视复制和停止复制。
author: savjani
ms.author: pariks
ms.service: mysql
ms.topic: conceptual
ms.date: 01/13/2021
ms.custom: references_regions
ms.openlocfilehash: c380a3edb556adb72d067cb2910c8afbf66b99a0
ms.sourcegitcommit: 25d1d5eb0329c14367621924e1da19af0a99acf1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/16/2021
ms.locfileid: "98250258"
---
# <a name="read-replicas-in-azure-database-for-mysql"></a>Azure Database for MySQL 中的只读副本

使用只读副本功能可将数据从 Azure Database for MySQL 服务器复制到只读服务器。 可将源服务器中的数据复制到最多 5 个副本。 使用 MySQL 引擎的基于本机二进制日志 (binlog) 文件位置的复制技术以异步方式更新副本。 若要了解有关 binlog 复制的详细信息，请参阅 [MySQL binlog 复制概述](https://dev.mysql.com/doc/refman/5.7/en/binlog-replication-configuration-overview.html)。

副本是新的服务器，可以像管理普通的 Azure Database for MySQL 服务器一样对其进行管理。 每个只读副本按照预配计算资源的 vCore 数量以及每月 GB 存储量计费。

如需了解有关 MySQL 复制功能和问题的详细信息，请参阅 [MySQL 复制文档](https://dev.mysql.com/doc/refman/5.7/en/replication-features.html)。

> [!NOTE]
> 本文包含对字词 _从属_ 的引用，这是 Microsoft 不再使用的术语。 在从软件中删除该术语后，我们会将其从本文中删除。
>

## <a name="when-to-use-a-read-replica"></a>何时使用只读副本

只读副本功能可帮助改善读取密集型工作负荷的性能与规模。 读取工作负荷可以独立于副本，而写入工作负荷可以定向到源。

常见方案是让 BI 和分析工作负载将只读副本用作报告的数据源。

由于副本是只读的，因此它们不会直接在源上减少写入容量的负担。 此功能并非面向写入密集型工作负荷。

只读副本功能使用 MySQL 本机异步复制。 该功能不适用于同步复制方案。 源与副本之间将会存在明显的延迟。 副本上的数据最终会与源中的数据保持一致。 对于能够适应这种延迟的工作负荷，可以使用此功能。

## <a name="cross-region-replication"></a>跨区域复制

可以在与源服务器不同的区域中创建只读副本。 跨区域复制对于灾难恢复规划或使数据更接近用户等方案非常有用。

任何 [Azure Database for MySQL 区域](https://azure.microsoft.com/global-infrastructure/services/?products=mysql)中都可以有源服务器。  源服务器可以在其配对区域或通用副本区域中拥有副本。 下图显示了根据源区域可用的副本区域。

[:::image type="content" source="media/concepts-read-replica/read-replica-regions.png" alt-text="读取副本区域":::](media/concepts-read-replica/read-replica-regions.png#lightbox)

### <a name="universal-replica-regions"></a>通用副本区域

无论源服务器位于何处，都可以在以下任何区域中创建读取副本。 支持的通用副本区域包括：

澳大利亚东部、澳大利亚东南部、巴西南部、加拿大中部、加拿大东部、美国中部、东亚、美国东部、美国东部2、日本东部、日本西部、韩国中部、韩国南部、美国中北部、北欧、美国中南部、东南亚、英国南部、英国西部、西欧、美国西部、美国西部2、美国中部。

### <a name="paired-regions"></a>配对区域

除通用副本区域外，还可以在源服务器的 Azure 配对区域中创建读取副本。 如果你不知道所在区域的配对，可以从 [Azure 配对区域](../best-practices-availability-paired-regions.md)一文中了解更多信息。

如果要使用跨区域副本进行灾难恢复计划，则建议在配对区域中创建副本，而不是在另一个区域中创建。 配对区域可避免同时进行更新，并会优先考虑物理隔离和数据驻留。  

但是，需要考虑以下限制： 

* 区域可用性： Azure Database for MySQL 在法国中部、阿拉伯联合酋长国北部和德国中部提供。 但是，它们的配对区域不可用。

* 单向对：某些 Azure 区域仅在一个方向上配对。 这些区域包括印度西部、巴西南部和 US Gov 弗吉尼亚州。
   这意味着印度西部的源服务器可以在印度南部创建副本。 但是，印度的源服务器不能在印度西部创建副本。 这是因为西部印度的次要区域是印度南部地区，但印度南部的次要区域不是印度西部。

## <a name="create-a-replica"></a>创建副本

> [!IMPORTANT]
> 只读副本功能仅适用于“常规用途”或“内存优化”定价层中的 Azure Database for MySQL 服务器。 请确保源服务器位于其中一个定价层中。

如果源服务器没有现有的副本服务器，该源服务器会先重启，以便为复制做好自身准备。

启动“创建副本”工作流时，将创建空白的 Azure Database for MySQL 服务器。 新服务器中会填充源服务器上的数据。 创建时间取决于源服务器上的数据量，以及自上次每周完整备份以来所经历的时间。 具体所需时间从几分钟到几小时不等。 始终会在与源服务器相同的资源组和订阅中创建副本服务器。 如果要将副本服务器创建到不同的资源组或不同的订阅，可以在创建后[移动副本服务器](../azure-resource-manager/management/move-resource-group-and-subscription.md)。

每个副本都启用了存储[自动增长](concepts-pricing-tiers.md#storage-auto-grow)。 自动增长功能允许副本与复制到它的数据保持同步，并防止由于存储空间不足错误而导致的复制中断。

了解如何[在 Azure 门户中创建只读副本](howto-read-replicas-portal.md)。

## <a name="connect-to-a-replica"></a>连接到副本

创建时，副本会继承源服务器的防火墙规则。 之后，这些规则将独立于源服务器。

副本会从源服务器继承管理员帐户。 源服务器上的所有用户帐户都会复制到只读副本。 只能通过使用源服务器上可用的用户帐户来连接到只读副本。

可以使用主机名和有效的用户帐户连接到副本，就像在常规的 Azure Database for MySQL 服务器上连接一样。 对于名称为 **myreplica**、管理员用户名为 **myadmin** 的服务器，可以使用 mysql CLI 连接到副本：

```bash
mysql -h myreplica.mysql.database.azure.com -u myadmin@myreplica -p
```

在提示符下，输入用户帐户的密码。

## <a name="monitor-replication"></a>监视复制

Azure Database for MySQL 在 Azure Monitor 中提供“复制滞后时间(秒)”指标。 此指标仅适用于副本。 此指标是使用 MySQL 的 `SHOW SLAVE STATUS` 命令中提供的 `seconds_behind_master` 指标计算的。 请设置警报，以便在复制滞后时间达到工作负荷不可接受的值时收到通知。

如果发现复制滞后时间增加，请参阅[排查复制延迟问题](howto-troubleshoot-replication-latency.md)，以便排除故障并了解可能的原因。

## <a name="stop-replication"></a>停止复制

可以停止源与副本之间的复制。 在源服务器与只读副本之间停止复制后，该副本会成为独立服务器。 独立服务器中的数据是启动“停止复制”命令时副本上可用的数据。 独立服务器不会与源服务器保持同步。

选择停止复制到副本时，它会丢失指向其以前的源和其他副本的所有链接。 源及其副本之间没有自动故障转移。

> [!IMPORTANT]
> 独立服务器不能再次成为副本。
> 在只读副本上停止复制之前，请确保副本包含所需的全部数据。

了解如何[停止复制到副本](howto-read-replicas-portal.md)。

## <a name="failover"></a>故障转移

源服务器和副本服务器之间没有自动故障转移。

由于复制是异步的，因此源和副本之间存在延迟。 延迟量可能会受到许多因素的影响，例如，源服务器上运行的工作负荷的大小和数据中心之间的延迟。 大多数情况下，副本验证在几秒钟到几分钟之间。 可以使用“副本延迟”指标来跟踪实际的副本延迟，该指标适用于每个副本。 该指标显示的是自上次重播事务以来所经历的时间。 建议观察一段时间的副本延迟，以便确定平均延迟。 可以针对副本延迟设置警报，这样，当它超出预期范围时，你就可以采取行动。

> [!Tip]
> 如果你故障转移到副本，在取消副本与源的链接时的滞后时间将表明会丢失多少数据。

确定要故障转移到副本后：

1. 请停止将数据复制到副本<br/>
   此步骤是使副本服务器能够接受写入所必需的。 作为此过程的一部分，将从源 delinked 副本服务器。 启动停止复制后，后端进程通常需要大约2分钟的时间才能完成。 请参阅本文的[停止复制](#stop-replication)部分，了解此操作的潜在影响。

2. 将应用程序指向（以前的）副本<br/>
   每个服务器都有唯一的连接字符串。 更新应用程序，使之指向 (以前) 副本，而不是源。

应用程序成功处理读取和写入后，就完成了故障转移。 你的应用程序体验的停机时间将取决于你何时检测到问题并完成前面列出的步骤1和2。

## <a name="global-transaction-identifier-gtid"></a>全局事务标识符 (GTID)

全局事务标识符 (GTID) 是使用源服务器上的每个提交的事务创建的唯一标识符，在 Azure Database for MySQL 中默认处于关闭状态。 GTID 在版本 5.7 和 8.0 中受支持，且仅在所支持的存储不超过 16 TB 的服务器上受支持。 若要详细了解 GTID 及其在复制中的使用方式，请参阅 MySQL 的[使用 GTID 进行复制](https://dev.mysql.com/doc/refman/5.7/en/replication-gtids.html)文档。

MySQL 支持两种类型的事务：GTID 事务（使用 GTID 标识）和匿名事务（未分配 GTID）

以下服务器参数可用于配置 GTID： 

|**服务器参数**|**说明**|**默认值**|**值**|
|--|--|--|--|
|`gtid_mode`|指示是否使用 GTID 标识事务。 若要在两个模式之间进行更改，只能按升序顺序一次完成一个步骤（例如 `OFF` -> `OFF_PERMISSIVE` -> `ON_PERMISSIVE` -> `ON`)|`OFF`|`OFF`：新事务和复制事务都必须是匿名事务 <br> `OFF_PERMISSIVE`：新事务是匿名事务。 复制的事务可以是匿名事务，也可以是 GTID 事务。 <br> `ON_PERMISSIVE`：新事务是 GTID 事务。 复制的事务可以是匿名事务，也可以是 GTID 事务。 <br> `ON`：新事务和复制事务都必须是 GTID 事务。|
|`enforce_gtid_consistency`|通过仅允许执行能够以事务性安全方式记录的语句来强制实施 GTID 一致性。 在启用 GTID 复制之前，必须将此值设置为 `ON`。 |`OFF`|`OFF`：允许所有事务违反 GTID 一致性。  <br> `ON`：不允许任何事务违反 GTID 一致性。 <br> `WARN`：允许所有事务违反 GTID 一致性，但系统会生成警告。 | 

> [!NOTE]
> 启用 GTID 后，无法将其重新打开。 如果需要关闭 GTID，请与支持人员联系。 

若要启用 GTID 并配置一致性行为，请使用 [Azure 门户](howto-server-parameters.md)、[Azure CLI](howto-configure-server-parameters-using-cli.md) 或 [PowerShell](howto-configure-server-parameters-using-powershell.md)更新 `gtid_mode` 和 `enforce_gtid_consistency` 服务器参数。

如果在源服务器上启用了 GTID（`gtid_mode` = 开），则新创建的副本也将启用 GTID 并使用 GTID 复制。 若要保持复制一致，你无法 `gtid_mode` 在源服务器或副本服务器上进行更新，)  (。

## <a name="considerations-and-limitations"></a>注意事项和限制

### <a name="pricing-tiers"></a>定价层

只读副本当前仅适用于“常规用途”和“内存优化”的定价层。

> [!NOTE]
> 运行副本服务器的成本取决于副本服务器的运行区域。

### <a name="source-server-restart"></a>源服务器重启

在为没有现有副本的源创建副本时，该源服务器会先重启，以便为复制做好自身准备。 请考虑这一点并在非高峰期执行这些操作。

### <a name="new-replicas"></a>新副本

只读副本创建为新的 Azure Database for MySQL 服务器。 无法将现有的服务器设为副本。 无法创建另一个只读副本的副本。

### <a name="replica-configuration"></a>副本配置

使用与源相同的服务器配置创建副本。 在创建副本后，可以独立于源服务器更改多项设置：计算代系、vCore 数、存储，以及备份保持期。 定价层也可以独立更改，但“基本”层除外。

> [!IMPORTANT]
> 将源服务器的配置更新为新值之前，请将副本配置更新为与这些新值相等或更大的值。 此操作可确保副本与源服务器发生的任何更改保持同步。

在创建副本时，防火墙规则和参数设置会从源服务器继承到副本。 之后，副本的规则便会独立。

### <a name="stopped-replicas"></a>停止的副本

如果停止源服务器与只读副本之间的复制，已停止的副本会成为接受读取和写入操作的独立服务器。 独立服务器不能再次成为副本。

### <a name="deleted-source-and-standalone-servers"></a>已删除的源服务器和独立服务器

在删除源服务器时，会对所有只读副本都停止复制。 这些副本会自动成为独立服务器，并且可以接受读取和写入。 源服务器本身会被删除。

### <a name="user-accounts"></a>用户帐户

源服务器上的用户会复制到只读副本。 只能使用源服务器上可用的用户帐户来连接到只读副本。

### <a name="server-parameters"></a>服务器参数

为了防止数据不同步并避免潜在的数据丢失或损坏，使用读取副本时，会锁定某些服务器参数以防止其更新。

源服务器和副本服务器上都会锁定以下服务器参数：

* [`innodb_file_per_table`](https://dev.mysql.com/doc/refman/8.0/en/innodb-file-per-table-tablespaces.html) 
* [`log_bin_trust_function_creators`](https://dev.mysql.com/doc/refman/5.7/en/replication-options-binary-log.html#sysvar_log_bin_trust_function_creators)

副本服务器上的 [`event_scheduler`](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_event_scheduler) 参数处于锁定状态。

若要在源服务器上更新上述参数之一，请删除副本服务器，更新源上的参数值，然后重新创建副本。

### <a name="gtid"></a>GTID

GTID 在以下版本上受支持：

* MySQL 版本5.7 和8.0。
* 支持不超过 16 TB 的存储的服务器。 若要获取支持 16 TB 存储的区域的完整列表，请参阅[定价层](concepts-pricing-tiers.md#storage)一文。

GTID 默认处于关闭状态。 启用 GTID 后，无法将其重新打开。 如果需要关闭 GTID，请联系支持人员。

如果在源服务器上启用了 GTID，则新创建的副本也将启用 GTID 并使用 GTID 复制。 若要保持复制一致，你无法 `gtid_mode` 在源服务器或副本服务器上进行更新，)  (。

### <a name="other"></a>其他

* 不支持创建副本副本。
* 内存中的表可能会导致副本服务器变得不同步。这是 MySQL 复制技术的限制。 有关详细信息，请阅读 [MySQL 参考文档](https://dev.mysql.com/doc/refman/5.7/en/replication-features-memory.html)中的更多信息。
* 请确保源服务器表具有主键。 缺少主键可能会导致源和副本之间出现复制延迟。
* 查看 [MySQL 文档](https://dev.mysql.com/doc/refman/5.7/en/replication-features.html)中 MySQL 复制限制的完整列表

## <a name="next-steps"></a>后续步骤

* 了解如何[使用 Azure 门户创建和管理只读副本](howto-read-replicas-portal.md)
* 了解如何[通过 Azure CLI 和 REST API 创建和管理只读副本](howto-read-replicas-cli.md)