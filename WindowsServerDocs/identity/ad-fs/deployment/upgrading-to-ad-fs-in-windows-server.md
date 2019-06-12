---
ms.assetid: 7671e0c9-faf0-40de-808a-62f54645f891
title: 升級為 Windows Server 2016 的 AD FS
description: ''
author: billmath
manager: femila
ms.date: 04/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6f3cdc34ee03fab1a8fb1d42ebed2d2f76e2618d
ms.sourcegitcommit: ccc802338b163abdad2e53b55f39addcfea04603
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/05/2019
ms.locfileid: "66687410"
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-using-a-wid-database"></a>升級至使用 WID 資料庫的 Windows Server 2016 AD FS


> [!NOTE]  
> 只有在明確的時間範圍內完成計劃與開始升級。 不建議將 AD FS 在混合的模式的狀態，長的時間，因為在混合的模式狀態中離開的 AD FS 可能會造成問題，與伺服器陣列。

## <a name="upgrading-a-windows-server-2012-r2-or-2016-ad-fs-farm-to-windows-server-2019"></a>Windows Server 2012 R2 或 2016 AD FS 伺服器陣列升級至 Windows Server 2019
下列文件將說明如何使用 WID 資料庫時，AD fs 的 Windows Server 2019 升級您的 AD FS 伺服器陣列。  

### <a name="ad-fs-farm-behavior-levels-fbl"></a>AD FS 伺服器陣列行為層級 (FBL)  
在適用於 Windows Server 2016 AD FS 中，引進了伺服器陣列行為層級 (FBL)。 這是整個伺服陣列的設定會決定可以使用功能的 AD FS 伺服器陣列。

下表列出 FBL 值由 Windows Server 版本：

| Windows Server 版本  | FBL | AD FS 設定資料庫名稱 |
| ------------- | ------------- | ------------- |
| 2012 R2  | 1  | AdfsConfiguration |
| 2016  | 3  | AdfsConfigurationV3 |
| 2019  | 4  | AdfsConfigurationV4 |

> [!NOTE]  
> 升級 FBL 會建立新的 AD FS 設定資料庫。  請參閱上表的組態資料庫的名稱，為每個 Windows Server AD FS 版本和 FBL 值

### <a name="new-vs-upgraded-farms"></a>新的 vs Upgraded 伺服器陣列
根據預設，新的 AD FS 伺服器陣列中 FBL 會符合安裝的第一個伺服器陣列節點的 Windows Server 版本的值。  

AD FS 伺服器的更新版本可聯結至 AD FS 2012 R2 或 2016年伺服器陣列，並在伺服器陣列會以相同的 FBL 為現有的節點上運作。 當您有多個作業系統 FBL 值最低的版本在相同的伺服器陣列中的 Windows Server 版本時，即稱為您的伺服器陣列，"mixed"。 不過，您將無法充分利用的更新版本的功能，直到 FBL，就會引發。 使用混合的伺服陣列中：  

-   系統管理員可以將新的 Windows Server 2019 同盟伺服器加入現有的 Windows Server 2012 R2 或 2016年伺服器陣列。 如此一來，伺服器陣列處於 「 混合模式 」，並在原始的伺服器陣列的相同伺服器陣列行為層級運作。 若要確保一致的行為，整個伺服器陣列，無法設定或使用較新的 Windows Server AD FS 版本的功能。  

- FBL 可能會引發之前，系統管理員必須移除舊版 Windows Server AD FS 節點，從伺服器陣列。  在 WID 伺服器陣列的情況下請注意，這會需要其中一個新的 Windows Server 2019 同盟伺服器 tp 會升級為主要節點伺服器陣列中的角色。

-   一旦在伺服器陣列中的所有同盟伺服器都都位於相同的 Windows Server 版本，可能會引發 FBL。  如此一來，任何新的 AD FS 的 Windows Server 2019 功能可以再進行設定和使用。

請注意，在混合式伺服器陣列模式時，AD FS 伺服器陣列不支援任何新功能或 Windows Server 2019 中的 AD FS 中引進的功能。 這表示想要試用新功能的組織無法這麼做之前，就會引發 FBL。 因此如果您的組織會想要測試之前 rasing FBL 的新功能，您必須部署個別的伺服器陣列，若要這樣做。  

其餘部分是文件提供 Windows Server 2019 同盟伺服器加入 Windows Server 2016 或 2012 R2 環境，則提高至 Windows Server 2019 FBL 的步驟。 架構圖所述的測試環境中執行這些步驟。  

