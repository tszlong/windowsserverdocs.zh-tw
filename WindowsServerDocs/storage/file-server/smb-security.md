---
title: SMB 安全性增強功能
description: Windows Server 2012 R2、 Windows Server 2012 與 Windows Server 2016 的 SMB 加密功能說明。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 831ca8266c3ec18ffb83227dcb2d39b3f953ad1a
ms.sourcegitcommit: 375e94dc70c76e7aa5679c32f0f4dbc26cf7dd83
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2018
ms.locfileid: "2233524"
---
# <a name="smb-security-enhancements"></a>SMB 安全性增強功能

>適用於： Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2016

本主題說明在 Windows Server 2012 R2、 Windows Server 2012 與 Windows Server 2016 SMB 安全性增強功能。

## <a name="smb-encryption"></a>SMB 加密

SMB 加密提供 SMB 資料的端對端加密並保護資料來自不受信任的網路上的竊聽發生次數。 您可以部署 SMB 加密以最，但它可能需要小型的其他成本的特定的硬體或軟體。 它具有網際網路通訊協定安全性 (IPsec) 或 WAN 加速器沒有需求。 SMB 加密可針對每個共用或整個檔案伺服器、 設定及啟用它的各種案例資料周遊不受信任的網路的位置。

>[!NOTE]
>SMB 加密並未涵蓋靜態，通常由 BitLocker 磁碟加密處理的安全性。

SMB 加密應考慮任何案例中的機密資料需要保護免受攔截攻擊。 可能的情況包括：

- 資訊工作者機密資料移動使用 SMB 通訊協定。 SMB 加密提供檔案伺服器與用戶端，不論周遊，例如廣域網路 (WAN) 連線的非 Microsoft 提供者所維護的網路之間端對端隱私權和完整性保證。
- SMB 3.0 可讓檔案伺服器的伺服器應用程式，例如 SQL Server 或 HYPER-V 提供持續可用儲存空間。 啟用 SMB 加密可讓您防止側錄攻擊的這項資訊。 SMB 加密會更容易使用比專屬的硬體解決方案所需的大部分儲存區域網路 (San)。

>[!IMPORTANT]
>您應該注意的作業系統與任何端對端加密保護相較於非加密的成本顯著效能。

## <a name="enable-smb-encryption"></a>啟用 SMB 加密

您可以啟用 SMB 加密整個檔案伺服器或是僅針對特定的檔案共用。 使用下列程序之一來啟用 SMB 加密：

### <a name="enable-smb-encryption-with-windows-powershell"></a>啟用 Windows powershell 的 SMB 加密

1. 若要啟用 SMB 加密的個別的檔案共用，請輸入下列指令碼的伺服器上：
    
    ```PowerShell
    Set-SmbShare –Name <sharename> -EncryptData $true
    ```
2. 若要啟用整個檔案伺服器 SMB 加密，輸入下列指令碼的伺服器上：
    
    ```PowerShell
    Set-SmbServerConfiguration –EncryptData $true
    ```
3. 若要建立已啟用的 SMB 加密新的 SMB 檔案共用，請輸入下列指令碼：
    
    ```PowerShell
    New-SmbShare –Name <sharename> -Path <pathname> –EncryptData $true
    ```

### <a name="enable-smb-encryption-with-server-manager"></a>啟用 SMB 加密與伺服器管理員

