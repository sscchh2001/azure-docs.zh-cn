---
title: 部署日志转发器，将 CEF 数据连接到 Azure Sentinel |Microsoft Docs
description: 了解如何部署代理以将 CEF 数据连接到 Azure Sentinel。
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/05/2021
ms.author: yelevin
ms.openlocfilehash: a4303f43dffa98f842bd3daf9e3a0cd5214932b1
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/17/2021
ms.locfileid: "100585367"
---
# <a name="step-1-deploy-the-log-forwarder"></a>步骤1：部署日志转发器


在此步骤中，你将指定并配置将日志从安全解决方案转发到 Azure Sentinel 工作区的 Linux 计算机。 此计算机可以是本地环境中的物理计算机、Azure VM 或另一个云中的虚拟机。 使用提供的链接，你将在执行以下任务的指定计算机上运行脚本：

- 安装适用于 Linux 的 Log Analytics agent (也称为 OMS 代理) ，并将其配置为以下目的：
    - 在 TCP 端口25226上侦听内置 Linux Syslog Syslog 后台程序中的 CEF 消息
    - 将消息通过 TLS 安全地发送到 Azure Sentinel 工作区，并在其中进行分析和控制

- 为以下目的配置内置 Linux Syslog (rsyslog/syslog) ：
    - 在 TCP 端口514上的安全解决方案中侦听 Syslog 消息
    - 使用 TCP 端口25226只将其标识为 CEF 的消息转发到 localhost 上的 Log Analytics 代理
 
## <a name="prerequisites"></a>先决条件

- 在指定的 Linux 计算机上，您必须具有提升的权限 (sudo) 。

- 必须在 Linux 计算机上安装 **python 2.7** 或 **3** 。<br>使用 `python -version` 命令检查。

- 安装 Log Analytics 代理之前，Linux 计算机不得连接到任何 Azure 工作区。

- Linux 计算机至少必须有 **4 个 CPU 核心和 8 GB RAM**。

    > [!NOTE]
    > - 使用 **rsyslog** 守护程序的单个日志转发器计算机的 **最大容量为每秒8500个事件 (EPS)** 收集。

- 在此过程中的某个时间点，可能需要工作区 ID 和工作区主键。 可以在工作区资源的 " **代理管理**" 下找到它们。

## <a name="run-the-deployment-script"></a>运行部署脚本
 
1. 在 Azure Sentinel 导航菜单中，单击 " **数据连接器**"。 在连接器列表中，单击 " **常见事件格式" (CEF)** "磁贴，然后单击右下角的" **打开连接器页** "按钮。 

1. **1.2 在 Linux 计算机上安装 CEF 收集器** 后，复制 "**运行以下脚本以安装和应用 CEF 收集器**" 下提供的链接，或从下面的文本中 (应用工作区 ID 和主密钥来替换占位符) ：

    ```bash
    sudo wget -O cef_installer.py https://raw.githubusercontent.com/Azure/Azure-Sentinel/master/DataConnectors/CEF/cef_installer.py&&sudo python cef_installer.py [WorkspaceID] [Workspace Primary Key]
    ```

