---
title: 網路檔案系統概觀
description: 說明網路檔案系統。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: fb31cff44cac6bd66f9aa5b7234ff3fd3b215ccf
ms.sourcegitcommit: bad0692ffd0773f61ac62bd140a421cb02c68acf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/22/2019
ms.locfileid: "9099137"
---
# 網路檔案系統概觀

>適用於： Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

本主題說明的網路檔案系統角色服務和隨附於 Windows Server 中的檔案和存放服務伺服器角色的功能。 網路檔案系統 (NFS) 可提供檔案共用解決方案，適用於具有包含 Windows 和非 Windows 電腦的異質環境的企業。

## 功能描述

使用 NFS 通訊協定，您可以在執行 Windows 和其他非 Windows 作業系統，例如 Linux 或 UNIX 電腦之間傳輸檔案。

Windows Server 中的 NFS NFS 和 Client for NFS 包含伺服器。 執行 Windows Server 的電腦，可用來做為其他非 Windows 用戶端電腦 NFS 檔案伺服器 Server for NFS。 NFS 用戶端可讓 Windows 為基礎的電腦存取的檔案儲存在非 Windows NFS 伺服器上執行 Windows Server。

## Windows 和 Windows Server 版本

Windows 支援多個 NFS 用戶端和伺服器，取決於作業系統版本及系列版本。 

| 作業系統 | NFS 伺服器版本 |NFS 用戶端版本|
| ----------------- | ------------------- | ----------------- |
| Windows 7、 Windows 8.1、 Windows 10 | 無 | NFSv2 NFSv3 |
| Windows Server 2008、 Windows Server 2008 R2 | NFSv2 NFSv3 | NFSv2 NFSv3 |
| Windows Server 2012、 Windows Server 2012 R2、 Windows Server 2016、 Windows Server 2019 | NFSv2，NFSv3，NFSv4.1  | NFSv2 NFSv3 |

## 實際應用

以下是一些您可以使用 NFS 的方式：

- 使用 Windows NFS 檔案伺服器相同的檔案共用提供多重通訊協定存取透過 SMB 和 NFS 通訊協定從多重平台用戶端。
- 部署 Windows NFS 檔案伺服器提供非 Windows 用戶端電腦 NFS 檔案共用存取主要非 Windows 作業系統環境中。
- 可透過 SMB 和 NFS 通訊協定存取的檔案共用上儲存資料，將應用程式從一種作業系統移轉到另一個。

## 新功能和變更的功能

新功能和變更網路檔案系統中包含支援 for NFS 版本 4.1 和改進的部署和管理性。 如已新增或變更 Windows Server 2012 中的功能的相關資訊，請檢閱下表：

