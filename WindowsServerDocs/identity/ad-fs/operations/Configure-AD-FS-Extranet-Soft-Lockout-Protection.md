---
ms.assetid: 777aab65-c9c7-4dc9-a807-9ab73fac87b8
title: 設定 AD FS 外部鎖定保護
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/01/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6612c05e664b50c5a50b10b712b91715cc85d230
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189877"
---
# <a name="configure-ad-fs-extranet-lockout-protection"></a>設定 AD FS 外部鎖定保護

在 Windows Server 2012 R2 上的 AD FS，我們引進了一項稱為外部網路鎖定的安全性功能。  利用此功能，AD FS 會 「 停止 」 的一段時間驗證以外的 「 惡意 」 的使用者帳戶。  這可防止您的使用者帳戶遭鎖定在 Active Directory 中。  除了保護您的使用者 AD 帳戶解除鎖定，AD FS 外部網路鎖定也會防止暴力密碼破解攻擊

> [!NOTE]
> 這項功能僅適用於**外部網路案例**其中驗證要求都透過 Web Application Proxy，而且只適用於**使用者名稱和密碼驗證**。

## <a name="advantages-of-extranet-lockout"></a>外部網路鎖定的優點
外部網路鎖定提供下列主要優點：
- 它能保護您的使用者帳戶，從**暴力密碼破解攻擊**攻擊者嘗試猜測使用者的密碼，藉由持續傳送驗證要求。 在此情況下，AD FS 會鎖定外部網路存取權的惡意使用者帳戶
- 它能保護您的使用者帳戶，從**惡意的帳戶鎖定**，攻擊者想要鎖定的使用者帳戶，藉由傳送錯誤密碼的驗證要求。 在此情況下，雖然使用者帳戶會鎖定外部網路存取的 AD fs，在 AD 中的實際的使用者帳戶並未鎖定，使用者仍然可以存取在組織內的公司資源。 這就所謂**軟式鎖定**。

## <a name="how-it-works"></a>運作方式
有 3 個設定，您需要設定以啟用此功能的 AD FS 中： 
- **EnableExtranetLockout&lt;布林&gt;** 設定此布林值設為 True，如果您想要啟用外部網路鎖定。
- **ExtranetLockoutThreshold&lt;整數&gt;** 這會定義不正確密碼嘗試次數上限。 一旦達到閾值時，AD FS 會立即會拒絕來自外部網路的要求，而不會嘗試連絡網域控制站進行驗證，不論，密碼是好是壞，直到傳遞外部網路的觀測視窗。 這表示的值**badPwdCount**的 AD 帳戶的屬性不會提高時的帳戶是軟體鎖定。
- **ExtranetObservationWindow &lt;TimeSpan&gt;** 這會決定多久的使用者帳戶將會是軟體鎖定。AD FS 會開始傳遞視窗時，執行一次使用者名稱和密碼驗證。 AD FS 會使用 AD 屬性 badPasswordTime 做為參考來判斷的外部網路的觀測視窗是否有通過或失敗。 期間過後，如果目前時間 > badPasswordTime + ExtranetObservationWindow。 

> [!NOTE]
> AD FS 外部網路鎖定函數分開 AD 鎖定原則。 不過，我們強烈建議您設定**ExtranetLockoutThreshold**小於 AD 帳戶鎖定閾值的值的參數值。 不這麼做的話，可能會導致無法防止帳戶被鎖定在 Active Directory 中的 AD FS。 

啟用外部網路鎖定功能與最大值為 15 錯誤密碼嘗試次數以及 30 分鐘軟式鎖定持續時間的範例，如下所示：

```powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30)
```

這些設定會套用至可驗證 AD FS 服務的所有網域。 它的運作方式是，當 AD FS 收到的驗證要求時，它會透過 LDAP 呼叫來存取網域控制站 (PDC)，並執行查閱**badPwdCount** pdc 使用者屬性。 如果 AD FS 會尋找的值**badPwdCount** > = ExtranetLockoutThreshold 設定並將外部網路的 [觀察] 視窗中定義的時間未跳過但 AD FS 會拒絕要求，立即執行，這表示無論是否使用者必須輸入正確或不正確的密碼，從外部網路，登入將會失敗，因為 AD FS 不會將認證傳送至 AD。 AD FS 不會維護方面的任何狀態**badPwdCount**或鎖定使用者帳戶。 AD FS 使用 AD 追蹤的所有狀態。 

> [!warning]
> 啟用 Server 2012 R2 上的 AD FS 外部網路鎖定時透過 WAP 的所有驗證要求都驗證的 PDC 上的 AD FS。 PDC 無法使用時，使用者將無法從外部網路驗證。

Server 2016 提供額外的參數，PDC 無法使用時，可讓遞補到另一個網域控制站的 AD FS:

- **ExtranetLockoutRequirePDC&lt;布林&gt;** -啟用時： 外部網路鎖定要求的主要網域控制站 (PDC)。 當停用： 外部網路鎖定會遞補到另一個網域控制站，以防 PDC 無法使用。

您可以使用下列 Windows PowerShell 命令來設定在 Server 2016 上的 AD FS 外部網路鎖定：

```powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30) -ExtranetLockoutRequirePDC $false
```

