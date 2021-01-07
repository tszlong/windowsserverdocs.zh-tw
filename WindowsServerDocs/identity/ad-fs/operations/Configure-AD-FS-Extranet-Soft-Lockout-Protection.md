---
ms.assetid: 777aab65-c9c7-4dc9-a807-9ab73fac87b8
title: 設定 AD FS 外部網路軟鎖定保護
description: 深入瞭解：設定 AD FS 外部網路鎖定保護
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/01/2019
ms.topic: article
ms.openlocfilehash: 0a4abb4f402a653f8b32a104c2d31f71c6dda589
ms.sourcegitcommit: 528bdff90a7c797cdfc6839e5586f2cd5f0506b0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/07/2021
ms.locfileid: "97977303"
---
# <a name="configure-ad-fs-extranet-lockout-protection"></a>設定 AD FS 外部網路鎖定保護

在 Windows Server 2012 R2 的 AD FS 中，我們引進了稱為外部網路鎖定的安全性功能。  使用這項功能，AD FS 將會「停止」在一段時間內從外部「停止」驗證「惡意」使用者帳戶。  這可防止您的使用者帳戶在 Active Directory 中被鎖定。  除了保護您的使用者免于 AD 帳戶鎖定之外，AD FS 外部網路鎖定也可防範暴力密碼破解攻擊

> [!NOTE]
> 這項功能僅適用于 **外部網路案例** ，其中驗證要求是透過 Web 應用程式 Proxy，而且只適用于使用者 **名稱和密碼驗證**。

## <a name="advantages-of-extranet-lockout"></a>外部網路鎖定的優點
外部網路鎖定提供下列主要優點：
- 它可保護您的使用者帳戶免于 **暴力密碼破解攻擊** ，攻擊者會持續傳送驗證要求，藉以猜測使用者的密碼。 在此情況下，AD FS 將會鎖定外部網路存取的惡意使用者帳戶
- 它可保護您的使用者帳戶免于 **惡意帳戶鎖定** ，其中攻擊者想要傳送錯誤的密碼給驗證要求，以鎖定使用者帳戶。 在此情況下，雖然 AD FS 外部網路存取的使用者帳戶會被鎖定，但 AD 中的實際使用者帳戶不會遭到鎖定，使用者仍可存取組織內的公司資源。 這就是所謂的 **軟鎖定**。

## <a name="how-it-works"></a>運作方式
您必須設定 AD FS 中的3個設定，才能啟用這項功能：
- **EnableExtranetLockout &lt;布林 &gt;** 值：如果您想要啟用外部網路鎖定，請將這個布林值設定為 True。
- **ExtranetLockoutThreshold &lt;整數 &gt;** ，定義錯誤密碼嘗試次數的上限。 一旦達到閾值，AD FS 將會立即拒絕外部網路的要求，而不會嘗試連線到網域控制站以進行驗證，不論密碼是否正確或不正確，直到通過外部網路觀察視窗為止。 這表示當帳戶已被鎖定時，AD 帳戶的 **badPwdCount** 屬性值不會增加。
- **ExtranetObservationWindow &lt;時間 &gt;** 範圍會決定使用者帳戶將會被虛鎖定的時間長度。當傳遞視窗時，AD FS 將會開始再次執行使用者名稱和密碼驗證。 AD FS 使用 AD 屬性 badPasswordTime 作為參考，以判斷是否已通過外部網路觀察視窗。 如果目前時間 > badPasswordTime + ExtranetObservationWindow，則會傳遞此視窗。

> [!NOTE]
> 獨立于 AD 鎖定原則之外，AD FS 外部網路鎖定功能。 不過，我們強烈建議您將 **ExtranetLockoutThreshold** 參數值設定為小於 AD 帳戶鎖定閾值的值。 如果無法這麼做，就會導致 AD FS 無法防止 Active Directory 中的帳戶遭到鎖定。

啟用外部網路鎖定功能的範例，最多15個錯誤密碼嘗試次數和30分鐘的軟鎖定持續時間，如下所示：

```powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30)
```

這些設定會套用至 AD FS 服務可以驗證的所有網域。 其運作方式是，當 AD FS 收到驗證要求時，它會透過 LDAP 呼叫來存取網域主控站 (PDC) ，並針對 PDC 上的使用者執行 **badPwdCount** 屬性查閱。 如果 AD FS 找到 **badPwdCount** >= ExtranetLockoutThreshold 設定的值，而且外部網路觀察視窗中定義的時間尚未通過，AD FS 將會立即拒絕要求，這表示無論使用者輸入的是來自外部網路的良好或不良密碼，登入將會失敗，因為 AD FS 不會將認證傳送給 AD。 AD FS 不會維護 **badPwdCount** 或鎖定使用者帳戶的任何狀態。 AD FS 使用 AD 進行所有狀態追蹤。

> [!warning]
> 當伺服器 2012 R2 上的 AD FS 外部網路鎖定啟用時，所有透過 WAP 的驗證要求都會由 PDC 上的 AD FS 進行驗證。 當 PDC 無法使用時，使用者將無法從外部網路進行驗證。

伺服器2016提供一個額外的參數，可讓 AD FS 在 PDC 無法使用時回復至另一個網域控制站：

- **ExtranetLockoutRequirePDC &lt;布林 &gt; 值**-啟用時：外部網路鎖定需要網域主控站 (PDC) 。 停用時：如果 PDC 無法使用，外部網路鎖定將會回復到另一個網域控制站。

您可以使用下列 Windows PowerShell 命令來設定伺服器2016上的 AD FS 外部網路鎖定：

```powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30) -ExtranetLockoutRequirePDC $false
```

