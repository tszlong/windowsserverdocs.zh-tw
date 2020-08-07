---
title: 虛擬機器資源控制
description: 使用 VM CPU 群組
author: allenma
ms.date: 06/18/2018
ms.topic: article
ms.service: windows-10-hyperv
ms.assetid: cc7bb88e-ae75-4a54-9fb4-fc7c14964d67
ms.openlocfilehash: 1f902a37dd4df28b2591380e78fe86c271f4ed3e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87963594"
---
# <a name="virtual-machine-resource-controls"></a>虛擬機器資源控制

> 適用於：Windows Server 2016、Microsoft Hyper-V Server 2016、Windows Server 2019、Microsoft Hyper-V Server 2019

本文說明虛擬機器的 Hyper-v 資源和隔離控制。  這些功能（我們將稱之為虛擬機器 CPU 群組，或只是「CPU 群組」）是在 Windows Server 2016 中引進。  CPU 群組可讓 Hyper-v 系統管理員更有效地管理和配置跨來賓虛擬機器的主機 CPU 資源。  使用 CPU 群組時，Hyper-v 系統管理員可以：

* 建立虛擬機器群組，其中每個群組各有不同的虛擬化主機總 CPU 資源配置，並在整個群組之間共用。 這可讓主機管理員針對不同類型的 Vm 執行服務的類別。

* 將 CPU 資源限制設定為特定群組。 此「群組上限」設定了整個群組可能會耗用的主機 CPU 資源上限，有效地強制執行該群組所需的服務類別。

* 限制 CPU 群組只能在一組特定的主機系統處理器上執行。 這可以用來隔離屬於不同 CPU 群組的 Vm。

## <a name="managing-cpu-groups"></a>管理 CPU 群組

