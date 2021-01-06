---
description: 深入瞭解：使用 SQL Server 升級至 Windows Server 2016 中的 AD FS
title: 使用 SQL Server 升級至 Windows Server 2016 中的 AD FS
author: billmath
manager: mtillman
ms.date: 04/11/2018
ms.topic: article
ms.assetid: 70f279bf-aea1-4f4f-9ab3-e9157233e267
ms.author: billmath
ms.openlocfilehash: 5fcc31c0b0f0482a545d17135603efaa97e174d7
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97948524"
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-with-sql-server"></a>使用 SQL Server 升級至 Windows Server 2016 中的 AD FS


> [!NOTE]
> 只會在已規劃完成的明確時間範圍內開始升級。 不建議將 AD FS 保持在混合模式的狀態一段長時間，因為在混合模式狀態中保持 AD FS 可能會導致伺服器陣列發生問題。


## <a name="moving-from-a-windows-server-2012-r2-ad-fs-farm-to-a-windows-server-2016-ad-fs-farm"></a>從 Windows Server 2012 R2 AD FS 伺服器陣列移至 Windows Server 2016 AD FS 伺服器陣列
下列檔將說明當您使用 AD FS 資料庫的 SQL Server 時，如何將 AD FS Windows Server 2012 R2 伺服器陣列升級為 Windows Server 2016 中的 AD FS。

### <a name="upgrading-ad-fs-to-windows-server-2016-fbl"></a>將 AD FS 升級為 Windows Server 2016 FBL\
Windows Server 2016 AD FS 的新功能是伺服器陣列行為等級功能 (FBL\) 。   這項功能是伺服器陣列範圍，可決定 AD FS 伺服器陣列可以使用的功能。   根據預設，Windows Server 2012 R2 AD FS 伺服器陣列中的 FBL\ 是在 Windows Server 2012 R2 FBL\。

Windows server 2016 AD FS Server 可以新增至 Windows Server 2012 R2 伺服器陣列，而且會在與 Windows Server 2012 R2 相同的 FBL\ 上運作。  當您以這種方式操作 Windows Server 2016 AD FS Server 時，您的伺服器陣列稱為「混合」。  不過，在 FBL\ 提升至 Windows Server 2016 之前，您將無法再利用新的 Windows Server 2016 功能。  使用混合伺服器陣列：

-   系統管理員可以將新的 Windows Server 2016 同盟伺服器新增至現有的 Windows Server 2012 R2 伺服器陣列。  如此一來，伺服器陣列就會處於「混合模式」，並操作 Windows Server 2012 R2 伺服器陣列行為等級。  為了確保整個伺服器陣列一致的行為，無法在此模式中設定或使用新的 Windows Server 2016 功能。

-   從混合模式伺服器陣列移除所有 Windows Server 2012 R2 同盟伺服器之後，如果是 WID 伺服器陣列，其中一個新的 Windows Server 2016 同盟伺服器已升級為主要節點的角色，系統管理員就可以將 FBL\ 從 Windows Server 2012 R2 提升為 Windows Server 2016。  如此一來，就可以設定和使用任何新的 AD FS Windows Server 2016 功能。

-   由於混合伺服器陣列功能的結果，AD FS 想要升級到 Windows Server 2016 的 Windows Server 2012 R2 組織，不需要部署全新的伺服器陣列、匯出和匯入設定資料。  相反地，他們可以將 Windows Server 2016 節點新增至現有的伺服器陣列（當它在線上時），而且只會產生與 FBL\ 引發相關的短暫停機時間。

請注意，在混合伺服器陣列模式下，AD FS 伺服器陣列無法在 Windows Server 2016 中 AD FS 引進的任何新功能或功能。  這表示，想要嘗試新功能的組織在 FBL\ 引發之前無法執行這項作業。  因此，如果您的組織想要在 FBL\ 之前測試新功能，您將需要部署個別的伺服器陣列來進行這項操作。

檔的其餘部分提供將 Windows Server 2016 同盟伺服器新增至 Windows Server 2012 R2 環境，然後將 FBL\ 提升至 Windows Server 2016 的步驟。  這些步驟是在下列架構圖表所概述的測試環境中執行。

> [!NOTE]
> 在您可以移至 Windows Server 2016 FBL\ 中的 AD FS 之前，您必須先移除所有的 Windows 2012 R2 節點。  您無法只將 Windows Server 2012 R2 OS 升級至 Windows Server 2016，並讓它成為2016節點。  您必須將它移除，並取代為新的2016節點。

以下架構圖顯示用來驗證和記錄下列步驟的設定。

![架構](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/arch.png)


#### <a name="join-the-windows-2016-ad-fs-server-to-the-ad-fs-farm"></a>將 Windows 2016 AD FS 伺服器加入 AD FS 伺服器陣列

1.  使用伺服器管理員在 Windows Server 2016 上安裝 Active Directory 同盟服務角色

