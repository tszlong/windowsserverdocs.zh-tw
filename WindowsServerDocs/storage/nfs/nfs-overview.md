---
title: 網路檔案系統概觀
description: 說明 Network File System 的功能。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: fb31cff44cac6bd66f9aa5b7234ff3fd3b215ccf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876299"
---
# <a name="network-file-system-overview"></a>網路檔案系統概觀

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

本主題說明隨附於 Windows Server 中的檔案和存放服務伺服器角色的功能與網路檔案系統的角色服務。 網路檔案系統 (NFS) 提供檔案共用有異質環境，包括 Windows 和非 Windows 電腦的企業解決方案。

## <a name="feature-description"></a>功能描述

使用 NFS 通訊協定，您可以執行 Windows 和其他非 Windows 作業系統，例如 Linux 或 UNIX 的電腦之間傳輸檔案。

Windows Server 中的 NFS 包含 Server for NFS 和 Client for NFS。 執行 Windows Server 的電腦可以使用 Server for NFS 做為其他非 Windows 用戶端電腦的 NFS 檔案伺服器。 NFS 用戶端可讓以 Windows 為基礎的電腦執行 Windows Server 存取儲存在非 Windows NFS 伺服器上的檔案。

## <a name="windows-and-windows-server-versions"></a>Windows 和 Windows Server 版本

Windows 支援多個版本的 NFS 用戶端和伺服器上的，根據作業系統版本和系列。 

| 作業系統 | NFS 伺服器版本 |NFS 用戶端版本|
| ----------------- | ------------------- | ----------------- |
| Windows 7、 Windows 8.1，Windows 10 | N/A | NFSv2, NFSv3 |
| Windows Server 2008、windows Server 2008 R2 | NFSv2, NFSv3 | NFSv2, NFSv3 |
| Windows Server 2012 中，Windows Server 2012 R2 中，Windows Server 2016、windows Server 2019 | NFSv2, NFSv3, NFSv4.1  | NFSv2, NFSv3 |

## <a name="practical-applications"></a>實際應用

以下是一些您可以使用 NFS 的方法：

- 若要從多平台的用戶端透過 SMB 或 NFS 的通訊協定提供多重通訊協定存取相同的檔案共用使用 Windows NFS 檔案伺服器。
- 部署 Windows NFS 檔案伺服器以非 Windows 作業系統環境中提供非 Windows 用戶端電腦存取 NFS 檔案共用。
- 可透過 SMB 或 NFS 的通訊協定存取的檔案共用上儲存資料，將應用程式從一個作業系統移轉至另一個。

## <a name="new-and-changed-functionality"></a>新功能和變更的功能

網路檔案系統中的新增和變更功能包括支援 NFS 4.1 版和改良的部署和管理能力。 如需 Windows Server 2012 中新增或變更的功能資訊，請檢閱下表：

