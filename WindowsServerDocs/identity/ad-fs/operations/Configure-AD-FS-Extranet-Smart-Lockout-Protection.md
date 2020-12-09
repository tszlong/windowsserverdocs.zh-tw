---
ms.assetid: 777aab65-c9c7-4dc9-a807-9ab73fac87b8
title: 設定 AD FS 外部網路智慧鎖定保護
author: billmath
ms.author: billmath
manager: mtilman
ms.date: 05/20/2019
ms.topic: article
ms.openlocfilehash: a5ce41597c7cb25202be61f47c640d3f749568b4
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96864537"
---
# <a name="ad-fs-extranet-lockout-and-extranet-smart-lockout"></a>AD FS 外部網路鎖定和外部網路智慧鎖定

## <a name="overview"></a>概觀

外部網路智慧鎖定 (ESL) 可保護使用者免于遭受惡意活動的外部網路帳戶鎖定。

ESL 可讓 AD FS 區分從熟悉的位置登入嘗試與使用者的登入嘗試，以及來自可能是攻擊者的登入嘗試。 AD FS 可以鎖定攻擊者，同時讓有效的使用者繼續使用他們的帳戶。 這可防止和防止使用者的拒絕服務和特定類別的密碼噴灑攻擊。 ESL 適用于 Windows Server 2016 中的 AD FS，並內建于 Windows Server 2019 的 AD FS 中。

