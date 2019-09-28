---
ms.assetid: 777aab65-c9c7-4dc9-a807-9ab73fac87b8
title: 設定 AD FS 外部網路鎖定保護
description: ''
author: billmath
ms.author: billmath
manager: mtilman
ms.date: 05/20/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9ef595cc98a95caca0f2043b011868e0573a5b19
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407703"
---
# <a name="ad-fs-extranet-lockout-and-extranet-smart-lockout"></a>AD FS 外部網路鎖定和外部網路智慧鎖定

## <a name="overview"></a>總覽

外部網路智慧鎖定（ESL）可保護使用者不會遇到惡意活動的外部網路帳戶鎖定。  

ESL 可讓 AD FS 區分使用者熟悉位置的登入嘗試，以及從可能是攻擊者進行的登入嘗試。 AD FS 可以鎖定攻擊者，同時讓有效的使用者繼續使用其帳戶。 這可防止並防止使用者的拒絕服務和特定類別的密碼噴灑攻擊。 ESL 適用于 Windows Server 2016 中的 AD FS，並內建于 Windows Server 2019 中的 AD FS。

ESL 僅適用于透過外部網路使用 Web 應用程式 Proxy 或協力廠商 proxy 的使用者名稱和密碼驗證要求。 任何協力廠商 proxy 都必須支援用來取代 Web 應用程式 Proxy 的 ADFSPIP 通訊協定，例如[F5 BIG IP 存取原則管理員](https://devcentral.f5.com/s/articles/ad-fs-proxy-replacement-on-f5-big-ip-30191)。 請參閱協力廠商 proxy 檔，以判斷 proxy 是否支援 MS ADFSPIP 通訊協定。   

## <a name="additional-features-in-ad-fs-2019"></a>AD FS 2019 中的其他功能
相較于 AD FS 2016，AD FS 2019 中的外部網路智慧鎖定增加了下列優點：
- 針對熟悉和不熟悉的位置設定獨立的鎖定閾值，讓已知良好位置的使用者可以有更多的錯誤空間，而不是來自可疑位置的要求
- 啟用智慧鎖定的 audit 模式，同時繼續強制執行先前的軟鎖定行為。 這可讓您瞭解使用者熟悉的位置，並仍然受到 AD FS 2012R2 提供之外部網路鎖定功能的保護。  

## <a name="how-it-works"></a>運作方式
### <a name="configuration-information"></a>設定資訊
啟用 ESL 時，會建立成品資料庫 AdfsArtifactStore. AccountActivity 中的新資料表，並在 AD FS 伺服器陣列中選取節點做為「使用者活動」主機。 在 WID 設定中，此節點一律是主要節點。 在 SQL 設定中，會選取一個節點做為使用者活動主機。  

以使用者活動主機的形式來查看選取的節點。 AdfsFarmInformation. FarmRoles

所有次要節點都會透過埠80，與每個全新登入的主要節點聯繫，以瞭解錯誤密碼計數和新熟悉位置值的最新值，並在處理登入之後更新該節點。

![組態](media/configure-ad-fs-extranet-smart-lockout-protection/esl1.png)

 如果次要節點無法連上主機，它會將錯誤事件寫入 AD FS 的系統管理員記錄中。 系統會繼續處理驗證，但是 AD FS 只會在本機寫入已更新的狀態。 AD FS 將會每隔10分鐘重試一次主機，並在主要複本可供使用之後，切換回主要主機。

### <a name="terminology"></a>詞彙
- **FamiliarLocation**：在驗證要求期間，ESL 會檢查所有顯示的 Ip。 這些 ip 會結合網路 IP、轉送的 IP 和選擇性的 x 轉送-作為 IP。 如果要求成功，則所有 Ip 都會新增至 [帳戶活動] 資料表中做為「熟悉的 Ip」。 如果要求的所有 Ip 都出現在「熟悉的 Ip」中，則會將要求視為「熟悉的」位置。
- **Unknownlocation.xsd**：如果傳入的要求至少有一個 IP 不存在於現有的 "FamiliarLocation" 清單中，則會將要求視為「不明」位置。 這是為了處理代理案例，例如 Exchange online 傳統驗證，其中 Exchange Online 位址會處理成功和失敗的要求。  
- **badPwdCount**：值，表示提交錯誤密碼的次數，且驗證不成功。 針對每個使用者，會針對熟悉的位置和未知位置保留個別的計數器。
- **UnknownLockout**：如果使用者遭到鎖定而無法從未知位置存取，則為每個使用者的布林值。 這個值是根據 badPwdCountUnfamiliar 和 ExtranetLockoutThreshold 值來計算。
- **ExtranetLockoutThreshold**：這個值會決定錯誤密碼嘗試次數的上限。 達到閾值時，ADFS 會拒絕外部網路的要求，直到 [觀察] 視窗通過為止。
- **ExtranetObservationWindow**：此值決定來自未知位置的使用者名稱和密碼要求被鎖定的持續時間。當視窗已通過時，ADFS 會開始從不明位置再次執行使用者名稱和密碼驗證。
- **ExtranetLockoutRequirePDC**：啟用時，外部網路鎖定需要主域控制站（PDC）。 停用時，外部網路鎖定會回到另一個網域控制站，以防 PDC 無法使用。  
- **ExtranetLockoutMode**：控制項僅記錄與外部網路智慧鎖定的強制執行模式
    - **ADFSSmartLockoutLogOnly**：外部網路智慧鎖定已啟用，但 AD FS 只會寫入 admin 和 audit 事件，但不會拒絕驗證要求。 此模式的目的是一開始先啟用 FamiliarLocation，才能在啟用 ' ADFSSmartLockoutEnforce ' 之前填入。
    - **ADFSSmartLockoutEnforce**：當達到臨界值時，完全支援封鎖不熟悉的驗證要求。

支援 IPv4 和 IPv6 位址。

### <a name="anatomy-of-a-transaction"></a>交易的剖析
- **預先驗證檢查**：在驗證要求期間，ESL 會檢查所有顯示的 Ip。 這些 ip 會結合網路 IP、轉送的 IP 和選擇性的 x 轉送-作為 IP。 在 audit 記錄中，這些 ip 會<IpAddress>依照 x 毫秒轉送-用戶端 ip、x 轉送-------------------------------------

  根據這些 Ip，ADFS 會判斷要求是否來自熟悉或不熟悉的位置，然後檢查個別的 badPwdCount 是否小於設定的臨界值限制，或上次**失敗**的嘗試時間是否超過 [觀察] 視窗時間框架. 如果其中一個條件為 true，ADFS 會允許此交易進行進一步的處理和認證驗證。 如果這兩個條件都為 false，則帳戶已處於鎖定狀態，直到觀察視窗通過為止。 在觀察時間範圍通過之後，使用者就可以嘗試驗證一次。 請注意，在2019中，ADFS 會根據是否有符合熟悉位置的 IP 位址，檢查適當的閾值限制。
- **成功的登**入：如果登入成功，則會將要求中的 Ip 新增至使用者的熟悉位置 IP 清單。  
- **登入失敗**：如果登入失敗，則會增加 badPwdCount。 如果攻擊者將更不正確的密碼傳送給系統，則使用者會進入鎖定狀態，而不是允許的閾值。 （badPwdCount > ExtranetLockoutThreshold）  

![組態](media/configure-ad-fs-extranet-smart-lockout-protection/esl2.png)

當帳戶被鎖定時，"UnknownLockout" 值會等於 true。這表示使用者的 badPwdCount 超過閾值，也就是有人嘗試的密碼多於系統允許的數目。 在此狀態下，有2種方式可讓有效的使用者登入。
- 使用者必須等候 ObservationWindow 時間，或
- 若要重設鎖定狀態，請將 badPwdCount 重設回零並加上 ' Reset-ADFSAccountLockout '。

如果沒有進行重設，則會允許帳戶對每個觀察視窗的 AD 進行單一密碼嘗試。 該帳戶會在該嘗試之後回到鎖定的狀態，而 [觀察] 視窗將會重新開機。 BadPwdCount 值只會在成功登入密碼之後自動重設。

### <a name="log-only-mode-versus-enforce-mode"></a>僅限記錄模式與「強制」模式
AccountActivity 資料表會在「僅限記錄」模式和「強制」模式期間填入。 如果略過「僅限記錄」模式，而 ESL 直接移到「強制」模式而不建議的等候期間，則 ADFS 不會知道這些使用者的熟悉 Ip。 在此情況下，ESL 的行為就像是「ADBadPasswordCounter」，如果使用者帳戶是在作用中的暴力密碼破解攻擊之下，可能會封鎖合法的使用者流量。 如果略過 [僅限記錄] 模式，而且使用者輸入的鎖定狀態具有 "UnknownLockout" = TRUE，並嘗試使用不在「熟悉的」 IP 清單中的 IP 登入，則他們將無法登入。 建議使用僅限記錄模式3-7 天，以避免發生這種情況。 如果帳戶主動受到攻擊，則至少需要24小時的「僅限記錄」模式，才能防止合法使用者的鎖定。  

## <a name="extranet-smart-lockout-configuration"></a>外部網路智慧鎖定設定  

### <a name="prerequisites-for-ad-fs-2016"></a>AD FS 2016 的必要條件

1. **在伺服器陣列中的所有節點上安裝更新**

   首先，請確定所有 Windows Server 2016 AD FS 伺服器都是最新的，從2018年6月的 Windows 更新開始，而且 AD FS 2016 伺服器陣列是在「2016伺服器陣列」行為層級執行。
1. **驗證許可權**

   外部網路智慧鎖定需要在每個 AD FS 伺服器上啟用 Windows 遠端系統管理。
3. **更新成品資料庫許可權**

   外部網路智慧鎖定要求 AD FS 服務帳戶必須擁有在 AD FS 成品資料庫中建立新資料表的許可權。 以 AD FS 系統管理員身分登入任何 AD FS 伺服器，然後在 PowerShell 命令提示字元視窗中執行下列命令，以授與此許可權：

   ``` powershell
   PS C:\>$cred = Get-Credential
   PS C:\>Update-AdfsArtifactDatabasePermission -Credential $cred
   ```
   >[!NOTE]
   >$Cred 預留位置是具有 AD FS 系統管理員許可權的帳戶。 這應該會提供建立資料表的寫入權限。

   上述命令可能會因為許可權不足而失敗，因為您的 AD FS 伺服器陣列正在使用 SQL Server，而上述提供的認證沒有 SQL Server 的系統管理員許可權。 在此情況下，當您連線到 AdfsArtifactStore 資料庫時，可以執行下列命令，在 SQL Server 資料庫中手動設定資料庫許可權。
    ```  
    # when prompted with “Are you sure you want to perform this action?”, enter Y.

    [CmdletBinding(SupportsShouldProcess=$true,ConfirmImpact = 'High')]
    Param()

    $fileLocation = "$env:windir\ADFS\Microsoft.IdentityServer.Servicehost.exe.config"

    if (-not [System.IO.File]::Exists($fileLocation))
    {
    write-error "Unable to open ADFS configuration file."
    return
    }

    $doc = new-object Xml
    $doc.Load($fileLocation)
    $connString = $doc.configuration.'microsoft.identityServer.service'.policystore.connectionString
    $connString = $connString -replace "Initial Catalog=AdfsConfigurationV[0-9]*", "Initial Catalog=AdfsArtifactStore"

    if ($PSCmdlet.ShouldProcess($connString, "Executing SQL command sp_addrolemember 'db_owner', 'db_genevaservice' "))
    {
    $cli = new-object System.Data.SqlClient.SqlConnection
    $cli.ConnectionString = $connString
    $cli.Open()

    try
    {     

    $cmd = new-object System.Data.SqlClient.SqlCommand
    $cmd.CommandText = "sp_addrolemember 'db_owner', 'db_genevaservice'"
    $cmd.Connection = $cli
    $rowsAffected = $cmd.ExecuteNonQuery()  
    if ( -1 -eq $rowsAffected )
    {
    write-host "Success"
    }
    }
    finally
    {
    $cli.CLose()
    }
    }
    ```

### <a name="ensure-ad-fs-security-audit-logging-is-enabled"></a>確定已啟用 AD FS 安全性審核記錄
這項功能會使用安全性 Audit 記錄，因此必須在 AD FS 中啟用審核功能，以及在所有 AD FS 伺服器上的本機原則。

### <a name="configuration-instructions"></a>設定指示
外部網路智慧鎖定會使用 ADFS 屬性**ExtranetLockoutEnabled**。 這個屬性先前是用來控制伺服器2012R2 中的「外部網路虛鎖定」。 如果已啟用外部網路虛鎖定，若要查看目前的屬性設定` Get-AdfsProperties` ，請執行。

### <a name="configuration-recommendations"></a>設定建議
設定外部網路智慧鎖定時，請遵循設定閾值的最佳做法：  

`ExtranetObservationWindow (new-timespan -Minutes 30)`

`ExtranetLockoutThreshold: – 2x AD Threshold Value`

AD 值：20，ExtranetLockoutThreshold：10

Active Directory 鎖定獨立于外部網路智慧鎖定。 不過，如果已啟用 Active Directory 鎖定，則 ExtranetLockoutThreshold 會在 AD 中 AD FS < 帳戶鎖定閾值

`ExtranetLockoutRequirePDC - $false`

啟用時，外部網路鎖定需要主域控制站（PDC）。 當停用並設定為 false 時，如果 PDC 無法使用，外部網路鎖定將會回復至另一個網域控制站。

若要設定此屬性，請執行：

``` powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30) -ExtranetLockoutRequirePDC $false
```
### <a name="enable-log-only-mode"></a>啟用僅記錄模式

在 [僅限記錄檔] 模式中，AD FS 會填入使用者熟悉的位置資訊，並寫入安全性 audit 事件，但不會封鎖任何要求。 此模式是用來驗證智慧鎖定是否正在執行，並可讓 AD FS 在啟用「強制」模式之前，先「瞭解」使用者熟悉的位置。 隨著 AD FS 學習，它會儲存每位使用者的登入活動（不論是以僅記錄模式或強制模式）。
藉由執行下列 commandlet，將鎖定行為設定為 [僅記錄]。  

`Set-AdfsProperties -ExtranetLockoutMode AdfsSmartlockoutLogOnly`

僅限記錄模式是暫時的狀態，因此系統可以在使用智慧鎖定行為來執行鎖定之前，先瞭解登入行為。 僅記錄模式的建議持續時間為3-7 天。 如果帳戶主動受到攻擊，則僅限記錄模式必須執行至少24小時。

在 AD FS 2016 上，如果在啟用外部網路智慧鎖定之前啟用2012R2 「外部網路軟鎖定」行為，僅限記錄模式會停用「外部網路軟鎖定」行為。 AD FS 智慧鎖定不會以僅限記錄模式鎖定使用者。 不過，內部部署 AD 可能會根據 AD 設定來鎖定使用者。 請參閱 AD 鎖定原則，以瞭解內部部署 AD 可以如何鎖定使用者。

在 AD FS 2019 上，還有一個額外的優點，就是能夠啟用智慧型鎖定的僅記錄模式，同時繼續使用下列 Powershell 強制執行先前的軟鎖定行為。

`Set-AdfsProperties -ExtranetLockoutMode 3`

若要讓新模式生效，請在伺服器陣列中的所有節點上重新開機 AD FS 服務

`Restart-service adfssrv`

設定模式之後，您可以使用 EnableExtranetLockout 參數來啟用智慧鎖定

`Set-AdfsProperties -EnableExtranetLockout $true`

### <a name="enable-enforce-mode"></a>啟用強制執行模式

在您熟悉 [鎖定閾值] 和 [觀察] 視窗之後，您可以使用下列 PSH Cmdlet，將 ESL 移至 [強制] 模式：

`Set-AdfsProperties -ExtranetLockoutMode AdfsSmartLockoutEnforce`

若要讓新的模式生效，請使用下列命令，在伺服器陣列中的所有節點上重新開機 AD FS 服務。

`Restart-service adfssrv`

## <a name="manage-user-account-activity"></a>管理使用者帳戶活動
AD FS 提供三個 Cmdlet 來管理帳戶活動資料。 這些 Cmdlet 會自動連接到伺服器陣列中具有主要角色的節點。
>[!NOTE]
>只有足夠的系統管理（JEA）可以用來委派 AD FS 的 commandlet，以重設帳戶鎖定。 例如，技術支援人員可以委派許可權來使用 ESL commandlet。 如需委派使用這些 Cmdlet 之許可權的相關資訊，請參閱[委派 AD FS Powershell Commandlet 存取非系統管理員使用者](delegate-ad-fs-pshell-access.md)

您可以藉由傳遞-Server 參數來覆寫此行為。

- ADFSAccountActivity-UserPrincipalName

  讀取使用者帳戶的目前帳戶活動。 Cmdlet 一律會使用帳戶活動 REST 端點自動連接到伺服器陣列主機。 因此，所有資料都應該一律一致。

`Get-ADFSAccountActivity user@contoso.com`

  屬性
    - BadPwdCountFamiliar:從已知位置成功驗證時，會遞增。
    - BadPwdCountUnknown:從未知位置驗證失敗時遞增
    - LastFailedAuthFamiliar:如果從熟悉的位置驗證失敗，LastFailedAuthUnknown 會設定為不成功驗證的時間
    - LastFailedAuthUnknown:如果驗證從未知的位置失敗，LastFailedAuthUnknown 會設定為不成功的驗證時間
    - FamiliarLockout:布林值，如果 "BadPwdCountFamiliar" > ExtranetLockoutThreshold 則為 "True"
    - UnknownLockout:布林值，如果 "BadPwdCountUnknown" > ExtranetLockoutThreshold 則為 "True"  
    - FamiliarIPs：最多20個熟悉使用者的 Ip。 超過此限制時，將會移除清單中最舊的 IP。
-    設定-ADFSAccountActivity

     新增熟悉的位置。 熟悉的 IP 清單最多可包含20個專案，如果超過此值，將會移除清單中最舊的 IP。

`Set-ADFSAccountActivity user@contoso.com -AdditionalFamiliarIps “1.2.3.4”`

- 重設-ADFSAccountLockout

  針對每個熟悉的位置（badPwdCountFamiliar）或不熟悉的位置計數器（badPwdCountUnfamiliar）重設使用者帳戶的鎖定計數器。 藉由重設計數器，"FamiliarLockout" 或 "UnfamiliarLockout" 值將會更新，因為 reset 計數器會小於閾值。  

`Reset-ADFSAccountLockout user@contoso.com -Location Familiar`
`Reset-ADFSAccountLockout user@contoso.com -Location Unknown`

## <a name="event-logging--user-activity-information-for-ad-fs-extranet-lockout"></a>事件記錄 & AD FS 外部網路鎖定的使用者活動資訊

### <a name="connect-health"></a>Connect Health
監視使用者帳戶活動的建議方式是透過 Connect Health。 Connect Health 會針對有風險的 Ip 和不正確的密碼嘗試，產生可下載的報告。 有風險的 IP 報告中的每個專案，都會顯示超過指定閾值之失敗 AD FS 登入活動的匯總資訊。 透過可自訂的電子郵件設定，您可以將電子郵件通知設定為警示系統管理員。 如需其他資訊和設定指示，請流覽[Connect Health 檔](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-adfs)。

### <a name="ad-fs-extranet-smart-lockout-events"></a>AD FS 外部網路智慧鎖定事件。
針對要寫入的外部網路智慧鎖定事件，必須在 [僅限記錄] 或 [強制] 模式中啟用 ESL，並啟用 ADFS 安全性審核。
AD FS 會將外部網路鎖定事件寫入至安全性審核記錄：
- 當使用者遭到鎖定時（達到嘗試登入失敗的鎖定閾值）
- 當 AD FS 收到已處於鎖定狀態之使用者的登入嘗試時

在 [僅記錄模式] 中，您可以檢查安全性 audit 記錄中的鎖定事件。 針對找到的任何事件，您可以使用 ADFSAccountActivity 指令程式來檢查使用者狀態，以判斷鎖定是否來自熟悉或不熟悉的 IP 位址，以及是否要再次檢查該使用者熟悉的 IP 位址清單。


|事件識別碼|描述|
|-----|-----|
|1203|這個事件會針對每個不正確的密碼嘗試而撰寫。 一旦 badPwdCount 到達 ExtranetLockoutThreshold 中指定的值，就會在 ADFS 中針對 ExtranetObservationWindow 中指定的持續時間鎖定帳戶。</br>活動識別碼：% 1</br>XML：% 2|
|1201|每次鎖定使用者時，就會寫入這個事件。 </br>活動識別碼：% 1</br>XML：% 2|
|557（ADFS 2019）| 嘗試與節點% 1 上的帳戶存放區 rest 服務通訊時發生錯誤。 如果這是 WID 伺服器陣列，主要節點可能已離線。 如果這是 SQL 伺服器陣列 ADFS，則會自動選取新的節點來裝載使用者存放區主機角色。|
|562（ADFS 2019）|使用伺服器% 1 上的帳戶存放區端點 communcating 時，發生錯誤。</br>例外狀況訊息：% 2|
|563（ADFS 2019）|計算外部網路鎖定狀態時發生錯誤。 由於% 1 設定會允許此使用者進行驗證，而權杖發行將會繼續。 如果這是 WID 伺服器陣列，主要節點可能已離線。 如果這是 SQL 伺服器陣列 ADFS，則會自動選取新的節點來裝載使用者存放區主機角色。</br>帳戶存放區伺服器名稱：% 2</br>使用者識別碼：% 3</br>例外狀況訊息：% 4|
|512|下列使用者的帳戶已被鎖定。因為系統組態，所以允許登入嘗試。</br>活動識別碼：% 1 </br>使用者：% 2 </br>用戶端 IP：% 3 </br>錯誤的密碼計數：% 4  </br>上次錯誤密碼嘗試：% 5|
|515|下列使用者帳戶處於已鎖定狀態，而且只提供正確的密碼。 此帳戶可能遭到入侵。</br>其他資料 </br>活動識別碼：% 1 </br>使用者：% 2 </br>用戶端 IP：% 3 |
|516|下列使用者帳戶已被鎖定，因為密碼嘗試錯誤過多。</br>活動識別碼：% 1  </br>使用者：% 2  </br>用戶端 IP：% 3  </br>錯誤的密碼計數：% 4  </br>上次錯誤密碼嘗試：% 5|

## <a name="esl-frequently-asked-questions"></a>ESL 常見問題

**在強制模式中使用外部網路智慧鎖定的 ADFS 伺服器陣列，是否會看到惡意的使用者鎖定？** 

答：如果 ADFS 智慧型鎖定設定為「強制」模式，您將永遠不會看到合法使用者的帳戶遭到暴力密碼破解或阻斷服務鎖定。 惡意帳戶鎖定的唯一方式，就是如果錯誤的執行者具有使用者密碼，或可以從該使用者的已知良好（熟悉的） IP 位址傳送要求。 

**啟用 ESL 會發生什麼情況，而不良執行者會有使用者密碼？** 

答：暴力密碼破解攻擊案例的目標，是要猜到一個密碼並成功登入。  如果使用者誘騙或密碼被猜測，ESL 功能就不會封鎖存取，因為登入會符合正確密碼的「成功」準則加上新的 IP。 錯誤的執行者 IP 會以「熟悉」的方式顯示。 在此案例中，最好的緩和措施是清除使用者在 ADFS 中的活動，並要求使用者進行多重要素驗證。 強烈建議您安裝 AAD 密碼保護，以確保猜到密碼不會進入系統中。

**如果我的使用者從未從 IP 成功登入，然後嘗試使用錯誤的密碼幾次，他們就能在最後正確輸入密碼之後，就能夠登入了嗎？** 

答：如果使用者提交多個不正確的密碼（也就是合法的錯誤輸入），而且在下列嘗試時，密碼會正確無誤，使用者就會立即成功登入。  這會清除不正確的密碼計數，並將該 IP 新增至 FamiliarIPs 清單。  不過，如果它們超過未知位置的失敗登入閾值，則會進入鎖定狀態，而且需要等候觀察時間範圍，並使用有效的密碼登入，或要求系統管理員介入以重設其帳戶。  
 
**ESL 也能在內部網路上運作嗎？**

答：如果用戶端直接連線到 ADFS 伺服器，而不是透過 Web 應用程式 Proxy 伺服器連線，則 ESL 行為將不適用。  

**我在 [用戶端 IP] 欄位中看到 Microsoft IP 位址。ESL block 會 EXO proxy 的暴力密碼破解攻擊嗎？**  

答：ESL 將能妥善預防 Exchange Online 或其他舊版驗證暴力密碼破解攻擊案例。 舊版驗證的「活動識別碼」為00000000-0000-0000-0000-000000000000。 在這些攻擊中，不良的執行者會利用 Exchange Online 基本驗證（也稱為舊版驗證），讓用戶端 IP 位址顯示為 Microsoft 帳戶。 雲端 proxy 中的 Exchange online 伺服器代表 Outlook 用戶端進行驗證驗證。 在這些情況下，惡意提交者的 IP 位址將會在 x 毫秒轉送的用戶端 ip 中，而 Microsoft Exchange Online server IP 則會在 [x-ms-用戶端-ip] 值中。
外部網路智慧鎖定會檢查網路 Ip、轉送的 Ip、x 轉送的用戶端 IP 和 x 毫秒-用戶端 ip 值。 如果要求成功，則所有 Ip 都會新增至熟悉的清單。 如果要求傳入，而任何顯示的 Ip 不在熟悉的清單中，則要求將會標示為不熟悉。 熟悉的使用者將能夠順利登入，而來自不熟悉位置的要求將會遭到封鎖。  

\* * 問：我可以在啟用 ESL 之前，先估計 ADFSArtifactStore 的大小嗎？

答：啟用 ESL 時，AD FS 會追蹤 ADFSArtifactStore 資料庫中使用者的帳戶活動和已知位置。 這個資料庫的大小會相對於所追蹤的使用者數目和已知位置而調整。 規劃啟用 ESL 時，您可以估計 ADFSArtifactStore 資料庫的大小，以每100000位使用者最多1GB 的速率成長。 如果 AD FS 伺服器陣列使用 Windows 內部資料庫（WID），則資料庫檔案的預設位置是 C:\Windows\WID\Data\。 若要避免填滿此磁片磁碟機，請在啟用 ESL 之前，先確定您有至少5GB 的可用儲存空間。 除了磁片儲存體之外，請在啟用 ESL 之後，針對500000或更少的使用者人口數增加1GB 的 RAM，規劃總處理常式記憶體的成長。


## <a name="additional-references"></a>其他參考資料  
[保護 Active Directory 同盟服務的最佳做法](../../ad-fs/deployment/best-practices-securing-ad-fs.md)

[設定-Set-adfsproperties](https://technet.microsoft.com/itpro/powershell/windows/adfs/set-adfsproperties)

[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)
