---
title: 管理伺服器，使用 Windows Admin Center
description: 管理伺服器，使用 Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 93a40345d05a6230596832b2d455d36eee2401b5
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296680"
---
# 管理伺服器，使用 Windows Admin Center

>適用於：Windows Admin Center、Windows Admin Center 預覽版

> [!Tip]
> 對 Windows Admin Center 不熟悉？
> [深入了解 Windows Admin Center](../understand/windows-admin-center.md) 或[立即下載](https://aka.ms/windowsadmincenter).

## 管理 Windows Server 的電腦

您可以新增個別伺服器執行 Windows Server 2012 或稍後對 Windows Admin Center 來管理的伺服器使用一組完整的工具包括憑證、 裝置、 事件、 處理程序、 角色和功能、 更新、 虛擬機器等等。

![伺服器連線概觀畫面](../media/manage-servers/server-overview.png)

## 將伺服器新增至 Windows Admin Center

若要將伺服器新增至 Windows Admin Center:

1. 按一下 [ **+ 新增**下所有的連線。
2. 選擇新增**伺服器連線**。
3. 輸入的伺服器名稱，如果出現提示，使用的認證。
4. 按一下 [**送出**以完成。

伺服器將會新增到您在 [概觀] 頁面上的連線清單。 按一下以連線到伺服器。

> [!NOTE]
> 您也可以為個別的連線，Windows Admin Center 中新增[超融合式叢集](manage-hyper-converged.md)或[容錯移轉叢集](manage-failover-clusters.md)。

## 工具

下列工具可供伺服器連線：

| 工具 | 描述 |
| ---- | ----------- |
| [概觀](#overview) | 檢視伺服器詳細資料，並控制的伺服器狀態 |
| [Active Directory](#active-directory-preview) | 管理 Active Directory |
| [備份](#backup) | 檢視及設定 Azure 備份 |  
| [憑證](#certificates) | 檢視和修改憑證 |
| [容器](#containers) | 檢視容器 |
| [裝置](#devices) | 檢視和修改裝置 |
| [DHCP](#dhcp) | 檢視和管理 DHCP 伺服器設定 |
| [DNS](#dns) | 檢視和管理的 DNS 伺服器設定 |
| [事件](#events) | 檢視事件 |
| [檔案](#files) | 瀏覽的檔案和資料夾 |
| [防火牆](#firewall) | 檢視和修改防火牆規則 |
| [已安裝應用程式](#installed-apps) | 檢視，並移除已安裝的應用程式 |
| [本機使用者和群組](#local-users-and-groups) | 檢視和修改本機使用者和群組 |
| [網路](#network) | 檢視和修改網路裝置 |
| [PowerShell](#powershell) | 透過 PowerShell 伺服器與互動 |
| [處理程序](#processes) | 檢視和修改執行處理程序 |
| [登錄](#registry) | 檢視和修改登錄項目 |
| [遠端桌面](#remote-desktop) | 透過遠端桌面的伺服器與互動 |
| [角色和功能](#roles-and-features) | 檢視和修改角色和功能 |
| [排定的工作](#scheduled-tasks) | 檢視和修改排定的工作 |
| [服務](#services) | 檢視和修改服務 |
| [設定](#settings) | 檢視和修改服務 |
| [儲存空間](#storage) | 檢視和修改存放裝置 |
| [存放裝置移轉服務](#storage-migration-service) | 將伺服器和檔案共用移轉至 Azure 或 Windows Server 2019 |
| [儲存體複本](#storage-replica) | 使用儲存體複本管理伺服器對伺服器儲存體複寫 |
| [系統深入解析](#system-insights) | 系統深入解析能讓您增加深入了解您的伺服器的正常運作。 |
| [更新](#updates) | 檢視已安裝並檢查有新的更新 |
| [虛擬機器](manage-virtual-machines.md) | 檢視和管理虛擬機器 |
| [虛擬交換器](#virtual-switches) | 檢視和管理虛擬交換器 |

## 概觀

**概觀**可讓您查看 CPU、 記憶體和網路效能的目前狀態，以及執行作業並修改目標電腦或伺服器上的設定。

### 功能

伺服器管理員概觀支援下列功能：

- 檢視伺服器詳細資料
- 檢視 CPU 活動
- 檢視記憶體活動
- 檢視網路活動
- 重新啟動伺服器
- 關閉伺服器
- 啟用伺服器上的磁碟計量
- 編輯伺服器上的電腦識別碼
- 檢視 BMC IP 位址與超連結 （需要 IPMI 相容 BMC）。

[**檢視意見反應和建議的功能，如伺服器概觀**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BOverview%5D)。

## Active Directory （預覽）

**Active Directory**是可用的[擴充功能摘要](../configure/using-extensions.md)的早期預覽。

### 功能

可用的下列 Active Directory 管理如下：

- 建立使用者
- 建立群組
- 搜尋使用者、 電腦及群組
- 詳細資料窗格中的使用者、 電腦及時在 grid 中選取的群組
- 全域 Grid 動作使用者、 電腦及群組 （停用/啟用，移除）
- 重設使用者密碼
- 使用者物件： 設定基本屬性 & 群組成員資格
- 電腦物件： 設定要在單一電腦的委派
- 群組物件： 管理會員資格 （一次新增/移除 1 使用者）  

[**檢視意見反應和 Active Directory 的建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BActive%20Directory%5D)。

## 備份

**備份**可讓您從損毀、 攻擊或嚴重損壞保護您的 Windows server，藉由直接向 Microsoft Azure 備份您的伺服器。
[深入了解 Azure 備份。](https://aka.ms/windows-admin-center-backup)

[提供意見反應的 Windows Admin Center 中的備份](https://aka.ms/backup-wac-feedback)

### 功能

備份支援下列功能：

- 檢視您的 Azure 備份狀態的概觀
- 設定備份的項目和排程
- 開始或停止備份的工作
- 檢視備份工作歷程記錄和狀態
- 檢視復原點和復原資料
- 刪除備份資料

## 憑證

**憑證**可讓您管理電腦或伺服器上的憑證存放區。

### 功能

憑證支援下列功能：

- 瀏覽和搜尋現有的憑證
- 檢視憑證的詳細資料
- 匯出憑證
- 更新的憑證
- 要求新憑證
- 刪除憑證

[**檢視意見反應和提出之憑證的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BCertificates%5D)。

## 容器

**容器**可讓您檢視在 Windows Server 容器主機上的容器。 在執行中的 Windows Server Core 容器，案例中，您可以檢視事件記錄檔和存取容器的 CLI。

[**檢視意見反應和建議適用於容器的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BContainers%5D)。

## 裝置

**裝置**可讓您管理連線的裝置上的電腦或伺服器。

### 功能

裝置支援下列功能：

- 瀏覽及搜尋的裝置
- 檢視裝置詳細資料
- 停用裝置
- 在裝置上的驅動程式更新

[**檢視意見反應和建議適用於裝置的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDevices%5D)。

## DHCP

**DHCP**可讓您管理連線的裝置上的電腦或伺服器。

### 功能

- 建立/設定/檢視 IPV4 與 IPV6 範圍
- 建立位址排除項目和設定開始和結束 IP 位址
- 建立位址保留區和設定用戶端的 MAC 位址 (IPV4)、 DUID 和 IAID (IPV6)

[**檢視意見反應與 DHCP 建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDHCP%5D)。

## DNS

**DNS**可讓您管理連線的裝置上的電腦或伺服器。

### 功能

- DNS 正向對應區域、 反向對應區域和 DNS 記錄的檢視詳細資料
- 建立正向對應區域 (主要、 次要或虛設常式)，並設定正向對應區域的內容
- 建立主機 （A 或 AAAA） CNAME 或 MX 類型的 DNS 記錄
- 設定 DNS 記錄內容
- 建立 IPV4 與 IPV6 反向對應區域 (主要、 次要和虛設常式)，設定反向對應區域的內容
- 建立 PTR，CNAME 類型的 DNS 記錄下反向對應區域。

[**檢視意見反應與 DHCP 建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDNS%5D)。

## 事件

**事件**可讓您管理電腦或伺服器上的事件記錄檔。

### 功能

在事件中支援下列功能：

- 瀏覽及搜尋事件
- 檢視事件詳細資料
- 清楚的事件記錄檔
- 匯出從記錄的事件

[**檢視意見反應和事件的建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BEvents%5D)。

## 檔案

**檔案**可讓您管理檔案和電腦或伺服器上的資料夾。

### 功能

在檔案中支援下列功能：

- 瀏覽的檔案和資料夾
- 搜尋檔案或資料夾
- 建立新的資料夾
- 刪除檔案或資料夾
- 下載檔案或資料夾
- 上傳檔案或資料夾
- 重新命名檔案或資料夾
- 將 zip 檔案解壓縮
- 檢視檔案或資料夾的內容
- 管理檔案伺服器資源管理員[的配額](https://docs.microsoft.com/windows-server/storage/fsrm/quota-management)
- 新增、 編輯或移除的檔案共用
- 修改使用者和群組上的檔案共用的權限

[**檢視意見反應和建議的檔案的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BFiles%5D)。

## 防火牆

**防火牆**可讓您管理防火牆設定和電腦或伺服器上的規則。

### 功能

防火牆支援下列功能：

- 檢視防火牆設定的概觀
- 檢視連入的防火牆規則
- 檢視傳出的防火牆規則
- 搜尋防火牆規則
- 檢視防火牆規則的詳細資料
- 建立新的防火牆規則
- 啟用或停用防火牆規則
- 刪除防火牆規則
- 編輯防火牆規則的內容

[**檢視意見反應與防火牆建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BFirewall%5D)。

## 已安裝應用程式

**安裝應用程式**可讓您以列出並解除安裝已安裝的應用程式。

[**檢視意見反應和安裝應用程式提出的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BInstalled%20Apps%5D)。

## 本機使用者和群組

**本機使用者和群組**可讓您管理安全性群組和存在於本機電腦或伺服器的使用者。

### 功能

本機使用者和群組支援下列功能：

- 檢視與搜尋的使用者和群組
- 建立新的使用者或群組
- 管理使用者的群組成員資格
- 刪除使用者或群組
- 變更使用者的密碼
- 編輯使用者或群組的內容

[**檢視意見反應和建議的功能本機使用者和群組**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BLocal%20users%20and%20Groups%5D)

## 網路

**網路**可讓您管理網路裝置和電腦或伺服器上的設定。

### 功能

網路支援下列功能：

- 瀏覽和搜尋現有的網路介面卡
- 檢視詳細資料的網路介面卡
- 編輯內容的網路介面卡
- 建立[Azure 網路介面卡 （預覽功能）](https://blogs.technet.microsoft.com/networking/2018/09/05/azurenetworkadapter/)

[**檢視意見反應和建議的網路功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BNetwork%5D)

## PowerShell

**PowerShell**可讓您的電腦或伺服器透過 PowerShell 工作階段與互動。

### 功能

在 PowerShell 中支援下列功能：

- 在伺服器上建立互動式 PowerShell 工作階段
- 中斷連線從伺服器上的 PowerShell 工作階段

[**檢視意見反應和 powershell 提出的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BPowerShell%5D)

## 處理程序

**處理程序**可讓您管理電腦或伺服器上執行的處理序。

### 功能

處理程序支援下列功能：

- 瀏覽並搜尋執行處理程序
- 檢視程序詳細資料
- 啟動處理程序
- 結束處理程序
- 建立程序傾印
- 尋找程序控點

[**檢視意見反應和建議的功能，來處理程序**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BProcesses%5D)。

## 登錄

**登錄**可讓您管理登錄機碼和電腦或伺服器上的值。

### 功能

在登錄中支援下列功能：

- 瀏覽登錄機碼和值
- 新增或修改登錄值
- 刪除登錄值

[**檢視意見反應和建議的功能，來登錄**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRegistry%5D)。

## 遠端桌面

**遠端桌面**可讓您的電腦或伺服器，透過互動式桌面工作階段與互動。

### 功能

遠端桌面支援下列功能：

- 啟動互動式的遠端桌面工作階段
- 中斷連接的遠端桌面工作階段
- Ctrl + Alt + Del 傳送到遠端桌面工作階段

[**檢視意見反應和遠端桌面的建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRemote%20Desktop%5D)。

## 角色和功能

**角色和功能**，可讓您管理角色和功能在伺服器上。

### 功能

在 [角色和功能支援下列功能：

- 瀏覽的角色和功能在伺服器上的清單
- 檢視角色或功能的詳細資料
- 安裝角色或功能
- 移除角色或功能

[**檢視意見反應和建議的角色和功能的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRoles%20and%20Features%5D)。

## 排定的工作

**排定的工作**，可讓您管理電腦或伺服器上的排程的工作。

### 功能

在排定的工作中支援下列功能：

- 瀏覽工作排程器程式庫
- 編輯排定的工作
- 啟用 & 停用排定工作
- 開始 & 停止排定工作
- 建立排定的工作

[**檢視意見反應與建議的功能排定的工作**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BScheduled%20Tasks%5D)。

## 服務

**服務**可讓您管理電腦或伺服器上的服務。

### 功能

服務支援下列功能：

- 在伺服器上的瀏覽及搜尋服務
- 服務的檢視詳細資料
- 啟動服務
- 暫停服務
- 編輯內容的服務

[**檢視意見反應和服務的建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BServices%5D)。

## 設定

**設定**是中央位置管理電腦或伺服器上的設定。

### 功能

- 檢視和修改使用者和系統環境變數
- 檢視監視來自[Azure 監視器](azure-monitor.md)的警示的組態
- 檢視和修改電源設定
- 檢視和修改遠端桌面設定
- 檢視和修改角色型存取控制設定
- 檢視和修改 HYPER-V 主機設定，如果適用

## 儲存空間

**儲存空間**可讓您管理電腦或伺服器上的存放裝置。

### 功能

儲存體支援下列功能：

- 瀏覽和搜尋的伺服器上的現有磁碟
- 檢視磁碟的詳細資料
- 建立磁碟區
- 初始化磁碟
- 建立、 連結和中斷連結虛擬硬碟 (VHD)
- 離線磁碟
- 格式化磁碟區
- 調整磁碟區的大小
- 編輯磁碟區內容
- 刪除磁碟區
- 安裝配額管理

[**檢視意見反應和建議的功能，做為儲存空間**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BStorage%5D)

## 存放裝置移轉服務

**存放裝置移轉服務**可讓您將伺服器移轉和檔案共用 Azure 或 Windows Server 2019，而不需要應用程式或使用者變更任何項目。
[取得存放裝置移轉服務的概觀](https://go.microsoft.com/fwlink/?linkid=2016155)

>[!NOTE]
>存放裝置移轉服務需要 Windows Server 2019。

## 儲存體複本

您可以使用**儲存體複本 」** 來管理伺服器對伺服器儲存體複寫。
[深入了解儲存體複本](https://docs.microsoft.com/windows-server/storage/storage-replica/storage-replica-ui)

## 系統深入解析

**系統深入解析**導入了在原生預測性分析 Windows Server，來協助讓您增加深入了解您的伺服器的正常運作。
[取得系統深入解析的概觀](http://aka.ms/systeminsights)

>[!NOTE]
>系統深入解析需要 Windows Server 2019。

## 更新

**更新**，可讓您在電腦或伺服器上管理 Microsoft 和/或 Windows 更新。

### 功能

更新支援下列功能：

- 檢視可用的 Windows 或 Microsoft 更新
- 檢視更新記錄的清單
- 安裝更新
- 線上檢查來自 Microsoft Update 的更新
- 管理[Azure 更新管理](https://docs.microsoft.com/azure/automation/automation-update-management)整合

[**檢視意見反應和建議的功能更新**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BUpdates%5D)

## 虛擬機器

請參閱[管理虛擬機器使用 Windows Admin Center](manage-virtual-machines.md)

## 虛擬交換器

**虛擬交換器**可讓您管理電腦或伺服器上的 HYPER-V 虛擬交換器。

### 功能

虛擬交換器支援下列功能：

- 瀏覽和搜尋的伺服器上的虛擬交換器
- 建立新的虛擬交換器
- 重新命名一部虛擬交換器
- 刪除現有的虛擬交換器
- 編輯內容的虛擬交換器

[**適用於虛擬交換器檢視意見反應和建議的功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BVirtual%20Switch%5D)
