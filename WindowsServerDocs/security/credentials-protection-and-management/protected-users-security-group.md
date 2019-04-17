---
title: "使用者安全性群組受保護狀態"
description: "Windows Server 安全性"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="protected-users-security-group"></a>使用者安全性群組受保護狀態

>適用於：Windows Server（以每年次管道）、Windows Server 2016

本主題適用於 IT 專業人員描述 Active Directory 安全性群組受保護的使用者，並解釋它的運作方式。 Windows Server 2012 R2 網域控制站推出此群組。

## <a name="BKMK_ProtectedUsers"></a>概觀

此安全性群組的設計目的是計一部份來管理 credential 曝光企業中。 此群組成員自動可套用至帳號非可設定的保護。 保護 Users 群組成員資格是要限制，以及主動預設安全。 修改這些系統的保護的唯一方式是移除 account 從安全性群組。

> [!WARNING]
> 帳號服務和電腦應該不會受 Users 群組成員。 此群組想吧提供完整的保護，因為密碼或憑證都可以在主機上使用。 驗證將會失敗，錯誤 \"the 使用者名稱或密碼 incorrect\」的服務或新增至受保護的使用者群組的電腦。

這個網域中相關、全域群組執行 Windows Server 2012 R2 的主要網域控制站觸發不可設定保護主機執行 Windows Server 2012 R2 和 Windows 8.1 的電腦上的裝置或更新版本的網域中的使用者。 當使用者登入以使用這些保護電腦，此大幅降低了認證的預設記憶體使用量。

