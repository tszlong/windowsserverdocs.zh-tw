---
title: Protected Users 安全性群組
description: Windows Server 安全性
ms.prod: windows-server
ms.technology: security-credential-protection
ms.topic: article
ms.assetid: 1b0b5180-f65a-43ac-8ef3-66014116f296
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: c6883513fdc02f4f4d1b874995780639279cc178
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857051"
---
# <a name="protected-users-security-group"></a>Protected Users 安全性群組

> 適用於：Windows Server (半年通道)、Windows Server 2016

這個 IT 專業人員主題描述 Active Directory 的 Protected Users 安全性群組，並說明它的運作方式。 此群組是在 Windows Server 2012 R2 網域控制站中引進。

## <a name="overview"></a><a name="BKMK_ProtectedUsers"></a>簡要

此安全性群組的設計是管理企業內認證暴露的策略之一部分。 此群組的成員帳戶會自動套用不可設定的保護。 Protected Users 群組中的成員資格預設應該要有限制性和主動防護。 修改帳戶的這些保護的唯一方式就是從安全性群組移除帳戶。

> [!WARNING]
> 服務和電腦的帳戶不應該是 Protected Users 群組的成員。 此群組仍提供不完整的保護，因為主機上的密碼或憑證一律可供使用。 驗證會失敗並出現錯誤 \"使用者名稱或密碼不正確，\" 適用于新增至 Protected Users 群組的任何服務或電腦。

此網域相關的全域群組會在執行 windows Server 2012 R2 的裝置和主機電腦上觸發不可設定的保護，並針對執行 Windows Server 2012 R2 的主域控制站的網域中的使用者，在 Windows 8.1 或更新版本。 當使用者登入具有這些保護的電腦時，這可大幅減少認證的預設記憶體使用量。

