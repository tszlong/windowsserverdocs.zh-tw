---
description: 深入瞭解：使用 WID 資料庫升級至 Windows Server 2016 中的 AD FS
ms.assetid: 7671e0c9-faf0-40de-808a-62f54645f891
title: 升級為 Windows Server 2016 的 AD FS
author: billmath
manager: femila
ms.date: 04/09/2018
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 6be8d01bc7e729ee15a1488f5721e7db5f3d364d
ms.sourcegitcommit: 3247e193d9fe1b57543fff215460a6d9db52f58b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/30/2020
ms.locfileid: "97814937"
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-using-a-wid-database"></a>升級至使用 WID 資料庫的 Windows Server 2016 AD FS


> [!NOTE]
> 只會在已規劃完成的明確時間範圍內開始升級。 不建議將 AD FS 保持在混合模式的狀態一段長時間，因為在混合模式狀態中保持 AD FS 可能會導致伺服器陣列發生問題。

## <a name="upgrading-a-windows-server-2012-r2-or-2016-ad-fs-farm-to-windows-server-2019"></a>將 Windows Server 2012 R2 或 2016 AD FS 伺服器陣列升級至 Windows Server 2019
下列檔將說明當您使用 WID 資料庫時，如何將 AD FS 伺服器陣列升級為 Windows Server 2019 中的 AD FS。

### <a name="ad-fs-farm-behavior-levels-fbl"></a>AD FS 伺服器陣列行為等級 (FBL\) 
在 Windows Server 2016 AD FS 中，引進了 (FBL\) 的伺服器陣列行為等級。 這是整個伺服器陣列的設定，決定 AD FS 伺服器陣列可以使用的功能。

下表列出 Windows Server 版本的 FBL\ 值：

| Windows Server 版本  | 聯邦 調查 局 | AD FS 設定資料庫名稱 |
| ------------- | ------------- | ------------- |
| 2012 R2  | 1  | AdfsConfiguration |
| 2016  | 3  | AdfsConfigurationV3 |
| 2019  | 4  | AdfsConfigurationV4 |

> [!NOTE]
> 升級 FBL\ 會建立新的 AD FS 設定資料庫。  請參閱上表，以取得每個 Windows Server AD FS 版本和 FBL\ 值的設定資料庫名稱。

### <a name="new-vs-upgraded-farms"></a>新增與升級的伺服器陣列
根據預設，新 AD FS 伺服陣列中的 FBL\ 會與安裝的第一個伺服器陣列節點的 Windows Server 版本值相符。

較新版本的 AD FS server 可以聯結至 AD FS 2012 R2 或2016伺服器陣列，而伺服器陣列的運作方式與現有節點 (的) 相同 FBL\。 當您在相同的伺服器陣列中有多個在最低版本的 FBL\ 值上操作的 Windows Server 版本時，您的伺服器陣列稱為「混合」。 不過，在 FBL\ 引發之前，您將無法再利用較新版本的功能。 使用混合伺服器陣列：

- 系統管理員可以將新的 Windows Server 2019 同盟伺服器新增至現有的 Windows Server 2012 R2 或2016伺服器陣列。 如此一來，伺服器陣列就會處於「混合模式」，並在與原始伺服器陣列相同的伺服器陣列行為層級運作。 為了確保整個伺服器陣列一致的行為，無法設定或使用較新的 Windows Server AD FS 版本的功能。

- 系統管理員必須先從伺服器陣列中移除舊版 Windows Server AD FS 節點，才能產生 FBL\。  在 WID 伺服器陣列的案例中，請注意，這需要將其中一個新的 Windows Server 2019 同盟伺服器升級為伺服器陣列中主要節點的角色。

- 一旦伺服器陣列中的所有同盟伺服器都位於相同的 Windows Server 版本，便可引發 FBL\。  如此一來，就可以設定和使用任何新的 AD FS Windows Server 2019 功能。