CPU 群組是透過 Hyper-v 主機計算服務或 HCS 來管理。 如需 HCS 的絕佳說明、其創世、HCS Api 的連結，以及更多相關資訊，請前往《[主機計算服務 (HCS) ](https://blogs.technet.microsoft.com/virtualization/2017/01/27/introducing-the-host-compute-service-hcs/)的張貼文章中的 Microsoft 虛擬化小組的 blog。

>[!NOTE]
>只有 HCS 可以用來建立和管理 CPU 群組;Hyper-v 管理員 applet、WMI 和 PowerShell 管理介面不支援 CPU 群組。

Microsoft 會在[Microsoft 下載中心](https://go.microsoft.com/fwlink/?linkid=865968)提供命令列公用程式 cpugroups.exe，其使用 HCS 介面來管理 CPU 群組。  此公用程式也可以顯示主機的 CPU 拓撲。

## <a name="how-cpu-groups-work"></a>CPU 群組的工作方式

Hyper-v 虛擬機器會使用計算的 CPU 群組上限，強制執行跨 CPU 群組的主機計算資源配置。 CPU 群組上限是 CPU 群組的總 CPU 容量的一小部分。 群組上限的值取決於群組類別或指派的優先權層級。 計算的群組上限可視為「一種 LP 的 CPU 時間數量」。 此群組預算是共用的，因此如果只有單一 VM 處於作用中狀態，則可以使用整個群組的 CPU 配置來進行本身。

CPU 群組上限的計算方式為 G = *n* x *C*，其中：

- *G*是我們想要指派給群組的主機 LP 數量
- *n*是群組中 (LPs) 的邏輯處理器總數
- *C*是 CPU 配置的最大值（也就是，群組所需的服務類別，以系統的總計算容量百分比表示）

例如，假設 CPU 群組設定了4個邏輯處理器， (LPs) ，而上限為50%。

- G = n * C
- G = 4 * 50%
- G = 2 LP 對整個群組的 CPU 時間

在此範例中，CPU 群組 G 會配置2個 LP 的 CPU 時間。

請注意，不論系結至群組的虛擬機器或虛擬處理器數目為何，不論狀態 (（例如，關閉或啟動) 指派給 CPU 群組的虛擬機器），都適用群組上限。 因此，系結至相同 CPU 群組的每個 VM 都會收到群組的總 CPU 配置，而這會隨著系結至 CPU 群組的 Vm 數目而變更。 因此，當 Vm 是從 CPU 群組系結或解除系結 Vm 時，整體 CPU 群組上限必須是進行調整並設定，以維持所需的每個 VM 上限。 VM 主機管理員或虛擬化管理軟體層負責視需要管理群組上限，以達到所需的每個 VM CPU 資源配置。

## <a name="example-classes-of-service"></a>服務的範例類別

讓我們來看一些簡單的範例。 首先，假設 Hyper-v 主機系統管理員想要支援來賓 Vm 的兩個服務層級：

1. 低端「C」層。 我們會為這一層提供整個主機計算資源的10%。

1. 中間範圍「B」層。 這一層配置了整部主機計算資源的50%。

在我們的範例中，我們將判斷提示不會使用其他 CPU 資源控制，例如個別 VM cap、權數和保留。
不過，個別的 VM cap 很重要，因為我們稍後會看到一些。

為了簡單起見，假設每個 VM 都有1個副總裁，而我們的主機有 8 LPs。 我們會從空白主機開始。

若要建立「B」層，主機 adminstartor 會將群組上限設定為50%：

- G = n * C
- G = 8 * 50%
- G = 4 LP 對整個群組的 CPU 時間

主機管理員會新增單一「B」層 VM。
此時，我們的「B」層 VM 最多可使用50% 的主機 CPU，或在我們的範例系統中相當於4個 LPs。

現在，管理員會新增第二個「層 B」 VM。 CPU 群組的配置，會平均分散到所有 Vm。 我們總共在群組 B 中有2個 Vm，因此每個 VM 現在都有一半的群組 B 總計50%、25%，或等於2個 LPs 值得計算時間。

## <a name="setting-cpu-caps-on-individual-vms"></a>在個別 Vm 上設定 CPU 限定

除了群組上限，每個 VM 也可以有個別的「VM 端點」。 自其簡介以來，每個 VM 的 CPU 資源控制項（包括 CPU 限定、權數和保留）都是 Hyper-v 的一部分。
結合群組上限時，即使群組有可用的 CPU 資源，VM 上限也會指定每個 VP 可以取得的 CPU 最大數量。

例如，主機管理員可能會想要在 "C" Vm 上放置10% 的 VM 上限。
如此一來，即使大部分的 "C" VPs 都閒置，每副總 VP 也無法超過10%。
如果沒有 VM 上限，"C" Vm 可能會都伺機達到其層級所允許的等級以外的效能。

## <a name="isolating-vm-groups-to-specific-host-processors"></a>將 VM 群組隔離到特定主機處理器

Hyper-v 主機系統管理員可能也想要能夠將計算資源專用於 VM。
例如，假設系統管理員想要提供高階「A」 VM，其類別上限為100%。
這些 premium Vm 也需要最低的排程延遲和抖動;也就是說，它們可能不會被其他任何 VM 取消排程。
若要達到此分隔，也可以使用特定的 LP 親和性對應來設定 CPU 群組。

例如，若要在我們的範例中配合主機上的「A」 VM，系統管理員會建立新的 CPU 群組，並將群組的處理器親和性設定為主機 LPs 的子集。
群組 B 和 C 會相似化為到剩餘的 LPs。
系統管理員可以在群組 A 中建立單一 VM，然後對群組 A 中的所有 LPs 具有獨佔存取權，而較低層群組 B 和 C 則會共用剩餘的 LPs。

## <a name="segregating-root-vps-from-guest-vps"></a>將根 VPs 從來賓 VPs 中分離

根據預設，Hyper-v 會在每個基礎實體 LP 上建立根副總。
這些根 VPs 會嚴格對應1:1 與系統 LPs，而且不會遷移—也就是說，每個根副總都一律會在相同的實體 LP 上執行。
來賓 VPs 可在任何可用的 LP 上執行，且會與根 VPs 共用執行。

不過，您可能會想要完全區分根副總活動與來賓 VPs。
請考慮我們的範例，我們將在其中實行 premium 「A」層 VM。
為確保我們的「A」 VM 的 VPs 具有最低的可能延遲和「抖動」，或排程變化，我們想要在一組專用的 LPs 上執行，並確保根目錄不會在這些 LPs 上執行。

這可以使用 "minroot" 設定的組合來完成，這會限制主機 OS 磁碟分割在整體系統邏輯處理器的子集上執行，以及一或多個相似化為的 CPU 群組。

虛擬化主機可以設定為將主機磁碟分割限制為特定 LPs，並將一或多個 CPU 群組相似化為到剩餘的 LPs。
如此一來，根和來賓磁碟分割就可以在專用的 CPU 資源上執行，而且完全隔離，而不會有 CPU 共用。

如需有關 "minroot" 設定的詳細資訊，請參閱[Hyper-v 主機 CPU 資源管理](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-minroot-2016)。

## <a name="using-the-cpugroups-tool"></a>使用 CpuGroups 工具

讓我們看看一些如何使用 CpuGroups 工具的範例。

>[!NOTE]
>CpuGroups 工具的命令列參數只會使用空格做為分隔符號來傳遞。 沒有 '/' 或 '-' 字元應該繼續所需的命令列參數。

### <a name="discovering-the-cpu-topology"></a>探索 CPU 拓撲

使用 GetCpuTopology 執行 CpuGroups 會傳回目前系統的相關資訊，如下所示，包括 LP 索引、LP 所屬的 NUMA 節點、封裝和核心識別碼，以及根 VP 索引。

下列範例顯示的系統具有2個 CPU 通訊端和 NUMA 節點、總計 32 LPs 和多執行緒啟用，並已設定為啟用具有8個根 VPs 的 Minroot，每個 NUMA 節點4個。
具有根 VPs 的 LPs 具有 RootVpIndex >= 0;根磁碟分割無法使用 RootVpIndex 為-1 的 LPs，但仍受限於虛擬程式管理，並會執行其他設定的允許來賓 VPs。

```console
C:\vm\tools>CpuGroups.exe GetCpuTopology

LpIndex NodeNumber PackageId CoreId RootVpIndex
------- ---------- --------- ------ -----------
      0          0         0      0           0
      1          0         0      0           1
      2          0         0      1           2
      3          0         0      1           3
      4          0         0      2          -1
      5          0         0      2          -1
      6          0         0      3          -1
      7          0         0      3          -1
      8          0         0      4          -1
      9          0         0      4          -1
     10          0         0      5          -1
     11          0         0      5          -1
     12          0         0      6          -1
     13          0         0      6          -1
     14          0         0      7          -1
     15          0         0      7          -1
     16          1         1     16           4
     17          1         1     16           5
     18          1         1     17           6
     19          1         1     17           7
     20          1         1     18          -1
     21          1         1     18          -1
     22          1         1     19          -1
     23          1         1     19          -1
     24          1         1     20          -1
     25          1         1     20          -1
     26          1         1     21          -1
     27          1         1     21          -1
     28          1         1     22          -1
     29          1         1     22          -1
     30          1         1     23          -1
     31          1         1     23          -1
```

### <a name="example-2--print-all-cpu-groups-on-the-host"></a>範例2–列印主機上的所有 CPU 群組

在這裡，我們將列出目前主機上的所有 CPU 群組、其 GroupId、群組的 CPU 限定，以及指派給該群組的 LPs indicies。

請注意，有效的 CPU 限定值位於 [0，65536] 範圍內，而這些值會以百分比表示群組上限 (例如 32768 = 50% ) 。

```console
C:\vm\tools>CpuGroups.exe GetGroups

CpuGroupId                          CpuCap  LpIndexes
------------------------------------ ------ --------
36AB08CB-3A76-4B38-992E-000000000002 32768  4,5,6,7,8,9,10,11,20,21,22,23
36AB08CB-3A76-4B38-992E-000000000003 65536  12,13,14,15
36AB08CB-3A76-4B38-992E-000000000004 65536  24,25,26,27,28,29,30,31
```

### <a name="example-3--print-a-single-cpu-group"></a>範例3–列印單一 CPU 群組

在此範例中，我們將使用 GroupId 作為篩選準則來查詢單一 CPU 群組。

```console
C:\vm\tools>CpuGroups.exe GetGroups /GroupId:36AB08CB-3A76-4B38-992E-000000000003
CpuGroupId                          CpuCap   LpIndexes
------------------------------------ ------ ----------
36AB08CB-3A76-4B38-992E-000000000003 65536  12,13,14,15
```

### <a name="example-4--create-a-new-cpu-group"></a>範例4–建立新的 CPU 群組

在這裡，我們將建立新的 CPU 群組，並指定要指派給群組的群組識別碼和 LPs 集。

```console
C:\vm\tools>CpuGroups.exe CreateGroup /GroupId:36AB08CB-3A76-4B38-992E-000000000001 /GroupAffinity:0,1,16,17
```

現在會顯示新加入的群組。

```console
C:\vm\tools>CpuGroups.exe GetGroups
CpuGroupId                          CpuCap LpIndexes
------------------------------------ ------ ---------
36AB08CB-3A76-4B38-992E-000000000001 65536 0,1,16,17
36AB08CB-3A76-4B38-992E-000000000002 32768 4,5,6,7,8,9,10,11,20,21,22,23
36AB08CB-3A76-4B38-992E-000000000003 65536 12,13,14,15
36AB08CB-3A76-4B38-992E-000000000004 65536 24,25,26,27,28,29,30,31
```

### <a name="example-5--set-the-cpu-group-cap-to-50"></a>範例5–將 CPU 群組上限設定為50%

在這裡，我們會將 CPU 群組上限設定為50%。

```console
C:\vm\tools>CpuGroups.exe SetGroupProperty /GroupId:36AB08CB-3A76-4B38-992E-000000000001 /CpuCap:32768
```

現在讓我們藉由顯示剛剛更新的群組來確認我們的設定。

```console
C:\vm\tools>CpuGroups.exe GetGroups /GroupId:36AB08CB-3A76-4B38-992E-000000000001

CpuGroupId                          CpuCap LpIndexes
------------------------------------ ------ ---------
36AB08CB-3A76-4B38-992E-000000000001 32768 0,1,16,17
```

### <a name="example-6--print-cpu-group-ids-for-all-vms-on-the-host"></a>範例6–針對主機上的所有 Vm 列印 CPU 群組識別碼

```console
C:\vm\tools>CpuGroups.exe GetVmGroup

VmName                                 VmId                           CpuGroupId
------ ------------------------------------ ------------------------------------
    G2 4ABCFC2F-6C22-498C-BB38-7151CE678758 36ab08cb-3a76-4b38-992e-000000000002
    P1 973B9426-0711-4742-AD3B-D8C39D6A0DEC 36ab08cb-3a76-4b38-992e-000000000003
    P2 A593D93A-3A5F-48AB-8862-A4350E3459E8 36ab08cb-3a76-4b38-992e-000000000004
    G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200 36ab08cb-3a76-4b38-992e-000000000002
    G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC 36ab08cb-3a76-4b38-992e-000000000002
```

### <a name="example-7--unbind-a-vm-from-the-cpu-group"></a>範例7–解除 VM 與 CPU 群組的系結

若要從 CPU 群組中移除 VM，請將設定為 VM 的 CpuGroupId 為 Null GUID。 這會將 VM 從 CPU 群組中解除系結。

```console
C:\vm\tools>CpuGroups.exe SetVmGroup /VmName:g1 /GroupId:00000000-0000-0000-0000-000000000000

C:\vm\tools>CpuGroups.exe GetVmGroup
VmName                                 VmId                           CpuGroupId
------ ------------------------------------ ------------------------------------
    G2 4ABCFC2F-6C22-498C-BB38-7151CE678758 36ab08cb-3a76-4b38-992e-000000000002
    P1 973B9426-0711-4742-AD3B-D8C39D6A0DEC 36ab08cb-3a76-4b38-992e-000000000003
    P2 A593D93A-3A5F-48AB-8862-A4350E3459E8 36ab08cb-3a76-4b38-992e-000000000004
    G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200 36ab08cb-3a76-4b38-992e-000000000002
    G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC 00000000-0000-0000-0000-000000000000
```

### <a name="example-8--bind-a-vm-to-an-existing-cpu-group"></a>範例8–將 VM 系結至現有的 CPU 群組

在這裡，我們會將 VM 新增至現有的 CPU 群組。
請注意，VM 不得系結至任何現有的 CPU 群組，或設定 CPU 群組識別碼將會失敗。

```console
C:\vm\tools>CpuGroups.exe SetVmGroup /VmName:g1 /GroupId:36AB08CB-3A76-4B38-992E-000000000001
```

現在，確認 VM G1 位於所需的 CPU 群組中。

```console
C:\vm\tools>CpuGroups.exe GetVmGroup
VmName                                 VmId                           CpuGroupId
------ ------------------------------------ ------------------------------------
    G2 4ABCFC2F-6C22-498C-BB38-7151CE678758 36ab08cb-3a76-4b38-992e-000000000002
    P1 973B9426-0711-4742-AD3B-D8C39D6A0DEC 36ab08cb-3a76-4b38-992e-000000000003
    P2 A593D93A-3A5F-48AB-8862-A4350E3459E8 36ab08cb-3a76-4b38-992e-000000000004
    G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200 36ab08cb-3a76-4b38-992e-000000000002
    G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC 36AB08CB-3A76-4B38-992E-000000000001
```

### <a name="example-9--print-all-vms-grouped-by-cpu-group-id"></a>範例 9-列印依 CPU 群組識別碼分組的所有 Vm

```console
C:\vm\tools>CpuGroups.exe GetGroupVms
CpuGroupId                           VmName                                 VmId
------------------------------------ ------ ------------------------------------
36AB08CB-3A76-4B38-992E-000000000001     G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC
36ab08cb-3a76-4b38-992e-000000000002     G2 4ABCFC2F-6C22-498C-BB38-7151CE678758
36ab08cb-3a76-4b38-992e-000000000002     G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200
36ab08cb-3a76-4b38-992e-000000000003     P1 973B9426-0711-4742-AD3B-D8C39D6A0DEC
36ab08cb-3a76-4b38-992e-000000000004     P2 A593D93A-3A5F-48AB-8862-A4350E3459E8
```

### <a name="example-10--print-all-vms-for-a-single-cpu-group"></a>範例10–列印單一 CPU 群組的所有 Vm

```console
C:\vm\tools>CpuGroups.exe GetGroupVms /GroupId:36ab08cb-3a76-4b38-992e-000000000002

CpuGroupId                           VmName                                VmId
------------------------------------ ------ ------------------------------------
36ab08cb-3a76-4b38-992e-000000000002     G2 4ABCFC2F-6C22-498C-BB38-7151CE678758
36ab08cb-3a76-4b38-992e-000000000002     G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200
```

### <a name="example-11--attempting-to-delete-a-non-empty-cpu-group"></a>範例11–嘗試刪除非空白的 CPU 群組

只能刪除空白的 CPU 群組（也就是沒有系結 Vm 的 CPU 群組）。
嘗試刪除非空白的 CPU 群組將會失敗。

```console
C:\vm\tools>CpuGroups.exe DeleteGroup /GroupId:36ab08cb-3a76-4b38-992e-000000000001
(null)
Failed with error 0xc0350070
```

### <a name="example-12--unbind-the-only-vm-from-a-cpu-group-and-delete-the-group"></a>範例12–從 CPU 群組解除系結唯一的 VM，並刪除群組

在此範例中，我們將使用數個命令來檢查 CPU 群組、移除屬於該群組的單一 VM，然後刪除該群組。

首先，讓我們列舉群組中的 Vm。

```console
C:\vm\tools>CpuGroups.exe GetGroupVms /GroupId:36AB08CB-3A76-4B38-992E-000000000001
CpuGroupId                           VmName                                VmId
------------------------------------ ------ ------------------------------------
36AB08CB-3A76-4B38-992E-000000000001     G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC
```

我們看到只有名為 G1 的單一 VM 屬於這個群組。
讓我們將 VM 的群組識別碼設定為 Null，以將 G1 VM 從群組中移除。

```console
C:\vm\tools>CpuGroups.exe SetVmGroup /VmName:g1 /GroupId:00000000-0000-0000-0000-000000000000
```

並驗證我們的變更 .。。

```console
C:\vm\tools>CpuGroups.exe GetVmGroup /VmName:g1
VmName                                 VmId                           CpuGroupId
------ ------------------------------------ ------------------------------------
    G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC 00000000-0000-0000-0000-000000000000
```

既然群組是空的，我們就可以安全地刪除它。

```console
C:\vm\tools>CpuGroups.exe DeleteGroup /GroupId:36ab08cb-3a76-4b38-992e-000000000001
```

並確認我們的群組已消失。

```console
C:\vm\tools>CpuGroups.exe GetGroups
CpuGroupId                          CpuCap                     LpIndexes
------------------------------------ ------ -----------------------------
36AB08CB-3A76-4B38-992E-000000000002 32768  4,5,6,7,8,9,10,11,20,21,22,23
36AB08CB-3A76-4B38-992E-000000000003 65536  12,13,14,15
36AB08CB-3A76-4B38-992E-000000000004 65536 24,25,26,27,28,29,30,31
```

### <a name="example-13--bind-a-vm-back-to-its-original-cpu-group"></a>範例13–將 VM 系結回到其原始的 CPU 群組

```console
C:\vm\tools>CpuGroups.exe SetVmGroup /VmName:g1 /GroupId:36AB08CB-3A76-4B38-992E-000000000002

C:\vm\tools>CpuGroups.exe GetGroupVms
CpuGroupId VmName VmId
------------------------------------ -------------------------------- ------------------------------------
36ab08cb-3a76-4b38-992e-000000000002 G2 4ABCFC2F-6C22-498C-BB38-7151CE678758
36ab08cb-3a76-4b38-992e-000000000002 G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200
36AB08CB-3A76-4B38-992E-000000000002 G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC
36ab08cb-3a76-4b38-992e-000000000003 P1 973B9426-0711-4742-AD3B-D8C39D6A0DEC
36ab08cb-3a76-4b38-992e-000000000004 P2 A593D93A-3A5F-48AB-8862-A4350E3459E8
```
