---
title: 使用 SQL Server 升級至 Windows Server 2016 中的 AD FS
author: billmath
manager: mtillman
ms.date: 04/11/2018
ms.topic: article
ms.assetid: 70f279bf-aea1-4f4f-9ab3-e9157233e267
ms.author: billmath
ms.openlocfilehash: 434ee97a352ad30caef83e495a387583da1f955b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87940556"
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-with-sql-server"></a>使用 SQL Server 升級至 Windows Server 2016 中的 AD FS


> [!NOTE]
> 只會開始升級，並已規劃完成的最終時間範圍。 不建議讓 AD FS 處於混合模式的狀態一段很長的時間，因為離開 AD FS 處於混合模式的狀態可能會導致伺服器陣列發生問題。


## <a name="moving-from-a-windows-server-2012-r2-ad-fs-farm-to-a-windows-server-2016-ad-fs-farm"></a>從 Windows Server 2012 R2 AD FS 伺服器陣列移至 Windows Server 2016 AD FS 伺服器陣列
下列檔將說明當您使用 AD FS 資料庫的 SQL Server 時，如何將 AD FS Windows Server 2012 R2 伺服器陣列升級至 Windows Server 2016 中的 AD FS。

### <a name="upgrading-ad-fs-to-windows-server-2016-fbl"></a>將 AD FS 升級至 Windows Server 2016 FBL
Windows Server 2016 AD FS 的新功能是伺服器陣列行為層級功能 (FBL) 。   此功能是整個伺服器陣列，並決定 AD FS 伺服器陣列可以使用的功能。   根據預設，Windows Server 2012 R2 AD FS 伺服器陣列中的 FBL 是在 Windows Server 2012 R2 FBL。

Windows Server 2016 AD FS 伺服器可以新增至 Windows Server 2012 R2 陣列，而且它會與 Windows Server 2012 R2 在相同的 FBL 上運作。  當您有以這種方式運作的 Windows Server 2016 AD FS 伺服器時，您的伺服器陣列稱為「混合」。  不過，在 FBL 提升至 Windows Server 2016 之前，您將無法利用新的 Windows Server 2016 功能。  使用混合伺服器陣列：

-   系統管理員可以將新的 Windows Server 2016 同盟伺服器新增至現有的 Windows Server 2012 R2 伺服器陣列。  因此，伺服器陣列處於「混合模式」，並可操作 Windows Server 2012 R2 伺服器陣列行為層級。  為了確保整個伺服器陣列的行為一致，新的 Windows Server 2016 功能無法在此模式中設定或使用。

-   當所有 Windows Server 2012 R2 同盟伺服器都已從混合模式伺服器陣列中移除，而且在 WID 伺服器陣列的情況下，其中一個新的 Windows 服務2016同盟伺服器已升級為主要節點的角色，然後系統管理員可以將 FBL 從 Windows Server 2012 R2 提升到 Windows Server 2016。  如此一來，就可以設定和使用任何新的 AD FS Windows Server 2016 功能。

-   由於「混合伺服器陣列」功能的結果，想要升級至 Windows Server 2016 的 Windows Server 2012 R2 組織 AD FS 不需要部署全新的伺服器陣列、匯出和匯入設定資料。  相反地，他們可以將 Windows Server 2016 節點加入至現有的伺服器陣列（當其上線時），而且只會產生與 FBL 引發相關的短暫停機時間。

請注意，在混合伺服器陣列模式中，AD FS 伺服器陣列無法提供 Windows Server 2016 AD FS 中引進的任何新功能或功能。  這表示想要嘗試新功能的組織在引發 FBL 之前無法執行此動作。  因此，如果您的組織想要在 rasing FBL 之前測試新功能，您必須部署個別的伺服器陣列來執行此動作。

其餘部分是檔，提供將 Windows Server 2016 同盟伺服器新增至 Windows Server 2012 R2 環境，然後將 FBL 提升至 Windows Server 2016 的步驟。  這些步驟是在測試環境中執行的，如下圖所述。

> [!NOTE]
> 您必須先移除所有 Windows 2012 R2 節點，才能移至 Windows Server 2016 FBL 中的 AD FS。  您不能只是將 Windows Server 2012 R2 作業系統升級到 Windows Server 2016，讓它成為2016節點。  您必須將它移除，並將它取代為新的2016節點。

以下架構圖顯示用來驗證及記錄下列步驟的設定。

![架構](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/arch.png)


#### <a name="join-the-windows-2016-ad-fs-server-to-the-ad-fs-farm"></a>將 Windows 2016 AD FS 伺服器加入 AD FS 伺服器陣列

1.  使用伺服器管理員在 Windows Server 2016 上安裝 Active Directory 同盟服務角色

2.  使用 AD FS 設定] wizard，將新的 Windows Server 2016 伺服器加入現有的 AD FS 伺服器陣列。  在 [**歡迎使用**] 畫面上按 **[下一步]**。
 ![加入伺服器陣列](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure1.png)