## <a name="working-with-the-active-directory-lockout-policy"></a>使用 Active Directory 鎖定原則
在 AD FS 中的外部網路鎖定功能以外獨立運作的 AD 鎖定原則。 不過，您需要確定正確設定外部網路鎖定，以便它可以提供其安全性目的，含有 AD 鎖定原則設定。
讓我們看看 AD 鎖定原則第一次。 在 AD 中有三個有關鎖定原則的設定：
- **帳戶鎖定閾值**： 此設定是與 AD FS 中 ExtranetLockoutThreshold 設定類似。 它會判斷將造成使用者帳戶被鎖定的登入失敗的嘗試次數。為了保護您的使用者帳戶受到惡意的帳戶鎖定攻擊，您想要在 AD FS 中設定的 ExtranetLockoutThreshold 值&lt;中 AD 的帳戶鎖定臨界值
- **帳戶鎖定期間**： 此設定會決定多久使用者帳戶被鎖定。此設定並不重要更此交談中的前 AD 鎖定設定是否正確，應該一律發生外部網路鎖定
- **之後，重設帳戶鎖定計數器**： 此設定會決定從使用者的最後一個登入失敗之前必須經過多少時間**badPwdCount**重設為 0。 為了讓外部網路鎖定 」 功能的 AD FS 以搭配 AD 鎖定原則，您想要確定 ExtranetObservationWindow 值在 AD FS &gt; ad 過後重設帳戶鎖定計數器的值。 下列範例會說明原因。  

讓我們看看兩個範例，請參閱如何**badPwdCount**經過一段時間根據不同的設定和狀態會變更。 讓我們假設這兩個範例**帳戶鎖定閾值**= 4 並**ExtranetLockoutThreshold** = 2。 **紅色**箭號表示錯誤密碼嘗試**綠色**箭號表示良好的密碼嘗試。 在範例 #1 **ExtranetObservationWindow** &gt; **過後重設帳戶鎖定計數器**。 在範例 #2 **ExtranetObservationWindow** &lt; **過後重設帳戶鎖定計數器**。 

### <a name="example-1"></a>範例 1
![範例 1](media/Configure-AD-FS-Extranet-Lockout-Protection/one.png)

### <a name="example-2"></a>範例 2
![範例 1](media/Configure-AD-FS-Extranet-Lockout-Protection/two.png)

如您從上述所見，有兩個條件的時機**badPwdCount**會重設為 0。 其中一個時，沒有成功的登入。 另一個則是重設此計數器，如中所定義的時候**過後重設帳戶鎖定計數器**設定。 當**過後重設帳戶鎖定計數器** &lt; **ExtranetObservationWindow**，帳戶並沒有任何遭鎖定，由 AD 的風險。 不過，如果**過後重設帳戶鎖定計數器** &gt; **ExtranetObservationWindow**，帳戶可能會被鎖定 ad，但在 「 延遲的方式 」 的可能。 可能需要一段時間，若要取得帳戶被鎖住 ad 根據您的設定為 AD FS 將只允許一個錯誤密碼嘗試，直到其觀察期間**badPwdCount**達到**帳戶鎖定閾值**.

如需詳細資訊，請參閱 <<c0> [ 設定帳戶鎖定](https://blogs.technet.microsoft.com/secguide/2014/08/13/configuring-account-lockout/)。 

## <a name="known-issues"></a>已知問題
沒有已知的問題所在的 AD 使用者帳戶因為無法驗證與 AD FS **badPwdCount**屬性不會複寫至 ADFS 查詢的網域控制站。 請參閱[2971171](https://support.microsoft.com/help/2971171/adfs-authentication-issue-for-active-directory-users-when-extranet-loc)如需詳細資訊。 您可以找到目前為止已發行的所有 AD FS Qfe[此處](../deployment/updates-for-active-directory-federation-services-ad-fs.md)。

## <a name="key-points-to-remember"></a>要記住的重點
- 外部網路鎖定功能僅適用於**外部網路案例**驗證要求會通過 Web 應用程式 Proxy
- 外部網路鎖定功能只適用於**使用者名稱和密碼驗證**
- AD FS 不會保留任何的 track **badPwdCount**或軟式鎖定的使用者。AD FS 使用 AD 追蹤的所有狀態
- AD FS 執行查閱**badPwdCount**透過 LDAP 呼叫，每次嘗試驗證 PDC 上的使用者屬性  
- 如果它無法存取 PDC，AD FS 2016 以前將會失敗。 AD FS 2016 導入了改善，以便在 AD FS 切換回 PDC 發生的其他網域控制站未提供。 
- AD FS 會允許驗證要求從外部網路的 if badPwdCount < ExtranetLockoutThreshold 
- 如果**badPwdCount** >= **ExtranetLockoutThreshold** AND **badPasswordTime** + **ExtranetObservationWindow** < 目前的時間，AD FS 會拒絕驗證要求從外部網路
- 若要避免惡意的帳戶鎖定，您應該確定**ExtranetLockoutThreshold** < **帳戶鎖定閾值**AND **ExtranetObservationWindow**  > **重設帳戶鎖定計數器**


## <a name="additional-references"></a>其他參考資料  
- [保護 Active Directory Federation Services 的最佳做法](../../ad-fs/deployment/best-practices-securing-ad-fs.md)
- [將 AD FS Powershell Cmdlet 存取權委派給非系統管理員使用者](delegate-ad-fs-pshell-access.md)
- [Set-AdfsProperties](https://technet.microsoft.com/itpro/powershell/windows/adfs/set-adfsproperties)

[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)

    
