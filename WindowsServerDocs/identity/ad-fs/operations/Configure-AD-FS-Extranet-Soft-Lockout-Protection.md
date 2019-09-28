---
ms.assetid: 777aab65-c9c7-4dc9-a807-9ab73fac87b8
title: 設定 AD FS 外部網路鎖定保護
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/01/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: bb5958f8205271fe3ab2258ed9812ae03f2a0be0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358206"
---
# <a name="configure-ad-fs-extranet-lockout-protection"></a>設定 AD FS 外部網路鎖定保護

在 Windows Server 2012 R2 的 AD FS 中，我們引進了一種稱為外部網路鎖定的安全性功能。  有了這項功能，AD FS 將會「停止」從外部驗證「惡意」使用者帳戶一段時間。  這可避免在 Active Directory 中鎖定您的使用者帳戶。  除了保護您的使用者免于 AD 帳戶鎖定之外，AD FS 外部網路鎖定也可防範暴力密碼破解攻擊

> [!NOTE]
> 這項功能僅適用于驗證要求通過 Web 應用程式 Proxy，而且僅適用于使用者**名稱和密碼驗證**的**外部網路案例**。

## <a name="advantages-of-extranet-lockout"></a>外部網路鎖定的優點
外部網路鎖定提供下列主要優點：
- 它可保護您的使用者帳戶免于遭受**暴力密碼破解攻擊**，攻擊者會藉由持續傳送驗證要求來猜測使用者的密碼。 在此情況下，AD FS 將會鎖定惡意使用者帳戶以供外部網路存取
- 它會保護您的使用者帳戶免于**惡意帳戶鎖定**，攻擊者想要藉由傳送密碼錯誤的驗證要求來鎖定使用者帳戶。 在此情況下，雖然 AD FS 外部網路存取將會鎖定使用者帳戶，但 AD 中的實際使用者帳戶不會被鎖定，而且使用者仍然可以存取組織內的公司資源。 這就是所謂的**軟鎖定**。

## <a name="how-it-works"></a>運作方式
您必須設定 AD FS 中的3項設定，才能啟用這項功能： 
- **EnableExtranetLockout &lt;Boolean @ no__t-2**如果您想要啟用外部網路鎖定，請將此布林值設定為 True。
- **ExtranetLockoutThreshold &lt;Integer @ no__t-2**這會定義錯誤密碼嘗試次數的上限。 一旦達到閾值，AD FS 就會立即拒絕外部網路的要求，而不會嘗試連線到網域控制站進行驗證，不論密碼是否良好或不正確，直到通過外部網路觀察視窗為止。 這表示當帳戶進行虛鎖定時，AD 帳戶的**badPwdCount**屬性值將不會增加。
- **ExtranetObservationWindow &lt;TimeSpan @ no__t-2**這會決定使用者帳戶將被軟鎖定的時間長度。當傳遞視窗時，AD FS 將會開始再次執行使用者名稱和密碼驗證。 AD FS 使用 AD 屬性 badPasswordTime 做為參考，以判斷外部網路觀察視窗是否已通過。 如果目前時間 > badPasswordTime + ExtranetObservationWindow，則視窗已通過。 

> [!NOTE]
> 獨立于 AD 鎖定原則之外，AD FS 外部網路鎖定功能。 不過，我們強烈建議您將**ExtranetLockoutThreshold**參數值設定為小於 AD 帳戶鎖定閾值的值。 若未這麼做，會導致 AD FS 無法保護帳戶，使其無法在 Active Directory 中遭到鎖定。 

啟用外部網路鎖定功能的範例，其中最多可有15個不正確的密碼嘗試次數，而30分鐘的軟鎖定期間如下所示：

```powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30)
```

這些設定會套用到 AD FS 服務可以驗證的所有網域。 其運作方式是，當 AD FS 收到驗證要求時，它會透過 LDAP 呼叫來存取主域控制站（PDC），並為 PDC 上的使用者執行**badPwdCount**屬性的查閱。 如果 AD FS 找到**badPwdCount** > = ExtranetLockoutThreshold 設定的值，而且外部網路觀察視窗中定義的時間尚未通過，AD FS 會立即拒絕要求，這表示使用者是否輸入良好或不良的來自外部網路的密碼，登入將會失敗，因為 AD FS 不會將認證傳送到 AD。 AD FS 不會維護**badPwdCount**或鎖定使用者帳戶的任何狀態。 AD FS 使用 AD 進行所有狀態追蹤。 

> [!warning]
> 當伺服器 2012 R2 上的 AD FS 外部網路鎖定啟用時，所有透過 WAP 的驗證要求都是由 PDC 上的 AD FS 進行驗證。 當 PDC 無法使用時，使用者將無法從外部網路進行驗證。

伺服器2016提供額外的參數，可讓 AD FS 在 PDC 無法使用時，回復至另一個網域控制站：

- **ExtranetLockoutRequirePDC &lt;Boolean @ no__t-2** -啟用時：外部網路鎖定需要主域控制站（PDC）。 停用時：外部網路鎖定會回到另一個網域控制站，以防 PDC 無法使用。

您可以使用下列 Windows PowerShell 命令來設定伺服器2016上的 AD FS 外部網路鎖定：

```powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30) -ExtranetLockoutRequirePDC $false
```

