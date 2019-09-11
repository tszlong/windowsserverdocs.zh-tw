---
title: SMB 安全性增強功能
description: Windows Server 2012 R2、Windows Server 2012 和 Windows Server 2016 中 SMB 加密功能的說明。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 4e57c77e6cfb792c90abf3840cf11461b4ad5f60
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867302"
---
# <a name="smb-security-enhancements"></a>SMB 安全性增強功能

>適用於：Windows Server 2012 R2、Windows Server 2012、Windows Server 2016

本主題說明 Windows Server 2012 R2、Windows Server 2012 和 Windows Server 2016 中的 SMB 安全性增強功能。

## <a name="smb-encryption"></a>SMB 加密

SMB 加密提供 SMB 資料的端對端加密，並保護資料免于不受信任網路上的竊聽發生。 您可以用最少的方式部署 SMB 加密，但可能需要較小的額外成本來進行特製化的硬體或軟體。 不需要網際網路通訊協定安全性（IPsec）或 WAN 加速器。 SMB 加密可以根據每個共用或整個檔案伺服器進行設定，而且可以在資料流程經不受信任網路的各種案例中啟用。

>[!NOTE]
>SMB 加密不涵蓋待用的安全性，這通常是由 BitLocker 磁碟機加密處理。

在需要保護機密資料免于攔截式攻擊的任何情況下，都應該考慮 SMB 加密。 可能的案例包括：

- 系統會使用 SMB 通訊協定來移動資訊工作者的敏感性資料。 SMB 加密可在檔案伺服器與用戶端之間提供端對端隱私權和完整性保證，而不論所進行的網路為何，例如非 Microsoft 提供者所維護的廣域網路（WAN）連線。
- SMB 3.0 可讓檔案伺服器為伺服器應用程式（例如 SQL Server 或 Hyper-v）提供持續可用的存放裝置。 啟用 SMB 加密可讓您有機會保護該資訊免于窺探攻擊。 SMB 加密比大部分存放區域網路（San）所需的專用硬體解決方案更容易使用。

>[!IMPORTANT]
>請注意，相較于非加密，任何端對端加密保護都有明顯的效能營運成本。

## <a name="enable-smb-encryption"></a>啟用 SMB 加密

您可以為整個檔案伺服器啟用 SMB 加密，或僅針對特定的檔案共用。 使用下列其中一個程式啟用 SMB 加密：

### <a name="enable-smb-encryption-with-windows-powershell"></a>使用 Windows PowerShell 啟用 SMB 加密

1. 若要啟用個別檔案共用的 SMB 加密，請在伺服器上輸入下列腳本：
    
    ```PowerShell
    Set-SmbShare –Name <sharename> -EncryptData $true
    ```
2. 若要為整個檔案伺服器啟用 SMB 加密，請在伺服器上輸入下列腳本：
    
    ```PowerShell
    Set-SmbServerConfiguration –EncryptData $true
    ```
3. 若要建立新的 SMB 檔案共用，並啟用 SMB 加密，請輸入下列腳本：
    
    ```PowerShell
    New-SmbShare –Name <sharename> -Path <pathname> –EncryptData $true
    ```

### <a name="enable-smb-encryption-with-server-manager"></a>使用伺服器管理員啟用 SMB 加密

1. 在伺服器管理員中，開啟 [檔案**和存放服務**]。
2. 選取 [**共用**] 以開啟 [共用] 管理頁面。
3. 在您要啟用 SMB 加密的共用上按一下滑鼠右鍵，然後選取 [**屬性**]。
4. 在共用的 [**設定**] 頁面上，選取 [**加密資料存取**]。 此共用的遠端檔案存取已加密。

### <a name="considerations-for-deploying-smb-encryption"></a>部署 SMB 加密的考慮

根據預設，當啟用檔案共用或伺服器的 SMB 加密時，只允許 SMB 3.0 用戶端存取指定的檔案共用。 這會強制系統管理員針對所有存取共用的用戶端保護資料的意圖。 不過，在某些情況下，系統管理員可能會想要允許不支援 SMB 3.0 的用戶端進行未加密的存取（例如，在使用混合用戶端作業系統版本的轉換期間）。 若要允許不支援 SMB 3.0 的用戶端進行未加密存取，請在 Windows PowerShell 中輸入下列腳本：

```PowerShell
Set-SmbServerConfiguration –RejectUnencryptedAccess $false
```

