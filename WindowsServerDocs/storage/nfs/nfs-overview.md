---
title: 網路檔案系統概觀
description: 說明什麼是網路檔案系統。
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 449ba7e095a372ff1743794723de28cdeba86d3f
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96866437"
---
# <a name="network-file-system-overview"></a>網路檔案系統概觀

>適用于： Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題說明 Windows Server 中的檔案和存放服務伺服器角色所包含的網路檔案系統角色服務和功能。 Network File System (NFS) 為具有異類環境（包含 Windows 和非 Windows 電腦）的企業提供檔案共用解決方案。

## <a name="feature-description"></a>功能說明

使用 NFS 通訊協定，您可以在執行 Windows 的電腦與其他非 Windows 作業系統（例如 Linux 或 UNIX）之間傳輸檔案。

Windows Server 中的 NFS 包括 Server for NFS 和 Client for NFS。 執行 Windows Server 的電腦可以使用 Server for NFS 作為其他非 Windows 用戶端電腦的 NFS 檔案伺服器。 NFS 用戶端允許執行 Windows Server 的 Windows 電腦存取非 Windows NFS 伺服器上儲存的檔案。

## <a name="windows-and-windows-server-versions"></a>Windows 和 Windows Server 版本

Windows 支援多個 NFS 用戶端和伺服器版本，視作業系統版本和系列而定。

| 作業系統 | NFS 伺服器版本 |NFS 用戶端版本|
| ----------------- | ------------------- | ----------------- |
| Windows 7、Windows 8.1、Windows 10 | N/A | NFSv2、NFSv3 |
| Windows Server 2008、Windows Server 2008 R2 | NFSv2、NFSv3 | NFSv2、NFSv3 |
| Windows Server 2012、Windows Server 2012 R2、Windows Server 2016、Windows Server 2019 | NFSv2、NFSv3、Nfsv4.1 4。1  | NFSv2、NFSv3 |

## <a name="practical-applications"></a>實際應用

以下是您可以使用 NFS 的一些方式：

- 使用 Windows NFS 檔案伺服器，可透過多個平臺用戶端的 SMB 和 NFS 通訊協定，提供相同檔案共用的多重通訊協定存取。
- 在主要的非 Windows 作業系統環境中部署 Windows NFS 檔案伺服器，以提供非 Windows 用戶端電腦對 NFS 檔案共用的存取權。
- 將應用程式從一個作業系統移至另一個作業系統，方法是將資料儲存在可透過 SMB 和 NFS 通訊協定存取的檔案共用上。

## <a name="new-and-changed-functionality"></a>新功能和變更的功能

網路檔案系統的新功能和已變更的功能，包括對 NFS 版本4.1 的支援，以及改良的部署和管理能力。 如需 Windows Server 2012 中新增或變更之功能的相關資訊，請參閱下表：

