---
title: SMB 安全性增強功能
description: 說明 Windows Server 2012 R2、Windows Server 2012 和 Windows Server 2016 中的 SMB 加密功能。
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: e81b5ca5d28c33187b90fbabebc3d3f36073124c
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954695"
---
# <a name="smb-security-enhancements"></a>SMB 安全性增強功能

>適用於：Windows Server 2012 R2、Windows Server 2012、Windows Server 2016

本主題說明 Windows Server 2012 R2、Windows Server 2012 和 Windows Server 2016 中的 SMB 安全性增強功能。

## <a name="smb-encryption"></a>SMB 加密

SMB 加密可提供 SMB 資料的端對端加密，並保護資料免於在不受信任的網路上遭到竊取。 您可以用最少量的工作部署 SMB 加密，但額外的特殊硬體或軟體可能會使成本略微提高。 此功能不需要網際網路通訊協定安全性 (IPsec) 或 WAN 加速器。 SMB 加密可就個別共用或對整個檔案伺服器進行設定，並且可針對讓資料周遊非信任網路的各種案例加以啟用。

>[!NOTE]
>SMB 加密不涵蓋待用資料的安全性，這通常會由 BitLocker 磁碟機加密來處理。

在任何需要保護敏感性資料免於遭受攔截式攻擊的情況下，都應考慮使用 SMB 加密。 可能的案例包括：

- 使用 SMB 通訊協定移動資訊工作者的敏感性資料。 SMB 加密可在檔案伺服器與用戶端之間提供端對端隱私權和完整性保證，無論所周遊的網路為何，例如非 Microsoft 提供者所維護的廣域網路 (WAN) 連線。
- SMB 3.0 可讓檔案伺服器為伺服器應用程式提供持續可用的儲存體，例如 SQL Server 或 Hyper-V。 啟用 SMB 加密，可讓您有機會保護該資訊免於遭受窺探攻擊。 SMB 加密比多數存放區域網路 (SAN) 所需的專用硬體解決方案更容易使用。

>[!IMPORTANT]
>但請注意，相較於非加密機制，任何端對端加密保護都會產生明顯的效能營運成本。

## <a name="enable-smb-encryption"></a>啟用 SMB 加密

您可以為整個檔案伺服器啟用 SMB 加密，或僅針對特定的檔案共用啟用。 請使用下列其中一個程序來啟用 SMB 加密：

### <a name="enable-smb-encryption-with-windows-powershell"></a>使用 Windows PowerShell 啟用 SMB 加密

1. 若要為個別的檔案共用啟用 SMB 加密，請在伺服器上輸入下列指令碼：

    ```PowerShell
    Set-SmbShare –Name <sharename> -EncryptData $true
    ```
2. 若要為整個檔案伺服器啟用 SMB 加密，請在伺服器上輸入下列指令碼：

    ```PowerShell
    Set-SmbServerConfiguration –EncryptData $true
    ```
3. 若要新建啟用 SMB 加密的 SMB 檔案共用，請輸入下列指令碼：

    ```PowerShell
    New-SmbShare –Name <sharename> -Path <pathname> –EncryptData $true
    ```

### <a name="enable-smb-encryption-with-server-manager"></a>使用伺服器管理員啟用 SMB 加密

1. 在伺服器管理員中，開啟 [檔案和存放服務]  。
2. 選取 [共用]  以開啟 [共用] 管理頁面。
3. 以滑鼠右鍵按一下要啟用 SMB 加密的共用，然後選取 [屬性]  。
4. 在共用的 [設定]  頁面上，選取 [加密資料存取]  。 對此共用的遠端檔案存取會進行加密。

### <a name="considerations-for-deploying-smb-encryption"></a>部署 SMB 加密的考量

根據預設，為檔案共用或伺服器啟用 SMB 加密時，只會允許 SMB 3.0 用戶端存取指定的檔案共用。 這會強制執行系統管理員為所有存取共用的用戶端進行資料保護的意圖。 但在某些情況下，系統管理員可能會想要允許不支援 SMB 3.0 的用戶端進行未加密存取 (例如，在使用混合的用戶端作業系統版本時進行轉換期間)。 若要允許不支援 SMB 3.0 的用戶端進行未加密存取，請在 Windows PowerShell 中輸入下列指令碼：

```PowerShell
Set-SmbServerConfiguration –RejectUnencryptedAccess $false
```

