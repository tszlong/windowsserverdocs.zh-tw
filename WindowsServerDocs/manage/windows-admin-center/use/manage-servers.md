---
title: 使用 Windows 系統管理中心管理伺服器
description: 使用 Windows 系統管理中心管理伺服器（Project 檀香山）
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 11/21/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: ddc8eea67cde9d6677836af1201e169c911e77e0
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950486"
---
# <a name="manage-servers-with-windows-admin-center"></a>使用 Windows 系統管理中心管理伺服器

>適用於：Windows Admin Center、Windows Admin Center 預覽版

> [!Tip]
> 對 Windows Admin Center 不熟悉？
> [下載或深入瞭解 Windows 系統管理中心](../overview.md)。

## <a name="managing-windows-server-machines"></a>管理 Windows Server 機器

您可以將執行 Windows Server 2012 或更新版本的個別伺服器新增到 Windows 管理中心，以一組完整的工具（包括憑證、裝置、事件、進程、角色和功能、更新虛擬機器等等）來管理伺服器。

![伺服器連接總覽畫面](../media/manage-servers/server-overview.png)

## <a name="adding-a-server-to-windows-admin-center"></a>將伺服器新增至 Windows 管理中心

若要將伺服器新增至 Windows 系統管理中心：

1. 按一下 [所有連接] 底下的 [ **+ 新增**]。
2. 加入宣告**伺服器連接**。
3. 輸入伺服器的名稱，如果出現提示，則為要使用的認證。
4. 按一下 [**提交**] 完成。

伺服器將會新增至 [總覽] 頁面上的連接清單。 按一下以連接到伺服器。

> [!NOTE]
> 您也可以將[容錯移轉](manage-failover-clusters.md)叢集或[超](manage-hyper-converged.md)交集叢集新增為 Windows 系統管理中心內的個別連接。

## <a name="tools"></a>工具

下列工具適用于伺服器連接：

