---
title: 部署網路檔案系統
description: 描述如何部署網路檔案系統。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: ab80b6d73a40256d5935635c9afc55b7c53727d3
ms.sourcegitcommit: 2977c707a299929c6ab0d1e0adab2e1c644b8306
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/24/2019
ms.locfileid: "63737828"
---
# <a name="deploy-network-file-system"></a>部署網路檔案系統

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

網路檔案系統 (NFS) 提供檔案共用解決方案，可讓您執行 Windows Server 和使用 NFS 通訊協定的 UNIX 作業系統的電腦之間傳輸檔案。 本主題說明部署 NFS 時，您應該遵循的步驟。

## <a name="whats-new-in-network-file-system"></a>什麼是網路檔案系統的新功能

以下是 Windows Server 2012 中的 nfs 變更的項目：

- **支援 NFS 版本 4.1**。 此通訊協定版本包含下列增強功能。
  - 瀏覽防火牆得更為簡單，改善協助工具。
  - 支援 RPCSEC\_GSS 通訊協定，提供更強的安全性，而且可讓用戶端與伺服器交涉安全性。
  - 支援 UNIX 和 Windows 檔案語意。
  - 利用叢集的檔案伺服器部署。
  - 支援且方便 WAN 複合程序。

- **適用於 Windows PowerShell 的 NFS 模組**。 可用性的內建的 NFS cmdlet 可讓您更輕鬆地自動化各種作業。 Cmdlet 名稱都必須配合其他 Windows PowerShell cmdlet （使用動詞命令，例如 「 Get 」 和 「 設定 」），方便您使用者熟悉 Windows PowerShell，來了解如何使用新的 cmdlet。
- **NFS 管理改善**。 新的集中式的 UI 為基礎的管理主控台可簡化設定和管理 SMB 與 NFS 共用、 配額、 檔案檢測和分類，除了管理叢集的檔案伺服器。
- **身分識別對應改進**。 新的 UI 支援和以工作為基礎的設定可讓快速地設定身分識別對應來源，然後再建立 使用者的個別對應身分識別的系統管理員的身分識別對應的 Windows PowerShell cmdlet。 增強功能，方便系統管理員可以設定多重通訊協定存取的共用，透過 NFS 和 SMB 兩者。
- **叢集資源模型進行重建**。 這項改善帶來 Windows nfs 叢集資源模型和 SMB 通訊協定伺服器之間的一致性並簡化管理。 有許多共用的 NFS 伺服器，資源網路和所需的 WMI 呼叫數目容錯移轉的磁碟區會包含大量的 NFS 共用會減少。
- **與繼續金鑰管理員整合**。 繼續金鑰管理員是可追蹤的檔案伺服器和檔案系統狀態，並可讓 Windows SMB 與 NFS 通訊協定伺服器容錯移轉，而不會中斷用戶端或伺服器應用程式，將其資料儲存在檔案伺服器上的元件。 這項改進是執行 Windows Server 2012 檔案伺服器的持續可用性功能的重要元件。

## <a name="scenarios-for-using-network-file-system"></a>使用網路檔案系統的案例

NFS 支援混合使用以 Windows 為基礎和以 UNIX 為基礎作業系統的環境。 下列部署案例是如何部署持續可用的 Windows Server 2012 檔案伺服器，使用 NFS 的範例。

### <a name="provision-file-shares-in-heterogeneous-environments"></a>在異質環境中的佈建檔案共用

此案例適用於 Windows 和其他作業系統，例如 UNIX 或 Linux 為基礎的用戶端所組成的異質環境的組織的電腦。 在此案例中，您可以透過 SMB 及 NFS 通訊協定提供多重通訊協定存取相同的檔案共用。 一般而言，當您部署的 Windows 檔案伺服器，在此案例中，您會想要加速在 Windows 上的使用者與以 UNIX 為基礎的電腦之間的共同作業。 設定檔案共用時，使用 SMB 及 NFS 通訊協定，透過 SMB 通訊協定存取其檔案的 Windows 使用者與共用，並以 UNIX 為基礎的電腦上的使用者通常透過 NFS 通訊協定存取其檔案。