請注意，在混合伺服器陣列模式下，AD FS 伺服器陣列無法在 Windows Server 2019 中 AD FS 引進的任何新功能或功能。 這表示，想要嘗試新功能的組織在 FBL\ 引發之前無法執行這項作業。 因此，如果您的組織想要在 FBL\ 之前測試新功能，您將需要部署個別的伺服器陣列來進行這項操作。

檔的其餘部分提供將 Windows Server 2019 同盟伺服器新增至 Windows Server 2016 或 2012 R2 環境，然後將 FBL\ 提升至 Windows Server 2019 的步驟。 這些步驟是在下列架構圖表所概述的測試環境中執行。

> [!NOTE]
> 在您可以移至 Windows Server 2019 FBL\ 中的 AD FS 之前，您必須先移除所有的 Windows Server 2016 或 2012 R2 節點。 您無法只將 Windows Server 2016 或 2012 R2 OS 升級至 Windows Server 2019，並讓它變成2019節點。 您必須將它移除，並取代為新的2019節點。

> [!NOTE]
> 如果 AD FS 中設定 AlwaysOnAvailability 群組或合併式複寫，請在升級前移除任何 ADFS 資料庫的所有複寫，並將所有節點指向主要 SQL 資料庫。 執行此工作之後，請執行伺服器陣列升級（如所述）。 升級之後，請將 AlwaysOnAvailability 群組或合併式複寫新增至新的資料庫。

##### <a name="to-upgrade-your-ad-fs-farm-to-windows-server-2019-farm-behavior-level"></a>將您的 AD FS 伺服器陣列升級至 Windows Server 2019 伺服器陣列行為等級

1. 使用伺服器管理員，在 Windows Server 2019 上安裝 Active Directory 同盟服務角色

2. 使用 AD FS 設定向導，將新的 Windows Server 2019 伺服器加入現有的 AD FS 伺服器陣列中。

![顯示如何將新的 Windows Server 2019 伺服器加入現有 AD FS 伺服器陣列的螢幕擷取畫面。](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_1.png)

3. 在 Windows Server 2019 同盟伺服器上，開啟 AD FS 管理]。 請注意，因為此同盟伺服器不是主伺服器，所以無法使用管理功能。

![顯示 A D F S 視窗的螢幕擷取畫面。](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_3.png)

4. 在 Windows Server 2019 伺服器上，開啟已提升許可權的 PowerShell 命令視窗，然後執行下列 Cmdlet：

```PowerShell
Set-AdfsSyncProperties -Role PrimaryComputer
```

![終端機視窗的螢幕擷取畫面，顯示如何使用 Set-AdfsSyncProperties 角色 PrimaryComputer Cmdlet。](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_4.png)

5. 在先前設定為主要的 AD FS 伺服器上，開啟已提升許可權的 PowerShell 命令視窗，然後執行下列 Cmdlet：

```PowerShell
Set-AdfsSyncProperties -Role SecondaryComputer -PrimaryComputerName {FQDN}
```

![顯示如何使用 Set-AdfsSyncProperties 角色 SecondaryComputer-PrimaryComputerName {FQDN} Cmdlet 的終端機視窗螢幕擷取畫面。](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_5.png)

6. 現在，在 Windows Server 2016 同盟伺服器上，開啟 AD FS 管理。 請注意，現在所有的系統管理員功能都會出現，是因為主要角色已轉移至此伺服器。

![顯示 Windows Server 2016 同盟伺服器開啟 AD FS 管理] 視窗的螢幕擷取畫面。](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_6.png)

7. 如果您要將 AD FS 2012 R2 伺服器陣列升級至2016或2019，伺服器陣列升級需要 AD 架構至少為層級85。  若要使用 Windows Server 2016 安裝媒體升級架構，請開啟命令提示字元並流覽至 support\adprep 目錄。 執行下列動作：  `adprep /forestprep`

![顯示如何流覽至 support\adprep 目錄的螢幕擷取畫面。](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_7.png)

完成執行之後 `adprep /domainprep`

> [!NOTE]
> 執行下一個步驟之前，請從 [設定] 執行 Windows Update，以確定 Windows Server 為最新狀態。 繼續此程序，直到不需要進一步更新。

