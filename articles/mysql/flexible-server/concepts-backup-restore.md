---
title: Azure Database for MySQL 灵活的服务器中的备份和还原
description: 了解 Azure Database for MySQL 灵活的服务器备份和还原的概念
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.topic: conceptual
ms.date: 09/21/2020
ms.openlocfilehash: 2d69427f9f11a47cedeccb4b1da38b770952f029
ms.sourcegitcommit: 80034a1819072f45c1772940953fef06d92fefc8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93240760"
---
# <a name="backup-and-restore-in-azure-database-for-mysql-flexible-server-preview"></a>Azure Database for MySQL 灵活的服务器 (预览版中的备份和还原) 

> [!IMPORTANT] 
> Azure Database for MySQL 灵活服务器当前以公共预览版提供。

Azure Database for MySQL 灵活的服务器，会自动创建服务器备份并将其安全地存储在区域内的本地冗余存储中。 备份可以用来将服务器还原到某个时间点。 备份和还原是任何业务连续性策略的基本组成部分，因为它们可以保护数据免遭意外损坏或删除。

## <a name="backup-overview"></a>备份概述

灵活的服务器采用数据文件的快照备份，并将它们存储在本地冗余存储中。 服务器还会执行事务日志备份，并将它们存储在本地冗余存储中。 可以通过这些备份将服务器还原到所配置的备份保留期中的任意时间点。 默认的备份保留期为七天。 你可以选择将数据库备份配置为1到35天。 所有备份都使用 AES 256 位加密为静态存储的数据加密。