此案例中，您必須使用有效的身分識別對應來源組態。 Windows Server 2012 支援下列身分識別對應存放區：

- 對應檔案
- Active Directory 網域服務 (AD DS)
- RFC 2307 相容 LDAP 存放區，例如 Active Directory 輕量型目錄服務 (AD LDS)
- 使用者名稱對應 (UNM) 伺服器

### <a name="provision-file-shares-in-unix-based-environments"></a>以 UNIX 為基礎的環境中的佈建檔案共用

在此案例中，Windows 檔案伺服器環境中部署主要以 UNIX 為基礎來提供對 NFS 檔案共用的存取權以 UNIX 為基礎的用戶端電腦。 未對應的 UNIX 使用者存取權 (UUUA) 選項一開始實作了 Windows Server 2008 R2 中的 NFS 共用，讓伺服器可用來儲存 NFS 資料，而不需要建立 UNIX 至 Windows 的 Windows 帳戶的對應。 UUUA 可讓系統管理員快速佈建和部署而不需要設定帳戶對應的 NFS。 當啟用 NFS，UUUA 建立自訂的安全性識別碼 (Sid) 來表示未對應的使用者。 對應的使用者帳戶使用標準的 Windows 安全性識別碼 (Sid) 和未對應的使用者使用自訂的 NFS Sid。

## <a name="system-requirements"></a>系統需求

可以在任何版本的 Windows Server 2012 上安裝 server for NFS。 您可以使用 NFS 與 unix 的電腦執行的 NFS 伺服器或 NFS 用戶端如果符合這些 NFS 伺服器和用戶端實作與其中一個下列的通訊協定規格：