|特色/功能|新功能或更新功能|描述|
|---|---|---|
|[NFS 版本 4.1](#nfs-version-4.1)|新增|增加的安全性、 效能和相較於 NFS 版本 3 的互通性。|
|[NFS 基礎結構](#nfs-infrastructure)|已更新|改善部署與管理性，並增加安全性。|
|[NFS 第 3 版持續可用性](#nfs-version-3-continuous-availability)|已更新|改善 NFS 第 3 版用戶端上的持續可用性。|
|[部署和管理性改進功能](#deployment-and-manageability-improvements)|已更新|可讓您輕鬆地部署與管理 NFS 與新的 Windows PowerShell cmdlet 和新的 WMI 提供者。|

## NFS 版本 4.1

NFS 版本 4.1 實作所有必要的方面，除了某些層面選用， [RFC 5661](https://tools.ietf.org/html/rfc5661):

- **虛擬檔案系統**，用來分隔實體和邏輯的命名空間，而且不相容於 NFS 版本 3 和 NFS 版本 2 的檔案系統。 別名提供匯出的檔案系統，也就是虛擬檔案系統的一部分。
- **複合 Rpc**結合相關的作業，並減少的交談功能。
- **工作階段與工作階段聚**啟用只有一部語意，並可讓持續可用性，並同時利用 NFS 4.1 用戶端和 NFS 伺服器之間的多個網路較佳的效能。

## NFS 基礎結構

Windows Server 2012 中的整體 NFS 基礎結構的改良功能中有詳細說明如下：

- **遠端程序呼叫 (RPC) / 外部資料表示法 (XDR)** 傳輸基礎結構，由 WinSock 網路通訊協定，適用於這兩個 Server for NFS 和 Client for NFS。 這會取代傳輸裝置介面 (TDI)、 提供更完善地支援，並提供更好的延展性和接收端調整 (RSS)。
- **RPC 連接埠多工器**功能的防火牆-方便 （來管理較少連接埠），可簡化部署 NFS。
- **自動調整快取和執行緒集區**是資源管理功能，新的 RPC/XDR 基礎結構的是動態的會自動調整快取，執行緒集區根據工作負載。 這完全移除涉及的猜測時調整參數，提供最佳效能，儘速 NFS 部署。
- **新的 Kerberos 隱私權實作和驗證選項**搭配 Kerberos 隱私權 (Krb5p) 支援，以及現有 krb5 和 krb5i 驗證選項。
- **身分識別對應的 Windows PowerShell 模組**cmdlet 可讓您更輕鬆地管理身分識別對應、 設定 Active Directory 輕量型目錄服務 (AD LDS)，以及設定 UNIX 和 Linux 的密碼與一般檔案。
- **磁碟區掛接點**可讓您存取掛接在與 NFS 版本 4.1 NFS 共用磁碟區。
- **連接埠多工處理**功能支援 RPC 連接埠多工器 （連接埠 2049年），這是防火牆-方便，可簡化 NFS 部署。

## NFS 第 3 版持續可用性

NFS 第 3 版用戶端可以有多個可用性和降低停機時間的快速和透明計劃容錯移轉。 容錯移轉程序 NFS 第 3 版用戶端是速度更快，因為：

- 叢集基礎結構現在可讓每個網路名稱，而不是一個資源共用，會大幅改善資源的容錯移轉時間的每一個資源。
- 較佳的效能調整 NFS 伺服器中的容錯移轉路徑。
- NFS 伺服器中的萬用字元註冊已不再需要，也更多微調容錯移轉。
- 網路狀態監視器 (NSM) 通知會在容錯移轉程序後送出和用戶端不再需要等待重新連線到失敗的 TCP 逾時透過伺服器。

請注意 Server for NFS 支援僅手動起始時，通常在規劃的維護期間透明的容錯移轉。 如果發生意外的容錯移轉，NFS 用戶端會遺失他們的連線。 Server for NFS 也不需要任何整合，以繼續金鑰篩選條件。 這表示如果在本機應用程式或 SMB 工作階段會嘗試存取相同 NFS 用戶端在計劃的容錯移轉後立即存取的檔案，NFS 用戶端可能喪失其連線 （不會成功透明的容錯移轉）。

## 部署和管理性改進功能

部署及管理 NFS 已經改良，以下列方式：

- 超過 40 新的 Windows PowerShell cmdlet 可讓您更輕鬆地設定及管理 NFS 檔案共用。 如需詳細資訊，請參閱[Windows PowerShell 中的 NFS Cmdlet](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps)。
- 身分識別對應本機的一般檔案對應存放區與新的 Windows PowerShell cmdlet 設定識別對應的改進。
- 伺服器管理員圖形化使用者介面是很容易使用。
- 新的 WMI 第 2 版提供者是適用於更容易管理。
- RPC 連接埠多工器 （連接埠 2049年） 是防火牆-方便，，可簡化部署 NFS。

## 伺服器管理員資訊

在伺服器管理員-或較新的[Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md) -使用 [新增角色及功能精靈新增 NFS 角色服務的伺服器 (底下的檔案和 iSCSI 服務角色)。 如需有關安裝功能的一般資訊，請參閱[安裝或解除安裝角色、 角色服務或功能](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831809(v=ws.11)>)。 伺服器針對 NFS 工具包含的服務網路檔案系統 MMC 嵌入式管理單元來管理 for NFS 伺服器及用戶端以使用 NFS 元件。 您可以使用嵌入式管理單元，來管理的伺服器安裝在電腦上的 NFS 元件。 Server for NFS 也包含數個 Windows 命令列的系統管理工具：

- **將掛接**在本機支撐架遠端 NFS 共用 （也稱為匯出），並將其對應至 Windows 用戶端電腦上本機磁碟機代號。
- **Nfsadmin**管理伺服器的 NFS 以及設定用戶端 NFS 元件。
- **Nfsshare**設定 NFS 共用設定共用使用 Server for NFS 的資料夾。
- **Nfsstat**顯示或重設的呼叫 for NFS 伺服器接收到的統計資料。
- **Showmount**會顯示由伺服器所匯出的 for NFS 掛接的檔案系統。
- **Umount**會移除 NFS 掛接磁碟機。

Windows Server 2012 中的 NFS 導入了 NFS 模組適用於 Windows PowerShell 與數個新的 cmdlet 專為 NFS。 這些 cmdlet 提供簡單的方法，來自動化 NFS 管理工作。 如需詳細資訊，請參閱[Windows PowerShell 中的 NFS cmdlet](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps)。

## 其他資訊

下表提供評估 NFS 其他資源。

|內容類型|參考|
|---|---|
|部署|[部署網路檔案系統](deploy-nfs.md)|
|操作|[Windows PowerShell 中的 NFS cmdlet](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps)|
|相關技術|[Windows Server 的儲存空間](../storage.md)|