下一節所說明的安全方言交涉功能，可防止攔截式攻擊將 SMB 3.0 的連線降級為 SMB 2.0 (後者會使用未加密的存取)。 不過，此功能不會防止降級至 SMB 1.0 (也會導致未加密的存取)。 若要確保 SMB 3.0 用戶端一律使用 SMB 加密來存取加密的共用，您必須停用 SMB 1.0 伺服器。 (如需指示，請參閱[停用 SMB 1.0](#disabling-smb-10) 一節。)如果 **–RejectUnencryptedAccess** 設定保留為預設設定 **$true**，則只會允許支援加密的 SMB 3.0 用戶端存取檔案共用 (SMB 1.0 用戶端也會遭到拒絕)。

>[!NOTE]
>* SMB 加密會使用進階加密標準 (AES)-CCM 演算法來加密和解密資料。 AES-CCM 也會為加密的檔案共用提供資料完整性驗證 (簽署)，無論 SMB 簽署設定為何。 如果您想要啟用不加密的 SMB 簽署，可以繼續執行此動作。 如需詳細資訊，請參閱 [SMB 簽署的基本概念](/archive/blogs/josebda/the-basics-of-smb-signing-covering-both-smb1-and-smb2)。
>* 如果您的組織使用廣域網路 (WAN) 加速設備，您在嘗試存取檔案共用或伺服器時可能會遇到問題。
>* 使用預設組態時 (此時將不允許對加密的檔案共用進行未加密存取)，如果不支援 SMB 3.0 的用戶端嘗試存取加密的檔案共用，則會事件識別碼 1003 記錄到 Microsoft-Windows-SmbServer/Operational 事件記錄檔中，且用戶端將會收到**拒絕存取**錯誤訊息。
>* SMB 加密與 NTFS 檔案系統中的加密檔案系統 (EFS) 無關，且 SMB 加密不需要或不依存於 EFS 的使用。
>* SMB 加密與 BitLocker 磁碟機加密無關，且 SMB 加密不需要或不依存於 BitLocker 磁碟機加密的使用。

## <a name="secure-dialect-negotiation"></a>安全方言交涉

SMB 3.0 能夠偵測試圖降級 SMB 2.0 或 SMB 3.0 通訊協定，或是將用戶端和伺服器的交涉能力降級的攔截式攻擊。 當用戶端或伺服器偵測到這類攻擊時，連線即會中斷，且會將事件識別碼 1005 記錄到 Microsoft-Windows-SmbServer/Operational 事件記錄檔中。 安全方言交涉無法偵測或防止從 SMB 2.0 或 3.0 降級到 SMB 1.0 的行為。 因此，若要充分發揮 SMB 加密的功能，強烈建議您停用 SMB 1.0 伺服器。 如需詳細資訊，請參閱[停用 SMB 1.0](#disabling-smb-10)。

下一節所說明的安全方言交涉功能，可防止攔截式攻擊將 SMB 3 的連線降級為 SMB 2 (後者會使用未加密的存取)，但不會防止降級至 SMB 1.0 (也會導致未加密的存取)。 若要進一步了解 SMB 的舊版非 Windows 實作可能發生的問題，請參閱 [Microsoft 知識庫](https://support.microsoft.com/kb/2686098)。

## <a name="new-signing-algorithm"></a>新的簽署演算法

SMB 3.0 會使用較新的加密演算法進行簽署：以進階加密標準 (AES) 密碼為基礎的訊息驗證碼 (CMAC)。 SMB 2.0 則使用較舊的 HMAC-SHA256 加密演算法。 在具有 AES 指令支援的多數新型 CPU 上，AES-CMAC 和 AES-CCM 可大幅提高資料加密的速度。 如需詳細資訊，請參閱 [SMB 簽署的基本概念](/archive/blogs/josebda/the-basics-of-smb-signing-covering-both-smb1-and-smb2)。

## <a name="disabling-smb-10"></a>停用 SMB 1.0

SMB 1.0 中的舊版電腦瀏覽器服務與遠端管理通訊協定功能現在是獨立的，並且可以排除。 這些功能仍預設為啟用，但如果您沒有舊版的 SMB 用戶端 (例如，執行 Windows Server 2003 或 Windows XP 的電腦)，您可以移除 SMB 1.0 功能以增加安全性，甚至減少修正的必要性。

>[!NOTE]
>SMB 2.0 是在 Windows Server 2008 和 Windows Vista 中導入的。 較舊的用戶端 (例如，執行 Windows Server 2003 或 Windows XP 的電腦) 不支援 SMB 2.0；因此若停用 SMB 1.0 伺服器，這類用戶端將無法存取檔案共用或列印共用。 此外，某些非 Microsoft SMB 用戶端可能無法存取 SMB 2.0 檔案共用或列印共用 (例如，具有「掃描至共用」功能的印表機)。

開始停用 SMB 1.0 之前，您必須先了解 SMB 用戶端目前是否連線至執行 SMB 1.0 的伺服器。 若要這樣做，請在 Windows PowerShell 中輸入下列 Cmdlet：

```PowerShell
Get-SmbSession | Select Dialect,ClientComputerName,ClientUserName | ? Dialect -lt 2
```

>[!NOTE]
>您應在一週當中重複執行此指令碼 (每天多次)，以建置稽核線索。 您也可以用排程工作的形式加以執行。

若要停用 SMB 1.0，請在 Windows PowerShell 中輸入下列指令碼：

```PowerShell
Set-SmbServerConfiguration –EnableSMB1Protocol $false
```

>[!NOTE]
>如果因執行 SMB 1.0 的伺服器已停用，而使 SMB 用戶端連線遭到拒絕，則會將事件識別碼 1001 記錄到 Microsoft-Windows-SmbServer/Operational 事件記錄檔中。

## <a name="more-information"></a>其他資訊

以下是與 Windows Server 2012 中的 SMB 和相關技術有關的一些額外資源。

- [伺服器訊息區](file-server-smb-overview.md)
- [Windows Server 的儲存空間](../storage.yml)
- [用於應用程式資料的向外延展檔案伺服器](../../failover-clustering/sofs-overview.md)
