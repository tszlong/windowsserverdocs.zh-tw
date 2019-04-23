---
title: Protected Users 安全性群組
description: Windows Server 安全性
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-credential-protection
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b0b5180-f65a-43ac-8ef3-66014116f296
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: bd6b53c0febdfb2d344136097a9654c981405568
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862359"
---
# <a name="protected-users-security-group"></a>Protected Users 安全性群組

>適用於：Windows Server （半年通道），Windows Server 2016

這個 IT 專業人員主題描述 Active Directory 的 Protected Users 安全性群組，並說明它的運作方式。 此群組已採用 Windows Server 2012 R2 網域控制站。

## <a name="BKMK_ProtectedUsers"></a>概觀

此安全性群組被設計策略的一部分，來管理企業內認證暴露。 此群組的成員帳戶會自動套用不可設定的保護。 Protected Users 群組中的成員資格預設應該要有限制性和主動防護。 修改帳戶的這些保護的唯一方式就是從安全性群組移除帳戶。

> [!WARNING]
> 服務與電腦帳戶永遠不應該是 Protected Users 群組的成員。 此群組會仍提供完整的保護，因為密碼或憑證一律在主機上使用。 驗證會失敗並出現錯誤\"的使用者名稱或密碼不正確\"任何服務或新增到 Protected Users 群組的電腦。

此網域相關的全域群組會觸發不可設定的保護裝置和執行 Windows Server 2012 R2 和 Windows 8.1 的主機電腦上，或更新版本的網域中的使用者，主要網域控制站執行 Windows Server 2012 R2。 這可大幅減少認證的預設記憶體使用量，當使用者登入這些保護的電腦。