1. 在 [伺服器管理員] 中，開啟**檔案並儲存服務**。
2. 選取 [開啟共用管理] 頁面上的**共用**。
3. 以滑鼠右鍵按一下您要啟用 SMB 加密的共用，然後選取 [**屬性**。
4. 在共用的 [**設定**] 頁面上選取 [**加密資料存取權**。 加密遠端檔案存取此共用資料夾。

### <a name="considerations-for-deploying-smb-encryption"></a>部署 SMB 加密的考量

預設會啟用 SMB 加密的檔案共用或伺服器時只 SMB 3.0 允許的用戶端存取指定的檔案共用。 這會強制執行的所有用戶端存取共用的保護資料的系統管理員的用途。 不過，在某些情況下，系統管理員可能會想要允許的用戶端 （例如，在混合用戶端作業系統版本時正在使用的轉換期間） 不支援 SMB 3.0 未加密的存取。 若要允許未加密的存取的用戶端不支援 SMB 3.0，請輸入下列指令碼中使用 Windows PowerShell：

```PowerShell
Set-SmbServerConfiguration –RejectUnencryptedAccess $false
```

下一節所述的安全方言交涉功能可避免攔截攻擊降級 SMB 3.0 SMB 2.0 （這會使用加密的存取） 的連線。 不過，它無法防止降級到 SMB 1.0，也會導致未加密的存取。 為確保 SMB 3.0 用戶端一律存取加密的共用使用 SMB 加密，您必須停用的 SMB 1.0 伺服器。 （如指示，請參閱] 區段的 [[停用 SMB 1.0](#disabling-smb-1.0)）。如果 **– RejectUnencryptedAccess**設定會維持 **$true**成預設設定，只加密能夠 SMB 3.0 允許的用戶端存取 （SMB 1.0 用戶端也會被拒絕） 檔案共用。

>[!NOTE]
>* SMB 加密使用進階加密標準 (AES)-CCM 演算法加密和解密資料。 AES CCM 也會提供 （簽署） 加密的檔案共用，不論 SMB 簽章設定的資料完整性驗證。 如果您想要啟用 SMB 加密沒有簽章，您可以繼續執行這項作業。 如需詳細資訊，請參閱[基礎的 SMB 簽章](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/)。
>* 當您嘗試存取的檔案共用或 server 如果貴組織使用廣域網路 (wan) 加速設備時，您可能會發生問題。
>* 使用預設設定 （其中有無未加密的權限允許加密的檔案共用）、 如果用戶端不支援 SMB 3.0 嘗試存取加密的檔案共用、 事件識別碼 1003年記錄至 Microsoft Windows SmbServer/操作事件記錄檔且用戶端將會收到 **「 拒絕存取 」** 錯誤訊息。
>* SMB 加密及加密檔案系統 (EFS) NTFS 檔案系統中沒有關聯，且 SMB 加密不需要或使用 EFS 而定。
>* SMB 加密和 BitLocker 磁碟加密無關，且 SMB 加密不需要或取決於使用 BitLocker 磁碟加密。

## <a name="secure-dialect-negotiation"></a>安全的方言交涉

SMB 3.0 都能夠偵測嘗試降級 SMB 2.0 或 SMB 3.0 通訊協定或用戶端和伺服器交涉 」 功能的攔截攻擊。 當用戶端或伺服器所偵測到這類攻擊時，連線中斷與 Microsoft Windows SmbServer/操作事件記錄檔中記錄事件識別碼 1005年。 安全方言交涉無法偵測或防止從 SMB 2.0 或 3.0 downgrades 到 SMB 1.0。 因此，並利用 SMB 加密的完整功能，我們強烈建議您停用 SMB 1.0 伺服器。 如需詳細資訊，請參閱 ＜[停用 SMB 1.0](#disabling-smb-1.0)。

下一節所述的安全方言交涉功能可避免攔截攻擊降級 SMB 2 （這會使用加密的存取）; 從 SMB 3 的連線不過，它無法防止 downgrades 為 SMB 1，也會導致未加密的存取。 如需有關稍早的 SMB 非 Windows 實作的潛在問題的詳細資訊，請參閱[Microsoft 知識庫](http://support.microsoft.com/kb/2686098)。

## <a name="new-signing-algorithm"></a>新的簽署演算法

SMB 3.0 會使用較新的加密演算法的簽章： 進階加密標準 (AES)-編碼器-根據訊息驗證碼 (CMAC)。 SMB 2.0 使用較舊的 HMAC SHA256 加密演算法。 AES CMAC 和 AES CCM 大幅可加速資料加密上有 AES 指示支援的最新式 Cpu。 如需詳細資訊，請參閱[基礎的 SMB 簽章](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/)。

## <a name="disabling-smb-10"></a>停用 SMB 1.0

舊版的電腦瀏覽器服務和 SMB 1.0 版中的遠端管理通訊協定功能現在不同，而且可以排除。 根據預設，仍會啟用這些功能，但如果您不需要較舊的 SMB 用戶端，例如電腦執行 Windows Server 2003 或 Windows XP 中，您可以移除 SMB 1.0 来增加安全性和功能可能會降低修補。

>[!NOTE]
>Windows Server 2008 和 Windows Vista 中已採用 SMB 2.0。 較舊的用戶端，例如執行 Windows Server 2003 或 Windows XP 的電腦不支援 SMB 2.0;與因此，他們將無法存取檔案共用或列印共用如果 SMB 1.0 server 已停用。 此外，有些非 Microsoft SMB 用戶端可能無法存取 SMB 2.0 檔案共用或列印共用 （例如"掃描-共用 」 功能與印表機）。

在開始停用 SMB 1.0 之前，您需要了解 SMB 用戶端的目前連線到執行 SMB 1.0 版的伺服器。 若要這樣做，輸入下列 cmdlet 中使用 Windows PowerShell：

```PowerShell
Get-SmbSession | Select Dialect,ClientComputerName,ClientUserName | ? Dialect -lt 2
```

>[!NOTE]
>您應該透過建立稽核記錄的週 （多次每一天） 的課程重複執行此指令碼。 您也可以執行這個做為排程工作。

若要停用 SMB 1.0 版，請輸入下列指令碼中使用 Windows PowerShell：

```PowerShell
Set-SmbServerConfiguration –EnableSMB1Protocol $false
```

>[!NOTE]
>如果因為執行 SMB 1.0 版的伺服器已停用被拒的 SMB 用戶端連線、 事件識別碼 1001年會記錄在 Microsoft Windows SmbServer/操作事件記錄檔。

## <a name="more-information"></a>更多資訊

以下是一些有關 SMB 和 Windows Server 2012 的相關的技術的其他資源。

- [伺服器訊息區塊](file-server-smb-overview.md)
- [Windows Server 的儲存空間](../storage.md)
- [向外延展檔案伺服器應用程式資料](../../failover-clustering/sofs-overview.md)