2.  使用 AD FS 設定向導，將新的 Windows Server 2016 伺服器加入現有的 AD FS 伺服器陣列中。  在 [ **歡迎使用** ] 畫面上按 **[下一步]**。
![螢幕擷取畫面，顯示 AD FS Configuration wizard 中的歡迎畫面。](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure1.png)
3.  在 [ **連接到 Active Directory Domain Services]** 畫面上，s 指定 p) 具有執行同盟服務設定許可權的 **系統管理員帳戶** ，然後按 **[下一步]**。
4.  在 [ **指定伺服器** 陣列] 畫面上，輸入 SQL server 和實例的名稱，然後按 **[下一步]**。
![顯示 AD FS 設定向導中 [指定伺服器陣列] 畫面的螢幕擷取畫面。](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure3.png)
5.  在 [ **指定 SSL 憑證** ] 畫面上指定憑證，然後按 **[下一步]**。
![加入伺服器陣列](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure4.png)
6.  在 [ **指定服務帳戶** ] 畫面上，指定服務帳戶，然後按 **[下一步]**。
7.  在 [ **審核選項** ] 畫面上，查看選項並按 **[下一步]**。
8.  在 [必要條件 **檢查** ] 畫面上，確定已通過所有先決條件檢查，然後按一下 [ **設定**]。
9.  在 [ **結果** ] 畫面上，確定已成功設定伺服器，然後按一下 [ **關閉**]。


#### <a name="remove-the-windows-server-2012-r2-ad-fs-server"></a>移除 Windows Server 2012 R2 AD FS Server

>[!NOTE]
>使用 SQL 做為資料庫時，您不需要使用 Set-AdfsSyncProperties 角色設定主要 AD FS 伺服器。  這是因為此設定中的所有節點都會被視為主要節點。

1.  在 Windows Server 2012 R2 AD FS Server 的伺服器管理員使用 [**管理**] 下的 [**移除角色及功能**]。
![醒目顯示 [移除角色及功能] 功能表選項的螢幕擷取畫面。](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/remove1.png)
2.  在 [在您開始前] 畫面上，按 [下一步]。
3.  在 [ **伺服器選取** ] 畫面上，按 **[下一步]**。
4.  在 [ **伺服器角色** ] 畫面上，移除 **Active Directory 同盟服務** 旁的核取記號，然後按一下 **[下一步]**。
![移除伺服器](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/remove2.png)
5.  在 [ **功能** ] 畫面上，按 **[下一步]**。
6.  在 **確認** 畫面上，按一下 [ **移除**]。
7.  完成之後，請重新開機伺服器。

#### <a name="raise-the-farm-behavior-level-fbl"></a>提高伺服器陣列行為等級 (FBL\) 
在這個步驟之前，您必須確定已在 Active Directory 環境中執行 forestprep 和「完成」，且 Active Directory 具有 Windows Server 2016 架構。  這份檔是從 Windows 2016 網域控制站開始，不需要執行這些作業，因為它們是在安裝 AD 時執行。

>[!NOTE]
>開始下列程式之前，請從 [設定] 執行 Windows Update，以確定 Windows Server 2016 為最新狀態。  繼續此程序，直到不需要進一步更新。 此外，請確定 ADFS 服務帳戶帳戶具有 SQL server 的系統管理許可權，以及 ADFS 伺服器陣列中的每部伺服器。

1. 現在，在 Windows Server 2016 伺服器上開啟 PowerShell，然後執行下列動作： **$cred = 取得認證** ，然後按 enter 鍵。
2. 輸入在 SQL Server 上具有系統管理員許可權的認證。
3. 現在在 PowerShell 中，輸入下列內容： **AdfsFarmBehaviorLevelRaise-Credential $cred**
2. 出現提示時，輸入 **Y**。 這會開始引發層級。  一旦完成，您就已成功產生 FBL\。
![完成更新](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish1.png)
3. 現在，如果您移至 AD FS 管理，您將會看到已針對 Windows Server 2016 中的 AD FS 新增的新節點
4. 同樣地，您可以使用 PowerShell Cmdlet： Get-AdfsFarmInformation 來顯示目前的 FBL\。
![顯示如何使用 Get-AdfsFarmInformation Cmdlet 來顯示您目前 F B L 的螢幕擷取畫面。](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish2.png)

#### <a name="upgrade-the-configuration-version-of-existing-wap-servers"></a>升級現有 WAP 伺服器的設定版本
1. 在每個 Web 應用程式 Proxy 上，在提高許可權的視窗中執行下列 PowerShell 命令，以重新設定 WAP：
    ```powershell
    $trustcred = Get-Credential -Message "Enter Domain Administrator credentials"
    Install-WebApplicationProxy -CertificateThumbprint {SSLCert} -fsname fsname -FederationServiceTrustCredential $trustcred
    ```
2. 藉由執行下列 PowerShell 命令，從叢集中移除舊的伺服器，並只保留執行最新伺服器版本的 WAP 伺服器。
    ```powershell
    Set-WebApplicationProxyConfiguration -ConnectedServersName WAPServerName1, WAPServerName2
    ```
3. 執行 Get-WebApplicationProxyConfiguration 命令來檢查 WAP 設定。 ConnectedServersName 將反映從先前命令執行的伺服器。
    ```powershell
    Get-WebApplicationProxyConfiguration
    ```
4. 若要升級 WAP 伺服器的 ConfigurationVersion，請執行下列 PowerShell 命令。
    ```powershell
    Set-WebApplicationProxyConfiguration -UpgradeConfigurationVersion
    ```
5. 確認已使用 Get-WebApplicationProxyConfiguration PowerShell 命令升級 ConfigurationVersion。
