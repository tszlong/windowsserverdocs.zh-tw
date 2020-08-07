---
title: 部署網路檔案系統
description: 說明如何部署網路檔案系統。
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 98213b594ee3ae41196bbbbef4da34dbf280e6ad
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87935767"
---
# <a name="deploy-network-file-system"></a>部署網路檔案系統

>適用于： Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

網路檔案系統 (NFS) 提供檔案共用解決方案，可讓您在執行 Windows Server 和 UNIX 作業系統的電腦之間，使用 NFS 通訊協定來傳輸檔案。 本主題說明部署 NFS 時應遵循的步驟。

## <a name="whats-new-in-network-file-system"></a>網路檔案系統的新功能

以下是 Windows Server 2012 中 NFS 的這是什麼變更：

- **NFS 4.1 版的支援**。 此通訊協定版本包含下列增強功能。
  - 流覽防火牆比較簡單，改善了協助工具。
  - 支援 RPCSEC \_ GSS 通訊協定，提供更強的安全性，並允許用戶端和伺服器協調安全性。
  - 支援 UNIX 和 Windows 檔案的語法。
  - 利用叢集檔案伺服器部署。
  - 支援 WAN 易懂的複合程式。

- **適用于 Windows PowerShell 的 NFS 模組**。 內建 NFS Cmdlet 的可用性可讓您更輕鬆地將各種作業自動化。 Cmdlet 名稱會與其他 Windows PowerShell Cmdlet 一致 (使用動詞命令，例如「Get」和「Set」 ) ，讓熟悉 Windows PowerShell 的使用者更容易瞭解如何使用新的 Cmdlet。
- **NFS 管理改良功能**。 新的集中式 UI 管理主控台除了管理叢集檔案伺服器之外，也簡化了 SMB 和 NFS 共用、配額、檔案檢測和分類的設定和管理。
- **識別對應的改善**。 新的 UI 支援和以工作為基礎的 Windows PowerShell Cmdlet，可讓系統管理員快速設定身分識別對應來源，然後為使用者建立個別的對應身分識別。 增強功能可讓系統管理員輕鬆地設定共用，以透過 NFS 和 SMB 進行多重通訊協定存取。
- 叢集**資源模型重建**。 這種改善可為 Windows NFS 與 SMB 通訊協定伺服器的叢集資源模型提供一致性，並簡化系統管理。 對於具有多個共用的 NFS 伺服器，資源網路和所需的 WMI 呼叫數目會降低包含大量 NFS 共用的磁片區。
- **與繼續金鑰管理員的整合**。 [繼續金鑰管理員] 是追蹤檔案伺服器和檔案系統狀態的元件，可讓 Windows SMB 和 NFS 通訊協定伺服器容錯回復，而不會中斷將其資料儲存在檔案伺服器上的用戶端或伺服器應用程式。 這項改善是執行 Windows Server 2012 之檔案伺服器的持續可用性功能的重要元件。

## <a name="scenarios-for-using-network-file-system"></a>使用網路檔案系統的案例

NFS 支援以 Windows 和 UNIX 為基礎的作業系統混合式環境。 下列部署案例是如何使用 NFS 部署持續可用的 Windows Server 2012 檔案伺服器的範例。

### <a name="provision-file-shares-in-heterogeneous-environments"></a>在不同的環境中布建檔案共用

此案例適用于包含 Windows 和其他作業系統（例如 UNIX 或 Linux 用戶端電腦）的異類環境的組織。 在此案例中，您可以在 SMB 和 NFS 通訊協定上，為相同的檔案共用提供多重通訊協定存取。 一般而言，當您在此案例中部署 Windows 檔案伺服器時，您會想要加速 Windows 和 UNIX 電腦上使用者之間的共同作業。 當您設定檔案共用時，它會與 SMB 和 NFS 通訊協定共用，而 Windows 使用者會透過 SMB 通訊協定存取其檔案，而 UNIX 電腦上的使用者通常會透過 NFS 通訊協定存取其檔案。

