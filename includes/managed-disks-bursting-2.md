---
title: include 文件
description: include 文件
services: virtual-machines
author: albecker1
ms.service: virtual-machines
ms.topic: include
ms.date: 04/27/2020
ms.author: albecker1
ms.custom: include file
ms.openlocfilehash: 801f0f03b49d20c84a4531bd0daad7630a0ed01d
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/17/2021
ms.locfileid: "100585080"
---
## <a name="common-scenarios"></a>常见场景
以下需求场景可显著受益于突发：
- **改善启动时间**  –使用突发，实例的启动速度将大大加快。 例如，启用高级层的 VM 的默认 OS 磁盘是 P4 磁盘，其预配性能最高可达 120 IOPS 和 25 MB/s。 在使用突发的情况下，P4 的性能最高可达 3500 IOPS 和 170 MB/s，可使启动速度加快 6 倍。
- **处理批处理作业** –某些应用程序的工作负荷本质上是循环的，在大多数时间都需要基线性能并且需要较长时间的性能。 这种情况的一个示例是一个会计程序，该程序每日处理需要少量磁盘流量的事务。 然后，在月末，该程序将运行需要大量磁盘流量的对帐报表。
- 应对 **流量高峰**– Web 服务器及其应用程序随时可以体验流量。 如果 Web 服务器由使用突发的 VM 或磁盘提供支持，那么这些服务器便可以更好地处理流量高峰。 

## <a name="bursting-flow"></a>突发流
突发额度系统以相同的方式应用于虚拟机级别和磁盘级别。 你的资源（VM 或磁盘）将从充分储备的额度开始。 有了这些额度，便能够以最大的突发速率进行 30 分钟的突发。 当资源在其性能磁盘存储限制下运行时，突发额度会进行累积。 如果资源使用的所有 IOPS 和 MB/s 低于性能限制，额度将开始累积。 如果资源已经积累了要用于突发的额度，并且你的工作负荷需要额外的性能，则资源可以使用这些额度超出你的性能限制，为工作负荷提供所需的磁盘 IO 性能以满足其需求。



![突发桶关系图](media/managed-disks-bursting/bucket-diagram.jpg)

这完全取决于您想要使用30分钟突发的方式。 可以连续使用 30 分钟，也可以在一天内分散地使用。 部署产品后，该产品为满额度；当其额度耗尽时，可在一天内再次充满额度。 你可以自行决定如何累积和花费其突发信用额度，不一定要再次充满 30 分钟的 Bucket 才能突发。 有关突发累积，需要注意的一件事是，各个资源的突发累积都各不相同，因为它基于资源在其性能限制下运行时未使用的 IOPS 和 MB/s。 这意味着，较高基线性能的产品积累其突发额度的速度可能会快于较低基线性能的产品。 例如，处于空闲状态、没有任何活动的 P1 磁盘将每秒积累 120 IOPS，而 P20 磁盘在处于空闲状态、没有任何活动时将每秒积累 2300 IOPS。

## <a name="bursting-states"></a>突发状态
启用了突发功能时，资源可能处于以下三种状态之一：
- **累积** –资源的 IO 流量小于性能目标。 为 IOPS 和 MB/s 累积额度是彼此分开执行的。 你的资源可能会积累 IOPS 额度并支出 MB/s 额度，也可能会支出 IOPS 额度并积累 MB/s 额度。
- **突发** –资源的流量超过性能目标。 突发流量将独立消耗 IOPS 或带宽的额度。
- **常量** –资源的流量与性能目标完全相同。

## <a name="examples-of-bursting"></a>突发的示例

以下示例显示了突发如何与各种 VM 和磁盘组合一起工作。 为了使示例易于理解，我们将重点放在 MB/s 上，但相同的逻辑也独立适用于 IOPS。

### <a name="non-burstable-virtual-machine-with-burstable-disks"></a>具有可突增磁盘的非可突增虚拟机
**VM 和磁盘组合：** 
- Standard_D8as_v4 
    - 未缓存的 MB/s：192
- P4 OS 磁盘
    - 预配的 MB/s：25
    - 最大突发 MB/s：170 
- 2 个 P10 数据磁盘 
    - 预配的 MB/秒：100
    - 最大突发 MB/s：170

 VM 启动时，将从 OS 磁盘检索数据。 由于 OS 磁盘是正在启动的 VM 的一部分，因此 OS 磁盘将满突发信用额度。 在这些信用额度内，操作系统磁盘将以 170 MB/秒的速度突然增长。

![VM 将针对 192 MB/秒的吞吐量发送到 OS 磁盘，操作系统磁盘响应的数据为 170 MB/s。](media/managed-disks-bursting/nonbursting-vm-busting-disk/nonbusting-vm-bursting-disk-startup.jpg)

启动完成后，应用程序将在 VM 上运行，并且具有非关键工作负荷。 此工作负荷需要 15 MB/S，这会在所有磁盘上均匀分配。

