---
ms.assetid: 7671e0c9-faf0-40de-808a-62f54645f891
title: 升級為 Windows Server 2016 的 AD FS
description: ''
author: billmath
manager: femila
ms.date: 04/09/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 169ee7d16fbac043c088c1e568600ee93e70d8a1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359333"
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-using-a-wid-database"></a>升級至使用 WID 資料庫的 Windows Server 2016 AD FS


> [!NOTE]  
> 只會開始升級，並已規劃完成的最終時間範圍。 不建議讓 AD FS 處於混合模式的狀態一段很長的時間，因為離開 AD FS 處於混合模式的狀態可能會導致伺服器陣列發生問題。

## <a name="upgrading-a-windows-server-2012-r2-or-2016-ad-fs-farm-to-windows-server-2019"></a>將 Windows Server 2012 R2 或 2016 AD FS 伺服器陣列升級至 Windows Server 2019
下列檔將說明當您使用 WID 資料庫時，如何將 AD FS 伺服器陣列升級至 Windows Server 2019 中的 AD FS。  

### <a name="ad-fs-farm-behavior-levels-fbl"></a>AD FS 伺服器陣列行為層級（FBL）  
在 Windows Server 2016 的 AD FS 中，會引進伺服器陣列行為層級（FBL）。 這是整個伺服器陣列的設定，用來決定 AD FS 伺服器陣列可以使用的功能。

下表依 Windows Server 版本列出 FBL 值：

| Windows Server 版本  | FBL | AD FS 設定資料庫名稱 |
| ------------- | ------------- | ------------- |
| 2012 R2  | 1  | AdfsConfiguration |
| 2016  | 3  | AdfsConfigurationV3 |
| 2019  | 4  | AdfsConfigurationV4 |

> [!NOTE]  
> 升級 FBL 會建立新的 AD FS 設定資料庫。  如需每個 Windows Server 的設定資料庫名稱 AD FS 版本和 FBL 值，請參閱上表

### <a name="new-vs-upgraded-farms"></a>新的 vs 升級的伺服器陣列
根據預設，新 AD FS 伺服器陣列中的 FBL 會符合所安裝第一個伺服器陣列節點之 Windows Server 版本的值。  

較新版本的 AD FS 伺服器可以聯結至 AD FS 2012 R2 或2016伺服器陣列，而伺服器陣列會在與現有節點相同的 FBL 上運作。 當您有多個 Windows Server 版本在 FBL 值最低版本的相同伺服器陣列中運作時，您的伺服器陣列就會被視為「混合」。 不過，在 FBL 引發之前，您將無法利用較新版本的功能。 使用混合伺服器陣列：  

-   系統管理員可以將新的 Windows Server 2019 同盟伺服器新增至現有的 Windows Server 2012 R2 或2016伺服器陣列。 因此，伺服器陣列會處於「混合模式」，並與原始伺服器陣列在相同的伺服器陣列行為層級上運作。 為了確保整個伺服器陣列的行為一致，無法設定或使用較新的 Windows Server AD FS 版本的功能。  

- 在引發 FBL 之前，系統管理員必須從伺服器陣列移除先前 Windows Server 版本的 AD FS 節點。  在 WID 伺服器陣列的案例中，請注意，這需要將其中一部新的 Windows Server 2019 同盟伺服器 tp 升級為伺服器陣列中主要節點的角色。

-   一旦伺服器陣列中的所有同盟伺服器都是相同的 Windows Server 版本，就可以引發 FBL。  如此一來，就可以設定和使用任何新的 AD FS Windows Server 2019 功能。

請注意，在混合伺服器陣列模式中，AD FS 伺服器陣列無法提供 Windows Server 2019 AD FS 中引進的任何新功能或功能。 這表示想要嘗試新功能的組織在引發 FBL 之前無法執行此動作。 因此，如果您的組織想要在 rasing FBL 之前測試新功能，您必須部署個別的伺服器陣列來執行此動作。  

其餘部分是檔，提供將 Windows Server 2019 同盟伺服器新增至 Windows Server 2016 或 2012 R2 環境，然後將 FBL 提升至 Windows Server 2019 的步驟。 這些步驟是在測試環境中執行的，如下圖所述。  

> [!NOTE]  
> 您必須先移除所有 Windows Server 2016 或 2012 R2 節點，才能移至 Windows Server 2019 FBL 中的 AD FS。 您不能只是將 Windows Server 2016 或 2012 R2 作業系統升級到 Windows Server 2019，並讓它成為一個2019節點。 您必須將它移除，並將它取代為新的2019節點。