|特色/功能|新功能或更新功能|描述|
|---|---|---|
|[NFS 4.1 版](#nfs-version-41)|新增|相較于 NFS 第3版，更高的安全性、效能和互通性。|
|[NFS 基礎結構](#nfs-infrastructure)|已更新|改善部署和管理性，並提高安全性。|
|[NFS 第3版持續可用性](#nfs-version-3-continuous-availability)|已更新|改善 NFS 第3版用戶端的持續可用性。|
|[部署與管理性改進](#deployment-and-manageability-improvements)|已更新|可讓您使用新的 Windows PowerShell Cmdlet 和新的 WMI 提供者，輕鬆地部署和管理 NFS。|

## <a name="nfs-version-41"></a>NFS 4.1 版

除了 [RFC 5661](https://tools.ietf.org/html/rfc5661)的一些選用層面以外，NFS 版本4.1 還可實現所有必要的層面：

- **虛擬檔案系統**，這是可分隔實體和邏輯命名空間的檔案系統，而且與 nfs 版本3和 nfs 版本2相容。 系統會提供別名給匯出的檔案系統，這是虛擬檔案系統的一部分。
- **複合 rpc** 結合了相關的作業並減少對話。
- **會話和會話的中繼** 只允許一個語義，並允許持續的可用性和更佳的效能，同時利用 nfs 4.1 用戶端和 nfs 伺服器之間的多個網路。

## <a name="nfs-infrastructure"></a>NFS 基礎結構

Windows Server 2012 中的整體 NFS 基礎結構的增強功能詳述如下：

- **遠端程序呼叫 (RPC) /External 資料表示 (XDR)** 傳輸基礎結構（由 WinSock 網路通訊協定支援）可用於 SERVER for Nfs 和 CLIENT for nfs。 這會取代 (TDI) 的傳輸設備介面、提供更佳的支援，並提供更佳的擴充性和接收端調整 (RSS) 。
- **RPC 埠** 多工器功能適合防火牆， (較少的埠來管理) 並簡化 NFS 的部署。
- **自動調整快取和執行緒** 集區是新 RPC/XDR 基礎結構的資源管理功能，動態、自動調整快取和以工作負載為基礎的執行緒集區。 這會完全移除調整參數時所牽涉到的猜測，並在部署 NFS 後立即提供最佳效能。
- **新的 kerberos 隱私權實施和驗證選項** ，加上 kerberos 隱私權 (Krb5p) 支援，以及現有的 krb5.keytab 和 krb5i 驗證選項。
- 身分 **識別對應 Windows PowerShell 模組** Cmdlet 可讓您更輕鬆地管理身分識別對應、設定 Active Directory 輕量型目錄服務 (AD LDS) ，以及設定 UNIX 和 Linux passwd 和一般檔案。
- **磁片區掛接點** 可讓您存取裝載于 nfs 版本4.1 的 nfs 共用下的磁片區。
- **埠多** 任務功能支援 RPC 通訊埠多工器， (埠 2049) ，此埠可方便防火牆並簡化 NFS 部署。

## <a name="nfs-version-3-continuous-availability"></a>NFS 第3版持續可用性

NFS 第3版用戶端可以有快速且透明的規劃容錯移轉，並提供更高的可用性及減少停機時間。 NFS 第3版用戶端的容錯移轉程式較快，因為：

- 叢集基礎結構現在可讓每個網路名稱有一個資源，而不是每個共用一個資源，如此可大幅改善資源的容錯移轉時間。
- NFS 伺服器內的容錯移轉路徑會經過調整，以獲得更好的效能。
- NFS 伺服器中的萬用字元註冊不再是必要的，而且容錯移轉的調整更精細。
- 網路狀態監視器 (NSM) 通知會在容錯移轉程式之後送出，而且用戶端不再需要等候 TCP 超時重新連線至已容錯移轉的伺服器。

請注意，Server for NFS 僅在手動起始時 (通常於規劃的維護期間) 支援透明容錯移轉。 如果發生未規劃的容錯移轉，NFS 用戶端的連線將會中斷。 Server for NFS 也與 Resume Key filter 沒有任何整合。 這表示如果本機應用程式或 SMB 工作階段在規劃的容錯移轉後，立即嘗試存取 NFS 用戶端正在存取的檔案，NFS 用戶端的連線可能會中斷 (透明容錯移轉不會成功)。

## <a name="deployment-and-manageability-improvements"></a>部署與管理性改進

以下列方式改善部署和管理 NFS：

- 超過40的新 Windows PowerShell Cmdlet 可讓您更輕鬆地設定及管理 NFS 檔案共用。 如需詳細資訊，請參閱 [Windows PowerShell 中的 NFS Cmdlet](/powershell/module/nfs/)。
- 使用本機一般檔案對應存放區和新的 Windows PowerShell Cmdlet 來設定識別對應，以改善識別對應。
- 伺服器管理員的圖形化使用者介面較容易使用。
- 新的 WMI 第2版提供者可以更輕鬆地進行管理。
- RPC 埠多工器 (埠 2049) 可感知防火牆，並簡化 NFS 的部署。

## <a name="server-manager-information"></a>伺服器管理員資訊

在伺服器管理員或較新的 [Windows Admin Center](../../manage/windows-admin-center/overview.md) -使用 [新增角色及功能] 嚮導，在 [檔案和 ISCSI 服務] 角色) 下新增 SERVER for NFS 角色服務 (。 如需安裝功能的一般資訊，請參閱 [安裝或解除安裝角色、角色服務或功能](</previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831809(v=ws.11)>)。 Server for NFS 工具組含 [Services for Network File System] MMC 嵌入式管理單元，可用來管理 Server for NFS 和 Client for NFS 元件。 您可以使用嵌入式管理單元來管理電腦上所安裝的 Server for NFS 元件。 Server for NFS 也包含數個 Windows 命令列管理工具：

- **掛接會掛接** 遠端 NFS 共用 (也稱為「匯出) 本機，並將其對應至 Windows 用戶端電腦上的本機磁碟機號。
- **Nfsadmin** 可管理 SERVER for Nfs 和 CLIENT for nfs 元件的設定。
- **Nfsshare** 會設定使用 SERVER for NFS 共用之資料夾的 NFS 共用設定。
- **Nfsstat** 會顯示或重設 SERVER for NFS 所接收之呼叫的統計資料。
- **Showmount –** 會顯示 SERVER for NFS 所匯出的已載入檔案系統。
- **Umount** 會移除裝載 NFS 的磁片磁碟機。

Windows Server 2012 中的 NFS 引進適用于 Windows PowerShell 的 NFS 模組，專為 NFS 提供數個新的 Cmdlet。 這些 Cmdlet 提供簡單的方式來自動化 NFS 管理工作。 如需詳細資訊，請參閱 [Windows PowerShell 中的 NFS Cmdlet](/powershell/module/nfs/)。

## <a name="additional-information"></a>其他資訊

下表提供評估 NFS 的其他資源。

|內容類型|參考|
|---|---|
|部署|[部署網路檔案系統](deploy-nfs.md)|
|作業|[Windows PowerShell 中的 NFS Cmdlet](/powershell/module/nfs/)|
|相關技術|[Windows Server 的儲存空間](../storage.yml)|
