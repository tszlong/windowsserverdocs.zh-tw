---
ms.assetid: 777aab65-c9c7-4dc9-a807-9ab73fac87b8
title: 設定 AD FS 外部鎖定保護
description: ''
author: billmath
ms.author: billmath
manager: mtilman
ms.date: 02/20/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 904b563da2f1404d873c7352db9eadb7bfe252f2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869759"
---
# <a name="ad-fs-extranet-lockout-and-extranet-smart-lockout"></a>AD FS 外部網路鎖定和外部網路的智慧鎖定

# <a name="overview"></a>總覽

>適用於：Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2

在 Windows Server 2012 R2 上的 AD FS，我們引進了一項稱為的安全性功能[軟性的外部網路鎖定](configure-ad-fs-extranet-soft-lockout-protection.md)。  這項功能，與 AD FS 會停止一段時間驗證從外部網路的使用者。  這可防止您的使用者帳戶遭鎖定在 Active Directory 中。 除了保護您的使用者 AD 帳戶解除鎖定，AD FS 外部網路鎖定也會防止暴力密碼破解攻擊。

在 2018 年 6 月，在 Windows Server 2016 上的 AD FS 引進了**外部網路的智慧鎖定 (ESL)**。  ESL 可讓您區分登入嘗試，看起來像是從有效的使用者和項目可能是攻擊者從登入的 AD FS。 如此一來，AD FS 可以鎖定攻擊者同時可讓繼續使用他們的帳戶的有效使用者。 這可防止阻絕服務的使用者，並防止目標的攻擊，例如 「 密碼噴灑 」 攻擊。  
ESL 適用於 Windows Server 2016 中的 AD FS，並內建於 Windows Server 2019 中的 AD FS。

> [!NOTE]
> 這項功能僅適用於**外部網路案例**中的驗證要求是透過 Web Application Proxy，而且只適用於**使用者名稱和密碼驗證**。

## <a name="advantages-of-extranet-smart-lockout-in-ad-fs-2016"></a>外部網路 AD FS 2016 中的智慧鎖定的優點
在 AD FS 2012 R2 中的外部網路軟式鎖定提供下列主要優點：
- 保護您的使用者帳戶，從**暴力密碼破解攻擊**中會攻擊者嘗試猜測使用者的密碼，藉由持續傳送驗證要求，以及從**噴灑防範密碼攻擊**位置攻擊者嘗試以許多不同的帳戶搭配使用一般密碼
- 保護您的使用者帳戶，從**Active Directory 帳戶鎖定**從使用錯誤密碼的惡意的驗證要求。 在此情況下，雖然使用者帳戶會鎖定外部網路存取，使用者仍然可以從公司網路至 AD 的登入。 這就所謂**軟式鎖定**。

外部網路智慧鎖定為建置基礎的外部網路的軟式鎖定，加上下列優點：
- 防止使用者遇到**外部網路帳戶鎖定**來自惡意的驗證要求。  智慧鎖定會防止潛在的惡意要求從不熟悉的位置，同時允許實際的使用者，可從熟悉的位置登入來自外部網路 （從中使用者已成功登入之前的位置）。
- 已記錄的唯一模式，以便系統可以學習良好且可能是惡意的登入活動，而不需要停用任何帳戶

## <a name="additional-advantages-of-extranet-smart-lockout-in-ad-fs-2019"></a>外部網路 ad FS 2019 的智慧鎖定的其他優點
外部網路 ad FS 2019 的智慧鎖定會加入相較於 AD FS 2016 的優點如下：
- 設定獨立的鎖定閾值熟悉且不熟悉的位置，讓已知的良好地點的使用者可以從可疑位置有更多的空間比要求時發生錯誤
- 啟用稽核模式中的智慧鎖定，同時繼續強制執行先前的軟式鎖定行為

## <a name="pre-requisites-for-extranet-smart-lockout-in-ad-fs-2016"></a>外部網路 AD FS 2016 中的智慧鎖定的必要條件
下列必要條件，才能與 AD FS 2019 ESL。