##### <a name="to-upgrade-your-ad-fs-farm-to-windows-server-2019-farm-behavior-level"></a>將您的 AD FS 伺服器陣列升級至 Windows Server 2019 伺服器陣列行為層級  

1.  使用伺服器管理員，在 Windows Server 2019 上安裝 Active Directory 同盟服務角色

2.  使用 AD FS 設定 wizard，將新的 Windows Server 2019 伺服器加入現有的 AD FS 伺服器陣列。  

    ![upgrade](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_1.png)  

3.  在 Windows Server 2019 同盟伺服器上，開啟 [AD FS 管理]。 請注意，因為此同盟伺服器不是主伺服器，所以無法使用管理功能。  

    ![upgrade](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_3.png)  

4.  在 Windows Server 2019 伺服器上，開啟提升許可權的 PowerShell 命令視窗，然後執行下列 cmdlt： `Set-AdfsSyncProperties -Role PrimaryComputer`

    ![upgrade](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_4.png)  

5.  在先前設定為主要的 AD FS 伺服器上，開啟已提升許可權的 PowerShell 命令視窗，然後執行下列 cmdlt： `Set-AdfsSyncProperties -Role SecondaryComputer -PrimaryComputerName {FQDN} `

    ![upgrade](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_5.png)  

6.  現在，在 Windows Server 2016 同盟伺服器上開啟 AD FS 管理。 請注意，由於主要角色已轉移到這部伺服器，因此現在會顯示所有的系統管理功能。  

    ![upgrade](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_6.png)  

7.  如果您要將 AD FS 2012 R2 伺服器陣列升級為2016或2019，伺服器陣列升級會要求 AD 架構至少為層級85。  若要使用 Windows Server 2016 安裝媒體升級架構，請開啟命令提示字元，並流覽至 support\adprep 目錄。 執行下列動作： `adprep /forestprep`

    ![upgrade](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_7.png)  

    完成執行後 `adprep/domainprep`
    >[!NOTE]
    >執行下一個步驟之前，請先從 [設定] 執行 Windows Update，以確定 Windows Server 為最新狀態。 繼續此程序，直到不需要進一步更新。
    >

    ![upgrade](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_8.png)  

8. 現在，在 Windows Server 2016 伺服器上開啟 PowerShell，並執行下列 cmdlt：
    >[!NOTE]
    > 在執行下一個步驟之前，必須先從伺服器陣列中移除所有 2012 R2 伺服器。

    `Invoke-AdfsFarmBehaviorLevelRaise`  

    ![upgrade](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_9.png)  

9. 出現提示時，輸入 Y。這會開始提高層級。 完成後，您就已成功引發 FBL。  

    ![upgrade](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_10.png)  

10. 現在，如果您移至 [AD FS 管理]，您會看到較新的 AD FS 版本已新增功能

    ![upgrade](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_12.png)  

11. 同樣地，您可以使用 PowerShell cmdlt： `Get-AdfsFarmInformation` 來顯示目前的 FBL。  

    ![upgrade](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_13.png)  

12. 若要將 WAP 伺服器升級至最新的層級，請在每個 Web 應用程式 Proxy 上，于提升許可權的視窗中執行下列 PowerShell 命令，以重新設定 WAP：  
    ```powershell
    $trustcred = Get-Credential -Message "Enter Domain Administrator credentials"
    Install-WebApplicationProxy -CertificateThumbprint {SSLCert} -fsname fsname -FederationServiceTrustCredential $trustcred  
    ```
    藉由執行下列 Powershell commandlet，從叢集中移除舊的伺服器，並只保留執行最新伺服器版本的 WAP 伺服器。
    ```powershell
    Set-WebApplicationProxyConfiguration -ConnectedServersName WAPServerName1, WAPServerName2
    ```
    執行 Set-webapplicationproxyconfiguration commmandlet 來檢查 WAP 設定。 ConnectedServersName 會反映伺服器從先前的命令執行。
    ```powershell
    Get-WebApplicationProxyConfiguration
    ```
    若要升級 WAP 伺服器的 ConfigurationVersion，請執行下列 Powershell 命令。
    ```powershell
    Set-WebApplicationProxyConfiguration -UpgradeConfigurationVersion
    ```
    這將會完成 WAP 伺服器的升級。