1. 脚本运行时，请检查以确保没有收到任何错误或警告消息。
    - 你可能会收到一条消息，指导你运行命令来更正 " *计算机* " 字段映射的问题。 有关详细信息，请参阅 [部署脚本中的说明](#mapping-command) 。

1. 继续执行 [步骤2：配置安全解决方案以转发 CEF 消息](connect-cef-solution-config.md)。


> [!NOTE]
> **使用同一台计算机转发普通 Syslog *和* CEF 消息**
>
> 如果你计划使用此日志转发器计算机转发 [syslog 消息](connect-syslog.md) 以及 CEF，则为了避免将事件重复到 Syslog 和 CommonSecurityLog 表：
>
> 1. 在以 CEF 格式将日志发送到转发器的每个源计算机上，必须编辑 Syslog 配置文件以删除用于发送 CEF 消息的工具。 这样一来，在 CEF 中发送的功能也不会在 Syslog 中发送。 有关如何执行此操作的详细说明，请参阅在 [Linux 代理上配置 Syslog](../azure-monitor/agents/data-sources-syslog.md#configure-syslog-on-linux-agent) 。
>
> 1. 必须在这些计算机上运行以下命令，才能在 Azure Sentinel 中通过 Syslog 配置禁用代理的同步。 这可以确保在上一步中所做的配置更改不会被覆盖。<br>
> `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py --disable'`

> [!NOTE]
> **更改 TimeGenerated 字段的源**
>
> - 默认情况下，Log Analytics 代理使用代理从 Syslog 守护程序收到事件的时间填充架构中的 *TimeGenerated* 字段。 因此，在源系统上生成事件的时间不会记录在 Azure Sentinel 中。
>
> - 但可以运行以下命令，该命令将下载并运行 `TimeGenerated.py` 脚本。 此脚本将 Log Analytics 代理配置为在其源系统上用事件的原始时间（而不是代理接收到的时间）填充 *TimeGenerated* 字段。
>
>    ```bash
>    wget -O TimeGenerated.py https://raw.githubusercontent.com/Azure/Azure-Sentinel/master/DataConnectors/CEF/TimeGenerated.py && python TimeGenerated.py {ws_id}
>    ```

## <a name="deployment-script-explained"></a>部署脚本说明

下面是有关部署脚本操作的命令分步说明。

选择 syslog 后台程序，查看相应的描述。

# <a name="rsyslog-daemon"></a>[rsyslog 后台程序](#tab/rsyslog)

1. **下载并安装 Log Analytics 代理：**

    - 下载 Log Analytics (OMS) Linux 代理的安装脚本。

        ```bash
        wget -O onboard_agent.sh https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/
            master/installer/scripts/onboard_agent.sh
        ```

    - 安装 Log Analytics 代理。
    
        ```bash
        sh onboard_agent.sh -w [workspaceID] -s [Primary Key] -d opinsights.azure.com
        ```

1. **将 Log Analytics 代理配置设置为侦听端口25226，并将 CEF 消息转发到 Azure Sentinel：**

    - 从 Log Analytics 代理 GitHub 存储库下载配置。

        ```bash
        wget -O /etc/opt/microsoft/omsagent/[workspaceID]/conf/omsagent.d/security_events.conf
            https://raw.githubusercontent.com/microsoft/OMS-Agent-for-Linux/master/installer/conf/
            omsagent.d/security_events.conf
        ```

1. **配置 Syslog 守护程序：**

    - 使用 syslog 配置文件打开端口514进行 TCP 通信 `/etc/rsyslog.conf` 。

    - 通过在 syslog 守护程序目录中插入一个特殊配置文件，将后台程序配置为将 CEF 消息转发到 TCP 端口25226上的 Log Analytics 代理 `security-config-omsagent.conf` `/etc/rsyslog.d/` 。

        文件的内容 `security-config-omsagent.conf` ：

        ```bash
        if $rawmsg contains "CEF:" or $rawmsg contains "ASA-" then @@127.0.0.1:25226 
        ```

1. **重新启动 Syslog 守护程序和 Log Analytics 代理：**

    - 重新启动 rsyslog 守护程序。
    
        ```bash
        service rsyslog restart
        ```

    - 重新启动 Log Analytics 代理。

        ```bash
        /opt/microsoft/omsagent/bin/service_control restart [workspaceID]
        ```

1. **按预期验证 " *计算机* " 字段的映射：**

    - 使用以下命令确保 syslog 源中的 " *计算机* " 字段已正确映射到 Log Analytics 代理中： 

        ```bash
        grep -i "'Host' => record\['host'\]"  /opt/microsoft/omsagent/plugin/filter_syslog_security.rb
        ```
    - <a name="mapping-command"></a>如果映射出现问题，该脚本将生成一条错误消息，指导您 **手动运行以下命令** (将工作区 ID 替换为占位符) 。 命令将确保正确的映射，然后重新启动代理。
    
        ```bash
        sed -i -e "/'Severity' => tags\[tags.size - 1\]/ a \ \t 'Host' => record['host']" -e "s/'Severity' => tags\[tags.size - 1\]/&,/" /opt/microsoft/omsagent/plugin/filter_syslog_security.rb && sudo /opt/microsoft/omsagent/bin/service_control restart [workspaceID]
        ```

# <a name="syslog-ng-daemon"></a>[syslog-ng 守护程序](#tab/syslogng)

1. **下载并安装 Log Analytics 代理：**

    - 下载 Log Analytics (OMS) Linux 代理的安装脚本。

        ```bash
        wget -O onboard_agent.sh https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/
            master/installer/scripts/onboard_agent.sh
        ```

    - 安装 Log Analytics 代理。
    
        ```bash
        sh onboard_agent.sh -w [workspaceID] -s [Primary Key] -d opinsights.azure.com
        ```

1. **将 Log Analytics 代理配置设置为侦听端口25226，并将 CEF 消息转发到 Azure Sentinel：**

    - 从 Log Analytics 代理 GitHub 存储库下载配置。

        ```bash
        wget -O /etc/opt/microsoft/omsagent/[workspaceID]/conf/omsagent.d/security_events.conf
            https://raw.githubusercontent.com/microsoft/OMS-Agent-for-Linux/master/installer/conf/
            omsagent.d/security_events.conf
        ```

1. **配置 Syslog 守护程序：**

    - 使用 syslog 配置文件打开端口514进行 TCP 通信 `/etc/syslog-ng/syslog-ng.conf` 。

    - 通过在 syslog 守护程序目录中插入一个特殊配置文件，将后台程序配置为将 CEF 消息转发到 TCP 端口25226上的 Log Analytics 代理 `security-config-omsagent.conf` `/etc/syslog-ng/conf.d/` 。

        文件的内容 `security-config-omsagent.conf` ：

        ```bash
        filter f_oms_filter {match(\"CEF\|ASA\" ) ;};destination oms_destination {tcp(\"127.0.0.1\" port(25226));};
        log {source(s_src);filter(f_oms_filter);destination(oms_destination);};
        ```

1. **重新启动 Syslog 守护程序和 Log Analytics 代理：**

    - 重新启动 syslog-ng 守护程序。
    
        ```bash
        service syslog-ng restart
        ```

    - 重新启动 Log Analytics 代理。

        ```bash
        /opt/microsoft/omsagent/bin/service_control restart [workspaceID]
        ```

1. **按预期验证 " *计算机* " 字段的映射：**

    - 使用以下命令确保 syslog 源中的 " *计算机* " 字段已正确映射到 Log Analytics 代理中： 

        ```bash
        grep -i "'Host' => record\['host'\]"  /opt/microsoft/omsagent/plugin/filter_syslog_security.rb
        ```
    - <a name="mapping-command"></a>如果映射出现问题，该脚本将生成一条错误消息，指导您 **手动运行以下命令** (将工作区 ID 替换为占位符) 。 命令将确保正确的映射，然后重新启动代理。
    
        ```bash
        sed -i -e "/'Severity' => tags\[tags.size - 1\]/ a \ \t 'Host' => record['host']" -e "s/'Severity' => tags\[tags.size - 1\]/&,/" /opt/microsoft/omsagent/plugin/filter_syslog_security.rb && sudo /opt/microsoft/omsagent/bin/service_control restart [workspaceID]
        ```
---

## <a name="next-steps"></a>后续步骤

本文档介绍了如何部署 Log Analytics 代理，将 CEF 设备连接到 Azure Sentinel。 要详细了解 Azure Sentinel，请参阅以下文章：
- 了解如何[洞悉数据和潜在威胁](quickstart-get-visibility.md)。
- 开始[使用 Azure Sentinel 检测威胁](./tutorial-detect-threats-built-in.md)。