在此案例中，您必須具有有效的身分識別對應來源設定。 Windows Server 2012 支援下列身分識別對應存放區：

- 對應檔
- Active Directory Domain Services (AD DS)
- RFC 2307 相容的 LDAP 存放區，例如 Active Directory 輕量型目錄服務 (AD LDS) 
- 消費者名稱對應 (UNM) 伺服器

### <a name="provision-file-shares-in-unix-based-environments"></a>在以 UNIX 為基礎的環境中布建檔案共用

在此案例中，Windows 檔案伺服器會部署在主要以 UNIX 為基礎的環境中，以提供 UNIX 用戶端電腦的 NFS 檔案共用存取權。 未對應的 UNIX 使用者存取 (UUUA) 選項最初是針對 Windows Server 2008 R2 中的 NFS 共用所執行，因此 Windows 伺服器可以用來儲存 NFS 資料，而不需要建立 UNIX 到 Windows 帳戶的對應。 UUUA 可讓系統管理員快速布建及部署 NFS，而不需要設定帳戶對應。 針對 NFS 啟用時，UUUA 會建立 (Sid) 的自訂安全識別碼，以代表未對應的使用者。 對應的使用者帳戶會使用標準的 Windows 安全識別碼 (Sid) ，而未對應的使用者會使用自訂 NFS Sid。

## <a name="system-requirements"></a>系統需求

Server for NFS 可以安裝在任何版本的 Windows Server 2012 上。 如果這些 NFS 伺服器和用戶端執行符合下列其中一種通訊協定規格，您就可以搭配使用 NFS 與 UNIX 型電腦，這些電腦是執行 NFS 伺服器或 NFS 用戶端：