如需詳細資訊，請參閱本主題中[的 Protected Users 群組的運作方式](#BKMK_HowItWorks)。


## <a name="protected-users-group-requirements"></a><a name="BKMK_Requirements"></a>Protected Users 群組需求
為 Protected Users 群組的成員提供裝置保護的需求包括：

- Protected Users 全域安全性群組已複寫到帳戶網域中的所有網域控制站。

- Windows 8.1 和 Windows Server 2012 R2 預設會新增支援。 [Microsoft 安全性諮詢 2871997](https://technet.microsoft.com/library/security/2871997)新增對 windows 7、windows Server 2008 R2 和 windows server 2012 的支援。

為 Protected Users 群組的成員提供網域控制站保護的需求包括：

- 使用者必須位於 Windows Server 2012 R2 或更高網域功能等級的網域中。

### <a name="adding-protected-user-global-security-group-to-down-level-domains"></a>將受保護的使用者全域安全性群組新增至下層網域

執行 Windows Server 2012 R2 之前的作業系統的網域控制站，可以支援將成員新增至新的受保護使用者安全性群組。 這可讓使用者在升級網域之前，先受益于裝置保護。 

> [!Note]
> 網域控制站將不支援網域保護。 

藉由將[主域控制站（PDC）模擬器角色轉移](https://technet.microsoft.com/library/cc816944(v=ws.10).aspx)至執行 Windows Server 2012 R2 的網域控制站，即可建立 Protected Users 群組。 該群組物件複寫到其他網域控制站之後，可以在執行舊版 Windows Server 的網域控制站上裝載 PDC 模擬器角色。

### <a name="protected-users-group-ad-properties"></a><a name="BKMK_ADgroup"></a>Protected Users 群組 AD 屬性

下表指定 Protected Users 群組的屬性。

|屬性|值|
|-------|-----|
|已知的 SID/RID|S-1-5-21-<domain>-525|
|類型|網域全域|
|預設容器|CN=Users, DC=<domain>, DC=|
|預設成員|無|
|下列群組的預設成員|無|
|是否受到 ADMINSDHOLDER 保護？|否|
|是否可從預設容器放心地移出？|是|
|是否可將此群組的管理放心地委派給非服務系統管理員？|否|
|預設使用者權利|沒有預設的使用者權利|

## <a name="how-protected-users-group-works"></a><a name="BKMK_HowItWorks"></a>Protected Users 群組的運作方式
本節說明 Protected Users 群組在下列情況下的運作方式：

- 已登入 Windows 裝置

- 使用者帳戶域位於 Windows Server 2012 R2 或更高版本的網域功能等級

### <a name="device-protections-for-signed-in-protected-users"></a>已登入受保護使用者的裝置保護
當已登入的使用者是 Protected Users 群組的成員時，會套用下列保護：

- 即使已啟用 [**允許委派預設認證**] 群組原則設定，認證委派（CredSSP）也不會快取使用者的純文字認證。

- 從 Windows 8.1 和 Windows Server 2012 R2 開始，即使啟用 Windows 摘要，Windows 摘要式也不會快取使用者的純文字認證。

> [!Note]
> 安裝[Microsoft 安全性諮詢 2871997](https://technet.microsoft.com/library/security/2871997)之後，Windows 摘要式會繼續快取認證，直到設定登錄機碼為止。 如需指示，請參閱[Microsoft 安全性摘要報告：更新以改善認證保護和管理： 2014 5 月13日](https://support.microsoft.com/help/2871997/microsoft-security-advisory-update-to-improve-credentials-protection-a)。

- NTLM 不會快取使用者的純文字認證或 NT 單向函式（NTOWF）。

- Kerberos 將不再建立 DES 或 RC4 金鑰。 此外，在取得初始 TGT 之後，它也不會快取使用者的純文字認證或長期金鑰。

- 在登入或解除鎖定時，不會建立快取的驗證器，因此不再支援離線登入。

將使用者帳戶新增到 Protected Users 群組後，將會在使用者登入裝置時開始保護。

### <a name="domain-controller-protections-for-protected-users"></a>受保護使用者的網域控制站保護
對 Windows Server 2012 R2 網域進行驗證的 Protected Users 群組成員的帳戶無法：

- 使用 NTLM 驗證進行驗證。

- 在 Kerberos 預先驗證中使用 DES 或 RC4 加密類型。

- 以非限制或限制委派方式受到委派。

- 超過初始的四小時存留期才更新 Kerberos TGT。

系統會為 Protected Users 群組中的每個帳戶建立 TGT 到期時間的不可設定的設定。 一般而言，網域控制站會根據網域原則、[使用者票證最長存留期] 及 [使用者票證更新的最長存留期]，設定 TGT 存留期和更新。 以 Protected Users 群組來說，會為這些網域原則設定 600 分鐘。

如需詳細資訊，請參閱[如何設定受保護的帳戶](how-to-configure-protected-accounts.md)。

## <a name="troubleshooting"></a>疑難排解
有兩個操作系統管理記錄檔可用來協助排解 Protected Users 相關事件的疑難。 這些新的記錄檔位於事件檢視器中，而且預設為停用，且位於 [**應用程式和服務 Logs\Microsoft\Windows\Authentication**] 底下。

|事件識別碼和記錄檔|描述|
|----------|--------|
|104<p>**ProtectedUser-Client**|原因：用戶端上的安全性套件未包含認證。<p>當帳戶是 Protected Users 安全性群組的成員時，錯誤會記錄在用戶端電腦上。 這個事件指出安全性封裝並未快取向伺服器驗證時所需的認證。<p>會顯示封裝名稱、使用者名稱、網域名稱及伺服器名稱。|
|304<p>**ProtectedUser-Client**|原因：安全性封裝不會儲存受保護使用者的認證。<p>用戶端中會記錄資訊事件，以指出安全性封裝不會快取使用者的登入認證。 預期摘要 (WDigest)、認證委派 (CredSSP) 及 NTLM 無法擁有 Protected Users 的登入認證。 如果應用程式提示輸入認證，則仍然可以登入成功。<p>會顯示封裝名稱、使用者名稱及網域名稱。|
|100<p>**ProtectedUserFailures-DomainController**|原因：受保護使用者安全性群組中的帳戶發生 NTLM 登入失敗。<p>錯誤會記錄在網域控制站中，指出因為帳戶是 Protected Users 安全性群組的成員，所以 NTLM 驗證失敗。<p>會顯示帳戶名稱和裝置名稱。|
|104<p>**ProtectedUserFailures-DomainController**|原因： DES 或 RC4 加密類型用於 Kerberos 驗證，而受保護使用者安全性群組中的使用者發生登入失敗。<p>Kerberos 預先驗證失敗，因為當帳戶是 Protected Users 安全性群組的成員時，不能使用 DES 與 RC4 加密類型。<p>(可以接受 AES)。|
|303<p>**ProtectedUserSuccesses-DomainController**|原因：已成功為受保護使用者群組的成員發出 Kerberos 票證授權票證（TGT）。|


## <a name="additional-resources"></a>其他資源

- [認證保護和管理](credentials-protection-and-management.md)

- [驗證原則和驗證原則定址接收器](authentication-policies-and-authentication-policy-silos.md)

- [如何設定受保護的帳戶](how-to-configure-protected-accounts.md)
