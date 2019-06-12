---
title: 升級至 SQL server 的 Windows Server 2016 中的 AD FS
description: ''
author: billmath
manager: mtillman
ms.date: 04/11/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 70f279bf-aea1-4f4f-9ab3-e9157233e267
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 0a3db2a095d1a31f55bd1c8bfc5bf3c9f6bb65b8
ms.sourcegitcommit: ccc802338b163abdad2e53b55f39addcfea04603
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/05/2019
ms.locfileid: "66687403"
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-with-sql-server"></a>升級至 SQL server 的 Windows Server 2016 中的 AD FS


> [!NOTE]  
> 只有在明確的時間範圍內完成計劃與開始升級。 不建議將 AD FS 在混合的模式的狀態，長的時間，因為在混合的模式狀態中離開的 AD FS 可能會造成問題，與伺服器陣列。


## <a name="moving-from-a-windows-server-2012-r2-ad-fs-farm-to-a-windows-server-2016-ad-fs-farm"></a>從 Windows Server 2012 R2 AD FS 伺服器陣列移至 Windows Server 2016 AD FS 伺服器陣列  
下列文件將說明如何使用 SQL Server 的 AD FS 資料庫時，您的 AD FS Windows Server 2012 R2 伺服器陣列升級至 Windows Server 2016 中的 AD FS。  

### <a name="upgrading-ad-fs-to-windows-server-2016-fbl"></a>將 AD FS 升級到 Windows Server 2016 FBL  
新的 Windows Server 2016 的 AD FS 中是伺服陣列行為層級功能 (FBL)。   這項功能是適用全伺服器陣列，並決定可以使用 AD FS 伺服器陣列的功能。   根據預設，Windows Server 2012 R2 AD FS 伺服器陣列中的 FBL 位於 Windows Server 2012 R2 FBL。  

Windows Server 2016 AD FS 伺服器新增到 Windows Server 2012 R2 的伺服器陣列，它會以如同 Windows Server 2012 R2 相同 FBL 運作。  當您在這種方式操作 Windows Server 2016 AD FS 伺服器，您的伺服器陣列就"mixed"。  不過，您將無法充分利用新的 Windows Server 2016 功能，直到 FBL 提升至 Windows Server 2016。  使用混合的伺服陣列中：  

-   系統管理員可以新增新的 Windows Server 2016 的同盟伺服器至現有的 Windows Server 2012 R2 伺服器陣列。  如此一來，伺服器陣列處於 「 混合模式 」，及運作的 Windows Server 2012 R2 伺服器陣列行為層級。  若要確保一致的行為，整個伺服器陣列，無法設定或在此模式中使用 Windows Server 2016 的新功能。  

-   一旦從混合的模式的伺服器陣列中，已移除所有的 Windows Server 2012 R2 同盟伺服器，並在 WID 伺服器陣列的情況下的其中一個新的 Windows Server 2016 同盟伺服器已升階至主要節點的角色，系統管理員可以請提出從 Win FBLdows Server 2012 R2 到 Windows Server 2016。  如此一來，任何新的 AD FS 的 Windows Server 2016 功能可以再進行設定和使用。  

-   如此一來混合伺服陣列功能，AD FS Windows Server 2012 R2 組織想要升級到 Windows Server 2016 不會部署全新的伺服器陣列，匯出和匯入組態資料。  而是，可以將 Windows Server 2016 節點新增至現有的伺服陣列中，而它已上線，並只會產生參與 FBL raise 的相當短暫的停機時間。  

請注意，在混合式伺服器陣列模式時，AD FS 伺服器陣列不支援任何新功能或 Windows Server 2016 中的 AD FS 中引進的功能。  這表示想要試用新功能的組織無法這麼做之前，就會引發 FBL。  因此如果您的組織會想要測試之前 rasing FBL 的新功能，您必須部署個別的伺服器陣列，若要這樣做。  

其餘部分是文件提供 Windows Server 2016 的同盟伺服器新增到 Windows Server 2012 R2 環境，則提高至 Windows Server 2016 FBL 的步驟。  架構圖所述的測試環境中執行這些步驟。  

> [!NOTE]  
> 您可以移至 Windows Server 2016 FBL 中的 AD FS 之前，您必須移除所有的 Windows 2012 R2 節點。  您就無法升級至 Windows Server 2016 的 Windows Server 2012 R2 作業系統，並將它變成 2016年節點。  您必須將它移除，並以新的 2016年節點取代。  

以下架構圖說明用來驗證和記錄步驟，安裝程式。

![Architecture](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/arch.png)


#### <a name="join-the-windows-2016-ad-fs-server-to-the-ad-fs-farm"></a>AD FS 伺服器陣列中加入 Windows 2016 AD FS 伺服器

1.  使用 Windows Server 2016 上的 伺服器管理員安裝 Active Directory Federation Services 角色  