无法导出这些备份文件。 备份仅可用于在灵活的服务器中执行还原操作。 你还可以使用 MySQL 客户端中的 [mysqldump](../concepts-migrate-dump-restore.md#dump-and-restore-using-mysqldump-utility) 来复制数据库。

## <a name="backup-frequency"></a>备份频率

灵活服务器上的备份基于快照。 第一次完整快照备份在创建服务器后立即进行计划。 第一次完整快照备份将作为服务器的基准备份保留。 后续快照备份仅为差异备份。

一天至少进行一次差异快照备份。 差异快照备份不按固定计划进行。 差异快照备份每 24 小时进行一次，除非自上次差异备份后 MySQL 中的二进制日志超过 50 GB。 一天内，最多允许有 6 张差异快照。 事务日志备份每五分钟进行一次。

## <a name="backup-retention"></a>备份保留

数据库备份存储在本地冗余存储 (LRS) 中，存储在区域内的三个副本中。 根据服务器上的备份保持期设置来保留备份。 你可以选择1到35天的保留期，默认保留期为七天。 可以通过使用 Azure 门户更新备份配置，在创建服务器或更高版本期间设置保留期。

备份保留期控制执行时间点还原操作的时间间隔，因为它基于可用的备份。 从恢复的角度来看，备份保留期也可以视为恢复时段。 在备份保留期间内执行时间点还原所需的所有备份都保留在备份存储中。 例如，如果备份保持期设置为七天，则会将恢复时段视为过去7天。 在这种情况下，将保留在过去7天内还原服务器所需的所有备份。 如果备份保留时段为七天，则数据库快照和事务日志备份将在窗口) 之前的过去八天内存储 (1 天。

## <a name="backup-storage-cost"></a>备份存储成本

灵活的服务器最多可提供100% 的预配服务器存储作为备份存储，无需额外付费。 超出的备份存储使用量按每月每 GB 标准收费。 例如，如果你预配了具有 250 GB 存储空间的服务器，则服务器备份有 250 GB 的存储空间，无需额外付费。 如果每日备份使用量为25GB，则最多可以有10天的免费备份存储空间。 备份所消耗的存储量超过 250GB 将按照[定价模型](https://azure.microsoft.com/pricing/details/mysql/)收费。

可以使用 Azure 门户中提供的 Azure Monitor 中的[已使用的备份存储](../concepts-monitoring.md)指标来监视服务器使用的备份存储。 "使用的 **备份存储** " 指标表示根据为服务器设置的备份保留期保留的所有数据库备份和日志备份占用的存储量。 无论数据库的总大小如何，如果服务器上的事务性活动繁重，都会导致备份存储使用率增加。

控制备份存储成本的主要方法是设置适当的备份保留期。 你可以选择介于1到35天之间的保留期。

> [!IMPORTANT]
> 从主数据库服务器进行在区域冗余高可用性配置中配置的数据库服务器备份，因为快照备份的开销较低。

> [!IMPORTANT]
> 灵活的服务器当前不支持异地冗余备份。

## <a name="point-in-time-restore"></a>时间点还原

在 Azure Database for MySQL 灵活的服务器中，执行时间点还原将从与源服务器相同的区域中的灵活服务器的备份创建新的服务器。 它是在计算层的原始服务器配置、Vcore 数、存储大小、备份保留期和备份冗余选项的情况下创建的。 此外，还会从源服务器继承标记和设置，例如虚拟网络和防火墙。 还原完成后，可以更改还原的服务器的计算和存储层、配置和安全设置。

> [!NOTE]
> 还原操作完成后，有两个服务器参数重置为默认值（而不是从主服务器复制）
> *   time_zone - 此值设置为默认值“SYSTEM”
> *   event_scheduler - 还原的服务器上的 event_scheduler 设置为“OFF”

多种情况下可以使用时间点还原。 常见的一些用例包括-
-   当用户意外删除数据库中的数据时
-   用户删除重要的表或数据库
-   由于应用程序缺陷，用户应用程序意外覆盖了包含错误数据的适当数据。

可以通过 [Azure 门户](how-to-restore-server-portal.md)在最新的还原点和自定义的还原点之间进行选择。

-   **最新还原点** ：最新的还原点可帮助你将服务器还原到在源服务器上执行的最后一个备份。 用于还原的时间戳还会显示在门户上。 此选项可用于快速将服务器还原到最新状态。
-   **自定义还原点** ：这将允许你在为此灵活服务器定义的保留期内选择任何时间点。 在精确的时间点还原服务器以从用户错误中恢复时，此选项很有用。

估计的恢复时间取决于多个因素，包括数据库大小、事务日志备份大小、SKU 计算大小和还原时间。 事务日志恢复是还原过程中最耗时的部分。 如果选择的还原时间接近完整快照备份计划或差异快照备份计划，则还原速度会更快，因为事务日志应用程序很少。 为了估计服务器的准确恢复时间，我们强烈建议在环境中对其进行测试，因为它具有太多环境特定的变量。

> [!IMPORTANT]
> 如果要还原配置了区域冗余高可用性的灵活服务器，则会在与主服务器相同的区域和区域中配置还原的服务器，并将其部署为非 HA 模式下的单个灵活服务器。 请参阅灵活服务器的 [区域冗余高可用性](concepts-high-availability.md) 。

> [!IMPORTANT]
> 已删除的服务器 **无法** 还原。 如果删除服务器，则属于该服务器的所有数据库也会被删除且不可恢复。 为了防止服务器资源在部署后遭意外删除或意外更改，管理员可以利用[管理锁](../../azure-resource-manager/management/lock-resources.md)。

## <a name="perform-post-restore-tasks"></a>执行还原后任务

从 **最新的还原点** 或 **自定义还原点** 恢复机制还原后，你应执行以下任务以使你的用户和应用程序恢复正常运行：

-   如果新服务器打算替换原始服务器，请将客户端和客户端应用程序重定向到新服务器。
-   确保用户可以连接到适当的服务器级防火墙和虚拟网络规则。
-   确保设置适当的登录名和数据库级别权限。
-   视情况配置警报。

## <a name="next-steps"></a>后续步骤

-   了解 [业务连续性](./concepts-business-continuity.md)
-   了解 [区域冗余高可用性](./concepts-high-availability.md)
-   了解 [备份和恢复](./concepts-backup-restore.md)