下一節所述的「安全方言」協商功能，可防止攔截式攻擊將 SMB 3.0 的連線降級為 SMB 2.0 （這會使用未加密的存取權）。 不過，它不會防止降級至 SMB 1.0，這也會導致未加密的存取。 若要保證 SMB 3.0 用戶端一律使用 SMB 加密來存取加密的共用，您必須停用 SMB 1.0 伺服器。 （如需指示，請參閱[停用 SMB 1.0](#disabling-smb-10)一節）。如果 **– RejectUnencryptedAccess**設定保留 **$true**的預設設定，則只允許支援加密的 SMB 3.0 用戶端存取檔案共用（smb 1.0 用戶端也會遭到拒絕）。

>[!NOTE]
>* SMB 加密使用進階加密標準（AES）-CCM 演算法來加密和解密資料。 AES-CCM 也會針對加密的檔案共用提供資料完整性驗證（簽署），而不論 SMB 簽署設定為何。 如果您想要啟用不加密的 SMB 簽署，可以繼續執行此動作。 如需詳細資訊，請參閱[SMB 簽署的基本概念](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/)。
>* 當您的組織使用廣域網路（WAN）加速設備時，如果您嘗試存取檔案共用或伺服器，可能會遇到問題。
>* 使用預設設定（不允許加密檔案共用的未加密存取）時，如果不支援 SMB 3.0 的用戶端嘗試存取加密的檔案共用，則會將事件識別碼1003記錄到 SmbServer/Operational 事件記錄檔，而且用戶端會收到**拒絕存取**的錯誤訊息。
>* NTFS 檔案系統中的 SMB 加密和加密檔案系統（EFS）不相關，而且 SMB 加密不需要或依存于使用 EFS。
>* SMB 加密和 BitLocker 磁碟機加密不相關，而且 SMB 加密不需要或相依于使用 BitLocker 磁碟機加密。

## <a name="secure-dialect-negotiation"></a>安全用語談判

SMB 3.0 能夠偵測攔截式攻擊，嘗試降級 SMB 2.0 或 SMB 3.0 通訊協定，或用戶端和伺服器所協商的功能。 當用戶端或伺服器偵測到這類攻擊時，連接會中斷連線，而事件識別碼1005會記錄在 SmbServer/Operational 事件記錄檔中。 安全方言的協商無法偵測或防止從 SMB 2.0 或3.0 降級到 SMB 1.0。 因此，若要利用 SMB 加密的完整功能，我們強烈建議您停用 SMB 1.0 伺服器。 如需詳細資訊，請參閱[停用 SMB 1.0](#disabling-smb-10)。

下一節所述的「安全方言」協商功能，可防止攔截式攻擊將 SMB 3 的連線降級為 SMB 2 （這會使用未加密的存取權）;不過，它不會防止對 SMB 1 的降級，這也會導致未加密的存取。 如需更多有關 SMB 的舊版非 Windows 實施問題的詳細資訊，請參閱[Microsoft 知識庫](http://support.microsoft.com/kb/2686098)。

## <a name="new-signing-algorithm"></a>新的簽署演算法

SMB 3.0 使用較新的加密演算法進行簽署：進階加密標準（AES）-以密碼為基礎的訊息驗證碼（CMAC）。 SMB 2.0 使用較舊的 HMAC-SHA256 加密演算法。 AES CMAC 和 AES-CCM 可以大幅加速在具有 AES 指示支援的最新 Cpu 上的資料加密。 如需詳細資訊，請參閱[SMB 簽署的基本概念](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/)。

## <a name="disabling-smb-10"></a>停用 SMB 1。0

SMB 1.0 中的舊版電腦瀏覽器服務和遠端系統管理通訊協定功能現在是分開的，可以將它們刪除。 這些功能仍預設為啟用，但如果您沒有較舊的 SMB 用戶端（例如執行 Windows Server 2003 或 Windows XP 的電腦），您可以移除 SMB 1.0 功能來提高安全性，並可能減少修補程式。

>[!NOTE]
>SMB 2.0 是在 Windows Server 2008 和 Windows Vista 中引進。 較舊的用戶端（例如執行 Windows Server 2003 或 Windows XP 的電腦）不支援 SMB 2.0;因此，如果 SMB 1.0 伺服器已停用，它們將無法存取檔案共用或列印共用。 此外，某些非 Microsoft SMB 用戶端可能無法存取 SMB 2.0 檔案共用或列印共用（例如，具有「掃描至共用」功能的印表機）。

開始停用 SMB 1.0 之前，您必須先瞭解 SMB 用戶端目前是否已連線到執行 SMB 1.0 的伺服器。 若要這樣做，請在 Windows PowerShell 中輸入下列 Cmdlet：

```PowerShell
Get-SmbSession | Select Dialect,ClientComputerName,ClientUserName | ? Dialect -lt 2
```

>[!NOTE]
>您應該在一周當中重複執行此腳本（每天多次），以建立一個 audit 記錄。 您也可以將它當做排程工作來執行。

若要停用 SMB 1.0，請在 Windows PowerShell 中輸入下列腳本：

```PowerShell
Set-SmbServerConfiguration –EnableSMB1Protocol $false
```

>[!NOTE]
>如果 SMB 用戶端連線遭到拒絕，因為執行 SMB 1.0 的伺服器已停用，則事件識別碼1001會記錄在 SmbServer/Operational 事件記錄檔中。

## <a name="more-information"></a>詳細資訊

以下是有關 SMB 的一些額外資源，以及 Windows Server 2012 中的相關技術。

- [伺服器訊息區](file-server-smb-overview.md)
- [Windows Server 的儲存空間](../storage.md)
- [適用于應用程式資料的向外延展檔案伺服器](../../failover-clustering/sofs-overview.md)