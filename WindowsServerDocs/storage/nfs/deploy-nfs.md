---
title: 部署網路檔案系統
description: 說明如何部署網路檔案系統。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 2f3671283720d515cd3e3e609d98e02343c15892
ms.sourcegitcommit: fcc26ec5a2cc73b59c5752377b39c070d288655e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/19/2018
ms.locfileid: "8976684"
---
# 部署網路檔案系統

>適用於： Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

網路檔案系統 (NFS) 可提供檔案共用解決方案，可讓您在執行 Windows Server 和使用 NFS 通訊協定的 UNIX 作業系統的電腦之間傳輸的檔案。 本主題說明您要部署 NFS 都應該遵循的步驟。

## 什麼是網路檔案系統中的新功能

以下是 Windows Server 2012 中 nfs 變更的項目：

- **NFS 4.1 版的支援**。 這個通訊協定版本包含下列增強功能。
  - 瀏覽防火牆是比較容易，進而改善協助工具。
  - 支援 RPCSEC\_GSS 通訊協定，提供更強大的安全性，並允許用戶端和伺服器交涉安全性。
  - 支援 UNIX 和 Windows 檔案語意。
  - 會利用叢集的檔案伺服器部署。
  - 支援 wan 複合程序。

- **適用於 Windows PowerShell NFS 模組**。 內建 NFS cmdlet 的可用性，讓您更容易自動化各種作業。 Cmdlet 名稱會與其他 Windows PowerShell cmdlet （使用動詞，例如 「 取得 」 和 「 設定 」），讓您更輕鬆的使用者熟悉 Windows PowerShell 來了解如何使用新的 cmdlet 一致。
- **NFS 管理改進功能**。 新的集中式的 UI 為基礎的管理主控台，以簡化設定和 SMB 和 NFS 共用、 配額、 檔案檢測和分類，除了管理叢集的檔案伺服器的管理。
- **識別對應的改進功能**。 新的 UI 支援和工作型的設定可讓系統管理員快速設定身分識別對應來源，並接著建立個別對應的身分識別為使用者的身分識別對應的 Windows PowerShell cmdlet。 改進功能可讓您輕鬆透過 SMB 和 NFS 設定多重通訊協定存取的共用的系統管理員。
- **重組叢集資源的模型**。 這項改良功能，以簡化管理，以及組合 Windows nfs 的叢集資源模型與 SMB 通訊協定的伺服器之間的一致性。 針對 NFS 有許多共用的伺服器，資源網路和 WMI 呼叫所需的數目容錯移轉磁碟區，包含大量的 NFS 共用已降低。
- **與繼續金鑰 Manager 整合**。 繼續金鑰管理員是會追蹤檔案伺服器和檔案系統狀態與啟用 Windows SMB 和 NFS 通訊協定伺服器容錯移轉，而不會中斷用戶端或將其資料儲存在檔案伺服器的伺服器應用程式的元件。 這項改良功能是執行 Windows Server 2012 的檔案伺服器的持續可用性功能的關鍵元件。

## 使用網路檔案系統的案例

NFS 支援 windows 與 unix 作業系統混合的環境。 下列的部署案例是您如何部署持續可用的 Windows Server 2012 檔案伺服器，使用 NFS 範例。

### 異質環境中的佈建檔案共用

這個案例中適用於 Windows 和其他作業系統，例如 UNIX 或 Linux 型用戶端所組成之異質環境的組織電腦。 此案例中，您可以透過 SMB 和 NFS 通訊協定提供多重通訊協定的存取權的相同的檔案共用。 一般而言，當您部署 Windows 檔案伺服器在此案例中，您會想要協助使用者在 Windows 上的與 unix 電腦之間的共同作業。 設定檔案共用時，它會與 SMB 通訊協定，透過存取其檔案的 Windows 使用者與 SMB 和 NFS 通訊協定，共用和 UNIX 為基礎的電腦上的使用者通常透過 NFS 通訊協定存取其檔案。

此案例中，您必須具備有效的身分識別對應來源組態。 Windows Server 2012 支援下列的身分識別對應存放區：

- 對應檔案
- Active Directory 網域服務 (AD DS)
- RFC 2307 相容 LDAP 儲存例如 Active Directory 輕量型目錄服務 (AD LDS)
- 使用者名稱對應 (UNM) 伺服器

### UNIX 型環境中的佈建檔案共用

在此案例中，為 UNIX 型用戶端電腦提供存取 NFS 檔案共用主要 UNIX 型環境中部署 Windows 的檔案伺服器。 讓 Windows 伺服器可以用來儲存 NFS 資料，而不需建立 UNIX-Windows 帳戶對應於 Windows Server 2008 R2 NFS 共用一開始就已實作未對應 UNIX 使用者存取 (UUUA) 選項。 UUUA 可讓系統管理員快速佈建及部署 NFS 而不需要設定帳戶的對應。 當啟用 for NFS，UUUA 會建立自訂的安全性識別碼 (Sid) 來代表未對應的使用者。 對應的使用者帳戶使用標準的 Windows 安全性識別碼 (Sid)，和未對應的使用者使用自訂 NFS Sid。

## 系統需求

Server for NFS 可以安裝任何版本的 Windows Server 2012 上。 您可以使用 NFS 搭配執行 NFS 伺服器或 NFS 用戶端如果符合這些 NFS 伺服器和用戶端實作與其中一個下列通訊協定規格 unix 電腦：