如需詳細資訊，請查看[的受保護的使用者如何群組運作](#BKMK_HowItWorks)本主題中。



## <a name="BKMK_Requirements"></a>受保護的使用者群組需求
提供的受保護的 Users 群組成員裝置保護的需求包括：

- Account 網域中的所有網域控制站都複製保護使用者安全性的全域群組。

- Windows 8.1 和 Windows Server 2012 R2 預設新增支援。 [Microsoft 安全性建議 2871997](https://technet.microsoft.com/library/security/2871997) Windows 7、Windows Server 2008 R2 和 Windows Server 2012 中新增支援。

為保護 Users 群組成員網域控制站保護的需求包括：

- 使用者必須在 Windows Server 2012 R2 或更高版本網域功能層級的網域。

### <a name="adding-protected-user-global-security-group-to-down-level-domains"></a>向下層級的網域新增保護使用者安全性的全域群組

執行作業系統以前 Windows Server 2012 R2 網域控制站可支援新增新的受保護的使用者安全性群組成員。 這可讓使用者網域升級之前受益保護裝置。 

> [!Note]
> 網域控制站將不支援網域保護。 

您可以建立受保護的使用者群組[傳輸主要網域控制站 (PDC) 模擬器角色](https://technet.microsoft.com/library/cc816944(v=ws.10).aspx)執行 Windows Server 2012 R2 網域控制站。 其他網域控制站複製物件群組之後，可以在執行 Windows Server 的較舊版本的網域控制站裝載 PDC 模擬器角色。

### <a name="BKMK_ADgroup"></a>受保護的使用者群組 AD 屬性

下表指定保護 Users 群組的屬性。

|屬性|值。|
|-------|-----|
|已知的 SID RID|S-1-5-21-<domain>-525|
|輸入|全球網域|
|預設容器|DATA-CN = DC 的使用者，=<domain>、DC =|
|預設成員|無|
|預設成員|無|
|受 ADMINSDHOLDER 嗎？|否]|
|將退出預設容器安全嗎？|[是]|
|委派管理此群組非服務系統管理員可以放心嗎？|否]|
|預設的使用者權限|無預設使用者權限|

## <a name="BKMK_HowItWorks"></a>保護 Users 群組的運作方式
本章節解釋保護 Users 群組的運作方式時：

-   登入 Windows 裝置

-   使用者 account 網域是在 Windows Server 2012 R2 或更高版本網域功能層級

### <a name="device-protections-for-signed-in-protected-users"></a>適用於裝置保護登入的受保護的使用者
登入的使用者時的受保護的 Users 群組成員適下列保護：

-   認證委派 (CredSSP) 將不快取一般的使用者的認證即使**允許將預設的認證委派**可以使用群組原則設定。

-   開始使用 Windows 8.1 和 Windows Server 2012 R2、Windows 摘要將快取一般的使用者的認證即使讓 Windows 摘要。

> [!Note]
> 安裝之後[Microsoft 安全性建議 2871997](https://technet.microsoft.com/library/security/2871997)設定登錄鍵，直到摘要 Windows 將會繼續快取的認證。 查看[Microsoft 安全性建議：更新，以改善認證保護和管理：2014 年月 13 日](https://support.microsoft.com/en-us/help/2871997/microsoft-security-advisory-update-to-improve-credentials-protection-a)的指示操作。

-   一般的使用者的認證或 NT 單向函式 (NTOWF) NTLM 將快取。

-   Kerberos 將不會再建立 DES 或 RC4 按鍵。 也它將會快取使用者的認證一般或長期按鍵之後，取得初始 TGT。

-   快取的驗證器不會建立在登入或解除鎖定，因此不支援離線登入。

使用者 account 新增至受保護的使用者群組之後，登入的裝置時，會開始保護。

### <a name="domain-controller-protections-for-protected-users"></a>保護使用者網域控制站保護
Windows Server 2012 R2 網域驗證保護 Users 群組成員帳號無法︰

-   驗證 NTLM 驗證。

-   用於 F:kerberos 預先驗證 DES 或 RC4 加密類型。

-   使用未限制或限制委派委派。

-   續約超過四小時的時間初始期間 Kerberos Tgt。

Tgt 到期日非可設定的設定建立的每個 account 保護 Users 群組。 網域控制站一樣，會將 Tgt 期間和續約，根據網域的原則，**的最大值期間使用者票證**和**使用者票證更新的最大值期間**。 「受保護的使用者群組中，這些網域原則設定 600 分鐘。

如需詳細資訊，請查看[設定保護帳號如何](how-to-configure-protected-accounts.md)。

## <a name="troubleshooting"></a>疑難排解
有兩個操作管理登了可幫助疑難排解受保護的使用者相關的活動。 這些新登位於事件檢視器都預設停用，並位於**的應用程式與服務 Logs\Microsoft\Windows\Microsoft\Authentication**。

|事件 ID 和登入|描述|
|----------|--------|
|104<br /><br />**ProtectedUser 工作**|理由：上 client security 套件不包含認證。<br /><br />這個錯誤是登入 client 電腦中 account 時的受保護的使用者安全性群組成員。 此事件表示安全性套件不會快取會需要驗證伺服器的憑證。<br /><br />顯示套件名稱、使用者名稱、網域名稱和伺服器的名稱。|
|304<br /><br />**ProtectedUser 工作**|理由：安全性套件不會儲存受保護的使用者的認證。<br /><br />事件資訊被登入 client 指出安全性套件不會快取使用者的認證登入。 它會如預期般摘要 (WDigest)、認證委派 (CredSSP)，以及 NTLM 無法登入認證的受保護的使用者。 如果他們提示您輸入認證，仍然可以成功應用程式。<br /><br />顯示套件名稱、使用者名稱和網域名稱。|
|100<br /><br />**ProtectedUserFailures-DomainController**|理由：帳號保護使用者安全性群組中的發生 NTLM 登入失敗。<br /><br />錯誤被登入網域控制站表示 NTLM 驗證失敗 account 因為的受保護的使用者安全性群組成員。<br /><br />顯示 account 名稱及裝置的名稱。|
|104<br /><br />**ProtectedUserFailures-DomainController**|理由：DES 或 RC4 加密類型用於 F:kerberos 驗證，發生保護使用者安全性群組中的使用者登入失敗。<br /><br />Kerberos 預先驗證失敗，因為的受保護的使用者安全性群組成員 account 時無法使用 DES 和 RC4 加密類型。<br /><br />（好一段是接受）。|
|303<br /><br />**ProtectedUserSuccesses-DomainController**|理由：的受保護的使用者群組成員成功發出 Kerberos 票證授與-票 (TGT)。|



## <a name="additional-resources"></a>其他資源

-   [認證保護與管理](credentials-protection-and-management.md)

-   [驗證原則和驗證原則筒倉](authentication-policies-and-authentication-policy-silos.md)

-   [如何設定帳號受保護狀態](how-to-configure-protected-accounts.md)