3.  在 [**連接到 Active Directory Domain Services]** 畫面上，指定 p) 具有執行同盟服務設定許可權的**系統管理員帳戶**，然後按 **[下一步]**。
4.  在 [**指定伺服器**陣列] 畫面上，輸入 SQL server 和實例的名稱，然後按 **[下一步]**。
![加入伺服器陣列](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure3.png)
5.  在 [**指定 SSL 憑證**] 畫面上，指定憑證，然後按 **[下一步]**。
![加入伺服器陣列](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure4.png)
6.  在 [**指定服務帳戶**] 畫面上，指定服務帳戶，然後按 **[下一步]**。
7.  在 [**審核選項**] 畫面上，檢查選項，然後按 **[下一步]**。
8.  在 [必要條件**檢查**] 畫面上，確定已通過所有先決條件檢查，然後按一下 [**設定**]。
9.  在 [**結果**] 畫面上，確認伺服器已成功設定，然後按一下 [**關閉**]。


#### <a name="remove-the-windows-server-2012-r2-ad-fs-server"></a>移除 Windows Server 2012 R2 AD FS 伺服器

>[!NOTE]
>使用 SQL 做為資料庫時，您不需要使用 AdfsSyncProperties-Role 來設定主要 AD FS 伺服器。  這是因為在此設定中，所有節點都會被視為主要。

1.  在 Windows Server 2012 R2 的 AD FS 伺服器上伺服器管理員使用 [**管理**] 底下的 [**移除角色及功能**]。
![移除伺服器](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/remove1.png)
2.  在 [在您開始前]**** 畫面上，按 [下一步]****。
3.  在 [**伺服器選取**] 畫面上，按 **[下一步]**。
4.  在 [**伺服器角色**] 畫面上，移除**Active Directory 同盟服務**旁的核取記號，然後按 **[下一步]**。
![移除伺服器](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/remove2.png)
5.  在 [**功能**] 畫面上，按 **[下一步]**。
6.  在 [**確認**] 畫面上，按一下 [**移除**]。
7.  完成後，請重新開機伺服器。

#### <a name="raise-the-farm-behavior-level-fbl"></a> (FBL 中提高伺服器陣列行為層級) 
在此步驟之前，您必須確定已在 Active Directory 環境上執行 forestprep 和「工作」，而且 Active Directory 具有 Windows Server 2016 架構。  本檔是從 Windows 2016 網域控制站開始，而且不需要執行這些作業，因為它們是在安裝 AD 時執行。

>[!NOTE]
>開始下列程式之前，請從 [設定] 執行 Windows Update，確定 Windows Server 2016 是最新的。  繼續此程序，直到不需要進一步更新。 此外，請確定 ADFS 服務帳戶帳戶具有 SQL server 的系統管理許可權，以及 ADFS 伺服器陣列中的每部伺服器。

1. 現在，在 Windows Server 2016 伺服器上開啟 PowerShell，並執行下列動作： **$cred = Get-Credential** ，然後按 enter 鍵。
2. 輸入在 SQL Server 上具有系統管理員許可權的認證。
3. 現在在 PowerShell 中，輸入下列內容： **AdfsFarmBehaviorLevelRaise-Credential $cred**
2. 出現提示時，輸入**Y**。 這會開始提高層級。  完成後，您就已成功引發 FBL。
![完成更新](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish1.png)
3. 現在，如果您移至 [AD FS 管理]，您會看到已針對 Windows Server 2016 中的 AD FS 新增的新節點
4. 同樣地，您可以使用 PowerShell cmdlt： Get-AdfsFarmInformation 來顯示目前的 FBL。
![完成更新](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish2.png)

#### <a name="upgrade-the-configuration-version-of-existing-wap-servers"></a>升級現有 WAP 伺服器的設定版本
1. 在每個 Web 應用程式 Proxy 上，在提升許可權的視窗中執行下列 PowerShell 命令，以重新設定 WAP：
    ```powershell
    $trustcred = Get-Credential -Message "Enter Domain Administrator credentials"
    Install-WebApplicationProxy -CertificateThumbprint {SSLCert} -fsname fsname -FederationServiceTrustCredential $trustcred
    ```
2. 藉由執行下列 Powershell commandlet，從叢集中移除舊的伺服器，並只保留執行最新伺服器版本的 WAP 伺服器。
    ```powershell
    Set-WebApplicationProxyConfiguration -ConnectedServersName WAPServerName1, WAPServerName2
    ```
3. 執行 Set-webapplicationproxyconfiguration commmandlet 來檢查 WAP 設定。 ConnectedServersName 會反映伺服器從先前的命令執行。
    ```powershell
    Get-WebApplicationProxyConfiguration
    ```
4. 若要升級 WAP 伺服器的 ConfigurationVersion，請執行下列 Powershell 命令。
    ```powershell
    Set-WebApplicationProxyConfiguration -UpgradeConfigurationVersion
    ```
5. 確認 ConfigurationVersion 已使用 Set-webapplicationproxyconfiguration Powershell 命令進行升級。