| 工具 | 說明 |
| ---- | ----------- |
| [概觀](#overview) | View server 詳細資料和控制伺服器狀態 |
| [Active Directory](#active-directory-preview) | 管理 Active Directory |
| [Backup](#backup) | 查看和設定 Azure 備份 |  
| [憑證](#certificates) | 查看和修改憑證 |
| [容器](#containers) | 檢視容器 |
| [裝置](#devices) | 查看和修改裝置 |
| [DHCP](#dhcp) | 查看和管理 DHCP 伺服器設定 |
| [DNS](#dns) | 查看和管理 DNS 伺服器設定 |
| [事件](#events) | 檢視活動 |
| [檔案](#files) | 流覽檔案和資料夾 |
| [防火牆](#firewall) | 查看和修改防火牆規則 |
| [已安裝的應用程式](#installed-apps) | 查看和移除已安裝的應用程式 |
| [本機使用者和群組](#local-users-and-groups) | 查看和修改本機使用者和群組 |
| [網路](#network) | 查看和修改網路裝置 |
| [封包監視](https://aka.ms/wac1908) | 監視網路封包 |
| [效能監視器](https://aka.ms/perfmon-blog) | 查看效能計數器和報告 |
| [PowerShell](#powershell) | 透過 PowerShell 與伺服器互動 |
| [處理序](#processes) | 查看和修改執行中的進程 |
| [Registry](#registry) | 查看和修改登錄專案 |
| [遠端桌面](#remote-desktop) | 透過遠端桌面與伺服器互動 |
| [角色和功能](#roles-and-features) | 查看和修改角色和功能 |
| [排定的工作](#scheduled-tasks) | 查看和修改排定的工作 |
| [服務](#services) | 查看和修改服務 |
| [設定](#settings) | 查看和修改服務 |
| [儲存空間](#storage) | 查看和修改存放裝置 |
| [儲存體遷移服務](#storage-migration-service) | 將伺服器和檔案共用遷移至 Azure 或 Windows Server 2019 |
| [儲存體複本](#storage-replica) | 使用儲存體複本來管理伺服器對伺服器儲存體複寫 |
| [系統深入解析](#system-insights) | 系統深入解析可讓您更深入瞭解伺服器的運作。 |
| [更新](#updates) | 已安裝視圖並檢查是否有新的更新 |
| [虛擬機器](manage-virtual-machines.md) | 查看和管理虛擬機器 |
| [虛擬交換器](#virtual-switches) | 查看和管理虛擬交換器 |

## <a name="overview"></a>概觀

「**總覽**」可讓您查看 CPU、記憶體和網路效能的目前狀態，以及執行作業及修改目的電腦或伺服器上的設定。

### <a name="features"></a>功能

伺服器管理員總覽支援下列功能：

- View server 詳細資料
- 查看 CPU 活動
- 查看記憶體活動
- 查看網路活動
- 重新啟動伺服器
- 關閉伺服器
- 在伺服器上啟用磁片計量
- 編輯服務器上的電腦識別碼
- 以超連結流覽 BMC IP 位址（需要與 IPMI 相容的 BMC）。

[**查看伺服器總覽的意見反應和建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BOverview%5D)。

## <a name="active-directory-preview"></a>Active Directory （預覽）

**Active Directory**是在[擴充](../configure/using-extensions.md)功能摘要上提供的早期預覽。

### <a name="features"></a>功能

以下是可用的 Active Directory 管理：

- 建立使用者
- 建立群組
- 搜尋使用者、電腦和群組
- 在方格中選取時，使用者、電腦和群組的詳細資料窗格
- 全域方格動作使用者、電腦和群組（停用/啟用、移除）
- 重設使用者密碼
- 使用者物件：設定 & 群組成員資格的基本屬性
- 電腦物件：設定對單一電腦的委派
- 群組物件：管理成員資格（一次新增/移除1個使用者）  

[**查看 Active Directory 的意見反應和建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BActive%20Directory%5D)。

## <a name="backup"></a>[備份]

**備份**可讓您將伺服器直接備份到 Microsoft Azure，來保護您的 Windows server 免于損毀、遭受攻擊或嚴重損壞。
[深入瞭解 Azure 備份。](https://aka.ms/windows-admin-center-backup)

[在 Windows 系統管理中心提供備份的意見反應](https://aka.ms/backup-wac-feedback)

### <a name="features"></a>功能

備份支援下列功能：

- 觀看 Azure 備份狀態的總覽
- 設定備份專案和排程
- 啟動或停止備份作業
- 查看備份作業歷程記錄和狀態
- 查看復原點並復原資料
- 刪除備份資料

## <a name="certificates"></a>憑證

**憑證**可讓您管理電腦或伺服器上的憑證存放區。

### <a name="features"></a>功能

憑證支援下列功能：

- 流覽及搜尋現有的憑證
- 查看憑證詳細資料
- 匯出憑證
- 更新憑證
- 要求新的憑證
- 刪除憑證

[**查看憑證的意見反應和建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BCertificates%5D)。

## <a name="containers"></a>容器

**容器**可讓您查看 Windows Server 容器主機上的容器。 在執行 Windows Server Core 容器的情況下，您可以查看事件記錄檔，並存取容器的 CLI。

[**查看容器的意見反應和建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BContainers%5D)。

## <a name="devices"></a>[裝置]

**裝置**可讓您管理電腦或伺服器上已連線的裝置。

### <a name="features"></a>功能

裝置支援下列功能：

- 流覽及搜尋裝置
- 檢視裝置詳細資料
- 停用裝置
- 更新裝置上的驅動程式

[**查看裝置的意見反應和建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDevices%5D)。

## <a name="dhcp"></a>DHCP

**DHCP**可讓您管理電腦或伺服器上已連線的裝置。

### <a name="features"></a>功能

- 建立/設定/查看 IPV4 和 IPV6 範圍
- 建立位址排除專案，並設定開始和結束 IP 位址
- 建立位址保留並設定用戶端 MAC 位址（IPV4）、DUID 和 IAID （IPV6）

[**查看 DHCP 的意見反應和建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDHCP%5D)。

## <a name="dns"></a>DNS

**DNS**可讓您管理電腦或伺服器上已連線的裝置。

### <a name="features"></a>功能

- 查看 DNS 正向對應區域、反向對應區域和 DNS 記錄的詳細資料
- 建立正向對應區域（主要、次要或存根），並設定正向對應區域屬性
- 建立主機（A 或 AAAA）、CNAME 或 MX 類型的 DNS 記錄
- 設定 DNS 記錄屬性
- 建立 IPV4 和 IPV6 反向對應區域（主要、次要和存根）、設定反向對應區域屬性
- 在反向對應區域底下，建立 DNS 記錄的 PTR、CNAME 類型。

[**查看 DHCP 的意見反應和建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDNS%5D)。

## <a name="events"></a>事件

**事件**可讓您管理電腦或伺服器上的事件記錄檔。

### <a name="features"></a>功能

事件支援下列功能：

- 流覽和搜尋事件
- 檢視事件詳細資料
- 清除記錄檔中的事件
- 從記錄檔匯出事件

[**查看事件的意見反應和建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BEvents%5D)。

## <a name="files"></a>Files

檔案**可讓您**管理電腦或伺服器上的檔案和資料夾。

### <a name="features"></a>功能

檔案支援下列功能：

- 流覽檔案和資料夾
- 搜尋檔案或資料夾
- 建立新的資料夾
- 刪除檔案或資料夾
- 下載檔案或資料夾
- 上傳檔案或資料夾
- 重新命名檔案或資料夾
- 解壓縮 zip 檔案
- View file 或 folder 屬性
- 新增、編輯或移除檔案共用
- 修改檔案共用上的使用者和群組許可權

[**查看檔案的意見反應和建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BFiles%5D)。

## <a name="firewall"></a>防火牆

**防火牆**可讓您管理電腦或伺服器上的防火牆設定和規則。

### <a name="features"></a>功能

防火牆支援下列功能：

- 觀看防火牆設定的總覽
- 查看傳入的防火牆規則
- 查看外寄的防火牆規則
- 搜尋防火牆規則
- 查看防火牆規則詳細資料
- 建立新的防火牆規則
- 啟用或停用防火牆規則
- 刪除防火牆規則
- 編輯防火牆規則的屬性

[**查看防火牆的意見反應和建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BFirewall%5D)。

## <a name="installed-apps"></a>已安裝的應用程式

**已安裝的應用**程式可讓您列出和卸載已安裝的應用程式。

[**查看已安裝應用程式的意見反應和建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BInstalled%20Apps%5D)。

## <a name="local-users-and-groups"></a>本機使用者和群組

**本機使用者和群組**可讓您管理存在於電腦或伺服器本機的安全性群組和使用者。

### <a name="features"></a>功能

本機使用者和群組支援下列功能：

- 查看及搜尋使用者和群組
- 建立新的使用者或群組
- 管理使用者的群組成員資格
- 刪除使用者或群組
- 變更使用者的密碼
- 編輯使用者或群組的屬性

[**查看本機使用者和群組的意見反應和建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BLocal%20users%20and%20Groups%5D)

## <a name="network"></a>Network

**網路**可讓您管理電腦或伺服器上的網路裝置和設定。

### <a name="features"></a>功能

網路支援下列功能：

- 流覽及搜尋現有的網路介面卡
- 查看網路介面卡的詳細資料
- 編輯網路介面卡的屬性
- 建立[Azure 網路介面卡（預覽功能）](https://blogs.technet.microsoft.com/networking/2018/09/05/azurenetworkadapter/)

[**查看網路的意見反應和建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BNetwork%5D)

## <a name="powershell"></a>PowerShell

**Powershell**可讓您透過 powershell 會話與電腦或伺服器進行互動。

### <a name="features"></a>功能

PowerShell 支援下列功能：

- 在伺服器上建立互動式 PowerShell 會話
- 從伺服器上的 PowerShell 會話中斷連線

[**查看 PowerShell 的意見反應和建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BPowerShell%5D)

## <a name="processes"></a>處理程序

**進程**可讓您管理電腦或伺服器上執行中的進程。

### <a name="features"></a>功能

程式支援下列功能：

- 流覽及搜尋執行中的進程
- 查看進程詳細資料
- 啟動處理序
- 結束處理程序
- 建立進程傾印
- 尋找進程控制碼

[**查看處理常式的意見反應和建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BProcesses%5D)。

## <a name="registry"></a>登錄

登錄**可讓您**管理電腦或伺服器上的登錄機碼和值。

### <a name="features"></a>功能

登錄中支援下列功能：

- 流覽登錄機碼和值
- 新增或修改登錄值
- 刪除登錄值

[**查看登錄的意見反應和建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRegistry%5D)。

## <a name="remote-desktop"></a>遠端桌面

**遠端桌面**可讓您透過互動式桌面會話與電腦或伺服器進行互動。

### <a name="features"></a>功能

遠端桌面支援下列功能：

- 啟動互動式遠端桌面會話
- 中斷與遠端桌面會話的連線
- 將 Ctrl + Alt + Del 傳送至遠端桌面會話

[**查看遠端桌面的意見反應和建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRemote%20Desktop%5D)。

## <a name="roles-and-features"></a>角色和功能

**角色和功能**可讓您管理伺服器上的角色和功能。

### <a name="features"></a>功能

角色和功能支援下列功能：

- 流覽伺服器上的角色和功能清單
- 查看角色或功能詳細資料
- 安裝角色或功能
- 移除角色或功能

[**查看角色和功能的意見反應和建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRoles%20and%20Features%5D)。

## <a name="scheduled-tasks"></a>排定的工作

**排定**的工作可讓您管理電腦或伺服器上的排程工作。

### <a name="features"></a>功能

已排程的工作中支援下列功能：

- 流覽工作排程器程式庫
- 編輯排程的工作
- 啟用 & 停用已排程的工作
- 啟動 & 停止排程的工作
- 建立排程工作

[**查看已排程工作的意見反應和建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BScheduled%20Tasks%5D)。

## <a name="services"></a>服務

**服務**可讓您管理電腦或伺服器上的服務。

### <a name="features"></a>功能

服務支援下列功能：

- 流覽及搜尋伺服器上的服務
- 查看服務的詳細資料
- 啟動服務
- 暫停服務
- 編輯服務的屬性

[**查看服務的意見反應和建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BServices%5D)。

## <a name="settings"></a>[設定]

**設定**是在電腦或伺服器上管理設定的集中位置。

### <a name="features"></a>功能

- 查看和修改使用者和系統內容變數
- 從[Azure 監視器](azure-monitor.md)查看監視警示的設定
- 查看和修改電源設定
- 查看和修改遠端桌面設定
- 查看和修改以角色為基礎的存取控制設定
- 查看和修改 Hyper-v 主機設定（如果適用）

## <a name="storage"></a>存放

**儲存體**可讓您管理電腦或伺服器上的存放裝置。

### <a name="features"></a>功能

存放裝置支援下列功能：

- 流覽及搜尋伺服器上的現有磁片
- 查看磁片詳細資料
- 建立磁碟區
- 初始化磁片
- 建立、附加和卸離虛擬硬碟（VHD）
- 讓磁片離線
- 格式化磁片區
- 調整磁碟區的大小
- 編輯磁片區屬性
- 刪除磁碟區
- 安裝配額管理
- 管理檔案伺服器 Resource Manager 配額[儲存體-> 建立/更新配額](https://docs.microsoft.com/windows-server/storage/fsrm/quota-management)

[**查看儲存裝置的意見反應和建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BStorage%5D)

## <a name="storage-migration-service"></a>存放裝置移轉服務

**儲存體遷移服務**可讓您將伺服器和檔案共用遷移至 Azure 或 Windows Server 2019，而不需要應用程式或使用者變更任何專案。
[取得儲存體遷移服務的總覽](https://go.microsoft.com/fwlink/?linkid=2016155)

>[!NOTE]
>儲存體遷移服務需要 Windows Server 2019。

## <a name="storage-replica"></a>儲存體複本

使用**儲存體複本**來管理伺服器對伺服器儲存體複寫。
[深入瞭解儲存體複本](https://docs.microsoft.com/windows-server/storage/storage-replica/storage-replica-ui)

## <a name="system-insights"></a>系統深入解析

**System Insights**以原生方式引進 Windows Server 中的預測性分析，協助您更深入瞭解伺服器的運作。
[深入瞭解 System Insights](https://aka.ms/systeminsights)

>[!NOTE]
>System Insights 需要 Windows Server 2019。

## <a name="updates"></a>更新

**更新**可讓您在電腦或伺服器上管理 Microsoft 及/或 Windows 更新。

### <a name="features"></a>功能

更新中支援下列功能：

- 查看可用的 Windows 或 Microsoft Update
- 查看更新歷程記錄清單
- 安裝更新
- 線上檢查是否有更新 Microsoft Update
- 管理[Azure 更新管理](https://docs.microsoft.com/azure/automation/automation-update-management)整合

[**查看更新的意見反應和建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BUpdates%5D)

## <a name="virtual-machines"></a>虛擬機器

請參閱[使用 Windows 系統管理中心管理虛擬機器](manage-virtual-machines.md)

## <a name="virtual-switches"></a>虛擬交換器

**虛擬交換器**可讓您管理電腦或伺服器上的 hyper-v 虛擬交換器。

### <a name="features"></a>功能

虛擬交換器支援下列功能：

- 流覽及搜尋伺服器上的虛擬交換器
- 建立新的虛擬交換器
- 重新命名虛擬交換器
- 刪除現有的虛擬交換器
- 編輯虛擬交換器的屬性

[**查看虛擬交換器的意見反應和建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BVirtual%20Switch%5D)
