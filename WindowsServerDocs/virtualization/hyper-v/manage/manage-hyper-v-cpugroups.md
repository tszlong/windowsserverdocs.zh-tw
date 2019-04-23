---
title: 虛擬機器資源控制
description: 使用 VM CPU 群組
keywords: windows 10, hyper-v
author: allenma
ms.date: 06/18/2018
ms.topic: article
ms.prod: windows-10-hyperv
ms.service: windows-10-hyperv
ms.assetid: cc7bb88e-ae75-4a54-9fb4-fc7c14964d67
ms.openlocfilehash: 7c4ddf3e5d2ff58eef844c50960327c27a3e0a3d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854759"
---
>適用於：Windows Server 2016、 Microsoft HYPER-V Server 2016 中，Windows Server 2019，Microsoft HYPER-V Server 2019

# <a name="virtual-machine-resource-controls"></a>虛擬機器資源控制

本文說明適用於虛擬機器的 HYPER-V 資源以及隔離控制項。  這些功能，我們將其稱為虛擬機器的 CPU 群組或只是 「 CPU 群組 」，是 Windows Server 2016 中導入。  CPU 群組可讓 HYPER-V 系統管理員，以更佳地管理和客體虛擬機器之間配置主機的 CPU 資源。  使用 CPU 群組，HYPER-V 系統管理員可以：

* 以不同的配置的虛擬化主機的總 CPU 資源，在整個群組之間共用每個群組建立的虛擬機器的群組。 這可讓主應用程式系統管理員，來實作服務的不同 Vm 類型的類別。

* 設定 CPU 資源限制到特定的群組。 此 「 群組上限 」 設定主機的上限整個群組可能會耗用，有效地強制執行所需的類別，該群組的服務的 CPU 資源。

* 限制只在一組特定的主機系統的處理器上執行的 CPU 群組。 這可用來隔離 Vm 都屬於不同的 CPU 群組彼此。

## <a name="managing-cpu-groups"></a>管理 CPU 群組