> [!NOTE]  
> 您可以移至 Windows Server 2019 FBL 中的 AD FS 之前，您必須移除所有的 Windows Server 2016 或 2012 R2 節點。 您就無法將 Windows Server 2016 或 2012 R2 作業系統升級到 Windows Server 2019，並將它變成 2019年節點。 您必須將它移除，並以新的 2019年節點取代。



##### <a name="to-upgrade-your-ad-fs-farm-to-windows-server-2019-farm-behavior-level"></a>若要升級您的 AD FS 伺服器陣列至 Windows Server 2019 伺服陣列行為層級  

1.  使用伺服器管理員，在 Windows Server 2019 上安裝 Active Directory Federation Services 角色

2.  使用 AD FS 設定精靈，將新的 Windows Server 2019 伺服器加入現有的 AD FS 伺服器陣列。  

    ![升級](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_1.png)  

3.  在 Windows Server 2019 同盟伺服器上，開啟 [AD FS 管理]。 請注意，因為此同盟伺服器不是主要伺服器不提供管理功能。  

    ![升級](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_3.png)  

4.  在 Windows Server 2019 伺服器上，開啟提升權限的 PowerShell 命令視窗並執行下列 cmdlet: `Set-AdfsSyncProperties -Role PrimaryComputer`

    ![升級](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_4.png)  

5.  先前已設定為主要 AD FS 伺服器上開啟提升權限的 PowerShell 命令視窗並執行下列 cmdlet: `Set-AdfsSyncProperties -Role SecondaryComputer -PrimaryComputerName {FQDN} `

    ![升級](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_5.png)  

6.  現在在 Windows Server 2016 的同盟伺服器上開啟 AD FS 管理。 請注意，現在所有的系統管理功能會出現，因為已移轉到此伺服器的主要角色。  

    ![升級](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_6.png)  

7.  如果您要升級 AD FS 2012 R2 伺服器陣列至 2016年或 2019年，伺服器陣列升級時需要 AD 結構描述是至少層級 85。  若要升級的結構描述、 使用 「 Windows Server 2016 安裝媒體，開啟命令提示字元並瀏覽至 support\adprep 目錄。 執行下列命令：  `adprep /forestprep`

    ![升級](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_7.png)  

    完成後執行 `adprep/domainprep`
    >[!NOTE]
    >然後再執行下一個步驟，請確定 Windows Server 為最新的設定從執行 Windows Update。 繼續此程序，直到不需要進一步更新。
    >

    ![升級](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_8.png)  

8. 現在在 Windows Server 2016 伺服器上開啟 PowerShell 並執行下列 cmdlet:
    >[!NOTE]
    > 所有 2012 R2 的伺服器必須從伺服器陣列中都移除，才能執行下一個步驟。

    `Invoke-AdfsFarmBehaviorLevelRaise`  

    ![升級](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_9.png)  

9. 出現提示時，請輸入 Y。這會開始引發層級。 一旦完成後您成功地提出 FBL。  

    ![升級](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_10.png)  

10. 現在，如果您移至 AD FS 管理，您會看到已新增的更新版本的 AD FS 版本的新功能

    ![升級](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_12.png)  

11. 同樣地，您可以使用 PowerShell cmdlt:`Get-AdfsFarmInformation`以顯示您目前的 FBL。  

    ![升級](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_13.png)  

12. 若要升級為最新的層級，每個 Web 應用程式 proxy，WAP 伺服器重新設定 WAP 藉由在提升權限的視窗執行下列 PowerShell 命令：  
    ```powershell
    $trustcred = Get-Credential -Message "Enter Domain Administrator credentials"
    Install-WebApplicationProxy -CertificateThumbprint {SSLCert} -fsname fsname -FederationServiceTrustCredential $trustcred  
    ```
    從叢集移除舊的伺服器，並保留只的 WAP 伺服器執行最新的伺服器版本中，已藉由執行下列 Powershell 指令程式來重新設定上面。
    ```powershell
    Set-WebApplicationProxyConfiguration -ConnectedServersName WAPServerName1, WAPServerName2
    ```
    藉由執行 Get WebApplicationProxyConfiguration commmandlet 檢查 WAP 組態。 ConnectedServersName 會反映來自上一個命令執行的伺服器。
    ```powershell
    Get-WebApplicationProxyConfiguration
    ```
    若要升級的 WAP 伺服器 ConfigurationVersion，執行下列 Powershell 命令。
    ```powershell
    Set-WebApplicationProxyConfiguration -UpgradeConfigurationVersion
    ```
    這將會完成 WAP 伺服器的升級。
