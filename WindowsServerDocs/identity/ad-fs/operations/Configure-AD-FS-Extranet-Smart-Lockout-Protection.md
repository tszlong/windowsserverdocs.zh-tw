---
ms.assetid: 777aab65-c9c7-4dc9-a807-9ab73fac87b8
title: 設定 AD FS 外部鎖定保護
description: ''
author: billmath
ms.author: billmath
manager: mtilman
ms.date: 05/20/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7a4ad8c0199f0f62d7cd69a43897cb4608ddb365
ms.sourcegitcommit: ccc802338b163abdad2e53b55f39addcfea04603
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/05/2019
ms.locfileid: "66687366"
---
# <a name="ad-fs-extranet-lockout-and-extranet-smart-lockout"></a>AD FS 外部網路鎖定和外部網路的智慧鎖定

## <a name="overview"></a>總覽

外部網路的智慧鎖定 (ESL) 會防止您的使用者發生不被惡意活動的外部網路帳戶鎖定。  

ESL 可讓您區分登入嘗試從熟悉的位置，為使用者與登入嘗試從可能的攻擊者的 AD FS。 AD FS 可以鎖定攻擊者，同時可讓繼續使用他們的帳戶的有效使用者。 這可防止，並防止阻絕服務以及對使用者的密碼噴灑攻擊的特定類別。 ESL 適用於 Windows Server 2016 中的 AD FS，並內建於 Windows Server 2019 中的 AD FS。