透過 HYPER-V 主機計算服務或 HCS 受到 CPU 群組。 很棒的 HCS，其發生，描述 HCS Api，以及更多的連結可用於在張貼的 Microsoft 虛擬化小組部落格[簡介主機計算服務 (HCS)](https://blogs.technet.microsoft.com/virtualization/2017/01/27/introducing-the-host-compute-service-hcs/)。

>[!NOTE] 
>HCS 只可用來建立及管理 CPU 群組;[HYPER-V 管理員] 小程式中，WMI 與 PowerShell 管理介面不支援 CPU 群組。

Microsoft 提供命令列公用程式、 cpugroups.exe，在[Microsoft 下載中心](https://go.microsoft.com/fwlink/?linkid=865968)使用 HCS 介面來管理 CPU 群組。  此公用程式也可以顯示主機的 CPU 拓撲。

## <a name="how-cpu-groups-work"></a>CPU 群組工作的方式

HYPER-V hypervisor，使用計算的 CPU 群組端點的主機 CPU 群組間的計算資源配置會強制執行。 CPU 群組端點是一小部分的 CPU 群組的 CPU 容量總計。 群組端點的值取決於群組類別或層級指派的優先權。 計算的群組上限可以視為 「 次數的 CPU 時間值得 LP's 」。 此群組預算被共用，因此如果只有單一 VM 正在作用中，可以使用自己的整個群組的 CPU 配置。

CPU 群組端點的計算方式為 G = *n* x *C*，其中：

    *G* is the amount of host LP we'd like to assign to the group
    *n* is the total number of logical processors (LPs) in the group
    *C* is the maximum CPU allocation — that is, the class of service desired for the group, expressed as a percentage of the system’s total compute capacity

例如，請考慮設定 4 個邏輯處理器 (LPs)，和限制的 50%的 CPU 群組。

    G = n * C
    G = 4 * 50%
    G = 2 LP's worth of CPU time for the entire group

在此範例中，CPU 群組 G 配置 CPU 時間的 2 個的 LP's 值得。  

請注意，不論虛擬機器或群組，結合的虛擬處理器數目，不論狀態為何，適用於群組端點 (例如關機或啟動) 的虛擬機器指派給 CPU 群組。 因此，繫結至相同的 CPU 群組每個 VM 會接收的總 CPU 配置的群組，一小部分，這會變更繫結到 CPU 群組的 Vm 數目。 因此，Vm 會繫結，或從 CPU 群組未繫結的 Vm，整體的 CPU 群組上限必須進行調整並設定為產生的每個 VM 端點所需的維護。 VM 主機的系統管理員或虛擬化管理軟體層會負責管理群組上限視達到所需的每個 VM 的 CPU 資源配置。

## <a name="example-classes-of-service"></a>服務的範例類別

讓我們看看一些簡單的範例。 若要開始，假設 HYPER-V 主機系統管理員想要支援的客體 Vm 的服務的兩個層級：

1. 低階的"C"層中。 我們將提供這一層的整個主機的計算資源的 10%。

1. 中低規模"B"層中。 此層會配置 50%的整個主機的計算資源。

此時在本例中我們會判斷提示，沒有其他 CPU 資源控制項中使用，例如個別 VM 的大寫字，加權值，而保留。
不過，個別 VM cap 很重要，稍後如稍後所示。

為了簡單起見，我們假設每個 VM 有 1 副總裁，和我們的主機具有 8 LPs。 我們將開始空白的主機。

若要建立 「 B 」 層，主機 adminstartor 群組將上限設定為 50%:

    G = n * C
    G = 8 * 50%
    G = 4 LP's worth of CPU time for the entire group

主機系統管理員會將單一的 「 B 」 層 VM。
到目前為止，我們 「 B 」 層 VM 可以使用最多 50%值得的主機 CPU 或相當的 4 個 LPs 我們範例中的系統中。

現在，系統管理員新增第二個 「 層 B 」 VM。 CPU 群組的配置，會平均分配的所有 Vm。 我們 2 個 Vm 的總計群組 B 中，因此每個 VM 現在取得的群組 B 的總計 50%、 25%或相當於 2 LPs 值得運算時間的一半。

## <a name="setting-cpu-caps-on-individual-vms"></a>設定個別的 Vm 上的 CPU 上限

除了群組端點，每個 VM 也可以有個別 「 VM 上限 」。 每個 VM 的 CPU 資源的控制項，包括 CPU 上限、 重量，以及保留，自問世以來已經對 HYPER-V 的一部分。
與群組端點結合時，VM 的端點會指定可以取得每個副總裁，CPU 的最大數量，即使群組具有可用的 CPU 資源。

比方說，主應用程式系統管理員可能會想要放在"C"的 Vm 上的 10%的 VM 端點。
如此一來，即使大部分的"C"VPs 處於閒置狀態，每個副總裁可能永遠不會取得超過 10%。
沒有 VM 上限，"C"的 Vm 都伺機無法達到超過其層所允許的層級的效能。

## <a name="isolating-vm-groups-to-specific-host-processors"></a>隔離的 VM 群組，以特定的主機處理器

HYPER-V 主機系統管理員也可以提供 vm 的計算資源的能力。
例如，假設有系統管理員想要提供進階"A"VM 具有類別上限為 100%。
這些進階 Vm 也需要最低排程的延遲和抖動也就是說，它們可能會取消排程的任何其他 VM。
若要達到這種區隔，CPU 群組也可以設定使用特定的 LP 親合型對應。

比方說，以符合我們的範例中的主機上的"A"VM，系統管理員會建立新的 CPU 群組，並設定主機的 LPs 子集群組的處理器親和性。
群組 B 和 C 會相似化為剩餘 LPs。
系統管理員可以建立會有獨佔存取權所有 LPs 群組 A，而可能較低層群組 B 中的群組 A 中的單一 VM 和 C 會共用剩餘 LPs。

## <a name="segregating-root-vps-from-guest-vps"></a>分離客體 VPs 從根 VPs

根據預設，HYPER-V 會在每個基礎實體 LP 建立根副總裁。
這些根目錄 VPs 嚴格對應與 1:1 系統 LPs，作業，而且不會移轉，也就是每個根副總裁將一律在上執行相同的實體 LP。
客體 VPs 可能會執行上任何可用的 LP，而且會與根 VPs 共用執行。

不過，可能是因為需要完全不同的根副總裁活動來自來賓 VPs。
請考慮上面我們用來實作進階"A"層 VM 的範例。
若要確保"A"VM 的 VPs 具有最低延遲和"抖動"，或排程的變化，我們想要在一組專用 LPs 上加以執行，並確定根本不會執行這些 LPs。

這可以使用"minroot 」 組態，這會限制主機 OS 分割區的總系統邏輯處理器，以及一個子集上執行的組合來完成，或多個相似化 CPU 群組。

虛擬化主機可以設定來限制特定 LPs，相似化到其餘 LPs 的一或多個 CPU 群組與主機磁碟分割。
如此一來，根和客體的分割區可以在專用的 CPU 資源上執行，並完全隔離且無 CPU 共用。

如需有關 「 minroot 」 組態的詳細資訊，請參閱[HYPER-V 主機 CPU 資源管理](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-minroot-2016)。

## <a name="using-the-cpugroups-tool"></a>使用 CpuGroups 工具

讓我們看看如何使用 CpuGroups 工具的一些範例。

>[!NOTE] 
>CpuGroups 工具的命令列參數會傳遞做為分隔符號使用僅含空格。 沒有 '/' 或 '-' 字元應該繼續進行所需的命令列參數。

### <a name="discovering-the-cpu-topology"></a>探索 CPU 拓樸

執行與 GetCpuTopology CpuGroups 會傳回目前的系統的相關資訊，包括 LP 索引 LP 所屬，封裝核心識別碼和根目錄副總裁索引的 NUMA 節點，如下所示。

下列範例示範具有 2 個 CPU 通訊端的系統和 NUMA 節點，總共 32 LPs，和多執行緒處理，啟用，而且設定為啟用 Minroot 8 根目錄 VPs，從每個 NUMA 節點 4。
具有根 VPs LPs 有 RootVpIndex > = 0;使用-1 RootVpIndex LPs 不適用於根磁碟分割，但仍由 hypervisor，允許的其他組態設定，將執行客體 VPs。

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

### <a name="example-2--print-all-cpu-groups-on-the-host"></a>範例 2 – 列印在主機上的所有 CPU 群組

在這裡，我們將會列出目前的主控件、 其 GroupId、 群組的 CPU 上限和 LPs 指派給該群組的索引上的所有 CPU 群組。

請注意，CPU 上限有效值的範圍 [0，65536]，而且這些值 express 群組端點，以百分比表示 (例如 32768 = 50%)。

```console
C:\vm\tools>CpuGroups.exe GetGroups

CpuGroupId                          CpuCap  LpIndexes
------------------------------------ ------ --------
36AB08CB-3A76-4B38-992E-000000000002 32768  4,5,6,7,8,9,10,11,20,21,22,23
36AB08CB-3A76-4B38-992E-000000000003 65536  12,13,14,15
36AB08CB-3A76-4B38-992E-000000000004 65536  24,25,26,27,28,29,30,31
```

### <a name="example-3--print-a-single-cpu-group"></a>範例 3 – 列印單一 CPU 群組

在此範例中，我們將查詢使用擁有的 GroupId，做為篩選條件的單一 CPU 群組。

```console
C:\vm\tools>CpuGroups.exe GetGroups /GroupId:36AB08CB-3A76-4B38-992E-000000000003
CpuGroupId                          CpuCap   LpIndexes
------------------------------------ ------ ----------
36AB08CB-3A76-4B38-992E-000000000003 65536  12,13,14,15
```

### <a name="example-4--create-a-new-cpu-group"></a>範例 4 – 建立新的 CPU 群組

在這裡，我們將建立新的 CPU 群組，指定要指派給群組的 群組識別碼和 LPs 組。

```console
C:\vm\tools>CpuGroups.exe CreateGroup /GroupId:36AB08CB-3A76-4B38-992E-000000000001 /GroupAffinity:0,1,16,17
```

現在會顯示我們新加入的群組。

```console
C:\vm\tools>CpuGroups.exe GetGroups
CpuGroupId                          CpuCap LpIndexes
------------------------------------ ------ ---------
36AB08CB-3A76-4B38-992E-000000000001 65536 0,1,16,17
36AB08CB-3A76-4B38-992E-000000000002 32768 4,5,6,7,8,9,10,11,20,21,22,23
36AB08CB-3A76-4B38-992E-000000000003 65536 12,13,14,15
36AB08CB-3A76-4B38-992E-000000000004 65536 24,25,26,27,28,29,30,31
```

### <a name="example-5--set-the-cpu-group-cap-to-50"></a>範例 5-設定為 50%的 CPU 群組上限

在這裡，我們會設定 CPU 群組上限為 50%。

```console
C:\vm\tools>CpuGroups.exe SetGroupProperty /GroupId:36AB08CB-3A76-4B38-992E-000000000001 /CpuCap:32768
```

現在讓我們確認我們設定顯示我們剛更新的群組。

```console
C:\vm\tools>CpuGroups.exe GetGroups /GroupId:36AB08CB-3A76-4B38-992E-000000000001

CpuGroupId                          CpuCap LpIndexes
------------------------------------ ------ ---------
36AB08CB-3A76-4B38-992E-000000000001 32768 0,1,16,17
```

### <a name="example-6--print-cpu-group-ids-for-all-vms-on-the-host"></a>範例 6 – 主機上的所有 Vm 的列印 CPU 群組識別碼

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

### <a name="example-7--unbind-a-vm-from-the-cpu-group"></a>範例 7 – 解除繫結從 CPU 群組的 VM

若要從 CPU 群組中移除 VM，設定 VM 的 CpuGroupId 為 NULL 的 GUID。 這會將 CPU 群組從 VM 解除繫結。

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

### <a name="example-8--bind-a-vm-to-an-existing-cpu-group"></a>範例 8 – 繫結至現有的 CPU 群組的 VM

在這裡，我們會將 VM 加入現有的 CPU 群組。
請注意，VM 必須未繫結至任何現有的 CPU 群組，或者設定 CPU 群組識別碼將會失敗。

```console
C:\vm\tools>CpuGroups.exe SetVmGroup /VmName:g1 /GroupId:36AB08CB-3A76-4B38-992E-000000000001
```

現在，請確認 VM G1 處於所需的 CPU 群組。

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

### <a name="example-9--print-all-vms-grouped-by-cpu-group-id"></a>範例 9 – 列印依 CPU 群組識別碼分組的所有 Vm

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

### <a name="example-10--print-all-vms-for-a-single-cpu-group"></a>範例 10 – 列印單一 CPU 群組的所有 Vm

```console
C:\vm\tools>CpuGroups.exe GetGroupVms /GroupId:36ab08cb-3a76-4b38-992e-000000000002

CpuGroupId                           VmName                                VmId
------------------------------------ ------ ------------------------------------
36ab08cb-3a76-4b38-992e-000000000002     G2 4ABCFC2F-6C22-498C-BB38-7151CE678758
36ab08cb-3a76-4b38-992e-000000000002     G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200
```

### <a name="example-11--attempting-to-delete-a-non-empty-cpu-group"></a>範例 11 – 嘗試進行刪除非空白的 CPU 群組

只有空的 CPU 群組 — 也就是 CPU 群組未繫結的 Vm，才能刪除。
嘗試刪除非空白的 CPU 群組將會失敗。

```console
C:\vm\tools>CpuGroups.exe DeleteGroup /GroupId:36ab08cb-3a76-4b38-992e-000000000001
(null)
Failed with error 0xc0350070
```

### <a name="example-12--unbind-the-only-vm-from-a-cpu-group-and-delete-the-group"></a>範例 12 – 從 CPU 群組的唯一 VM 解除繫結，並刪除群組

在此範例中，我們將使用數個命令來檢查 CPU 群組，請移除屬於該群組中，單一 VM，並刪除群組。

首先，讓我們來列舉在我們的群組中的 Vm。

```console
C:\vm\tools>CpuGroups.exe GetGroupVms /GroupId:36AB08CB-3A76-4B38-992E-000000000001
CpuGroupId                           VmName                                VmId
------------------------------------ ------ ------------------------------------
36AB08CB-3A76-4B38-992E-000000000001     G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC
```

我們可以看到只有單一 VM，名為 G1，屬於此群組。
藉由設定為 NULL 的 VM 群組識別碼，讓我們從我們的群組移除 G1 VM。

```console
C:\vm\tools>CpuGroups.exe SetVmGroup /VmName:g1 /GroupId:00000000-0000-0000-0000-000000000000
```

並驗證變更...

```console
C:\vm\tools>CpuGroups.exe GetVmGroup /VmName:g1
VmName                                 VmId                           CpuGroupId
------ ------------------------------------ ------------------------------------
    G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC 00000000-0000-0000-0000-000000000000
```

現在，群組是空的我們可以安全地刪除它。

```console
C:\vm\tools>CpuGroups.exe DeleteGroup /GroupId:36ab08cb-3a76-4b38-992e-000000000001
```

並確認我們的團隊都不見了。

```console
C:\vm\tools>CpuGroups.exe GetGroups
CpuGroupId                          CpuCap                     LpIndexes
------------------------------------ ------ -----------------------------
36AB08CB-3A76-4B38-992E-000000000002 32768  4,5,6,7,8,9,10,11,20,21,22,23
36AB08CB-3A76-4B38-992E-000000000003 65536  12,13,14,15
36AB08CB-3A76-4B38-992E-000000000004 65536 24,25,26,27,28,29,30,31
```

### <a name="example-13--bind-a-vm-back-to-its-original-cpu-group"></a>回到其原始的 CPU 群組範例 13 – 繫結的 VM

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