## <a name="working-with-the-active-directory-lockout-policy"></a>使用 Active Directory 鎖定原則
AD FS 中的外部網路鎖定功能可獨立于 AD 鎖定原則運作。 不過，您必須確定已正確設定外部網路鎖定的設定，讓它可以使用 AD 鎖定原則來提供其安全性目的。
讓我們先看一下 AD 鎖定原則。 AD 中有三個關於鎖定原則的設定：
- **帳戶鎖定閾值**：此設定類似 AD FS 中的 ExtranetLockoutThreshold 設定。 它會決定導致使用者帳戶遭到鎖定的失敗登入嘗試次數。為了保護您的使用者帳戶免于遭受惡意帳戶鎖定攻擊，您想要在 AD FS 中設定 ExtranetLockoutThreshold 的值 &lt; 將 AD 中的帳戶鎖定臨界值設為
- **帳戶鎖定持續時間**：此設定會決定使用者帳戶被鎖定的時間長度。這項設定在此交談中並不重要，因為如果正確設定，在 AD 鎖定發生之前，應該一律會進行外部網路鎖定
- **重設帳戶鎖定計數器之後**：此設定會決定在**badPwdCount**重設為0之前，使用者最後一次登入失敗必須經過多少時間。 為了讓 AD FS 中的外部網路鎖定功能適用于 AD 鎖定原則，您想要確定 AD FS 中的 ExtranetObservationWindow 值 &gt; 在 AD 中的值後重設帳戶鎖定計數器。 以下範例將說明原因。  

讓我們看看兩個範例，並根據不同的設定和狀態，查看**badPwdCount**如何隨著時間變更。 讓我們假設在這兩個範例中，**帳戶鎖定臨界值**= 4， **ExtranetLockoutThreshold** = 2。 **紅色**箭號代表不正確的密碼嘗試，**綠色**箭號代表正確的密碼嘗試。 在範例 #1 中， **ExtranetObservationWindow** &gt;**重設帳戶鎖定計數器**。 在範例 #2 中， **ExtranetObservationWindow** &lt;**重設帳戶鎖定計數器**。 

### <a name="example-1"></a>範例 1
![Example1](media/Configure-AD-FS-Extranet-Lockout-Protection/one.png)

### <a name="example-2"></a>範例 2
![Example1](media/Configure-AD-FS-Extranet-Lockout-Protection/two.png)

如您所見，在**badPwdCount**會重設為0的情況下，有兩個條件。 其中一種是成功登入。 另一個則是在設定後重設 [**重設帳戶鎖定計數器**] 中定義的此計數器的時機。 在 &lt; **ExtranetObservationWindow** **之後重設帳戶鎖定計數器**時，帳戶不會有 AD 鎖定的任何風險。 不過，如果在 &gt; **ExtranetObservationWindow** **之後重設帳戶鎖定計數器**，可能會有帳戶被 AD 鎖定，但會以「延遲」的方式受到封鎖。 視您的設定而定，取得 AD 鎖定的帳戶可能需要一些時間，因為 AD FS 在其觀察期間內只允許一個不正確的密碼嘗試，直到**badPwdCount**達到**帳戶鎖定閾值**為止。

如需詳細資訊，請參閱設定[帳戶鎖定](https://blogs.technet.microsoft.com/secguide/2014/08/13/configuring-account-lockout/)。 

## <a name="known-issues"></a>已知問題
有一個已知的問題，就是因為**badPwdCount**屬性不會複寫到 ADFS 正在查詢的網域控制站，所以 AD 使用者帳戶無法使用 AD FS 進行驗證。 如需詳細資訊，請參閱[2971171](https://support.microsoft.com/help/2971171/adfs-authentication-issue-for-active-directory-users-when-extranet-loc) 。 您可以在[這裡](../deployment/updates-for-active-directory-federation-services-ad-fs.md)找到目前為止已發行的所有 AD FS qfe。

## <a name="key-points-to-remember"></a>要記住的重點
- 外部網路鎖定功能僅適用于驗證要求通過 Web 應用程式 Proxy 的**外部網路案例**
- 外部網路鎖定功能僅適用于使用者**名稱 & 密碼驗證**
- AD FS 不會追蹤已被軟鎖定的**badPwdCount**或使用者。AD FS 使用 AD 進行所有狀態追蹤
- AD FS 會針對每個驗證嘗試，透過 PDC 上使用者的 LDAP 呼叫來執行**badPwdCount**屬性的查閱  
- 如果無法存取 PDC，則2016以前的 AD FS 將會失敗。 AD FS 2016 引進了改良功能，可讓 AD FS 在 PDC 無法使用的情況下，切換回其他網域控制站。 
- 如果 badPwdCount < ExtranetLockoutThreshold，AD FS 將會允許來自外部網路的驗證要求 
- 如果**badPwdCount** >= **ExtranetLockoutThreshold**和**badPasswordTime** + **ExtranetObservationWindow** < 目前時間，AD FS 將會拒絕外部網路的驗證要求
- 若要避免惡意帳戶鎖定，您應該確定**ExtranetLockoutThreshold**@no__t 1**帳戶鎖定閾值**和**ExtranetObservationWindow** > **重設帳戶鎖定計數器**


## <a name="additional-references"></a>其他參考資料  
- [保護 Active Directory 同盟服務的最佳做法](../../ad-fs/deployment/best-practices-securing-ad-fs.md)
- [將 AD FS Powershell Cmdlet 存取權委派給非系統管理員使用者](delegate-ad-fs-pshell-access.md)
- [設定-Set-adfsproperties](https://technet.microsoft.com/itpro/powershell/windows/adfs/set-adfsproperties)

[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)

    