2.  使用 AD FS 設定精靈，將新的 Windows Server 2016 伺服器加入現有的 AD FS 伺服器陣列。  在 [**歡迎**畫面上，按一下**下一步]** 。
 ![加入伺服器陣列](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure1.png)  
3.  上**連接到 Active Directory 網域服務**螢幕、 s**指定系統管理員帳戶**具有執行同盟服務設定，按 的權限**下一步**.
4.  在 [**指定的伺服器陣列**畫面中，輸入 SQL server 和執行個體的名稱，然後按**下一步]** 。
![加入伺服器陣列](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure3.png)
5.  在 **指定 SSL 憑證**畫面上，指定的憑證並按一下 **下一步**。
![加入伺服器陣列](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure4.png)
6.  在 **指定服務帳戶**畫面上，指定服務帳戶並按一下 **下一步**。
7.  在 **檢閱選項**畫面上，檢閱選項並按一下 **下一步**。
8.  在 **必要條件檢查**畫面上，確認所有先決條件檢查通過，然後按一下**設定**。
9.  在 **結果**畫面中，確定該伺服器已成功設定，然後按一下**關閉**。


#### <a name="remove-the-windows-server-2012-r2-ad-fs-server"></a>移除 Windows Server 2012 R2 AD FS 伺服器

>[!NOTE]
>您不需要設定主要 AD FS 伺服器使用組 AdfsSyncProperties-角色時使用 SQL 資料庫。  這是因為所有節點被視為主要在此設定。

1.  在 伺服器管理員使用 Windows Server 2012 R2 AD FS 伺服器上**移除角色及功能**下方**管理**。
![移除伺服器](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/remove1.png)
2.  在 [在您開始前]  畫面上，按 [下一步]  。
3.  在 [**伺服器選取項目**畫面上，按一下**下一步]** 。
4.  在 **伺服器角色**畫面上，取消核取旁**Active Directory Federation Services** ，按一下 **下一步**。
![移除伺服器](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/remove2.png)
5.  在 [**功能**畫面上，按一下**下一步]** 。
6.  在 **確認**畫面上，按一下**移除**。
7.  完成之後，重新啟動伺服器。

#### <a name="raise-the-farm-behavior-level-fbl"></a>引發伺服器陣列行為層級 (FBL)
在此步驟之前，您需要確保 forestprep 及 domainprep，具有 Active Directory 環境上執行，且 Active Directory 有 Windows Server 2016 結構描述。  這份文件開始使用 Windows 2016 的網域控制站，並不需要執行這些，因為 AD 安裝時執行它們。

>[!NOTE]
>開始之前下列程序，請確定 Windows Server 2016 為最新的設定從執行 Windows Update。  繼續此程序，直到不需要進一步更新。

1. 現在在 Windows Server 2016 伺服器上開啟 PowerShell，並執行下列命令： **$cred = Get-credential**然後按 enter 鍵。
2. 輸入 SQL Server 具有管理員權限的認證。
3. 現在在 PowerShell 中，輸入下列內容：**Invoke-AdfsFarmBehaviorLevelRaise -Credential $cred**
2. 出現提示時，輸入**Y**。這會開始引發層級。  一旦完成後您成功地提出 FBL。  
![完成更新](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish1.png)
3. 現在，如果您移至 AD FS 管理，您會看到已新增適用於 Windows Server 2016 中的 AD FS 的新節點  
4. 同樣地，您可以使用 PowerShell cmdlet:若要顯示您目前的 FBL get AdfsFarmInformation。  
![完成更新](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish2.png)

#### <a name="upgrade-the-configuration-version-of-existing-wap-servers"></a>升級現有的 WAP 伺服器的設定版本
1. 每個 Web 應用程式 proxy，請重新設定 WAP 藉由在提升權限的視窗執行下列 PowerShell 命令：  
    ```powershell
    $trustcred = Get-Credential -Message "Enter Domain Administrator credentials"
    Install-WebApplicationProxy -CertificateThumbprint {SSLCert} -fsname fsname -FederationServiceTrustCredential $trustcred  
    ```
2. 從叢集移除舊的伺服器，並保留只的 WAP 伺服器執行最新的伺服器版本中，已藉由執行下列 Powershell 指令程式來重新設定上面。
    ```powershell
    Set-WebApplicationProxyConfiguration -ConnectedServersName WAPServerName1, WAPServerName2
    ```
3. 藉由執行 Get WebApplicationProxyConfiguration commmandlet 檢查 WAP 組態。 ConnectedServersName 會反映來自上一個命令執行的伺服器。
    ```powershell
    Get-WebApplicationProxyConfiguration
    ```
4. 若要升級的 WAP 伺服器 ConfigurationVersion，執行下列 Powershell 命令。
    ```powershell
    Set-WebApplicationProxyConfiguration -UpgradeConfigurationVersion
    ```
5. 請確認 ConfigurationVersion 已升級使用 Get WebApplicationProxyConfiguration Powershell 命令。
