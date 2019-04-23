---
title: 管理 Windows Admin Center 伺服器
description: 使用 Windows Admin Center （專案檀香山） 管理伺服器
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 04ade4a14272c7840b5036ca6ad5a3bf3d09bcf9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59891149"
---
# <a name="manage-servers-with-windows-admin-center"></a>管理 Windows Admin Center 伺服器

>適用於：Windows Admin Center，Windows Admin Center 預覽

> [!Tip]
> 對 Windows Admin Center 不熟悉？
> [深入了解 Windows Admin Center](../understand/windows-admin-center.md) 或[立即下載](https://aka.ms/windowsadmincenter).

## <a name="managing-windows-server-machines"></a>管理 Windows Server 機器

您可以新增個別的伺服器執行 Windows Server 2012 或更新版本到 Windows Admin Center 來管理伺服器與一組完整的工具包括憑證、 裝置、 事件、 處理程序、 角色和功能、 更新、 虛擬機器等等。

![伺服器連線 概觀畫面](../media/manage-servers/server-overview.png)

## <a name="adding-a-server-to-windows-admin-center"></a>將伺服器新增至 Windows Admin Center

若要將伺服器新增至 Windows Admin Center:

1. 按一下  **+ 新增**下所有連接。
2. 選擇新增**伺服器連線**。
3. 輸入伺服器的名稱，如果出現提示，要使用的認證。
4. 按一下 **送出**才能完成。

伺服器會將您在 [概觀] 頁面上的連接清單。 按一下以連線到伺服器。

> [!NOTE]
> 您也可以加入[容錯移轉叢集](manage-failover-clusters.md)或是[超交集叢集](manage-hyper-converged.md)做為 Windows Admin Center 中的個別連線。

## <a name="tools"></a>工具

下列工具可供伺服器連接：

| 工具 | 描述 |
| ---- | ----------- |
| [概觀](#overview) | 檢視伺服器詳細資料和控制伺服器狀態 |
| [備份](#backup) | 檢視和設定 Azure 備份 |  
| [憑證](#certificates) | 檢視及修改憑證 |
| [容器](#containers) | 檢視容器 |
| [裝置](#devices) | 檢視及修改裝置 |
| [事件](#events) | 檢視事件 |
| [檔案](#files) | 瀏覽檔案和資料夾 |
| [防火牆](#firewall) | 檢視及修改防火牆規則 |
| [已安裝應用程式](#installed-apps) | 檢視並移除已安裝應用程式 |
| [本機使用者和群組](#local-users-and-groups) | 檢視及修改本機使用者和群組 |
| [網路](#network) | 檢視及修改網路裝置 |
| [PowerShell](#powershell) | 透過 PowerShell 的伺服器互動 |
| [Processes](#processes) | 檢視及修改執行中處理序 |
| [Registry](#registry) | 檢視及修改登錄項目 |
| [遠端桌面](#remote-desktop) | 透過遠端桌面伺服器互動 |
| [角色和功能](#roles-and-features) | 檢視及修改角色和功能 |
| [排定的工作](#scheduled-tasks) | 檢視及修改排程的工作 |
| [服務](#services) | 檢視及修改服務 |
| [儲存空間](#storage) | 檢視及修改存放裝置 |
| [儲存體移轉服務](#storage-migration-service) | 將伺服器和檔案共用移轉至 Azure 或 Windows Server 2019 |
| [儲存體複本](#storage-replica) | 使用管理伺服器對伺服器儲存體複寫的儲存體複本 |
| [系統的深入解析](#system-insights) | 系統 Insights 可讓您更深入了解您的伺服器的運作狀況。 |
| [更新](#updates) | 檢視已安裝並檢查有新的更新 |
| [虛擬機器](manage-virtual-machines.md) | 檢視及管理虛擬機器 |
| [虛擬交換器](#virtual-switches) | 檢視及管理虛擬交換器 |

## <a name="overview"></a>總覽

**概觀**也可讓您查看 CPU、 記憶體和網路效能的目前狀態為執行作業和修改目標電腦或伺服器上的設定。

### <a name="features"></a>功能

在 [伺服器管理員概觀] 支援下列功能：

- 檢視伺服器詳細資料
- 檢視 CPU 活動
- 檢視記憶體活動
- 檢視網路活動
- 重新啟動伺服器
- 關閉伺服器
- 啟用在伺服器上的磁碟計量
- 編輯伺服器上的電腦識別碼

[**檢視意見反應及建議的功能 for Server 概觀**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BOverview%5D)。

## <a name="backup"></a>備份

**備份**可讓您直接部署至 Microsoft Azure 備份伺服器，從損毀、 攻擊或災害時保護您的 Windows 伺服器。
[深入了解 Azure 備份。](https://aka.ms/windows-admin-center-backup)

[提供 Windows Admin Center 中備份的意見反應](https://aka.ms/backup-wac-feedback)

### <a name="features"></a>功能

在備份中支援下列功能：

- 檢視您的 Azure 備份狀態的概觀
- 設定備份的項目和排程
- 啟動或停止備份作業
- 檢視備份作業歷程記錄和狀態
- 檢視復原點和復原資料
- 刪除備份資料

## <a name="certificates"></a>憑證

**憑證**可讓您管理的電腦或伺服器上的憑證存放區。

### <a name="features"></a>功能

在憑證中支援下列功能：

- 瀏覽和搜尋現有的憑證
- 檢視憑證詳細資料
- 匯出憑證
- 更新憑證
- 要求新憑證
- 刪除憑證

[**檢視憑證的意見反應及建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BCertificates%5D)。

## <a name="containers"></a>容器

**容器**可讓您檢視 Windows Server 容器主機上的容器。 在執行 Windows Server Core 容器的情況下，您可以檢視事件記錄檔，並存取容器的 CLI。

[**檢視 適用於容器的 意見反應及建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BContainers%5D)。

## <a name="devices"></a>裝置

**裝置**可讓您管理的電腦或伺服器上連接的裝置。

### <a name="features"></a>功能

在裝置中支援下列功能：

- 瀏覽和搜尋裝置
- 檢視裝置詳細資料
- 停用裝置
- 在裝置上的更新驅動程式

[**檢視裝置的意見反應及建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDevices%5D)。

## <a name="events"></a>事件

**事件**可讓您管理的電腦或伺服器上的事件記錄檔。

### <a name="features"></a>功能

在事件中支援下列功能：

- 瀏覽和搜尋的事件
- 檢視事件詳細資料
- 清除事件記錄檔
- 從記錄檔匯出事件

[**檢視事件的意見反應及建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BEvents%5D)。

## <a name="files"></a>檔案

**檔案**可讓您管理的電腦或伺服器上檔案和資料夾。

### <a name="features"></a>功能

在檔案中支援下列功能：

- 瀏覽檔案和資料夾
- 搜尋檔案或資料夾
- 建立新的資料夾
- 刪除檔案或資料夾
- 下載檔案或資料夾
- 上傳檔案或資料夾
- 重新命名檔案或資料夾
- 將 zip 檔案解壓縮
- 檢視檔案或資料夾屬性
- 管理檔案伺服器資源管理員[配額](https://docs.microsoft.com/windows-server/storage/fsrm/quota-management)
- 新增、 編輯或移除檔案共用
- 修改使用者和群組在檔案共用上的權限

[**檢視檔案的意見反應及建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BFiles%5D)。

## <a name="firewall"></a>防火牆

**防火牆**可讓您管理防火牆設定與電腦或伺服器上的規則。

### <a name="features"></a>功能

在防火牆中支援下列功能：

- 檢視防火牆設定的概觀
- 檢視連入防火牆規則
- 檢視傳出防火牆規則
- 搜尋的防火牆規則
- 檢視防火牆規則詳細資料
- 建立新的防火牆規則
- 啟用或停用防火牆規則
- 刪除防火牆規則
- 編輯防火牆規則的屬性

[**檢視防火牆的意見反應及建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BFirewall%5D)。

## <a name="installed-apps"></a>已安裝應用程式

**安裝的應用程式**可讓您列出及解除安裝已安裝的應用程式。

[**檢視安裝的應用程式的意見反應及建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BInstalled%20Apps%5D)。

## <a name="local-users-and-groups"></a>本機使用者和群組

**本機使用者和群組**可讓您管理安全性群組和存在於本機電腦或伺服器的使用者。

### <a name="features"></a>功能

在 本機使用者和群組支援下列功能：

- 檢視及搜尋的使用者和群組
- 建立新的使用者或群組
- 管理使用者的群組成員資格
- 刪除使用者或群組
- 變更使用者的密碼
- 編輯使用者或群組的屬性

[**檢視 本機使用者和群組的 意見反應及建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BLocal%20users%20and%20Groups%5D)

## <a name="network"></a>Network

**網路**可讓您管理網路裝置和電腦或伺服器上的設定。

### <a name="features"></a>功能

在網路中支援下列功能：

- 瀏覽和搜尋現有的網路介面卡
- 檢視網路介面卡的詳細資料
- 編輯網路介面卡內容
- 建立[Azure 網路介面卡 （預覽功能）](https://blogs.technet.microsoft.com/networking/2018/09/05/azurenetworkadapter/)

[**檢視網路的意見反應及建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BNetwork%5D)

## <a name="powershell"></a>PowerShell

**PowerShell**可讓您的電腦或透過 PowerShell 工作階段的伺服器進行互動。

### <a name="features"></a>功能

在 PowerShell 中支援下列功能：

- 在伺服器上建立互動式 PowerShell 工作階段
- 從伺服器上的 PowerShell 工作階段中斷連線

[**適用於 PowerShell 檢視意見反應及建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BPowerShell%5D)

## <a name="processes"></a>處理程序

**處理序**可讓您管理的電腦或伺服器上執行的處理序。

### <a name="features"></a>功能

處理序中支援下列功能：

- 瀏覽和搜尋執行中處理序
- 檢視處理序詳細資料
- 啟動處理序
- 結束處理程序
- 建立處理序傾印
- 尋找處理程序控制代碼

[**檢視處理程序的意見反應及建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BProcesses%5D)。

## <a name="registry"></a>登錄

**登錄**可讓您管理登錄機碼和電腦或伺服器上的值。

### <a name="features"></a>功能

在登錄中支援下列功能：

- 瀏覽登錄機碼和值
- 新增或修改登錄值
- 刪除登錄值

[**檢視登錄的意見反應及建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRegistry%5D)。

## <a name="remote-desktop"></a>遠端桌面

**遠端桌面**可讓您的電腦或透過互動式桌面工作階段的伺服器進行互動。

### <a name="features"></a>功能

在 遠端桌面支援下列功能：

- 啟動互動式的遠端桌面工作階段
- 從遠端桌面工作階段中斷連線
- 傳送 Ctrl + Alt + Del 到遠端桌面工作階段

[**檢視 遠端桌面的 意見反應及建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRemote%20Desktop%5D)。

## <a name="roles-and-features"></a>角色和功能

**角色和功能**可讓您管理角色和功能的伺服器上。

### <a name="features"></a>功能

在 角色和功能支援下列功能：

- 瀏覽伺服器上角色和功能的清單
- 檢視角色或功能的詳細資料
- 安裝角色或功能
- 移除角色或功能

[**檢視角色與功能的意見反應及建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRoles%20and%20Features%5D)。

## <a name="scheduled-tasks"></a>排定的工作

**排定的工作**可讓您管理的電腦或伺服器上的排程的工作。

### <a name="features"></a>功能

工作排程器中支援下列功能：

- 瀏覽工作排程器程式庫
- 編輯排程的工作
- 啟用及停用排程的工作
- 啟動與停止排程的工作
- 建立排定的工作

[**檢視排程工作的意見反應及建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BScheduled%20Tasks%5D)。

## <a name="services"></a>服務

**服務**可讓您管理的電腦或伺服器上的服務。

### <a name="features"></a>功能

在服務中支援下列功能：

- 在伺服器上的瀏覽和搜尋服務
- 檢視服務的詳細資料
- 啟動服務
- 暫停服務
- 編輯服務的內容

[**檢視服務的意見反應及建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BServices%5D)。

## <a name="storage"></a>儲存體

**儲存體**可讓您管理的電腦或伺服器上的存放裝置。

### <a name="features"></a>功能

在儲存體支援下列功能：

- 瀏覽和搜尋的伺服器上的現有磁碟
- 檢視磁碟詳細資料
- 建立磁碟區
- 初始化磁碟
- 建立、 附加及中斷連結虛擬硬碟 (VHD)
- 將磁碟離線
- 格式化磁碟區
- 調整磁碟區
- 編輯磁碟區屬性
- 刪除磁碟區
- 安裝 配額管理

[**檢視儲存體意見反應及建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BStorage%5D)

## <a name="storage-migration-service"></a>存放裝置移轉服務

**儲存體移轉服務**可讓您移轉伺服器，以及檔案至 Azure 或 Windows Server 2019 的共用，而不需要應用程式或使用者變更任何項目。
[取得儲存體移轉服務的概觀](https://go.microsoft.com/fwlink/?linkid=2016155)

>[!NOTE]
>儲存體移轉服務需要 Windows Server 2019。

## <a name="storage-replica"></a>儲存體複本

使用**儲存體複本**來管理伺服器對伺服器儲存體複寫。
[深入了解儲存體複本](https://docs.microsoft.com/windows-server/storage/storage-replica/storage-replica-ui)

## <a name="system-insights"></a>系統深入解析

**系統 Insights**導入了原生方式在預測性分析可協助您的 Windows Server 增加深入了解您的伺服器的運作狀況。
[取得系統深入解析的概觀](http://aka.ms/systeminsights)

>[!NOTE]
>系統 Insights 需要 Windows Server 2019。

## <a name="updates"></a>更新

**更新**可讓您管理的電腦或伺服器上的 Microsoft 及/或 Windows 更新。

### <a name="features"></a>功能

在更新中支援下列功能：

- 檢視可用的 Windows 或 Microsoft Update
- 檢視更新歷程記錄清單
- 安裝更新
- 線上檢查來自 Microsoft Update 的更新
- 管理[Azure 更新管理](https://docs.microsoft.com/azure/automation/automation-update-management)整合

[**檢視更新的意見反應及建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BUpdates%5D)

## <a name="virtual-machines"></a>虛擬機器

請參閱[管理虛擬機器與 Windows Admin Center](manage-virtual-machines.md)

## <a name="virtual-switches"></a>虛擬交換器

**虛擬交換器**可讓您管理的電腦或伺服器上的 HYPER-V 虛擬交換器。

### <a name="features"></a>功能

在 虛擬交換器支援下列功能：

- 瀏覽和搜尋的伺服器上的虛擬交換器
- 建立新的虛擬交換器
- 重新命名虛擬交換器
- 刪除現有的虛擬交換器
- 編輯虛擬交換器內容

[**檢視 虛擬交換器的 意見反應及建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BVirtual%20Switch%5D)