### <a name="install-updates-on-all-nodes-in-the-farm"></a>伺服器陣列中的所有節點上安裝更新
首先，請確定所有的 Windows Server 2016 AD FS 伺服器是最新的自 2018 年 6 月的 Windows 更新和 AD FS 2016 伺服器陣列 2016年伺服器陣列行為層級執行。

### <a name="update-artifact-database-permissions"></a>更新成品資料庫權限
外部網路的智慧鎖定會需要將 ADFS 成品資料庫中的新資料表的權限的 AD FS 服務帳戶。  藉由在 PowerShell 命令視窗執行下列命令，授與此權限：
``` powershell
PS C:\>$cred = Get-Credential
PS C:\>Update-AdfsArtifactDatabasePermission -Credential $cred
```
其中`$cred`是 AD FS 系統管理員權限的帳戶 （AD FS 系統管理員權限所做變更的資料庫）。

>[!NOTE]
>在多重伺服器陣列使用 WID 資料庫，在上述 cmdlet 需要，每個 AD FS 伺服器上啟用 Windows 遠端管理

如果您沒有 AD FS 系統管理員權限，您可以設定資料庫權限以手動方式 SQL 或 WID 執行下列命令連接到 AdfsArtifactStore 資料庫時。
```
sp_addrolemember 'db_owner', 'db_genevaservice'
```
### <a name="ensure-ad-fs-security-audit-logging-is-enabled"></a>請確定已啟用 AD FS 安全性稽核記錄
這項功能會使用安全性稽核記錄檔，因此稽核必須啟用 AD FS，以及所有的 AD FS 伺服器上的本機原則。

## <a name="pre-requisites-for-extranet-smart-lockout-in-ad-fs-2019"></a>外部網路的智慧鎖定在 AD FS 2019 的必要條件
下列必要條件，才能與 AD FS 2016 ESL。

### <a name="ensure-ad-fs-security-audit-logging-is-enabled"></a>請確定已啟用 AD FS 安全性稽核記錄
這項功能會使用安全性稽核記錄檔，因此稽核必須啟用 AD FS，以及所有的 AD FS 伺服器上的本機原則。

## <a name="lockout-settings"></a>鎖定設定
外部網路的智慧鎖定是由新的和現有的 AD FS 屬性所控管的新功能的一組所組成。

### <a name="extranet-lockout-enabled"></a>啟用外部網路鎖定
外部網路的智慧鎖定會使用相同的 AD FS 屬性只是為了控制 「 軟性 」 的外部網路鎖定先前已使用。  屬性稱為 ExtranetLockoutEnabled，而且可以透過 Get-adfsproperties 檢視。

### <a name="extranet-smart-lockout-mode"></a>外部網路的智慧鎖定模式
已新增新的 AD FS 屬性，稱為 ExtranetLockoutMode 來控制智慧與 「 軟性 」 的鎖定行為。  它可以透過 Set-adfsproperties 設定，並含有 3 個值：

    - **ADPasswordCounter** – 這是 ADFS 「 軟性外部網路鎖定 」 模式不會區分其位置為基礎的舊版。  這是預設值。

    - **ADFSSmartLockoutLogOnly** – 這是外部網路的智慧鎖定，但而不是拒絕驗證要求，AD FS 會只寫入管理和稽核事件。

    - **ADFSSmartLockoutEnforce** -這是外部網路的智慧鎖定封鎖不熟悉的要求，當達到臨界值的完整支援。

在 AD FS 2019 中，因此該軟體的鎖定會繼續強制執行，而您所準備智慧鎖定可以結合 ADPasswordCounter 和 ADFSSmartLockoutLogOnly 的值。

### <a name="lockout-threshold-and-observation-window"></a>鎖定閾值] 和 [觀測視窗
在 AD FS 2019 的智慧鎖定會使用相同兩個為軟式鎖定先前使用 AD FS 屬性：ExtranetObservationWindow 和 ExtranetLockoutThreshold。

