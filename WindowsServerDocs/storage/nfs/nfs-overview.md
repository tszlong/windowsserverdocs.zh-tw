---
title: 網路檔案系統概觀
description: 說明什麼是網路檔案系統。
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: f18c880dd673b17f53815a57fa2fcc66558dad71
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86961320"
---
# <a name="network-file-system-overview"></a>網路檔案系統概觀

>適用于： Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題說明 Windows Server 中的檔案和存放服務伺服器角色所包含的網路檔案系統角色服務和功能。 網路檔案系統（NFS）為具有同時包含 Windows 和非 Windows 電腦的異類環境的企業，提供檔案共用解決方案。

## <a name="feature-description"></a>功能說明

使用 NFS 通訊協定，您可以在執行 Windows 的電腦與其他非 Windows 作業系統（例如 Linux 或 UNIX）之間傳輸檔案。

Windows Server 中的 NFS 包含 Server for NFS 和 Client for NFS。 執行 Windows Server 的電腦可以使用 Server for NFS 作為其他非 Windows 用戶端電腦的 NFS 檔案伺服器。 Client for NFS 可讓執行 Windows Server 的 Windows 電腦存取儲存在非 Windows NFS 伺服器上的檔案。

## <a name="windows-and-windows-server-versions"></a>Windows 和 Windows Server 版本

Windows 支援多個版本的 NFS 用戶端和伺服器，視作業系統版本和系列而定。 

| 作業系統 | NFS 伺服器版本 |NFS 用戶端版本|
| ----------------- | ------------------- | ----------------- |
| Windows 7、Windows 8.1、Windows 10 | 不適用 | NFSv2、NFSv3 |
| Windows Server 2008、Windows Server 2008 R2 | NFSv2、NFSv3 | NFSv2、NFSv3 |
| Windows Server 2012、Windows Server 2012 R2、Windows Server 2016、Windows Server 2019 | NFSv2、NFSv3、NFSv 4。1  | NFSv2、NFSv3 |

## <a name="practical-applications"></a>實際應用

以下是您可以使用 NFS 的一些方式：

- 使用 Windows NFS 檔案伺服器，為多平臺用戶端的 SMB 和 NFS 通訊協定，提供相同檔案共用的多重通訊協定存取。
- 在主要非 Windows 作業系統環境中部署 Windows NFS 檔案伺服器，為非 Windows 用戶端電腦提供 NFS 檔案共用的存取權。
- 透過 SMB 和 NFS 通訊協定，將資料儲存在可存取的檔案共用上，將應用程式從某個作業系統遷移至另一個作業系統。

## <a name="new-and-changed-functionality"></a>新功能和變更的功能

網路檔案系統中新增和變更的功能包含 NFS 4.1 版的支援，以及改良的部署和管理性。 如需 Windows Server 2012 中新增或變更之功能的相關資訊，請參閱下表：