![应用程序向 VM 发送 15 MB/秒的吞吐量请求，VM 将请求并发送其中每个磁盘 a 请求 5 MB/秒，每个磁盘将返回 5 MB/秒，而 VM 则会将 15 MB/秒返回到应用程序。](media/managed-disks-bursting/nonbursting-vm-busting-disk/nonbusting-vm-bursting-disk-idling.jpg)

然后，应用程序需要处理一个批处理作业，该作业需要 192 MB/s。 OS 磁盘使用 2 MB/秒，其余部分在数据磁盘之间平均剥离。

![应用程序将针对 192 MB/秒的吞吐量的请求发送到 VM，VM 会执行请求，并将其请求发送到数据磁盘， (每) 95 MB/秒到 OS 磁盘、数据磁盘脉冲以满足需求，并且所有磁盘都将请求的吞吐量返回给 VM，并将其返回给应用程序。](media/managed-disks-bursting/nonbursting-vm-busting-disk/nonbusting-vm-bursting-disk-bursting.jpg)

### <a name="burstable-virtual-machine-with-non-burstable-disks"></a>具有非可突发磁盘的可突发虚拟机
**VM 和磁盘组合：** 
- Standard_L8s_v2 
    - 未缓存的 MB/s：160
    - 最大突发 MB/s：1280
- P50 OS 磁盘
    - 预配的 MB/s：250 
- 2 个 P10 数据磁盘 
    - 预配的 MB/s：250

 初始启动后，应用程序将在 VM 上运行，并且具有非关键工作负荷。 此工作负荷需要 30 MB/s，可在所有磁盘上均匀分配。
![应用程序将请求的吞吐量从 30 MB/秒发送到 VM，VM 将请求并发送其每个磁盘 a 请求 10 MB/秒，每个磁盘将返回 10 mb/s，而 VM 则会将 30 MB/秒返回到应用程序。](media/managed-disks-bursting/bursting-vm-nonbursting-disk/burst-vm-nonbursting-disk-normal.jpg)

然后，应用程序需要处理一个批处理作业，该作业需要 600 MB/s。 Standard_L8s_v2 突发以满足这一需求，然后对磁盘的请求会均匀地分散到 P50 磁盘。

![应用程序将针对 600 MB/秒的吞吐量发送到 VM 的请求，VM 会将请求发送到请求，并向其每个磁盘发送 200 MB/秒的请求，每个磁盘返回 200 MB/秒，VM 猝发返回 600 MB/秒到应用程序。](media/managed-disks-bursting/bursting-vm-nonbursting-disk/burst-vm-nonbursting-disk-bursting.jpg)
### <a name="burstable-virtual-machine-with-burstable-disks"></a>可突增包含可突增磁盘的虚拟机
**VM 和磁盘组合：** 
- Standard_L8s_v2 
    - 未缓存的 MB/s：160
    - 最大突发 MB/s：1280
- P4 OS 磁盘
    - 预配的 MB/s：25
    - 最大突发 MB/s：170 
- 2 个 P4 数据磁盘 
    - 预配的 MB/s：25
    - 最大突发 MB/s：170 

VM 启动时，会将其从 OS 磁盘请求突发为 1280 MB/秒的突发限制，操作系统磁盘将响应其突发性能 170 MB/秒。

![在启动时，VM 会向后发送 1280 MB/秒的请求到 OS 磁盘，操作系统磁盘会出现猝发，返回 1280 MB/秒。](media/managed-disks-bursting/bursting-vm-bursting-disk/burst-vm-burst-disk-startup.jpg)

启动后，启动一个具有非关键工作负荷的应用程序。 此应用程序需要 15 MB/s，这会在所有磁盘上均匀分配。

![应用程序向 VM 发送 15 MB/秒的吞吐量请求，VM 将请求并发送其中每个磁盘 a 请求 5 MB/秒，每个磁盘将返回 5 MB/秒，而 VM 则会将 15 MB/秒返回到应用程序。](media/managed-disks-bursting/bursting-vm-bursting-disk/burst-vm-burst-disk-idling.jpg)

然后，应用程序需要处理一个批处理作业，该作业需要 360 MB/s。 Standard_L8s_v2 发生突发以满足此要求，然后进行请求。 OS 磁盘只需要 20 MB/s。 剩余 340 MB/秒由突发 P4 数据磁盘处理。

![应用程序将针对 360 MB/秒的吞吐量发送到 VM 的请求，VM 会将请求发送到请求，并将其每个数据磁盘的请求从 OS 磁盘发送到 170 MB/秒和 20 MB/秒，每个磁盘将返回所请求的 MB/秒，将 VM 猝发360到应用程序。](media/managed-disks-bursting/bursting-vm-bursting-disk/burst-vm-burst-disk-bursting.jpg)