1. 如 RFC [5661](https://tools.ietf.org/html/rfc5661)中所定義的 NFS 4.1 版通訊協定規格 () 
2.  (如 RFC [1813](https://tools.ietf.org/html/rfc1813)中所定義的 NFS 版本3通訊協定規格) 
3.  (如 RFC [1094](https://tools.ietf.org/html/rfc1094)中所定義的 NFS 版本2通訊協定規格) 

## <a name="deploy-nfs-infrastructure"></a>部署 NFS 基礎結構

您必須部署下列電腦，並將它們連線到區域網路 (LAN) ：

- 一或多部執行 Windows Server 2012 的電腦，您將在其上安裝兩個主要的 Services for NFS 元件： Server for NFS 和 Client for NFS。 您可以將這些元件安裝在同一部電腦或不同的電腦上。
- 執行 NFS 伺服器和 NFS 用戶端軟體的一或多部 UNIX 電腦。 執行 NFS 伺服器的 UNIX 電腦會裝載 NFS 檔案共用或匯出，這是使用 Client for NFS 以用戶端的身分執行 Windows Server 2012 的電腦所存取的。 您可以視需要在相同的 UNIX 電腦或不同的 UNIX 電腦上安裝 NFS 伺服器和用戶端軟體。
- 在 Windows Server 2008 R2 功能等級執行的網域控制站。 網域控制站會提供 Windows 環境的使用者驗證資訊與對應。
- 未部署網域控制站時，您可以使用網路資訊服務 (NIS) 伺服器來提供 UNIX 環境的使用者驗證資訊。 或者，如果您想要的話，也可以使用儲存在執行消費者名稱對應服務之電腦上的密碼和群組檔案。

### <a name="install-network-file-system-on-the-server-with-server-manager"></a>在具有伺服器管理員的伺服器上安裝網路檔案系統

1. 從 [新增角色及功能精靈] 中的 [伺服器角色] 之下，選取 [檔案和存放服務]**** (若尚未安裝)。
2. 在 [檔案**和 ISCSI 服務**] 底下，選取 [**檔案伺服器**和**Server for NFS**]。 選取 [**新增功能**] 以包含選取的 NFS 功能。
3. 選取 [**安裝**] 以在伺服器上安裝 NFS 元件。

### <a name="install-network-file-system-on-the-server-with-windows-powershell"></a>使用 Windows PowerShell 在伺服器上安裝網路檔案系統

1. 啟動 Windows PowerShell。 用滑鼠右鍵按一下工作列上的 PowerShell 圖示，然後選取 [以系統管理員身分執行]****。
2. 執行下列 Windows PowerShell 命令：

```PowerShell
Import-Module ServerManager
Add-WindowsFeature FS-NFS-Service
Import-Module NFS
```

## <a name="configure-nfs-authentication"></a>設定 NFS 驗證

使用 NFS 版本4.1 和 NFS 版本3.0 通訊協定時，您有下列驗證和安全性選項。

- RPCSEC \_ GSS
  - **Krb5**。 會先使用 Kerberos 第5版通訊協定來驗證使用者，然後再授與檔案共用的存取權。
  - **Krb5i**。 使用 Kerberos 第5版通訊協定來驗證完整性檢查 (總和檢查碼) ，這會確認資料尚未改變。
  - **Krb5p** 使用 Kerberos 版本 5 通訊協定，以加密的方式驗證 NFS 流量來確保私密性。
- 驗證 \_ SYS

您也可以選擇不使用伺服器授權 (AUTH \_ SYS) ，這可讓您選擇啟用未對應的使用者存取。 使用未對應的使用者存取時，您可以指定以允許 UID/GID 的未對應使用者存取（預設值）或允許匿名存取。

下一節將討論如何設定 NFS 驗證的指示。

## <a name="create-an-nfs-file-share"></a>建立 NFS 檔案共用

您可以使用伺服器管理員或 Windows PowerShell NFS Cmdlet 來建立 NFS 檔案共用。

### <a name="create-an-nfs-file-share-with-server-manager"></a>使用伺服器管理員建立 NFS 檔案共用

1. 以本機 Administrators 群組成員的身分登入伺服器。
2. [伺服器管理員] 將自動啟動。 如果未自動啟動，請選取 [**開始**]，輸入**servermanager.exe**，然後選取 [**伺服器管理員**]。
3. 在左側選取 [檔案**和存放服務**]，然後選取 [**共用**]。
4. 選取**以建立檔案共用，啟動 [新增共用] 嚮導**。
5. 在 [**選取設定檔**] 頁面上，選取 [ **NFS 共用-快速**] 或 [ **nfs 共用-先進**]，然後選取 **[下一步]**。
6. 在 [**共用位置**] 頁面上，選取伺服器和磁片區，然後選取 **[下一步]**。
7. 在 [**共用名稱**] 頁面上，指定新共用的名稱，然後選取 **[下一步]**。
8. 在 [**驗證**] 頁面上，指定您要用於此共用的驗證方法。
9. 在 [**共用許可權**] 頁面上，選取 [**新增**]，然後指定您想要授與共享許可權的主機、用戶端群組或網路群組。
10. 在 [**許可權**] 中，設定您想要讓使用者擁有的存取控制類型，然後選取 **[確定]**。
11. 在 [**確認**] 頁面上，檢查您的設定，然後選取 [**建立**] 以建立 NFS 檔案共用。

### <a name="windows-powershell-equivalent-commands"></a>Windows PowerShell 對應的命令

下列 Windows PowerShell Cmdlet 也可以建立 NFS 檔案共用 (，其中 `nfs1` 是共用的名稱，而是檔案 `C:\\shares\\nfsfolder` 路徑) ：

```PowerShell
New-NfsShare -name nfs1 -Path C:\shares\nfsfolder
```

### <a name="known-issue"></a>已知問題
NFS 4.1 版允許使用不合法的字元來建立或複製檔案名。 如果您嘗試使用 vi 編輯器開啟檔案，它會顯示為已損毀。 您無法從 vi、rename、move 或 change 許可權儲存檔案。 避免使用不合法的字元。