![顯示如何執行 adprep/domainprep。的螢幕擷取畫面](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_8.png)

8. 現在，在 Windows Server 2016 伺服器上，開啟 PowerShell 並執行下列 Cmdlet：


> [!NOTE]
> 執行下一個步驟之前，必須先從伺服器陣列中移除所有 2012 R2 伺服器。

```PowerShell
Invoke-AdfsFarmBehaviorLevelRaise
```

![顯示如何執行 Invoke-AdfsFarmBehaviorLevelRaise Cmdlet 的終端機視窗螢幕擷取畫面。](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_9.png)

9. 出現提示時，輸入 Y。這會開始引發層級。 一旦完成，您就已成功產生 FBL\。

![顯示何時輸入 Y 的終端機視窗螢幕擷取畫面。](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_10.png)

10. 現在，如果您移至 AD FS 管理，您會看到稍後的 AD FS 版本已新增新功能

![顯示已新增功能的螢幕擷取畫面。](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_12.png)

11. 同樣地，您可以使用 PowerShell Cmdlet：  `Get-AdfsFarmInformation` 顯示目前的 fbl\。

![升級](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_13.png)

12. 若要將 WAP 伺服器升級至最新層級，請在每個 Web 應用程式 Proxy 上，在提高許可權的視窗中執行下列 PowerShell Cmdlet，以重新設定 WAP：

```PowerShell
$trustcred = Get-Credential -Message "Enter Domain Administrator credentials"
Install-WebApplicationProxy -CertificateThumbprint {SSLCert} -fsname fsname -FederationServiceTrustCredential $trustcred
```

藉由執行下列 Powershell Cmdlet，從叢集中移除舊的伺服器，並只保留執行最新伺服器版本的 WAP 伺服器。

```PowerShell
Set-WebApplicationProxyConfiguration -ConnectedServersName WAPServerName1, WAPServerName2
```

執行 Get-WebApplicationProxyConfiguration Cmdlet 來檢查 WAP 設定。 ConnectedServersName 將反映從先前命令執行的伺服器。

```PowerShell
Get-WebApplicationProxyConfiguration
```
> [!NOTE]
> 如果 ConfigurationVersion 是 Windows Server 2016，請略過下一個步驟。 這是 Windows Server 2016/2019 上的 Web 應用程式 Proxy 的正確值。

若要升級 WAP 伺服器的 ConfigurationVersion，請執行下列 Powershell 命令。

```PowerShell
Set-WebApplicationProxyConfiguration -UpgradeConfigurationVersion
```

這會完成 WAP 伺服器的升級。


> [!NOTE]
> 如果執行具有混合式憑證信任的 Windows Hello 企業版，AD FS 2019 中存在已知的 PRT 問題。 您可能會在 ADFS 系統管理員事件記錄檔中遇到此錯誤：收到不正確 Oauth 要求。 已禁止用戶端 'NAME' 存取範圍為 'ugs' 的資源。
> 若要補救此錯誤：
> 1. 啟動 AD FS 管理主控台。 瀏覽至「服務 > 範圍描述」
> 2. 以滑鼠右鍵按一下 [範圍描述]，然後選取 [新增範圍描述]
> 3. 在名稱下輸入 "ugs"，然後按一下 [套用] > [確定]
> 4. 以系統管理員身分執行 Powershell。
> 5. 執行 "Get-AdfsApplicationPermission" 命令。 尋找具有 ClientRoleIdentifier 的 ScopeNames :{openid, aza}。 記下 ObjectIdentifier。
> 6. 執行命令 "Set-AdfsApplicationPermission -TargetIdentifier <步驟 5 的 ObjectIdentifier> -AddScope 'ugs'
> 7. 重新啟動 ADFS 服務。
> 8. 在用戶端上：重新啟動用戶端。 系統應該會提示使用者佈建 WHFB。
> 9. 如果 [佈建] 視窗未出現，則需要收集 NGC 追蹤記錄並執行進一步的疑難排解。