如需詳細資訊，請參閱 < [Protected Users 群組運作的方式](#BKMK_HowItWorks)本主題中。



## <a name="BKMK_Requirements"></a>受保護的使用者群組需求
若要提供的 Protected Users 群組成員的裝置保護的需求包括：

- Protected Users 全域安全性群組已複寫到帳戶網域中的所有網域控制站。

- Windows 8.1 和 Windows Server 2012 R2 預設新增支援。 [Microsoft 安全性諮詢 2871997](https://technet.microsoft.com/library/security/2871997)將支援新增至 Windows 7、 Windows Server 2008 R2 和 Windows Server 2012。

為 Protected Users 群組的成員提供網域控制站保護的需求包括：

- 使用者必須是 Windows Server 2012 R2 或更高版本的網域功能等級的網域中。

### <a name="adding-protected-user-global-security-group-to-down-level-domains"></a>將 Protected Users 全域安全性群組至下層網域

執行作業系統早於 Windows Server 2012 R2 的網域控制站可以支援新增至新的 Protected Users 安全性群組的成員。 這可讓使用者可以享受裝置保護，然後在升級網域。 

> [!Note]
> 網域控制站將不支援網域保護。 

可以建立受保護的使用者群組[傳輸網域主控站 (PDC) 模擬器角色](https://technet.microsoft.com/library/cc816944(v=ws.10).aspx)為執行 Windows Server 2012 R2 的網域控制站。 該群組物件複寫到其他網域控制站之後，可以在執行舊版 Windows Server 的網域控制站上裝載 PDC 模擬器角色。

### <a name="BKMK_ADgroup"></a>受保護的使用者群組的 AD 屬性

下表指定 Protected Users 群組的屬性。

|屬性|值|
|-------|-----|
|已知的 SID/RID|S-1-5-21-<domain>-525|
|類型|網域全域|
|預設容器|CN=Users, DC=<domain>, DC=|
|預設成員|None|
|下列群組的預設成員|None|
|是否受到 ADMINSDHOLDER 保護？|否|
|是否可從預設容器放心地移出？|是|
|是否可將此群組的管理放心地委派給非服務系統管理員？|否|
|預設使用者權限|沒有預設使用者權限|

## <a name="BKMK_HowItWorks"></a>Protected Users 群組的運作方式
本節說明 Protected Users 群組在下列情況下的運作方式：

-   登入 Windows 裝置

-   使用者帳戶的網域是在 Windows Server 2012 R2 或更高版本的網域功能等級

### <a name="device-protections-for-signed-in-protected-users"></a>登入受保護的使用者的裝置保護
當登入的使用者是 Protected Users 群組的成員會套用下列保護：

-   認證委派 (CredSSP) 將未快取使用者的純文字認證進行驗證，即使**允許委派預設認證**啟用群組原則設定。

-   從 Windows 8.1 和 Windows Server 2012 R2 開始，Windows 摘要不會快取使用者的純文字認證即使啟用了 Windows 摘要。

> [!Note]
> 在安裝後[Microsoft 安全性諮詢 2871997](https://technet.microsoft.com/library/security/2871997)必須等到已設定的登錄機碼，將會繼續快取認證的 Windows 摘要。 請參閱[Microsoft 安全性摘要報告：改善認證保護和管理的更新：2014 5 月 13 日](https://support.microsoft.com/en-us/help/2871997/microsoft-security-advisory-update-to-improve-credentials-protection-a)如需相關指示。

-   NTLM 不會快取，使用者的純文字認證或 NT 單向函式 (NTOWF)。

-   Kerberos 將無法再建立 DES 或 RC4 金鑰。 也就不會快取使用者的純文字認證進行驗證或長期金鑰之後會取得初始 TGT。

-   快取驗證器不會建立在登入，或解除鎖定，因此不再支援離線登入。

使用者帳戶新增到 Protected Users 群組之後，當使用者登入裝置時，會開始保護。

### <a name="domain-controller-protections-for-protected-users"></a>Protected Users 的網域控制站保護
Windows Server 2012 R2 網域進行驗證的 Protected Users 群組成員的帳戶是無法：

-   使用 NTLM 驗證進行驗證。

-   在 Kerberos 預先驗證中使用 DES 或 RC4 加密類型。

-   以非限制或限制委派方式受到委派。

-   超過初始的四小時存留期才更新 Kerberos TGT。

系統會為 Protected Users 群組中的每個帳戶建立 TGT 到期時間的不可設定的設定。 一般而言，網域控制站會根據網域原則、[使用者票證最長存留期] 及 [使用者票證更新的最長存留期]，設定 TGT 存留期和更新。 以 Protected Users 群組來說，會為這些網域原則設定 600 分鐘。

如需詳細資訊，請參閱[如何設定受保護的帳戶](how-to-configure-protected-accounts.md)。

## <a name="troubleshooting"></a>疑難排解
有兩個操作系統管理記錄檔可用來協助排解 Protected Users 相關事件的疑難。 這些新的記錄檔位於「事件檢視器」中且預設為停用，位於 [應用程式及服務記錄檔\Microsoft\Windows\Microsoft\驗證] 底下。

|事件識別碼和記錄檔|描述|
|----------|--------|
|104<br /><br />**ProtectedUser-Client**|原因：在用戶端的安全性封裝沒有包含認證。<br /><br />當帳戶是 Protected Users 安全性群組的成員時，錯誤會記錄在用戶端電腦上。 這個事件指出安全性封裝並未快取向伺服器驗證時所需的認證。<br /><br />會顯示封裝名稱、使用者名稱、網域名稱及伺服器名稱。|
|304<br /><br />**ProtectedUser-Client**|原因：安全性封裝沒有儲存 Protected User 的認證。<br /><br />參考事件會記錄在用戶端，指出安全性封裝不會快取使用者的登入認證。 預期摘要 (WDigest)、認證委派 (CredSSP) 及 NTLM 無法擁有 Protected Users 的登入認證。 如果應用程式提示輸入認證，則仍然可以登入成功。<br /><br />會顯示封裝名稱、使用者名稱及網域名稱。|
|100<br /><br />**ProtectedUserFailures-DomainController**|原因：Protected Users 安全性群組中的帳戶發生 NTLM 登入失敗。<br /><br />錯誤會記錄在網域控制站中，指出因為帳戶是 Protected Users 安全性群組的成員，所以 NTLM 驗證失敗。<br /><br />會顯示帳戶名稱和裝置名稱。|
|104<br /><br />**ProtectedUserFailures-DomainController**|原因：使用 DES 或 RC4 加密類型進行 Kerberos 驗證，而 Protected Users 安全性群組中的使用者發生登入失敗。<br /><br />Kerberos 預先驗證失敗，因為當帳戶是 Protected Users 安全性群組的成員時，不能使用 DES 與 RC4 加密類型。<br /><br />(可以接受 AES)。|
|303<br /><br />**ProtectedUserSuccesses-DomainController**|原因：已成功為 Protected Users 群組的成員發出 Kerberos 票證授權票證 (TGT)。|



## <a name="additional-resources"></a>其他資源

-   [認證保護和管理](credentials-protection-and-management.md)

-   [驗證原則和驗證原則定址接收器](authentication-policies-and-authentication-policy-silos.md)

-   [如何設定受保護的帳戶](how-to-configure-protected-accounts.md)