|特色/功能|新功能或更新功能|描述|
|---|---|---|
|[NFS 4.1 版](#nfs-version-4.1)|新的|更高的安全性、 效能和相較於 NFS 版本 3 的互通性。|
|[NFS 基礎結構](#nfs-infrastructure)|已更新|改善部署與管理性，並增加安全性。|
|[持續可用性的 NFS 版本 3](#nfs-version-3-continuous-availability)|已更新|改善 NFS 版本 3 的用戶端上的持續可用性。|
|[部署和管理能力增強功能](#deployment-and-manageability-improvements)|已更新|可讓您輕鬆地部署及管理 NFS 使用新的 Windows PowerShell cmdlet 和新的 WMI 提供者。|

## <a name="nfs-version-41"></a>NFS 4.1 版

NFS 4.1 版會實作所有必要的層面，除了一些選擇性的層面的[RFC 5661](https://tools.ietf.org/html/rfc5661):

- **虛擬檔案系統**，會分隔實體和邏輯的命名空間，並使用 NFS 版本 3 和第 2 版相容的檔案系統。 別名提供匯出的檔案系統中，這是虛擬檔案系統的一部分。
- **複合 Rpc**相關作業結合，並減少的對話。
- **工作階段和工作階段的主幹連線**可讓其中一個語意，並允許持續的可用性和更佳的效能，同時還能利用 NFS 4.1 用戶端及 NFS 伺服器之間的多個網路。

## <a name="nfs-infrastructure"></a>NFS 基礎結構

Windows Server 2012 中的整體 NFS 基礎結構的增強功能，詳述如下：

- **Remote Procedure Call (RPC) / 外部資料表示法 (XDR)** 傳輸基礎結構，由 WinSock 網路通訊協定，可供這兩個 Server for NFS 和 Client for NFS。 這會取代傳輸裝置介面 (TDI)、 提供更佳的支援，並提供更好的延展性和接收端調整 (RSS)。
- **RPC 連接埠多工器**功能是防火牆互動良好 （管理較少連接埠），並簡化部署的 NFS。
- **自動調整快取和執行緒集區**是新的 RPC/XDR 基礎結構的管理功能，是動態的自動調整快取，以及執行緒集區根據工作負載的資源。 這樣可以完全避免涉及的猜測時調整參數，提供最佳效能，只要在部署 NFS。
- **Kerberos 隱私權實作和新驗證選項**加上 Kerberos 隱私權 (Krb5p) 支援，以及現有 krb5 和 krb5i 驗證選項。
- **身分識別對應的 Windows PowerShell 模組**cmdlet 讓您更輕鬆地管理身分識別對應、 設定 Active Directory 輕量型目錄服務 (AD LDS) 和設定 UNIX 和 Linux 密碼與一般檔案。
- **磁碟區掛接點**可讓您存取下使用 NFS 版本 4.1 的 NFS 共用掛接磁碟區。
- **連接埠多工**功能支援 RPC 連接埠多工器 （連接埠 2049年），這是防火牆互動良好，可簡化 NFS 部署。

## <a name="nfs-version-3-continuous-availability"></a>持續可用性的 NFS 版本 3

NFS 版本 3 的用戶端可以有多個可用性和縮短停機時間的快速又透明計劃性容錯移轉。 容錯移轉程序的 NFS 版本 3 的用戶端是更快，因為：

- 現在，叢集基礎結構可讓每個網路名稱，而不是每個共用，這可大幅改善資源的容錯移轉時間的一項資源的一項資源。
- NFS 伺服器內的容錯移轉路徑會進行調整，以提升效能。
- NFS 伺服器中的萬用字元註冊不再需要，而且更微調容錯移轉。
- 網路狀態監視 (NSM) 通知容錯移轉程序之後，會送出，而且用戶端不再需要等候 TCP 逾時，若要重新連線到容錯移轉伺服器。

請注意，Server for NFS 僅在手動起始時 (通常於規劃的維護期間) 支援透明容錯移轉。 如果發生未規劃的容錯移轉，NFS 用戶端的連線將會中斷。 Server for NFS 也不會有與繼續金鑰篩選任何整合。 這表示如果本機應用程式或 SMB 工作階段在規劃的容錯移轉後，立即嘗試存取 NFS 用戶端正在存取的檔案，NFS 用戶端的連線可能會中斷 (透明容錯移轉不會成功)。

## <a name="deployment-and-manageability-improvements"></a>部署和管理能力增強功能

部署及管理 NFS 已透過下列方式改進：

- 超過 40 個新的 Windows PowerShell cmdlet 輕鬆地設定及管理 NFS 檔案共用。 如需詳細資訊，請參閱 < [Windows PowerShell 中的 NFS Cmdlet](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps)。
- 識別對應已獲得改善，與一般的本機檔案對應儲存區和新的 Windows PowerShell cmdlet 來設定識別對應。
- 伺服器管理員圖形化使用者介面很容易使用。
- 為了方便管理使用新的 WMI 版本 2 提供者。
- RPC 連接埠多工器 （連接埠 2049年） 是防火牆互動良好，並簡化部署的 NFS。

## <a name="server-manager-information"></a>伺服器管理員資訊

在 伺服器管理員-或更新版本[Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md) -使用 [新增角色及功能精靈] 將 Server for NFS 角色服務 (底下的檔案和 iSCSI 服務角色)。 如需安裝功能的一般資訊，請參閱 [安裝或解除安裝角色、角色服務或功能](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831809(v=ws.11)>)。 Server for NFS 工具包括 for Network File System MMC 嵌入式管理單元中的服務來管理 Server for NFS 和 Client for NFS 元件。 您可以使用嵌入式管理單元，來管理 Server for NFS 的電腦上安裝的元件。 Server for NFS 也包含數個 Windows 命令列管理工具：

- **掛接**本機掛接遠端的 NFS 共用 （也就是匯出），並將其對應到 Windows 用戶端電腦上的本機磁碟機代號。
- **Nfsadmin**管理組態設定的 Server for NFS 和 Client for NFS 元件。
- **Nfsshare**設定 NFS 共用設定為使用 Server for NFS 共用的資料夾。
- **Nfsstat**顯示或重設統計資料的 nfs 伺服器接收到的呼叫。
- **Showmount –** 顯示 nfs 伺服器匯出的已掛接的檔案系統。
- **Umount**移除 NFS 掛接的磁碟機。

Windows Server 2012 中的 NFS 適用於 Windows PowerShell，使用數個新的 cmdlet 特別針對 NFS，引入 NFS 模組。 這些 cmdlet 會提供簡單的方式，將 NFS 管理工作自動化。 如需詳細資訊，請參閱 < [Windows PowerShell 中的 NFS cmdlet](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps)。

## <a name="additional-information"></a>其他資訊

下表提供評估 NFS 的其他資源。

|內容類型|參考|
|---|---|
|部署|[部署網路檔案系統](deploy-nfs.md)|
|操作|[Windows PowerShell 中的 NFS cmdlet](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps)|
|相關技術|[Windows Server 中的儲存體](../storage.md)|