- **ExtranetLockoutThreshold&lt;整數&gt;** 這會定義不正確密碼嘗試次數上限。 一旦達到閾值時，在 ADFSSmartLockoutEnforce 模式 AD FS 將拒絕要求從外部網路直到觀察期間過後。  在 ADFSSmartLockoutLogOnly 模式中，AD FS 會寫入只記錄項目。  
- **ExtranetObservationWindow &lt;TimeSpan&gt;** 這會決定多久的使用者名稱和密碼從不熟悉的位置的要求會遭到鎖定。AD FS 會開始傳遞視窗時，執行一次使用者名稱和密碼驗證。

> [!NOTE]
> AD FS 外部網路鎖定函數分開 AD 鎖定原則。 我們建議您設定**ExtranetLockoutThreshold**小於 AD 帳戶鎖定閾值的值的參數值。 不這麼做的話，可能會導致無法防止帳戶被鎖定在 Active Directory 中的 AD FS。 

在 AD FS 2019 中，我們引進了新的鎖定閾值特定已知良好的位置：ExtranetLockoutThresholdFamiliarLocation。
- **ExtranetLockoutThresholdFamiliarLocation&lt;整數&gt;** 這會定義從熟悉的位置不正確的密碼嘗試次數的數目上限。 在 AD FS 2019 中，原始的參數 ExtranetLockoutThreshold 適用於不熟悉的位置 （沒有已知為良好的 IP 位址）。

### <a name="primary-domain-controller-requirement"></a>主要網域控制站需求
AD FS 2016 提供參數，可讓遞補到另一個網域控制站，PDC 無法使用時。

- **ExtranetLockoutRequirePDC&lt;布林&gt;** 外部網路鎖定啟用時，需要主要網域控制站 (PDC)。 停用時，外部網路鎖定，以防 PDC 無法使用，就會將恢復為另一個網域控制站。

   下列範例會顯示啟用鎖定，PDC 必須停用 cmdlet:

    ```powershell
    Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30) -ExtranetLockoutRequirePDC $false
    ```

## <a name="configuring-ad-fs-with-smart-lockout-in-log-only-mode"></a>設定 AD FS 與記錄模式的智慧鎖定

### <a name="ad-fs-2016"></a>AD FS 2016
我們建議您先設定的鎖定行為，以記錄只能由執行下列 cmdlet:

 ```powershell
PS C:\>Set-AdfsProperties -ExtranetLockoutMode AdfsSmartlockoutLogOnly
 ```

在此模式中，AD FS 會填入使用者熟悉的位置資訊和寫入安全性稽核事件，但不會封鎖任何要求。  此模式用來驗證的智慧鎖定正在執行，而且若要啟用 AD FS，以 「 了解 」 之前啟用的使用者熟悉的位置 「 強制執行 」 模式。
當 AD FS 學習，它會儲存每位使用者的登入活動 (是否記錄模式或強制模式)。 

