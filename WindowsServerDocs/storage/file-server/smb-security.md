---
title: SMB 安全性增強功能
description: Windows Server 2012 R2、 Windows Server 2012 和 Windows Server 2016 中的 SMB 加密功能的說明。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 831ca8266c3ec18ffb83227dcb2d39b3f953ad1a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838049"
---
# <a name="smb-security-enhancements"></a>SMB 安全性增強功能

>適用於：Windows Server 2012 R2 中，Windows Server 2012 中，Windows Server 2016

本主題說明 Windows Server 2012 R2、 Windows Server 2012 和 Windows Server 2016 中的 SMB 安全性增強功能。

## <a name="smb-encryption"></a>SMB 加密

SMB 加密提供的 SMB 資料的端對端加密，並可以保護資料免於不受信任網路上遭到竊聽。 您可以使用最少的工作，來部署 SMB 加密，但可能需要小型的額外成本的特殊化的硬體或軟體。 它有沒有網際網路通訊協定安全性 (IPsec) 或 WAN 加速器的需求。 可以設定 SMB 加密，以每個共用為基礎，或整個檔案伺服器，並可針對各種資料周遊不受信任的網路的案例。

>[!NOTE]
>SMB 加密並未涵蓋在其餘部分，通常由 BitLocker 磁碟機加密的安全性。

SMB 加密應該視為機密資料要受到保護，以免攔截攻擊的任何案例。 可能的案例包括：

- 資訊工作者的敏感性資料的移動是利用 SMB 通訊協定。 SMB 加密 可提供端對端的隱私權和完整性保證之間的檔案伺服器和用戶端，不論周遊，例如廣域網路 (WAN) 連線，由非 Microsoft 提供者所維護的網路。
- SMB 3.0 可讓伺服器應用程式，例如 SQL Server 或 HYPER-V 提供持續可用的儲存體的檔案伺服器。 啟用 SMB 加密提供保護免於受到窺探攻擊的機會。 SMB 加密是使用比所需的大部分的存放區域網路 (San) 的專用的硬體解決方案的工作變得更容易。

>[!IMPORTANT]
>請注意，是值得注意的效能，運作成本，相較於非加密任何端對端加密保護。

## <a name="enable-smb-encryption"></a>啟用 SMB 加密

您可以啟用 SMB 加密，整個檔案伺服器，還是只能用於特定的檔案共用。 若要啟用 SMB 加密，使用下列程序的其中一個：

### <a name="enable-smb-encryption-with-windows-powershell"></a>啟用 SMB 加密，使用 Windows PowerShell

1. 若要啟用個別的檔案共用的 SMB 加密，請在伺服器上輸入下列指令碼：
    
    ```PowerShell
    Set-SmbShare –Name <sharename> -EncryptData $true
    ```
2. 若要啟用 SMB 加密整個檔案伺服器，請在伺服器上輸入下列指令碼：
    
    ```PowerShell
    Set-SmbServerConfiguration –EncryptData $true
    ```
3. 若要建立新的 SMB 檔案共用，並啟用 SMB 加密，請輸入下列指令碼：
    
    ```PowerShell
    New-SmbShare –Name <sharename> -Path <pathname> –EncryptData $true
    ```

### <a name="enable-smb-encryption-with-server-manager"></a>啟用 SMB 加密使用伺服器管理員

1. 在 [伺服器管理員] 中，開啟**檔案和存放服務**。
2. 選取 **共用**以開啟 共用 管理頁面。
3. 以滑鼠右鍵按一下您想要啟用 SMB 加密，然後選取的共用**屬性**。
4. 在 **設定**頁面上的共用上，選取**加密資料存取**。 遠端檔案存取此共用已加密。

### <a name="considerations-for-deploying-smb-encryption"></a>部署 SMB 加密的考量

根據預設，當啟用 SMB 加密的檔案共用或伺服器上，只有 SMB 3.0 用戶端可以存取指定的檔案共用。 這會強制執行的保護資料存取共用的所有用戶端的系統管理員的意圖。 不過，在某些情況下，系統管理員可能會想要允許未加密的存取 （比方說，在轉換期間，當所使用混合的用戶端作業系統版本），不支援 SMB 3.0 的用戶端。 若要允許未加密的存取不支援 SMB 3.0 的用戶端，請在 Windows PowerShell 中輸入下列指令碼：

```PowerShell
Set-SmbServerConfiguration –RejectUnencryptedAccess $false
```