1. NFS 版本 4.1 通訊協定規格 (RFC 中定義[5661](https://tools.ietf.org/html/rfc5661))
2. NFS 版本 3 通訊協定規格 (RFC 中定義[1813年](https://tools.ietf.org/html/rfc1813))
3. NFS 版本 2 通訊協定規格 (RFC 中定義[1094年](https://tools.ietf.org/html/rfc1094))

## <a name="deploy-nfs-infrastructure"></a>部署 NFS 基礎結構

您必須部署以下的電腦，並將其連接區域網路 (LAN) 上：

- 一或多部執行 Windows Server 2012，您將會安裝兩個主要的 Services for NFS 元件：Server for NFS 和 Client for NFS。 您可以在同一部電腦上或在不同電腦上安裝這些元件。
- 一或多個以 UNIX 為基礎的電腦執行的 NFS 伺服器及 NFS 用戶端軟體。 以 UNIX 為基礎的電腦執行的 NFS 伺服器會裝載 NFS 檔案共用或匯出，做為使用 NFS 用戶端的用戶端執行 Windows Server 2012 的電腦存取。 在相同的 unix 電腦或不同的 unix 電腦，視需要，您可以安裝 NFS 伺服器和用戶端軟體。
- 在 Windows Server 2008 R2 功能層級執行網域控制站。 網域控制站會提供使用者驗證資訊和 Windows 環境的對應。
- 當沒有部署的網域控制站時，您可以使用網路資訊服務 (NIS) 伺服器提供 UNIX 環境的使用者驗證資訊。 或者，如果您想，您可以使用儲存在執行使用者名稱對應服務的電腦的密碼及群組檔案。

### <a name="install-network-file-system-on-the-server-with-server-manager"></a>使用伺服器管理員在伺服器上安裝網路檔案系統

1. 從 [新增角色及功能精靈] 中的 [伺服器角色] 之下，選取 [檔案和存放服務]  (若尚未安裝)。
2. 底下**檔案和 iSCSI 服務**，選取**檔案伺服器**並**Server for NFS**。 選取 **將功能加入**包含選取的 NFS 功能。
3. 選取 **安裝**伺服器上安裝 NFS 元件。

### <a name="install-network-file-system-on-the-server-with-windows-powershell"></a>使用 Windows PowerShell 的伺服器上安裝網路檔案系統

1. 啟動 Windows PowerShell。 用滑鼠右鍵按一下工作列上的 PowerShell 圖示，然後選取 [以系統管理員身分執行]  。
2. 執行下列 Windows PowerShell 命令：

```PowerShell
Import-Module ServerManager
Add-WindowsFeature FS-NFS-Service
Import-Module NFS
```

## <a name="configure-nfs-authentication"></a>設定 NFS 驗證

使用 NFS 版本 4.1 和 NFS 版本 3.0 通訊協定時，您會有下列的驗證和安全性選項。

- RPCSEC\_GSS
  - **Krb5**。 使用 Kerberos 版本 5 通訊協定來驗證使用者之後再授與檔案共用的存取權。
  - **Krb5i**。 使用 Kerberos 版本 5 通訊協定透過完整性檢查 （總和檢查碼），驗證會驗證資料尚未改變。
  - **Krb5p**使用 Kerberos 版本 5 通訊協定、 驗證 NFS 流量以加密的隱私權。
- 驗證\_SYS

您也可以選擇不將伺服器授權 (AUTH\_SYS)，可讓您啟用未對應的使用者存取的選項。 使用未對應的使用者存取時，您可以指定允許未對應的使用者存取依 UID / GID，預設值，或允許匿名存取。

設定 NFS 驗證上的指示是由下列一節所述。

## <a name="create-an-nfs-file-share"></a>建立 NFS 檔案共用

您可以建立使用 伺服器管理員 或 Windows PowerShell 的 NFS cmdlet 的 NFS 檔案共用。

### <a name="create-an-nfs-file-share-with-server-manager"></a>使用伺服器管理員建立 NFS 檔案共用

1. 以本機 Administrators 群組成員的身分登入伺服器。
2. [伺服器管理員] 將自動啟動。 如果沒有自動啟動，請選取**開始**，型別**servermanager.exe**，然後選取**伺服器管理員**。
3. 在左側，選取**檔案和存放服務**，然後選取**共用**。
4. 選取 **若要建立檔案共用，請啟動 新增共用精靈**。
5. 在上**選取設定檔**頁面上，選取**NFS 共用-快速**或是**NFS 共用-進階**，然後選取**下一步** 。
6. 在 [**共用位置**頁面上，選取一部伺服器和磁碟區，然後選取**下一步]** 。
7. 在 [**共用名稱**頁面中指定的新共用的名稱，然後選取**下一步]** 。
8. 在 **驗證**頁面上，指定您想要使用此共用的驗證方法。
9. 在 **共用的權限**頁面上，選取**新增**，然後指定主機、 用戶端群組或您想要共用的權限授與的 netgroup。
10. 在 **權限**，設定您想要有此項目，然後選取使用者的存取控制項的型別 **確定** 。
11. 在 **確認**頁面上，檢閱您的設定，然後選取**建立**建立 NFS 檔案共用。

### <a name="windows-powershell-equivalent-commands"></a>Windows PowerShell 對應的命令

下列 Windows PowerShell cmdlet 也可以建立 NFS 檔案共用 (其中`nfs1`是共用的名稱和`C:\\shares\\nfsfolder`是檔案路徑):

```PowerShell
New-NfsShare -name nfs1 -Path C:\shares\nfsfolder
```

### <a name="known-issue"></a>已知問題
NFS 4.1 版可讓檔案名稱，要建立或使用不合法的字元複製。 如果您嘗試使用 vi 編輯器中開啟檔案時，它會顯示為已損毀。 您無法將檔案儲存從 vi、 重新命名、 移動或變更權限。 避免使用 illigal 字元。