>[!NOTE]
>設定`ExtranetLockoutMode`要`AdfsSmartlockoutLogOnly`將會導致舊版 AD FS 「 外部網路軟式鎖定 」 行為，不再作用中，即使`EnableExtranetLockout`屬性設定為 True。  這表示，超過鎖定臨界值，從熟悉的或不熟悉 IP 位址的使用者就不會被鎖定的 AD FS 中的智慧鎖定。 不過，內部部署 AD 可能會鎖定的設定，並為基礎的使用者。   請參閱[帳戶鎖定原則](https://docs.microsoft.com/windows/security/threat-protection/security-policy-settings/account-lockout-policy)若要了解如何在內部部署 AD 可以鎖定使用者。 」  這被為了是暫時性的狀態，以便系統可以了解之前重新引入新的智慧鎖定行為與鎖定強制的登入行為。

新的模式，才會生效，重新啟動 AD FS 服務的所有節點上，伺服器陣列中
  
  ``` powershell
PS C:\>Restart-service adfssrv
  ```
將模式設定之後，您可以啟用智慧鎖定使用`EnableExtranetLockout`參數


``` powershell
PS C:\>Set-AdfsProperties -EnableExtranetLockout $true
```

請注意，您可以使用相同的 cmdlet 停用鎖定

範例：停用鎖定

``` powershell
PS C:\>Set-AdfsProperties -EnableExtranetLockout $false
```
### <a name="ad-fs-2019"></a>AD FS 2019
如果您目前未使用 AD FS 外部網路軟式鎖定，我們建議您遵循與 AD FS 2016 上述相同的指引。
如果您使用的彈性的鎖定，不過，我們建議您設定 AD FS 2019 鎖定行為以記錄的智慧鎖定，但請強制執行軟體的鎖定，請使用下列 powershell:

 ```powershell
PS C:\>Set-AdfsProperties -ExtranetLockoutMode 3
 ```

一旦您執行這個指令程式時，您可以使用 Get-adfsproperties 查詢 ExtranetLockoutMode AD FS 屬性的值。  您會看到它的值，已更新為 ADPasswordCounter 和 ADFSSmartLockoutLogOnly 的位元組合。

## <a name="observing-audit-events"></a>觀察稽核事件
AD FS 會將外部網路鎖定事件寫入安全性稽核記錄檔：
-   當使用者鎖定時外 （如登入失敗嘗試，達到鎖定閾值）
-   當 AD FS 收到已處於鎖定狀態之使用者的登入嘗試

在 記錄檔的唯一模式，您可以檢查鎖定事件的安全性稽核記錄檔。  針對找不到任何事件，您可以檢查使用 Get ADFSAccountActivity cmdlet 來判斷發生鎖定熟悉或不熟悉 IP 位址，以及詳細的檢查清單中的熟悉的 IP 位址，該使用者的使用者狀態。

範例事件：
```
Log Name:      Security
Source:        AD FS Auditing
Date:          5/21/2018 12:55:59 AM
Event ID:      1210
Task Category: (3)
Level:         Information
Keywords:      Classic,Audit Failure
User:          CONTOSO\adfssvc
Computer:      ADFS2016FS1.corp.contoso.com
Description:
An extranet lockout event has occurred. See XML for failure details. 

Activity ID: fa7a8052-0694-48f0-84e2-b51cde40ac3d 

Additional Data 
XML: <?xml version="1.0" encoding="utf-16"?>
<AuditBase xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="ExtranetLockoutAudit">
  <AuditType>ExtranetLockout</AuditType>
  <AuditResult>Failure</AuditResult>
  <FailureType>ExtranetLockoutError</FailureType>
  <ErrorCode>AccountRestrictedAudit</ErrorCode>
  <ContextComponents>
    <Component xsi:type="ResourceAuditComponent">
      <RelyingParty>http://fs.contoso.com/adfs/services/trust</RelyingParty>
      <ClaimsProvider>N/A</ClaimsProvider>
      <UserId>CONTOSO\user</UserId>
    </Component>
    <Component xsi:type="RequestAuditComponent">
      <Server>N/A</Server>
      <AuthProtocol>WSFederation</AuthProtocol>
      <NetworkLocation>Extranet</NetworkLocation>
      <IpAddress>64.187.173.10</IpAddress>
      <ForwardedIpAddress>64.187.173.10</ForwardedIpAddress>
      <ProxyIpAddress>N/A</ProxyIpAddress>
      <NetworkIpAddress>N/A</NetworkIpAddress>
      <ProxyServer>ADFS2016PROXY2</ProxyServer>
      <UserAgentString>Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.181 Safari/537.36</UserAgentString>
      <Endpoint>/adfs/ls/</Endpoint>
    </Component>
    <Component xsi:type="LockoutConfigAuditComponent">
      <CurrentBadPasswordCount>5</CurrentBadPasswordCount>
      <ConfigBadPasswordCount>5</ConfigBadPasswordCount>
      <LastBadAttempt>05/21/2018 00:55:05</LastBadAttempt>
      <LockoutWindowConfig>00:30:00</LockoutWindowConfig>
    </Component>
  </ContextComponents>
</AuditBase>
Event Xml:
<Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
  <System>
    <Provider Name="AD FS Auditing" />
    <EventID Qualifiers="0">1210</EventID>
    <Level>0</Level>
    <Task>3</Task>
    <Keywords>0x8090000000000000</Keywords>
    <TimeCreated SystemTime="2018-05-21T00:55:59.921880300Z" />
    <EventRecordID>35521235</EventRecordID>
    <Channel>Security</Channel>
    <Computer>ADFS2016FS1.contoso.com</Computer>
    <Security UserID="S-1-5-21-1156273042-1594504307-2076964089-1104" />
  </System>
  <EventData>
    <Data>fa7a8052-0694-48f0-84e2-b51cde40ac3d</Data>
    <Data><?xml version="1.0" encoding="utf-16"?>
<AuditBase xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="ExtranetLockoutAudit">
  <AuditType>ExtranetLockout</AuditType>
  <AuditResult>Failure</AuditResult>
  <FailureType>ExtranetLockoutError</FailureType>
  <ErrorCode>AccountRestrictedAudit</ErrorCode>
  <ContextComponents>
    <Component xsi:type="ResourceAuditComponent">
      <RelyingParty>http://fs.contoso.com/adfs/services/trust</RelyingParty>
      <ClaimsProvider>N/A</ClaimsProvider>
      <UserId>CONTOSO\user</UserId>
    </Component>
    <Component xsi:type="RequestAuditComponent">
      <Server>N/A</Server>
      <AuthProtocol>WSFederation</AuthProtocol>
      <NetworkLocation>Extranet</NetworkLocation>
      <IpAddress>64.187.173.10</IpAddress>
      <ForwardedIpAddress>64.187.173.10</ForwardedIpAddress>
      <ProxyIpAddress>N/A</ProxyIpAddress>
      <NetworkIpAddress>N/A</NetworkIpAddress>
      <ProxyServer>ADFS2016PROXY2</ProxyServer>
      <UserAgentString>Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.181 Safari/537.36</UserAgentString>
      <Endpoint>/adfs/ls/</Endpoint>
    </Component>
    <Component xsi:type="LockoutConfigAuditComponent">
      <CurrentBadPasswordCount>5</CurrentBadPasswordCount>
      <ConfigBadPasswordCount>5</ConfigBadPasswordCount>
      <LastBadAttempt>05/21/2018 00:55:05</LastBadAttempt>
      <LockoutWindowConfig>00:30:00</LockoutWindowConfig>
    </Component>
  </ContextComponents>
</AuditBase></Data>
  </EventData>
</Event>
```

## <a name="observing-user-activity"></a>觀察使用者活動
AD FS 提供 powershell cmdlet 來檢視及管理使用者帳戶的活動資料。  讀取目前的帳戶活動的使用者帳戶。  使用下列 cmdlet

``` powershell
PS C:\>Get-ADFSAccountActivity user@contoso.com
```

範例輸出
```
Identifier             : CONTOSO\user
BadPwdCountFamiliar    : 0
BadPwdCountUnknown     : 0
LastFailedAuthFamiliar : 1/1/0001 12:00:00 AM
LastFailedAuthUnknown  : 1/1/0001 12:00:00 AM
FamiliarLockout        : False
UnknownLockout         : False
FamiliarIps            : {}
```

目前的活動輸出包含下列資料：

**識別項**： 這是使用者名稱

**BadPwdCountFamiliar**： 這是目前的不正確的密碼登入嘗試從 IP 位址已在"FamiliarIps 」 清單上的時間的計數

**BadPwdCountUnknown**： 這是目前的不正確的密碼登入嘗試，嘗試在不是"FamiliarIps 」 清單上的 IP 位址計數

**LastFailedAuthFamiliar**： 這是當時的"FamiliarIps 」 清單上嘗試的 IP 位址的最後一個錯誤密碼登入嘗試的時間

**LastFailedAuthUnknown**： 這是在嘗試的時間不是"FamiliarIps 」 清單上的 IP 位址的最後一個不正確的密碼登入嘗試的時間

**FamiliarLockout**： 這表示是否使用者是目前處於鎖定狀態的正確密碼嘗試從"FamiliarIps 」 清單上的 IP 位址 

**UnknownLockout**： 這表示是否使用者是目前處於鎖定狀態的正確密碼嘗試來自不在 「 FamiliarIps"FamiliarIps 清單上的 IP 位址： 這是目前的使用者熟悉的 IP 位址清單

## <a name="adjust-threshold-and-window"></a>調整閾值和視窗
一旦您有足夠的一段時間，若要了解登入位置的 AD FS 的執行記錄模式，您可能想要調整閾值或觀察從視窗的預設設定。  這是使用`Set-AdfsProperties`如下列範例所示：

使用設定觀測視窗`ExtranetObservationWindow`:

範例： 

``` powershell
PS C:\>Set-AdfsProperties -ExtranetObservationWindow ( new-timespan -minutes 30 )
```

其中的值是 TimeSpan

### <a name="setting-threshold-value-in-ad-fs-2016"></a>在 AD FS 2016 中的設定臨界值
在 AD FS 2016 中，臨界值是使用 ExtranetLockoutThreshold 來設定：

範例：

``` powershell
PS C:\>Set-AdfsProperties -ExtranetLockoutThreshold 5
```

### <a name="setting-threshold-values-in-ad-fs-2019"></a>在 AD FS 2019 中設定臨界值
在 AD FS 2019 中，有已知的良好且不熟悉位置的不同的臨界值

若要設定的臨界值，如不熟悉的位置，使用相同的屬性用於 AD FS 2016 上方：

範例：

``` powershell
PS C:\>Set-AdfsProperties -ExtranetLockoutThreshold 5
```

若要設定臨界值的已知良好的位置，請使用 ExtranetLockoutThresholdFamiliarLocation，新的屬性，如下列範例所示：

範例：

``` powershell
PS C:\>Set-AdfsProperties -ExtranetLockoutThresholdFamiliarLocation 10
```


## <a name="enable-enforce-mode"></a>啟用強制模式
一旦您已有足夠的時間適用於 AD FS，若要了解登入位置，並觀察到的任何鎖定活動，執行記錄模式和智慧鎖定熟悉鎖定閾值和觀測視窗之後，可以移至 「 強制執行 」 模式使用下列的 PSH cmdlet:

``` powershell
PS C:\>Set-AdfsProperties -ExtranetLockoutMode AdfsSmartLockoutEnforce
```

新的模式，才會生效，重新啟動 AD FS 服務的所有節點上，伺服器陣列中

``` powershell
PS C:\>Restart-service adfssrv
```


## <a name="manage-user-account-activity"></a>管理使用者帳戶活動
AD FS 提供 3 個 cmdlet，來管理使用者帳戶的活動資料。  這些 cmdlet 自動連線至之保存的主要角色的伺服器陣列中的節點 (雖然可以覆寫這個行為，藉由傳遞-Server 參數)。

> [!NOTE] 
> 如需委派使用這些 cmdlet 的權限資訊，請參閱[委派 AD FS Powershell Commandlet 存取給非系統管理員使用者](delegate-ad-fs-pshell-access.md)

這些 cmdlet 是：

`Get-ADFSAccountActivity`

讀取目前的帳戶活動的使用者帳戶。  Cmdlet 一律會自動連線到伺服器陣列主機使用的帳戶活動 REST 端點，因此所有的資料應該一律是一致

``` powershell
Get-ADFSAccountActivity user@contoso.com
```
`
Set-ADFSAccountActivity
`

更新使用者帳戶的帳戶活動。  這可以用來加入新的熟悉位置，或清除的任何帳戶的狀態

``` powershell
Set-ADFSAccountActivity user@upnsuffix.com -FamiliarLocation “1.2.3.4”
```
`Reset-ADFSAccountLockout`

會重設的使用者帳戶鎖定計數器

``` powershell
Reset-ADFSAccountLockout user@upnsuffix.com -Familiar
```

## <a name="troubleshooting-esl"></a>疑難排解 ESL
下列可協助您進行疑難排解的外部網路的智慧鎖定功能。

### <a name="updating-database-permissions-for-esl"></a>正在更新 ESL 的資料庫權限
如果任何錯誤會傳回從`Update-AdfsArtifactDatabasePermission`cmdlet，確認下列

1.  伺服器陣列節點的清單正確無誤。  如果節點是在 AD FS 伺服器陣列的清單中，但不再作用中的修補程式的驗證將會失敗。  這可以藉由執行補救 `remove-adfsnode <node name >`
2.  確認伺服器陣列中的所有節點上已部署的修補程式
3.  請確認傳遞至 cmdlet 的認證具有修改 ad fs 成品的資料庫結構描述的擁有者的權限。  

### <a name="logging--auditing"></a>記錄/稽核
AD FS 時的驗證要求遭到拒絕，因為帳戶超過鎖定臨界值，將會寫入`ExtranetLockoutEvent`安全性稽核資料流。  

範例事件：

發生外部網路鎖定事件。 如需失敗的詳細資訊，請參閱 XML。 

**活動識別碼：172332e1-1301-4e56-0e00-0080000000db**

```
Additional Data 
XML: <?xml version="1.0" encoding="utf-16"?>
<AuditBase xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="ExtranetLockoutAudit">
  <AuditType>ExtranetLockout</AuditType>
  <AuditResult>Failure</AuditResult>
  <FailureType>ExtranetLockoutError</FailureType>
  <ErrorCode>AccountRestrictedAudit</ErrorCode>
  <ContextComponents>
    <Component xsi:type="ResourceAuditComponent">
      <RelyingParty>http://contoso.com/adfs/services/trust</RelyingParty>
      <ClaimsProvider>N/A</ClaimsProvider>
      <UserId>TQDFTD\Administrator</UserId>
    </Component>
    <Component xsi:type="RequestAuditComponent">
      <Server>N/A</Server>
      <AuthProtocol>WSFederation</AuthProtocol>
      <NetworkLocation>Intranet</NetworkLocation>
      <IpAddress>4.4.4.4</IpAddress>
      <ForwardedIpAddress />
      <ProxyIpAddress>1.2.3.4</ProxyIpAddress>
      <NetworkIpAddress>1.2.3.4</NetworkIpAddress>
      <ProxyServer>N/A</ProxyServer>
      <UserAgentString>Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.132 Safari/537.36</UserAgentString>
      <Endpoint>/adfs/ls</Endpoint>
    </Component>
    <Component xsi:type="LockoutConfigAuditComponent">
      <CurrentBadPasswordCount>5</CurrentBadPasswordCount>
      <ConfigBadPasswordCount>5</ConfigBadPasswordCount>
      <LastBadAttempt>02/07/2018 21:47:44</LastBadAttempt>
      <LockoutWindowConfig>00:30:00</LockoutWindowConfig>
    </Component>
  </ContextComponents>
</AuditBase>

```

## <a name="banned-ip-addresses"></a>禁止的 IP 位址
除了外部網路的智慧鎖定功能，AD FS 2018 年 6 月更新可讓您在 AD FS 中，全域設定一組 IP 位址，讓來自這些 IP 位址，或要求中有這些 IP 位址**x 轉送的**或是**x-ms-轉送-用戶端-ip**標頭，將會封鎖 AD fs。

##### <a name="adding-banned-ips"></a>新增帳戶已被禁用的 Ip
若要加入的全域清單遭到禁用的 Ip，請使用下列 Powershell cmdlet:

``` powershell
PS C:\ >Set-AdfsProperties -AddBannedIps "1.2.3.4", "::3", "1.2.3.4/16"
```

允許的格式

1.  IPv4
2.  IPv6
3.  使用 IPv4 或 v6 CIDR 格式
4.  使用 IPv4 或 v6 的 IP 範圍 (也就是 1.2.3.4-1.2.3.6)

#### <a name="removing-banned-ips"></a>移除禁止 Ip
若要移除的全域清單遭到禁用的 Ip，請使用下列 Powershell cmdlet:

``` powershell
PS C:\ >Set-AdfsProperties -RemoveBannedIps "1.2.3.4"
```

#### <a name="read-banned-ips"></a>讀取禁用 Ip
若要讀取目前的遭到禁用的 IP 位址集，使用下列 Powershell cmdlet:

``` powershell
PS C:\ >Get-AdfsProperties 
```

輸出範例：

```
BannedIpList                   : {1.2.3.4, ::3,1.2.3.4/16}
```



## <a name="additional-references"></a>其他參考資料  
[保護 Active Directory Federation Services 的最佳做法](../../ad-fs/deployment/best-practices-securing-ad-fs.md)

[Set-AdfsProperties](https://technet.microsoft.com/itpro/powershell/windows/adfs/set-adfsproperties)

[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)

    