## <a name="working-with-the-active-directory-lockout-policy"></a>使用 Active Directory 鎖定原則
AD FS 中的外部網路鎖定功能獨立于 AD 鎖定原則。 不過，您必須確定已正確設定外部網路鎖定的設定，讓它能夠使用 AD 鎖定原則來提供其安全性目的。
讓我們先看看 AD 鎖定原則。 AD 中的鎖定原則有三個相關設定：
- **帳戶鎖定閾值**：此設定類似于 AD FS 中的 ExtranetLockoutThreshold 設定。 它會判斷導致使用者帳戶遭到鎖定的失敗登入嘗試次數。為了保護您的使用者帳戶免于遭受惡意帳戶鎖定攻擊，您想要在 AD 中 AD FS &lt; 帳戶鎖定閾值值，設定 ExtranetLockoutThreshold 的值。
- **帳戶鎖定期間**：此設定會決定使用者帳戶鎖定的時間長度。這項設定在此對話中並不重要，因為在設定正確的情況下進行 AD 鎖定之前，應該一律發生外部網路鎖定
- **重設帳戶鎖定計數器**：此設定會決定在將 **badPwdCount** 重設為0之前，使用者上次登入失敗必須經過多少時間。 為了讓 AD FS 中的外部網路鎖定功能能與 AD 鎖定原則搭配運作，您想要確定在 AD 中的 &gt; 值之後 AD FS 重設帳戶鎖定計數器中的 ExtranetObservationWindow 值。 下列範例將說明原因。

讓我們看看兩個範例，並根據不同的設定和狀態，查看 **badPwdCount** 如何隨時間改變。 讓我們假設兩個範例 **帳戶鎖定臨界值** = 4， **ExtranetLockoutThreshold** = 2。 **紅色** 箭號代表不正確的密碼嘗試，**綠色** 箭號代表良好的密碼嘗試。 例如 #1，之後的 **ExtranetObservationWindow** &gt; **重設帳戶鎖定計數器**。 例如 #2，之後的 **ExtranetObservationWindow** &lt; **重設帳戶鎖定計數器**。

### <a name="example-1"></a>範例 1
![此圖顯示 badPwdCount 如何根據不同的設定和狀態來變更時間。](media/Configure-AD-FS-Extranet-Lockout-Protection/one.png)

### <a name="example-2"></a>範例 2
![範例1](media/Configure-AD-FS-Extranet-Lockout-Protection/two.png)

如您在上面所見， **badPwdCount** 將重設為0時有兩種情況。 其中一個是登入成功。 另一種方式是在設定之後重設此計數器，如 **重設帳戶鎖定計數器** 中所定義。 在 **ExtranetObservationWindow 之後重設帳戶鎖定計數器** 時 &lt; ****，帳戶沒有任何受 AD 鎖定的風險。 但是，如果 **在 ExtranetObservationWindow 之後重設帳戶鎖定計數器** &gt; ****，可能會有帳戶被 AD 鎖定，但以「延遲的方式」表示。 視您的設定而定，可能需要一段時間才能取得 AD 鎖定的帳戶，因為 AD FS 在 **badPwdCount** 達到 **帳戶鎖定閾值** 之前，只會在其觀察期間內允許一個錯誤的密碼嘗試。

如需詳細資訊，請參閱設定 [帳戶鎖定](/archive/blogs/secguide/configuring-account-lockout)。

## <a name="known-issues"></a>已知問題
有一個已知的問題，AD 使用者帳戶無法向 AD FS 驗證，因為 **badPwdCount** 屬性不會複寫到 ADFS 正在查詢的網域控制站。 如需詳細資訊，請參閱 [2971171](https://support.microsoft.com/help/2971171/adfs-authentication-issue-for-active-directory-users-when-extranet-loc) 。 您可以在 [這裡](../deployment/updates-for-active-directory-federation-services-ad-fs.md)找到目前為止已發行的所有 AD FS qfe。

## <a name="key-points-to-remember"></a>要記住的重點
- 外部網路鎖定功能只適用于驗證要求通過 Web 應用程式 Proxy 的 **外部網路案例**
- 外部網路鎖定功能只適用于使用者 **名稱 & 密碼驗證**
- AD FS 不會追蹤已被鎖定的 **badPwdCount** 或使用者。AD FS 使用 AD 進行所有狀態追蹤
- AD FS 透過針對 PDC 上使用者的 LDAP 呼叫來執行 **badPwdCount** 屬性的查閱，以進行每個驗證嘗試
- 如果無法存取 PDC，則早于2016的 AD FS 將會失敗。 AD FS 2016 引進了改良功能，可讓 AD FS 在 PDC 無法使用時，切換回其他網域控制站。
- 如果 badPwdCount <，AD FS 會允許來自外部網路的驗證要求 ExtranetLockoutThreshold
- 如果 **badPwdCount**  >=  **ExtranetLockoutThreshold** 和 **badPasswordTime**  +  **ExtranetObservationWindow** < 目前時間，AD FS 將會拒絕來自外部網路的驗證要求
- 若要避免惡意帳戶鎖定，您應該確定 **ExtranetLockoutThreshold**  <  **帳戶鎖定閾值** 和 **ExtranetObservationWindow**  >  **重設帳戶鎖定計數器**


## <a name="additional-references"></a>其他參考資料
- [保護 Active Directory 同盟服務的最佳作法](../../ad-fs/deployment/best-practices-securing-ad-fs.md)
- [將 AD FS Powershell Cmdlet 存取權委派給非系統管理員使用者](delegate-ad-fs-pshell-access.md)
- [設定-Set-adfsproperties](/powershell/module/adfs/set-adfsproperties)

[AD FS 操作](../ad-fs-operations.md)