ESL 僅適用於使用者名稱和密碼驗證要求都透過外部網路，透過 Web Application Proxy 或第 3 方 proxy。 第 3 的任何合作對象 proxy 必須支援 MS ADFSPIP 通訊協定，例如用來取代 Web 應用程式 Proxy [F5 BIG-IP Access Policy Manager](https://devcentral.f5.com/s/articles/ad-fs-proxy-replacement-on-f5-big-ip-30191)。 請參閱第 3 個合作對象 proxy 文件，以判斷 proxy 是否支援 MS ADFSPIP 通訊協定。   

## <a name="additional-features-in-ad-fs-2019"></a>在 AD FS 2019 的其他功能
外部網路 ad FS 2019 的智慧鎖定會加入相較於 AD FS 2016 的優點如下：
- 設定獨立的鎖定閾值熟悉且不熟悉的位置，讓已知的良好地點的使用者可以從可疑位置有更多的空間比要求時發生錯誤
- 啟用稽核模式中的智慧鎖定，同時繼續強制執行先前的軟式鎖定行為。 這可讓您深入了解使用者熟悉的位置，並仍然會受到所提供的 AD FS 2012 r2 的外部網路鎖定功能。  

## <a name="how-it-works"></a>它的運作方式
### <a name="configuration-information"></a>組態資訊
啟用 ESL 時，會建立新資料庫資料表中的成品，AdfsArtifactStore.AccountActivity，並為 「 使用者活動 」 主要 AD FS 伺服器陣列中選取節點。 在 WID 組態中，此節點一律是主要節點。 在 SQL 組態中，使用者活動主要選取一個節點。  

若要檢視選取作為使用者活動的主要節點。 Get-AdfsFarmInformation.FarmRoles

所有的次要節點會連絡主要節點透過連接埠 80，若要了解最新的值不正確密碼計數的和新的熟悉的位置值，每個新的登入，並在處理登入之後，更新該節點。

![組態](media/configure-ad-fs-extranet-smart-lockout-protection/esl1.png)

 如果第二個節點無法連絡主機，它將錯誤事件寫入 AD FS 系統管理記錄檔。 驗證將會繼續進行處理，但是 AD FS 將只會寫入本機更新的狀態。 AD FS 會重試每隔 10 分鐘連絡主要和主要可用之後，將會切換回 master。

### <a name="terminology"></a>詞彙
- **FamiliarLocation**:驗證要求，進行 ESL 會檢查所有顯示的 Ip。 這些 Ip 會組成網路的 IP，轉送 IP 和選擇性 x 轉送的 IP。 如果要求成功，所有的 Ip 會新增至帳戶活動資料表為 「 熟悉 Ip 」。 如果要求有這些 「 熟悉的 Ip 」 中的所有 Ip，要求會視為 「 熟悉的 」 的位置。
- **UnknownLocation**:如果傳入的要求有現有"FamiliarLocation 」 清單中沒有至少一個 IP，然後會將要求視為做為 「 不明 」 的位置。 這是為了處理 proxy 處理的案例，例如 Exchange Online 的舊版驗證 Exchange Online 的位址處理成功和失敗要求的位置。  
- **badPwdCount**:值，表示已提交錯誤密碼的次數和驗證成功。 每位使用者，為分開的計數器會保留熟悉的位置和未知的位置。
- **UnknownLockout**:布林值，每位使用者，如果使用者從未知位置存取，已遭鎖定。 這個值會根據計算的 badPwdCountUnfamiliar 和 ExtranetLockoutThreshold 值。
- **ExtranetLockoutThreshold**:這個值會決定錯誤密碼嘗試次數上限。 當達到臨界值時，ADFS 將拒絕來自外部網路的要求，直到已超過觀測視窗。
- **ExtranetObservationWindow**:這個值會決定從未知位置的使用者名稱和密碼的要求都被鎖定的持續時間。當期間過後時，ADFS 會開始從未知位置中再次執行使用者名稱和密碼驗證。
- **ExtranetLockoutRequirePDC**:啟用時，外部網路鎖定要求的主要網域控制站 (PDC)。 停用時，外部網路鎖定，以防 PDC 無法使用，就會將恢復為另一個網域控制站。  
- **ExtranetLockoutMode**:控制項記錄只與強制執行的外部網路的智慧鎖定的模式
    - **ADFSSmartLockoutLogOnly**:已啟用外部網路的智慧鎖定，但 AD FS 只會寫入系統管理員和稽核事件，但將會拒絕驗證要求。 此模式被要啟用 'ADFSSmartLockoutEnforce' 之前填入 FamiliarLocation 最初啟用。
    - **ADFSSmartLockoutEnforce**:當達到臨界值時，封鎖不熟悉的驗證要求的完整支援。

支援 IPv4 和 IPv6 位址。

### <a name="anatomy-of-a-transaction"></a>交易的結構
- **預先驗證核取**:驗證要求，進行 ESL 會檢查所有顯示的 Ip。 這些 Ip 會組成網路的 IP，轉送 IP 和選擇性 x 轉送的 IP。 在稽核記錄檔中，這些 Ip 會列在<IpAddress>欄位順序 x-ms-轉送-用戶端-ip，x-轉送-，x-ms-proxy-用戶端-ip。

  根據這些 Ip，ADFS 會決定如果要求是來自熟悉或不熟悉的位置，然後檢查 是否個別 badPwdCount 小於設定的閾值限制或上次**失敗**在發生嘗試超過觀察視窗時間範圍。 如果其中一個條件為 true，ADFS 允許進一步處理這個交易和認證驗證。 如果兩個條件為 false，此帳戶已處於鎖定狀態觀測視窗傳遞。 觀測視窗之後，使用者可以嘗試一次驗證。 請注意，在 2019，ADFS 會檢查相對於根據適當的臨界值限制的 IP 位址是否符合熟悉的位置，或不。
- **成功登入**:如果登入成功，請從要求的 Ip 會加入使用者熟悉的位置 IP 清單。  
- **登入失敗**:如果登入失敗 badPwdCount 會增加。 如果攻擊者會傳送多個不正確密碼系統超過閾值允許使用者將會進入鎖定狀態。 (badPwdCount > ExtranetLockoutThreshold)  

![組態](media/configure-ad-fs-extranet-smart-lockout-protection/esl2.png)

帳戶鎖定時，"UnknownLockout 」 值會等於 true。這表示使用者的 badPwdCount 透過臨界值也就是有人嘗試超過所允許的系統更多的密碼。 在此狀態下，有 2 種方式可以是有效的使用者可以登入。
- 使用者必須等候 ObservationWindow 時間，以經過或
- 若要重設的鎖定狀態，重設回零與 ' 重設 ADFSAccountLockout' 的 badPwdCount。

如果不發生任何重設，帳戶將可針對每一個觀測視窗 AD 的單一密碼嘗試。 此帳戶將會傳回為鎖定狀態之後，嘗試與觀測視窗會重新啟動。 BadPwdCount 值將只有自動重設密碼成功登入之後。

### <a name="log-only-mode-versus-enforce-mode"></a>僅限記錄檔的模式，與 '強制' 模式
同時在 ' 記錄檔-僅限 」 的模式和 '強制' 模式時，會填入 AccountActivity 資料表。 如果會略過 ' 記錄檔-僅限 」 的模式和 ESL 直接變成 '強制執行' 的模式，沒有建議的等待期間，使用者熟悉 Ip 並不知道至 ADFS。 在此情況下，ESL 會表現 'ADBadPasswordCounter'，可能封鎖合法的使用者流量，如果使用者帳戶受到 active 暴力密碼破解攻擊。 如果 ' 記錄檔-僅限 」 模式則會略過使用者輸入一部鎖定狀態時，使用 「 UnknownLockout"out = TRUE，而且會嘗試使用良好的密碼，從 IP 不在 「 熟悉的 」 的 IP 清單中，登入，則無法登入。 3-7 天，以避免這種情況，建議僅限記錄檔的模式。 帳戶目前正遭受攻擊，至少 24 小時的 ' 記錄檔-僅限 」 的模式需要以防止合法使用者的鎖定。  

## <a name="extranet-smart-lockout-configuration"></a>外部網路的智慧鎖定設定  

### <a name="prerequisites-for-ad-fs-2016"></a>適用於 AD FS 2016 的必要條件

1. **伺服器陣列中的所有節點上安裝更新**

   首先，請確定所有的 Windows Server 2016 AD FS 伺服器是最新的自 2018 年 6 月的 Windows 更新和 AD FS 2016 伺服器陣列 2016年伺服器陣列行為層級執行。
1. **確認權限**

   外部網路的智慧鎖定要求，每個 AD FS 伺服器上啟用 Windows 遠端管理。
3. **更新成品資料庫權限**

   外部網路的智慧鎖定會需要 AD FS 服務帳戶必須要有 AD FS 成品資料庫中建立新的資料表的權限。 登入為 AD FS 系統管理員，為任何 AD FS 伺服器，然後在 PowerShell 命令提示字元 視窗中執行下列命令，以授與此權限：

   ``` powershell
   PS C:\>$cred = Get-Credential
   PS C:\>Update-AdfsArtifactDatabasePermission -Credential $cred
   ```
   >[!NOTE]
   >$Cred 預留位置是具有 AD FS 系統管理員權限的帳戶。 這應會提供建立資料表的寫入權限。

   上述命令的失敗可能因為缺乏足夠的權限，因為您的 AD FS 伺服器陣列使用 SQL Server，並在上面提供的認證沒有您的 SQL server 的系統管理員權限。 在此情況下，您可以設定資料庫權限以手動方式在 SQL Server 資料庫中執行下列命令，當您已連線到 AdfsArtifactStore 資料庫。
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

### <a name="ensure-ad-fs-security-audit-logging-is-enabled"></a>請確定已啟用 AD FS 安全性稽核記錄
這項功能會使用安全性稽核記錄檔，因此稽核必須啟用 AD FS，以及所有的 AD FS 伺服器上的本機原則。

### <a name="configuration-instructions"></a>組態指示
外部網路的智慧鎖定會使用 ADFS 屬性**ExtranetLockoutEnabled**。 這個屬性先前已使用 Server 2012 r2 中的控制項 「 軟性外部網路鎖定 」。 如果已啟用外部網路的軟式鎖定，若要檢視目前的屬性設定，請執行` Get-AdfsProperties`。

### <a name="configuration-recommendations"></a>組態建議
當設定外部網路的智慧鎖定，請遵循最佳作法設定臨界值：  

`ExtranetObservationWindow (new-timespan -Minutes 30)`

`ExtranetLockoutThreshold: – 2x AD Threshold Value`

AD 值：20，ExtranetLockoutThreshold:10

Active Directory 鎖定獨立運作方式與外部網路智慧鎖定。 不過，如果已啟用 Active Directory 鎖定，在 AD FS ExtranetLockoutThreshold < 在 AD 中的帳戶鎖定閾值

`ExtranetLockoutRequirePDC - $false`

啟用時，外部網路鎖定要求的主要網域控制站 (PDC)。 當停用並設定為 false 時，外部網路鎖定，以防 PDC 無法使用，就會將恢復為另一個網域控制站。

若要設定執行此屬性：

``` powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30) -ExtranetLockoutRequirePDC $false
```
### <a name="enable-log-only-mode"></a>啟用僅記錄模式

記錄模式，AD FS 會填入使用者熟悉的位置資訊和寫入安全性稽核事件，但不會封鎖任何要求。 此模式用來驗證的智慧鎖定正在執行，而且若要啟用 AD FS，以 「 了解 」 之前啟用的使用者熟悉的位置 「 強制執行 」 模式。 當 AD FS 學習，它會儲存每位使用者的登入活動 (是否記錄模式或強制模式)。
設定記錄只能由執行下列 commandlet 的鎖定行為。  

`Set-AdfsProperties -ExtranetLockoutMode AdfsSmartlockoutLogOnly`

記錄檔的唯一模式被要是暫時性的狀態，以便系統可以了解之前介紹的智慧鎖定行為與鎖定強制的登入行為。 僅限記錄模式建議的持續時間為 3 到 7 天。 如果帳戶目前正遭受攻擊，僅限記錄模式必須執行至少 24 小時。

在 AD FS 2016 中，如果已啟用 2012R2 '虛外部網路鎖定' 行為，然後才啟用外部網路的智慧鎖定，則僅限記錄模式會停用 '虛外部網路鎖定' 行為。 AD FS 中的智慧鎖定會被鎖定在僅限記錄模式中的使用者。 不過，內部部署 AD 可能會鎖定 AD 設定為基礎的使用者。 請檢閱 AD 鎖定原則，以了解如何在內部部署 AD 可以鎖定使用者。

在 AD FS 2019，另一個優點是能夠啟用智慧鎖定的僅限記錄模式，同時繼續強制執行先前的軟式鎖定行為使用下方的 Powershell。

`Set-AdfsProperties -ExtranetLockoutMode 3`

新的模式，才會生效，重新啟動 AD FS 服務的所有節點上，伺服器陣列中

`Restart-service adfssrv`

將模式設定之後，您可以讓使用 EnableExtranetLockout 參數的智慧鎖定

`Set-AdfsProperties -EnableExtranetLockout $true`

### <a name="enable-enforce-mode"></a>啟用強制模式

您已經熟悉的鎖定閾值和觀測視窗之後，ESL 可以移至 「 強制執行 」 使用下列的 PSH cmdlet 的模式：

`Set-AdfsProperties -ExtranetLockoutMode AdfsSmartLockoutEnforce`

新模式中才會生效，重新啟動所有節點上的 AD FS 服務伺服陣列中使用下列命令。

`Restart-service adfssrv`

## <a name="manage-user-account-activity"></a>管理使用者帳戶活動
AD FS 提供三個 cmdlet 來管理帳戶的活動資料。 這些 cmdlet 會自動連接到擁有主要角色的伺服器陣列中的節點。
>[!NOTE]
>Just Enough Administration (JEA) 可用來委派 AD FS commandlet 以重設帳戶鎖定。 比方說，讓支援服務中心人員可以使用 ESL commandlet 的委派權限。 如需委派使用這些 cmdlet 的權限資訊，請參閱[委派 AD FS Powershell Commandlet 存取給非系統管理員使用者](delegate-ad-fs-pshell-access.md)

可以覆寫這個行為，藉由傳遞-Server 參數。

- Get-ADFSAccountActivity -UserPrincipalName

  讀取目前的帳戶活動的使用者帳戶。 此 cmdlet 一律會自動連線到伺服器陣列主要使用的帳戶活動 REST 端點。 因此，所有的資料一律應該一致。

`Get-ADFSAccountActivity user@contoso.com`

  屬性：
    - BadPwdCountFamiliar:當驗證已成功從已知位置時，就會遞增。
    - BadPwdCountUnknown:當從未知位置驗證不成功時，遞增
    - LastFailedAuthFamiliar:如果驗證成功從熟悉的位置，LastFailedAuthUnknown 設為驗證失敗的時間
    - LastFailedAuthUnknown:如果驗證成功從未知位置，LastFailedAuthUnknown 設為驗證失敗的時間
    - FamiliarLockout:布林值，該值將會是"True"，如果 「 BadPwdCountFamiliar"> ExtranetLockoutThreshold
    - UnknownLockout:布林值，該值將會是"True"，如果 「 BadPwdCountUnknown"> ExtranetLockoutThreshold  
    - FamiliarIPs： 最大值為 20 個 Ip，也就是熟悉的使用者。 超出此配額時，將會移除最舊的 IP 清單中。
-    Set-ADFSAccountActivity

     加入新熟悉的位置。 熟悉的 IP 清單中有最多 20 個項目，若超出此配額，清單中最舊的 IP 會被移除。

`Set-ADFSAccountActivity user@contoso.com -AdditionalFamiliarIps “1.2.3.4”`

- Reset-ADFSAccountLockout

  針對每個熟悉的位置 (badPwdCountFamiliar) 的使用者帳戶鎖定計數器或熟悉的位置計數器 (badPwdCountUnfamiliar) 重設。 藉由重設計數器，會更新 「 FamiliarLockout"或"UnfamiliarLockout"值，因為重設計數器會少於臨界值。  

`Reset-ADFSAccountLockout user@contoso.com -Location Familiar`
`Reset-ADFSAccountLockout user@contoso.com -Location Unknown`

## <a name="event-logging--user-activity-information-for-ad-fs-extranet-lockout"></a>事件記錄與 AD FS 外部網路鎖定的使用者活動資訊

### <a name="connect-health"></a>Connect Health
若要監視的使用者帳戶活動的建議的方式是透過 Connect Health。 連線健全狀況，會產生 「 具風險 Ip 的可下載報告，並不正確密碼嘗試。 具風險的 IP 報告中的每個項目會顯示 AD FS 登入活動失敗超過指定臨界值的彙總的資訊。 使用可自訂的電子郵件設定發生此狀況時，可以設定電子郵件通知系統管理員發出警示。 如需詳細資訊和安裝指示，請瀏覽[Connect Health 文件](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-health-adfs)。

### <a name="ad-fs-extranet-smart-lockout-events"></a>AD FS 外部網路的智慧鎖定事件。
針對外部網路的智慧鎖定事件可供寫入，必須在 '記錄檔僅' 強制執行' 的模式下啟用 ESL 和 ADFS 安全性稽核已啟用。
AD FS 會將外部網路鎖定事件寫入安全性稽核記錄檔：
- 當使用者鎖定時外 （如登入失敗嘗試，達到鎖定閾值）
- 當 AD FS 收到已處於鎖定狀態之使用者的登入嘗試

在 記錄檔的唯一模式，您可以檢查鎖定事件的安全性稽核記錄檔。 針對找不到任何事件，您可以檢查使用 Get ADFSAccountActivity cmdlet 來判斷發生鎖定熟悉或不熟悉 IP 位址，以及詳細的檢查清單中的熟悉的 IP 位址，該使用者的使用者狀態。


|事件識別碼|描述|
|-----|-----|
|1203|此事件會寫入每個錯誤密碼嘗試。 BadPwdCount 達到 ExtranetLockoutThreshold 中指定的值，因為帳戶將會被鎖定在 ADFS 上 ExtranetObservationWindow 中指定的持續時間。</br>活動識別碼： %1</br>XML: %2|
|1201|此事件會寫入每次使用者遭到鎖定。 </br>活動識別碼： %1</br>XML: %2|
|557 (ADFS 2019)| 嘗試在節點 %1 上的帳戶存放區 rest 服務通訊時發生錯誤。 如果這是 WID 伺服器陣列的主要節點可能已離線。 如果這是 SQL 陣列 ADFS 會自動選取新的節點，來裝載使用者存放區的主要角色。|
|562 (ADFS 2019)|發生錯誤時 communcating 與帳戶儲存在伺服器 %1 的端點。</br>例外狀況訊息： %2|
|563 (ADFS 2019)|計算外部網路鎖定狀態時，發生錯誤。 由於 %1 的值設定允許驗證此使用者和權杖發行將會繼續。 如果這是 WID 伺服器陣列的主要節點可能已離線。 如果這是 SQL 陣列 ADFS 會自動選取新的節點，來裝載使用者存放區的主要角色。</br>帳戶存放區伺服器名稱： %2</br>使用者識別碼： %3</br>例外狀況訊息： %4|
|512|以下的使用者帳戶被鎖定。登入嘗試會被封鎖，因為系統組態。</br>活動識別碼： %1 </br>使用者： %2 </br>用戶端 IP: %3 </br>不正確密碼計數： %4  </br>上次錯誤密碼嘗試： %5|
|515|下列使用者帳戶處於鎖定狀態，並只提供了正確的密碼。 此帳戶可能會被洩露。</br>其他資料 </br>活動識別碼： %1 </br>使用者： %2 </br>用戶端 IP: %3 |
|516|下列使用者帳戶已經鎖定時由於不正確密碼嘗試次數過多。</br>活動識別碼： %1  </br>使用者： %2  </br>用戶端 IP: %3  </br>不正確密碼計數： %4  </br>上次錯誤密碼嘗試： %5|

## <a name="esl-frequently-asked-questions"></a>ESL 常見問題集

**將 ADFS 伺服器陣列使用外部網路中的智慧鎖定會強制模式看到惡意使用者鎖定？** 

答：如果 ADFS 智慧鎖定會設定為 '強制' 模式，則將永遠不會看到合法使用者的帳戶鎖定的暴力密碼破解或阻斷服務。 惡意的帳戶鎖定可以防止使用者登入的唯一方法是，如果不良執行者具有使用者密碼，或從已知的良好 （熟悉） IP 位址可以傳送的要求該使用者。 

**已啟用 ESL 和不良執行者具有使用者密碼，會發生什麼事？** 

答：暴力密碼破解攻擊案例的一般目的是猜測密碼，並已成功登入。  如果使用者是防範，或密碼猜測然後 ESL 功能不會封鎖存取因為登入將會符合 「 成功 」 的準則，正確的密碼，再加上新的 IP。 不良執行者 IP 則會出現與 「 熟悉的 」。 最佳的緩和措施，在此案例中是以清除 在 ADFS 中的使用者活動，並需要多重要素驗證的使用者。 我們強烈建議安裝 AAD 密碼保護，可確保猜到的密碼不會陷入系統。

**如果我的使用者從未登入已成功從 IP 然後嘗試以錯誤的密碼幾次會，讓它們能夠登入一次最後輸入他們的密碼正確嗎？** 

答：如果在使用者提交多個不正確的密碼 （也就是合法正確輸入），並在下列程式碼嘗試取得的密碼正確，則使用者會立即成功登入。  這會清除錯誤密碼計數，並新增至 FamiliarIPs 清單該 IP。  不過，如果它們從未知位置超過失敗登入的臨界值，會進入鎖定狀態，而且必須等候過去的觀測視窗並使用有效的密碼登入，或需要重設其帳戶的系統管理員介入。  

**ESL 運作內部太？**   
答：如果用戶端連線，直接向 ADFS 伺服器，而且不是透過 Web Application Proxy 伺服器將不會套用 ESL 行為。 

**我看到 [用戶端 IP] 欄位中的 Microsoft IP 位址。沒有 ESL 區塊 EXO 代理暴力密碼破解攻擊？**  

答：ESL 運作以防止 Exchange Online 或其他舊版驗證暴力密碼破解攻擊案例。 舊版驗證有 00000000-0000-0000-0000-000000000000"活動 ID"。 在這些攻擊中，不良執行者就利用 Exchange Online 的基本驗證 （也稱為傳統驗證），讓用戶端 IP 位址會顯示為其中一個 Microsoft。 雲端 proxy 驗證驗證代表 Outlook 用戶端中的 Exchange online 的伺服器。 在這些情況下，惡意的傳送者的 IP 位址會在 x-ms-轉送-用戶端-ip] 及 [Microsoft Exchange Online 的伺服器 IP 會在 x ms-用戶端 ip 值。
外部網路的智慧鎖定會檢查網路 Ip 轉送的 Ip、 x-轉送-用戶端-IP，以及 x ms-用戶端 ip 值。 如果要求成功，所有的 Ip 會新增到熟悉的清單中。 如果收到的要求和任何顯示的 Ip 不在熟悉的清單則要求會標示為不熟悉。 熟悉的使用者能夠登入成功時從不熟悉的位置的要求將遭到封鎖。  


## <a name="additional-references"></a>其他參考資料  
[保護 Active Directory Federation Services 的最佳做法](../../ad-fs/deployment/best-practices-securing-ad-fs.md)

[Set-AdfsProperties](https://technet.microsoft.com/itpro/powershell/windows/adfs/set-adfsproperties)

[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)