ESL 僅適用于透過外部網路進行 Web 應用程式 Proxy 或協力廠商 proxy 的使用者名稱和密碼驗證要求。 任何協力廠商 proxy 都必須支援使用 ADFSPIP 通訊協定來取代 Web 應用程式 Proxy，例如 [F5 BIG IP 存取原則管理員](https://devcentral.f5.com/s/articles/ad-fs-proxy-replacement-on-f5-big-ip-30191)。 請參閱協力廠商 proxy 檔，以判斷 proxy 是否支援 MS ADFSPIP 通訊協定。

## <a name="additional-features-in-ad-fs-2019"></a>AD FS 2019 中的其他功能
相較于 AD FS 2016，AD FS 2019 中的外部網路智慧鎖定新增了下列優點：
- 針對熟悉且不熟悉的位置設定獨立的鎖定臨界值，讓已知良好位置的使用者在發生錯誤時，可能會有比來自可疑位置的要求更多的空間
- 啟用智慧鎖定的 audit 模式，同時繼續強制執行先前的軟鎖定行為。 這可讓您瞭解使用者熟悉的位置，並且仍受到 AD FS 2012R2 提供的外部網路鎖定功能保護。

## <a name="how-it-works"></a>運作方式
### <a name="configuration-information"></a>組態資訊
當啟用 ESL 時，會建立成品資料庫（AdfsArtifactStore. AccountActivity）中的新資料表，並在 AD FS 陣列中選取一個節點做為「使用者活動」主伺服器。 在 WID 設定中，此節點一律是主要節點。 在 SQL 設定中，已選取一個節點作為使用者活動主機。

來查看選取做為使用者活動主機的節點。  (AdfsFarmInformation) 。FarmRoles

所有次要節點都會透過埠80來連線每個全新登入上的主要節點，以瞭解錯誤密碼計數的最新值和新熟悉的位置值，並在處理登入之後更新該節點。

![組態](media/configure-ad-fs-extranet-smart-lockout-protection/esl1.png)

 如果次要節點無法連絡主機，則會將錯誤事件寫入 AD FS 管理員記錄檔中。 驗證將繼續進行處理，但 AD FS 只會在本機寫入更新的狀態。 AD FS 會每10分鐘重試一次連線到主伺服器，並在有可用的主機之後切換回主要主機。

### <a name="terminology"></a>術語
- **FamiliarLocation**：在驗證要求期間，ESL 會檢查所有顯示的 ip。 這些 ip 將會是網路 IP、轉送 IP 和選擇性的 x 轉送 IP 的組合。 如果要求成功，則所有 Ip 都會新增至帳戶活動資料表做為「熟悉的 Ip」。 如果要求中有所有 Ip 出現在「熟悉的 Ip」中，則會將要求視為「熟悉的」位置。
- **Unknownlocation.xsd**：如果傳入的要求至少有一個 IP 沒有存在於現有的 "FamiliarLocation" 清單中，則會將要求視為「未知」的位置。 這是為了處理 proxy 處理案例，例如 Exchange online 的傳統驗證，其中 Exchange Online 位址可處理成功和失敗的要求。
- **badPwdCount**：代表不正確的密碼提交次數以及驗證失敗次數的值。 針對每個使用者，會針對熟悉的位置和未知的位置保留個別的計數器。
- **UnknownLockout**：如果使用者被鎖定而無法從未知的位置存取，則為每個使用者的布林值。 這個值是根據 badPwdCountUnfamiliar 和 ExtranetLockoutThreshold 值來計算。
- **ExtranetLockoutThreshold**：這個值會決定錯誤密碼嘗試次數的上限。 達到閾值時，ADFS 將會拒絕來自外部網路的要求，直到通過觀察視窗為止。
- **ExtranetObservationWindow**：這個值會決定封鎖未知位置的使用者名稱和密碼要求的持續時間。當視窗通過時，ADFS 會開始再次從不明的位置執行使用者名稱和密碼驗證。
- **ExtranetLockoutRequirePDC**：啟用時，外部網路鎖定需要 (PDC) 的主域控制站。 停用時，如果 PDC 無法使用，外部網路鎖定將會回復到另一個網域控制站。
- **ExtranetLockoutMode**：控制僅記錄與外部網路智慧鎖定的強制模式
    - **ADFSSmartLockoutLogOnly**：已啟用外部網路智慧鎖定，但 AD FS 只會寫入系統管理員和審核事件，但不會拒絕驗證要求。 這種模式的目的是要在啟用 ' ADFSSmartLockoutEnforce ' 之前，先啟用 FamiliarLocation 才能填入。
    - **ADFSSmartLockoutEnforce**：在達到閾值時封鎖不熟悉驗證要求的完整支援。

支援 IPv4 和 IPv6 位址。

### <a name="anatomy-of-a-transaction"></a>交易的剖析
- **預先驗證檢查**：在驗證要求期間，ESL 會檢查所有顯示的 ip。 這些 ip 將會是網路 IP、轉送 IP 和選擇性的 x 轉送 IP 的組合。 在 audit 記錄中，這些 Ip 會依照 x---------------------------------------------- <IpAddress>

  根據這些 Ip，ADFS 會判斷要求是否來自熟悉或不熟悉的位置，然後檢查個別的 badPwdCount 是否小於設定的臨界值限制，或者，如果上次 **失敗** 的嘗試時間超過觀察視窗時間範圍，則為。 如果上述其中一個條件為 true，則 ADFS 允許此交易進行進一步的處理和認證驗證。 如果這兩個條件都為 false，則在觀察視窗通過之前，帳戶已處於鎖定狀態。 在觀察視窗通過之後，使用者就可以嘗試進行驗證。 請注意，在2019中，ADFS 會根據 IP 位址是否符合熟悉的位置來檢查適當的閾值限制。
- **成功登** 入：如果登入成功，則會將來自要求的 ip 新增至使用者的熟悉位置 IP 清單。
- **登入失敗**：如果登入失敗，badPwdCount 會增加。 如果攻擊者傳送更多錯誤的密碼給系統，而不是閾值允許的，則使用者會進入鎖定狀態。  (badPwdCount > ExtranetLockoutThreshold) 

![組態](media/configure-ad-fs-extranet-smart-lockout-protection/esl2.png)

當帳戶被鎖定時，"UnknownLockout" 值會等於 true。這表示使用者的 badPwdCount 超過閾值，也就是有人嘗試的密碼比系統所允許的還要多。 在此狀態下，有效的使用者可以使用2種方式登入。
- 使用者必須等待 ObservationWindow 時間經過，或
- 若要重設鎖定狀態，請使用 ' Reset-ADFSAccountLockout ' 將 badPwdCount 重設回零。

如果未進行重設，則會允許帳戶針對每個觀察視窗進行 AD 單一密碼嘗試。 帳戶會在該嘗試之後返回鎖定的狀態，而觀察視窗將會重新開機。 BadPwdCount 值只會在密碼登入成功後自動重設。

### <a name="log-only-mode-versus-enforce-mode"></a>Log-Only 模式與 ' 強制 ' 模式比較
AccountActivity 資料表會在「僅限記錄」模式和「強制執行」模式中填入。 如果略過「僅限記錄」模式，且 ESL 直接移至「強制執行」模式而不建議等待期間，則 ADFS 將不知道使用者熟悉的 Ip。 在此情況下，ESL 的行為類似 ' ADBadPasswordCounter '，如果使用者帳戶在主動暴力密碼破解攻擊下，可能會封鎖合法的使用者流量。 如果略過「僅限記錄」模式，且使用者以 "UnknownLockout" = TRUE 進入鎖定的狀態，並嘗試使用不在「熟悉的」 IP 清單中的 IP 登入，則他們將無法登入。 建議使用 Log-Only 模式3-7 天，以避免發生這種情況。 如果帳戶主動遭受攻擊，則至少需要24小時的「僅限記錄」模式，以防止合法使用者鎖定。

## <a name="extranet-smart-lockout-configuration"></a>外部網路智慧鎖定設定

### <a name="prerequisites-for-ad-fs-2016"></a>AD FS 2016 的必要條件

1. **在伺服器陣列中的所有節點上安裝更新**

   首先，請確定所有 Windows Server 2016 AD FS 伺服器都是最新狀態，從年 6 2018 月的 Windows 更新開始，而且 AD FS 2016 伺服器陣列是以2016伺服器陣列行為層級執行。
1. **驗證許可權**

   外部網路智慧鎖定需要在每一部 AD FS 伺服器上啟用 Windows 遠端系統管理。
3. **更新成品資料庫許可權**

   外部網路智慧鎖定要求 AD FS 服務帳戶必須具有在 AD FS 成品資料庫中建立新資料表的許可權。 以 AD FS 系統管理員身分登入任何 AD FS 伺服器，然後在 PowerShell 命令提示字元視窗中執行下列命令，以授與此許可權：

   ``` powershell
   PS C:\>$cred = Get-Credential
   PS C:\>Update-AdfsArtifactDatabasePermission -Credential $cred
   ```
   >[!NOTE]
   >$Cred 預留位置是具有 AD FS 系統管理員許可權的帳戶。 這應該會提供用來建立資料表的寫入權限。

   上述命令可能因為缺乏足夠的許可權而失敗，因為您的 AD FS 伺服器陣列使用 SQL Server，而且上述提供的認證沒有您 SQL Server 的系統管理員許可權。 在此情況下，您可以在連接到 AdfsArtifactStore 資料庫時，執行下列命令，以手動方式在 SQL Server 資料庫中設定資料庫許可權。
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
這項功能會使用安全性審核記錄，因此必須在 AD FS 中啟用審核，並在所有 AD FS 伺服器上啟用本機原則。

### <a name="configuration-instructions"></a>組態指示
外部網路智慧鎖定使用 ADFS 屬性 **ExtranetLockoutEnabled**。 這個屬性之前用來控制伺服器2012R2 中的「外部網路軟鎖定」。 如果已啟用外部網路軟鎖定，若要查看目前的屬性設定，請執行 ` Get-AdfsProperties` 。

### <a name="configuration-recommendations"></a>組態建議
設定外部網路智慧鎖定時，請遵循設定閾值的最佳作法：

`ExtranetObservationWindow (new-timespan -Minutes 30)`

`ExtranetLockoutThreshold: Half of AD Threshold Value`

AD 值：20、ExtranetLockoutThreshold：10

Active Directory 鎖定可獨立于外部網路智慧鎖定運作。 不過，如果已啟用 Active Directory 鎖定，ExtranetLockoutThreshold 會在 AD 中 AD FS < 帳戶鎖定閾值

`ExtranetLockoutRequirePDC - $false`

啟用時，外部網路鎖定需要 (PDC) 的主域控制站。 當停用並設定為 false 時，如果 PDC 無法使用，外部網路鎖定將會回復到另一個網域控制站。

若要設定此屬性，請執行：

``` powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30) -ExtranetLockoutRequirePDC $false
```
### <a name="enable-log-only-mode"></a>啟用 Log-Only 模式

在僅限記錄模式中，AD FS 會填入使用者熟悉的位置資訊，並寫入安全性審核事件，但不會封鎖任何要求。 這個模式是用來驗證智慧鎖定是否正在執行，並可讓 AD FS 在啟用「強制」模式之前，「瞭解」使用者熟悉的位置。 當 AD FS 學習時，它會將每位使用者的登入活動儲存 (是否為僅限記錄模式或強制執行模式) 。
藉由執行下列 commandlet，將鎖定行為設定為僅記錄。

`Set-AdfsProperties -ExtranetLockoutMode AdfsSmartlockoutLogOnly`

[僅限記錄] 模式的目的是暫時的狀態，讓系統可以在使用智慧鎖定行為來導入鎖定強制之前，先瞭解登入行為。 僅限記錄模式的建議持續時間為3-7 天。 如果帳戶主動遭受攻擊，則只需執行最少24小時的記錄模式。

在 AD FS 2016 上，如果在啟用外部網路智慧鎖定之前啟用2012R2 「外部網路軟鎖定」行為，Log-Only 模式將會停用「外部網路軟鎖定」行為。 AD FS 智慧鎖定不會在 Log-Only 模式中鎖定使用者。 不過，內部部署 AD 可能會根據 AD 設定鎖定使用者。 請參閱 AD 鎖定原則，以瞭解內部部署 AD 如何鎖定使用者。

在 AD FS 2019 上，另一個優點是能夠啟用僅限記錄模式的智慧型鎖定，同時繼續使用下列 Powershell 來強制執行先前的軟鎖定行為。

`Set-AdfsProperties -ExtranetLockoutMode 3`

若要讓新的模式生效，請在伺服器陣列中的所有節點上重新開機 AD FS 服務

`Restart-service adfssrv`

設定模式之後，您可以使用 EnableExtranetLockout 參數啟用智慧鎖定

`Set-AdfsProperties -EnableExtranetLockout $true`

### <a name="enable-enforce-mode"></a>啟用強制模式

在您熟悉鎖定閾值和觀察視窗之後，可以使用下列 PSH Cmdlet 將 ESL 移至「強制」模式：

`Set-AdfsProperties -ExtranetLockoutMode AdfsSmartLockoutEnforce`

若要讓新的模式生效，請使用下列命令，在伺服器陣列中的所有節點上重新開機 AD FS 服務。

`Restart-service adfssrv`

## <a name="manage-user-account-activity"></a>管理使用者帳戶活動
AD FS 提供三個 Cmdlet 來管理帳戶活動資料。 這些 Cmdlet 會自動連接到伺服器陣列中保存主要角色的節點。
>[!NOTE]
>只有足夠的系統管理 (JEA) 可以用來委派 AD FS 的 commandlet，以重設帳戶鎖定。 例如，技術支援人員可以委派許可權，以使用 ESL commandlet。 如需委派使用這些 Cmdlet 之許可權的相關資訊，請參閱 [委派 AD FS 非系統管理員使用者的 Powershell Commandlet 存取權](delegate-ad-fs-pshell-access.md)

您可以藉由傳遞-Server 參數來覆寫此行為。

- Get-ADFSAccountActivity-UserPrincipalName

  讀取使用者帳戶的目前帳戶活動。 此 Cmdlet 一律會使用帳戶活動 REST 端點，自動連接到伺服器陣列主機。 因此，所有資料都應該保持一致。

`Get-ADFSAccountActivity user@contoso.com`

  內容：
    - BadPwdCountFamiliar：從已知位置成功驗證時遞增。
    - BadPwdCountUnknown：從未知的位置驗證失敗時遞增
    - LastFailedAuthFamiliar：如果從熟悉的位置驗證失敗，LastFailedAuthUnknown 會設定為驗證失敗的時間
    - LastFailedAuthUnknown：如果驗證從未知的位置失敗，LastFailedAuthUnknown 會設定為驗證失敗的時間
    - FamiliarLockout：布林值，如果 "BadPwdCountFamiliar" > ExtranetLockoutThreshold，則會是 "True"
    - UnknownLockout：布林值，如果 "BadPwdCountUnknown" > ExtranetLockoutThreshold，則會是 "True"
    - FamiliarIPs：最多20個對使用者很熟悉的 Ip。 當超過此限制時，將會移除清單中最舊的 IP。
-    Set-ADFSAccountActivity

     新增熟悉的位置。 熟悉的 IP 清單最多可有20個專案，如果超過此值，則會移除清單中最舊的 IP。

`Set-ADFSAccountActivity user@contoso.com -AdditionalFamiliarIps “1.2.3.4”`

- Reset-ADFSAccountLockout

  針對每個熟悉的位置重設使用者帳戶的鎖定計數器 (badPwdCountFamiliar) 或不熟悉的位置計數器 (badPwdCountUnfamiliar) 。 藉由重設計數器，"FamiliarLockout" 或 "UnfamiliarLockout" 值將會更新，因為重設計數器將會小於臨界值。

`Reset-ADFSAccountLockout user@contoso.com -Location Familiar`
`Reset-ADFSAccountLockout user@contoso.com -Location Unknown`

## <a name="event-logging--user-activity-information-for-ad-fs-extranet-lockout"></a>事件記錄 & AD FS 外部網路鎖定的使用者活動資訊

### <a name="connect-health"></a>Connect Health
監視使用者帳戶活動的建議方式是透過 Connect Health。 Connect Health 會產生具風險 Ip 的可下載報告和錯誤的密碼嘗試。 「具風險的 IP 報告」中的每個項目會顯示有關已超過指定閾值之失敗 AD FS 登入活動的彙總資訊。 您可以設定電子郵件通知，在這種情況下，使用可自訂的電子郵件設定來警示系統管理員。 如需其他資訊和安裝指示，請造訪 [Connect Health 檔](/azure/active-directory/hybrid/how-to-connect-health-adfs)。

### <a name="ad-fs-extranet-smart-lockout-events"></a>AD FS 外部網路智慧鎖定事件。

>[!NOTE]
> 使用[AD FS 協助外部網路鎖定疑難排解指南來](https://adfshelp.microsoft.com/TroubleshootingGuides/Workflow/a73d5843-9939-4c03-80a1-adcbbf3ccec8)疑難排解外部網路智慧鎖定

針對要寫入的外部網路智慧鎖定事件，必須在 [僅限記錄] 或 [強制執行] 模式中啟用 ESL，並啟用 ADFS 安全性審核。
AD FS 會將外部網路鎖定事件寫入安全性審核記錄：
- 當使用者被鎖定時 (會達到失敗登入嘗試失敗的鎖定臨界值) 
- 當 AD FS 收到已處於鎖定狀態之使用者的登入嘗試時

在 [僅限記錄] 模式中，您可以檢查安全性審核記錄檔中的鎖定事件。 針對找到的任何事件，您可以使用 Get-ADFSAccountActivity Cmdlet 來檢查使用者狀態，以判斷是否從熟悉或不熟悉的 IP 位址進行鎖定，並再次檢查該使用者熟悉的 IP 位址清單。


|事件識別碼|描述|
|-----|-----|
|1203|此事件是針對每個錯誤密碼嘗試所撰寫。 一旦 badPwdCount 達到 ExtranetLockoutThreshold 中指定的值，系統就會在 ADFS 中于 ExtranetObservationWindow 指定的期間內鎖定帳戶。</br>活動識別碼： %1</br>XML： %2|
|1201|每次使用者被鎖定時，就會寫入此事件。 </br>活動識別碼： %1</br>XML： %2|
|557 (ADFS 2019) | 嘗試與節點 %1 上的帳戶存放區 rest 服務通訊時發生錯誤。 如果這是 WID 伺服器陣列，主要節點可能會離線。 如果這是 SQL 伺服器陣列 ADFS，則會自動選取新的節點來裝載使用者存放區主機角色。|
|562 (ADFS 2019) |與伺服器 %1 上的帳戶存放區端點 communcating 時發生錯誤。</br>例外狀況訊息： %2|
|563 (ADFS 2019) |計算外部網路鎖定狀態時發生錯誤。 由於 %1 的值將允許此使用者進行驗證，因此權杖發行將會繼續。 如果這是 WID 伺服器陣列，主要節點可能會離線。 如果這是 SQL 伺服器陣列 ADFS，則會自動選取新的節點來裝載使用者存放區主機角色。</br>帳戶存放區伺服器名稱： %2</br>使用者識別碼： %3</br>例外狀況訊息： %4|
|512|下列使用者的帳戶已被鎖定。因為系統組態，所以允許登入嘗試。</br>活動識別碼： %1 </br>使用者： %2 </br>用戶端 IP： %3 </br>錯誤的密碼計數： %4  </br>上次錯誤密碼嘗試： %5|
|515|下列使用者帳戶處於鎖定狀態，且只提供正確的密碼。 此帳戶可能會遭到盜用。</br>其他資料 </br>活動識別碼： %1 </br>使用者： %2 </br>用戶端 IP： %3 |
|516|因為嘗試的密碼太多次，所以已鎖定下列使用者帳戶。</br>活動識別碼： %1  </br>使用者： %2  </br>用戶端 IP： %3  </br>錯誤的密碼計數： %4  </br>上次錯誤密碼嘗試： %5|

## <a name="esl-frequently-asked-questions"></a>ESL 常見問題

**在強制執行模式中使用外部網路智慧鎖定的 ADFS 伺服器陣列是否看到惡意的使用者鎖定？** 

答：如果 ADFS 智慧鎖定設定為「強制」模式，則您永遠不會看到合法使用者的帳戶遭到暴力密碼破解或阻絕服務鎖定。 惡意帳戶鎖定可以防止使用者登入的唯一方法是，如果不正確的動作專案具有使用者密碼，或可從已知良好的 (熟悉的) IP 位址，傳送要求給該使用者。 

**ESL 已啟用，而不良執行者有使用者密碼時，會發生什麼事？** 

答：暴力密碼破解攻擊案例的典型目標是猜測密碼並成功登入。如果使用者是誘騙或猜測密碼，ESL 功能將不會封鎖存取，因為登入將符合正確密碼的「成功」準則加上新的 IP。 錯誤的動作專案 IP 接著會顯示為「熟悉的」。在此案例中，最大的緩和措施是在 ADFS 中清除使用者的活動，並要求使用者進行多重要素驗證。 強烈建議您安裝 AAD 密碼保護，以確保猜到的密碼不會進入系統中。

**如果我的使用者從未從 IP 成功登入，然後嘗試使用錯誤的密碼，就可以在最後一次正確地輸入密碼時登入？** 

答：如果使用者送出多個不正確的密碼 (也就是合法的錯誤輸入) ，而在下列嘗試會得到正確的密碼，則使用者會立即成功登入。 這會清除錯誤的密碼計數，並將該 IP 新增至 FamiliarIPs 清單。但是，如果他們超過未知位置的失敗登入閾值，則會進入鎖定狀態，而且需要等候觀察視窗，然後以有效的密碼登入，或需要系統管理員介入才能重設其帳戶。

**ESL 也可以在內部網路上運作嗎？**

答：如果用戶端直接連接到 ADFS 伺服器，而不是透過 Web 應用程式 Proxy 伺服器，則不會套用 ESL 行為。 

**我在 [用戶端 IP] 欄位中看到 Microsoft IP 位址。ESL 封鎖 EXO proxy 的暴力密碼破解攻擊嗎？**  

答： ESL 適用于防止 Exchange Online 或其他舊版驗證暴力密碼破解攻擊案例。 舊版驗證的「活動識別碼」為00000000-0000-0000-0000-000000000000。在這些攻擊中，不良執行者利用 Exchange Online 基本驗證 (也稱為舊版驗證) ，因此用戶端 IP 位址會顯示為 Microsoft。 雲端 proxy 中的 Exchange online 伺服器代表 Outlook 用戶端進行驗證驗證。 在這些案例中，惡意提交者的 IP 位址將會是以 x 為單位的---------------------------------------
外部網路智慧鎖定會檢查網路 Ip、轉送的 Ip、x 轉送的用戶端 IP，以及 x-ms-用戶端 ip 值。 如果要求成功，則所有 Ip 都會新增至熟悉的清單。 如果提出要求，且任何顯示的 Ip 都不在熟悉的清單中，則會將要求標示為不熟悉。 熟悉的使用者將能夠順利登入，而來自不熟悉位置的要求將會遭到封鎖。

**我可以在啟用 ESL 之前，先估計 ADFSArtifactStore 的大小嗎？**

答：啟用 ESL 之後，AD FS 會在 ADFSArtifactStore 資料庫中追蹤使用者的帳戶活動和已知位置。 此資料庫的大小會隨著所追蹤的使用者數目和已知位置數目而相對調整。 規劃要啟用 ESL 時，您可以估計 ADFSArtifactStore 資料庫的大小，以每 10 萬個使用者最多 1GB 的速率成長。 如果 AD FS 伺服器陣列使用 Windows 內部資料庫 (WID) ，則資料庫檔案的預設位置為 C:\Windows\WID\Data\。 若要避免填滿此磁碟機，請在啟用 ESL 之前，先確定您至少有 5GB 的可用儲存體。 除了磁碟儲存體以外，請在啟用 ESL 之後，針對總處理序記憶體進行規劃，50 萬或更少使用者人口數最多可成長額外 1GB 的 RAM。


## <a name="additional-references"></a>其他參考資料
[保護 Active Directory 同盟服務的最佳作法](../../ad-fs/deployment/best-practices-securing-ad-fs.md)

[設定-Set-adfsproperties](/powershell/module/adfs/set-adfsproperties)

[AD FS 操作](../ad-fs-operations.md)