1. NFS 版本 4.1 通訊協定規格 （如 RFC [5661](https://tools.ietf.org/html/rfc5661)中所定義）
2. NFS 版本 3 通訊協定規格 （如 RFC [1813年](https://tools.ietf.org/html/rfc1813)中所定義）
3. NFS 版本 2 通訊協定規格 （如 RFC [1094年](https://tools.ietf.org/html/rfc1094)中所定義）

## 部署 NFS 基礎結構

您需要部署下列電腦，並將它們連接區域網路 (LAN) 上：

- 執行 Windows Server 2012 的您將會安裝 NFS 元件的兩個主要服務的一或多個電腦： Server for NFS 和 NFS 用戶端。 您可以在同一部電腦上或不同電腦上安裝這些元件。
- 一或多個 unix 電腦執行 NFS 伺服器和 NFS 用戶端軟體。 Unix 電腦執行 NFS 伺服器裝載 NFS 檔案共用或匯出，進行存取時正在執行 Windows Server 2012 做為用戶端使用 for NFS 用戶端電腦。 您可以在相同的 unix 電腦或不同 unix 在電腦上，視安裝 NFS 伺服器和用戶端軟體。
- 執行 Windows Server 2008 R2 功能層級的網域控制站。 網域控制站提供使用者驗證資訊及對應的 Windows 環境。
- 網域控制站不部署時，您可以使用網路服務 (NIS) 伺服器來提供 UNIX 環境的使用者驗證資訊。 或者，如果您想要的話，您可以使用密碼及群組正在執行的使用者名稱對應服務的電腦儲存的檔案。

### 使用伺服器管理員的伺服器上安裝網路檔案系統

1. 從 [新增角色及功能精靈]，底下的伺服器角色，選取 [**檔案和存放服務**如果它不已經安裝。
2. 下方**檔案和 iSCSI 服務**，選取 [**檔案伺服器**和**Server for NFS**。 選取要包含所選的 NFS 功能的**新增功能**。
3. 選取**安裝**在伺服器上安裝 NFS 元件。

### 使用 Windows PowerShell 的伺服器上安裝網路檔案系統

1. 啟動 Windows PowerShell。 以滑鼠右鍵按一下工作列上的 PowerShell 圖示，然後選取 [**以系統管理員身分執行**。
2. 執行下列 Windows PowerShell 命令：

```PowerShell
Import-Module ServerManager
Add-WindowsFeature FS-NFS-Service
Import-Module NFS
```

## 設定 NFS 驗證

使用 NFS 版本 4.1 和 NFS 3.0 版通訊協定時，您會有下列驗證和安全性選項。

- RPCSEC\_GSS
  - **Krb5**。 使用 Kerberos 版本 5 通訊協定來驗證使用者授與存取檔案共用。
  - **Krb5i**。 使用 Kerberos 版本 5 通訊協定向完整性檢查 （總和檢查碼），這會驗證資料未遭竄改。
  - **Krb5p**使用 Kerberos 版本 5 通訊協定，這會驗證與加密基於隱私權 NFS 流量。
- AUTH\_SYS

您也可以選擇不使用伺服器授權 (AUTH\_SYS)，讓您能夠啟用未對應的使用者存取的選項。 當使用未對應的使用者存取，您可以指定允許由 UID 未對應的使用者存取 / GID，這是預設值，，或允許匿名存取。

在下一節討論上設定 NFS 驗證的指示。

## 建立 NFS 檔案共用

您可以建立使用伺服器管理員或 Windows PowerShell NFS cmdlet NFS 檔案共用。

### 使用伺服器管理員建立 NFS 檔案共用

1. 本機系統管理員群組的成員登入伺服器。
2. 伺服器管理員會自動啟動。 如果它不會自動啟動，選取 [**開始**、 輸入**servermanager.exe**，，然後選取 [**伺服器管理員**。
3. 在左側，選取 [**檔案和存放服務**，然後選取 [**共用**。
4. 選取 [**建立檔案共用，開始新的共用精靈**。
5. 在**選取設定檔**頁面上，選取**NFS 共用 – 快速**或**NFS 共用-進階**，然後選取 [**下一步**。
6. 在**共用位置**頁面上，選取伺服器和磁碟區，並選取 [**下一步**。
7. **共用名稱**在頁面上，指定新的共用的名稱，並選取 [**下一步**。
8. 在**驗證**頁面上，指定您想要使用此共用的驗證方法。
9. 在**共用的權限**頁面上，選取 [**新增**]，然後指定的主機、 用戶端群組或 netgroup 您想要授與共用的權限。
10. 在**權限**，設定您想要有，使用者存取控制類型，然後選取 **[確定]**。
11. 在**確認**頁面上，檢閱您的設定，並選取 [**建立**建立 NFS 檔案共用。

### 對等的 Windows PowerShell 指令

下列 Windows PowerShell cmdlet 也可以建立 NFS 檔案共用 (其中`nfs1`是共用的名稱和`C:\\shares\\nfsfolder`是檔案路徑):

```PowerShell
New-NfsShare -name nfs1 -Path C:\shares\nfsfolder
```

### 已知問題
NFS 4.1 版可讓檔案名稱，以進行建立或複製使用不正確的字元。 如果您嘗試使用 vi 編輯器中開啟檔案時，它會顯示為已損毀。 您無法從 vi 儲存檔案、 重新命名、 將它移動或變更的權限。 避免使用 illigal 字元。