|特色/功能|新功能或更新功能|描述|
|---|---|---|
|[NFS 版本4。1](#nfs-version-41)|新增|相較于 NFS 第3版，提高安全性、效能和互通性。|
|[NFS 基礎結構](#nfs-infrastructure)|已更新|改善部署和管理性，並提高安全性。|
|[NFS 第3版持續可用性](#nfs-version-3-continuous-availability)|已更新|改善 NFS 第3版用戶端的持續可用性。|
|[部署和管理能力改善](#deployment-and-manageability-improvements)|已更新|可讓您使用新的 Windows PowerShell Cmdlet 和新的 WMI 提供者，輕鬆部署及管理 NFS。|

## <a name="nfs-version-41"></a>NFS 版本4。1

除了[RFC 5661](https://tools.ietf.org/html/rfc5661)的一些選擇性層面以外，NFS 版本4.1 還會實行所有必要的層面：

- **虛擬檔案系統**，這是分隔實體與邏輯命名空間，且與 nfs 第3版和 nfs 第2版相容的檔案系統。 為匯出的檔案系統提供別名，這是虛擬檔案系統的一部分。
- **複合 rpc**結合了相關的作業，並減少對話。
- **會話和會話中繼**只會啟用一個語義，並可在 nfs 4.1 用戶端與 nfs 伺服器之間使用多個網路時，允許持續的可用性和更佳的效能。

## <a name="nfs-infrastructure"></a>NFS 基礎結構

Windows Server 2012 中整體 NFS 基礎結構的改良功能詳述如下：

- Server for NFS 和 Client for NFS 提供的**遠端程序呼叫（RPC）/External 資料標記法（XDR）** 傳輸基礎結構由 WinSock 網路通訊協定提供技術支援。 這會取代傳輸裝置介面（TDI）、提供更佳的支援，並提供更好的擴充性和接收端調整（RSS）。
- **RPC 通訊埠**多工器功能可供防火牆（較少管理埠）和簡化 NFS 的部署。
- **自動調整快取和執行緒**集區是新 RPC/XDR 基礎結構的資源管理功能，它是動態的，會根據工作負載自動調整快取和執行緒集區。 如此一來，當您在部署 NFS 時，就能盡速提供最佳效能，這就完全免除了微調參數時的猜測
- **新的 kerberos 隱私權實行和驗證選項**，加上 kerberos 隱私權（Krb5p）支援，以及現有的 krb5 和 krb5i 驗證選項。
- **識別對應 Windows PowerShell 模組**Cmdlet 可讓您更輕鬆地管理身分識別對應、設定 Active Directory 輕量型目錄服務（AD LDS），以及設定 UNIX 和 Linux passwd 和一般檔案。
- **磁片區掛接點**可讓您存取以 nfs 版本4.1 裝載于 nfs 共用下的磁片區。
- 通訊**埠多**任務功能支援 RPC 通訊埠多工器（埠2049），其具備防火牆的便利性，並簡化 NFS 部署。

## <a name="nfs-version-3-continuous-availability"></a>NFS 第3版持續可用性

NFS 第3版用戶端可以有快速且透明的規劃容錯移轉，並具有更高的可用性和減少停機時間。 NFS 第3版用戶端的容錯移轉程式速度較快，因為：

- 叢集基礎結構現在允許每個網路名稱有一個資源，而不是每個共用一個資源，這可大幅改善資源的容錯移轉時間。
- NFS 伺服器內的容錯移轉路徑會進行微調，以獲得更好的效能。
- NFS 伺服器中的萬用字元註冊已不再需要，而且容錯移轉更精細。
- 網路狀態監視器（NSM）通知會在容錯移轉程式之後送出，而且用戶端不再需要等待 TCP 超時重新連線到容錯移轉的伺服器。

請注意，Server for NFS 僅在手動起始時 (通常於規劃的維護期間) 支援透明容錯移轉。 如果發生未規劃的容錯移轉，NFS 用戶端的連線將會中斷。 Server for NFS 也不會與繼續金鑰篩選器進行任何整合。 這表示如果本機應用程式或 SMB 工作階段在規劃的容錯移轉後，立即嘗試存取 NFS 用戶端正在存取的檔案，NFS 用戶端的連線可能會中斷 (透明容錯移轉不會成功)。

## <a name="deployment-and-manageability-improvements"></a>部署和管理能力改善

部署和管理 NFS 已透過下列方式改善：

- 超過40的新 Windows PowerShell Cmdlet 可讓您更輕鬆地設定及管理 NFS 檔案共用。 如需詳細資訊，請參閱[Windows PowerShell 中的 NFS Cmdlet](/powershell/module/nfs/?view=win10-ps)。
- 識別對應已透過本機一般檔案對應存放區，以及用於設定身分識別對應的新 Windows PowerShell Cmdlet 來改善。
- 伺服器管理員圖形化使用者介面較容易使用。
- 新的 WMI 第2版提供者可讓您更輕鬆地進行管理。
- RPC 通訊埠多工器（埠2049）具有防火牆易懂，而且簡化了 NFS 的部署。

## <a name="server-manager-information"></a>伺服器管理員資訊

在伺服器管理員或較新的[Windows 管理中心](../../manage/windows-admin-center/overview.md)-使用 [新增角色及功能] 嚮導來新增 SERVER for NFS 角色服務（在 [檔案] 和 [ISCSI 服務] 角色底下）。 如需安裝功能的一般資訊，請參閱 [安裝或解除安裝角色、角色服務或功能](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831809(v=ws.11)>)。 Server for NFS 工具組含適用于網路檔案系統的服務 MMC 嵌入式管理單元，可用來管理 Server for NFS 和 Client for NFS 元件。 使用嵌入式管理單元，您可以管理安裝在電腦上的 Server for NFS 元件。 Server for NFS 也包含數個 Windows 命令列管理工具：

- **Mount**會在本機掛接遠端 NFS 共用（也稱為匯出），並將它對應至 Windows 用戶端電腦上的本機磁碟機號。
- **Nfsadmin**會管理 SERVER for Nfs 和 CLIENT for nfs 元件的設定。
- **Nfsshare**會為使用 SERVER for NFS 共用的資料夾設定 NFS 共用設定。
- **Nfsstat**顯示或重設 SERVER for NFS 所接收之呼叫的統計資料。
- **Showmount –** 顯示 SERVER for NFS 所匯出的已掛接檔案系統。
- **Umount**會移除裝載 NFS 的磁片磁碟機。

Windows Server 2012 中的 NFS 引進了適用于 Windows PowerShell 的 NFS 模組，其中有數個專門用於 NFS 的新 Cmdlet。 這些 Cmdlet 可讓您輕鬆地將 NFS 管理工作自動化。 如需詳細資訊，請參閱[Windows PowerShell 中的 NFS Cmdlet](/powershell/module/nfs/?view=win10-ps)。

## <a name="additional-information"></a>其他資訊

下表提供評估 NFS 的其他資源。

|內容類型|參考|
|---|---|
|部署|[部署網路檔案系統](deploy-nfs.md)|
|作業|[Windows PowerShell 中的 NFS Cmdlet](/powershell/module/nfs/?view=win10-ps)|
|相關技術|[Windows Server 的儲存空間](../storage.yml)|