下一節中所述的安全方言交涉功能會防止攔截攻擊降級 （這會將未加密的存取） 的 SMB 2.0 從 SMB 3.0 的連線。 不過，它無法防止降級為 SMB 1.0 時，也可能會導致未加密的存取。 若要保證 SMB 3.0 用戶端永遠以存取加密的共用使用 SMB 加密，您必須停用 SMB 1.0 伺服器。 (如需指示，請參閱下節[停用 SMB 1.0](#disabling-smb-1.0)。)如果 **– RejectUnencryptedAccess**設定會保留其預設值為 **$true**，僅支援加密的 SMB 3.0 用戶端可以存取檔案共用 （SMB 1.0 用戶端也會遭到拒絕）。

>[!NOTE]
>* SMB 加密使用進階加密標準 (AES)-CCM 演算法來加密和解密資料。 AES CCM 也會提供 （簽章） 加密的檔案共用，不論 SMB 簽章設定的驗證資料完整性。 如果您想要啟用 SMB 簽署未加密，您可以繼續執行這項操作。 如需詳細資訊，請參閱 <<c0> [ 基本概念的 SMB 簽章](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/)。
>* 當您嘗試存取檔案共用或伺服器，如果您的組織使用廣域網路 (wan) 加速設備時，您可能會遇到問題。
>* 使用預設組態 （如果沒有加密的檔案共用允許未加密的存取），則不支援 SMB 3.0 嘗試存取加密的檔案共用，事件識別碼 1003年的用戶端會記錄在 Microsoft Windows SmbServer/Operational 事件記錄檔用戶端會收到**拒絕存取**錯誤訊息。
>* SMB 加密和加密檔案系統 (EFS) 在 NTFS 檔案系統中不相關，和 SMB 加密不需要或依賴使用 EFS。
>* SMB 加密和 BitLocker 磁碟機加密無關，和 SMB 加密不需要或依賴使用 BitLocker Drive Encryption。

## <a name="secure-dialect-negotiation"></a>安全的方言交涉

SMB 3.0 可以偵測嘗試要降級的 SMB 2.0 或 SMB 3.0 通訊協定或用戶端與伺服器交涉的功能的攔截層攻擊。 用戶端或伺服器所偵測到這類的攻擊時，中斷連線，並會記錄事件識別碼為 1005年，Microsoft Windows SmbServer/Operational 事件記錄檔中。 安全方言交涉無法偵測或防止從 SMB 2.0 或 3.0 為 SMB 1.0 的降級。 基於這個原因，並利用 SMB 加密的完整功能，我們強烈建議您停用 SMB 1.0 伺服器。 如需詳細資訊，請參閱 <<c0> [ 停用 SMB 1.0](#disabling-smb-1.0)。

下一節中所述的安全方言交涉功能可防止攔截攻擊降級 SMB 3 （這會將未加密的存取） 的 SMB 2; 的連線不過，它無法防止降級為 SMB 1，也可能會導致未加密的存取。 如需有關使用稍早的 SMB 的非 Windows 實作的潛在問題的詳細資訊，請參閱[Microsoft Knowledge Base](http://support.microsoft.com/kb/2686098)。

## <a name="new-signing-algorithm"></a>新的簽署演算法

SMB 3.0 會使用較新的加密演算法進行簽署：進階的加密標準 (AES)-加密-基礎訊息驗證碼 (CMAC)。 SMB 2.0 使用較舊的 HMAC-SHA256 加密演算法。 AES CMAC 和 AES CCM 大幅可以加速資料加密有 AES 指示支援的最新型 Cpu 上。 如需詳細資訊，請參閱 <<c0> [ 基本概念的 SMB 簽章](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/)。

## <a name="disabling-smb-10"></a>停用 SMB 1.0

SMB 1.0 中的遠端管理通訊協定功能與舊版電腦瀏覽器服務現在是獨立的並可以排除。 根據預設，仍然會啟用這些功能，但如果您沒有舊版的 SMB 用戶端，例如執行 Windows Server 2003 或 Windows XP 的電腦，您就可以移除 SMB 1.0 功能來提高安全性並降低修補。

>[!NOTE]
>SMB 2.0 是 Windows Server 2008 和 Windows Vista 中引進。 舊版的用戶端，例如執行 Windows Server 2003 或 Windows XP 的電腦不支援 SMB 2.0;因此，它們將無法存取檔案共用或列印共用，如果已停用 SMB 1.0 伺服器。 此外，有些非 Microsoft SMB 用戶端可能無法存取 SMB 2.0 檔案共用或列印共用 （例如，「 共用來掃描 」 功能的印表機）。

開始停用 SMB 1.0 之前，您必須了解是否 SMB 用戶端目前正連接到執行 SMB 1.0 的伺服器。 若要這樣做，請在 Windows PowerShell 中輸入下列 cmdlet:

```PowerShell
Get-SmbSession | Select Dialect,ClientComputerName,ClientUserName | ? Dialect -lt 2
```

>[!NOTE]
>過去一週 （多次每一天） 建置的稽核記錄，請重複執行此指令碼。 您也可以執行此排程的工作。

若要停用 SMB 1.0，請在 Windows PowerShell 中輸入下列指令碼：

```PowerShell
Set-SmbServerConfiguration –EnableSMB1Protocol $false
```

>[!NOTE]
>如果 SMB 用戶端連線被拒，因為已停用執行 SMB 1.0 的伺服器，將會記錄事件識別碼為 1001年，Microsoft Windows SmbServer/Operational 事件記錄檔中。

## <a name="more-information"></a>詳細資訊

以下是一些有關 SMB 及相關的技術的 Windows Server 2012 的其他資源。

- [伺服器訊息區](file-server-smb-overview.md)
- [Windows Server 中的儲存體](../storage.md)
- [用於應用程式資料的向外延展檔案伺服器](../../failover-clustering/sofs-overview.md)