---
title: 認證保護和管理
description: Windows Server 安全性
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-credential-protection
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e457229c-0126-40fe-948c-101c943e1b57
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 16cc1f2260bf0552da6902dc3e97de65d29c7931
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812199"
---
# <a name="credentials-protection-and-management"></a>認證保護和管理

>適用於：Windows Server （半年通道），Windows Server 2016

本主題適用於 IT 專業人員討論功能和 Windows Server 2012 R2 和 Windows 8.1 認證保護和網域驗證控制，以降低認證竊取中導入的方法。

## <a name="BKMK_CredentialsProtectionManagement"></a>
### <a name="restricted-admin-mode-for-remote-desktop-connection"></a>遠端桌面連線的受限制系統管理員模式
受限制的系統管理員模式提供互動式登入遠端主機伺服器而不需要將認證傳送到伺服器的方法。 如果伺服器已經被破壞，這個方法可以在初始連線程序期間防止別人蒐集到您的認證。

使用此模式搭配系統管理員認證時，遠端桌面用戶端會嘗試以互動方式登入也支援這種模式的主機，而不傳送認證。 當主機確認與它連線的使用者帳戶具有系統管理員權限並支援受限制的系統管理員模式時，連線就會成功。 否則，嘗試連線會失敗。 受限制的系統管理員模式任何時候都不會傳送純文字或其他可重複使用形式的認證給遠端電腦。

### <a name="lsa-protection"></a>LSA 保護
本機安全性授權 (LSA) (於本機安全性授權安全性服務 (LSASS) 處理程序中) 會驗證使用者的本機和遠端登入，以及強制執行本機安全性原則。 Windows 8.1 作業系統會提供額外的保護，以避免未受保護的處理序的程式碼插入為 lsa。 這為 LSA 儲存和管理的認證提供了額外的安全性。 此 lsa 的受保護的處理程序設定可以在 Windows 8.1 中設定 Windows RT 8.1 中的預設為開啟但無法變更。

如需相關資訊以了解如何設定 LSA 保護，請參閱 [Configuring Additional LSA Protection](configuring-additional-lsa-protection.md)。

### <a name="protected-users-security-group"></a>Protected Users 安全性群組
這個新的網域全域群組會觸發新的不可設定保護，在裝置和執行 Windows Server 2012 R2 和 Windows 8.1 的主機電腦上。 Protected Users 群組可讓網域控制站和網域在 Windows Server 2012 R2 網域中的其他防護措施。 這將大幅減少當使用者從未受入侵電腦登入網路上的電腦時可用憑證的類型。

Protected Users 群組的成員進一步受到下列驗證方法的限制：

-   Protected Users 群組的成員只能使用 Kerberos 通訊協定登入。 無法使用 NTLM、摘要式驗證或 CredSSP 驗證帳戶。 在執行 Windows 8.1 的裝置，密碼是未快取，因此使用任一這些安全性支援提供者 (Ssp) 的裝置將無法驗證網域，當帳戶是 Protected Users 群組的成員。

-   Kerberos 通訊協定不會在預先驗證處理程序中使用較弱的 DES 或 RC4 加密類型。 這表示網域必須設定為至少支援 AES 加密套件。

-   無法委派使用者的帳戶，透過 Kerberos 限制或未受限制委派。 這表示如果使用者是 Protected Users 群組的成員，先前連到其他系統的連線可能會失敗。

-   四個小時的預設 Kerberos 票證授與票證 (TGT) 存留期，可透過 Active Directory 管理中心 (ADAC) 使用驗證原則和定址接收器來加以設定。 這表示經過四小時之後，使用者就必須再驗證一次。

> [!WARNING]
> 服務和電腦的帳戶不應該是 Protected Users 群組的成員。 此群組不提供本機保護，因為密碼或憑證一律會在主機上。 驗證將會失敗並出現錯誤 「 使用者名稱或密碼不正確 」 的任何服務或新增到 Protected Users 群組的電腦。

如需此群組的詳細資訊，請參閱 [Protected Users 安全性群組](protected-users-security-group.md)。

### <a name="authentication-policy-and-authentication-policy-silos"></a>驗證原則和驗證原則定址接收器
導入樹系為基礎的 Active Directory 原則，並可將它們套用至 Windows Server 2012 R2 網域功能等級之網域中的帳戶。 這些驗證原則能控制使用者可以使用哪些主機來登入。 這些原則與 Protected Users 安全性群組搭配運作，且系統管理員可以套用存取控制條件來驗證帳戶。 這些驗證原則會隔離相關的帳戶來限制網路的範圍。

新 Active Directory 物件類別 「 驗證原則，可讓您驗證設定套用至具有 Windows Server 2012 R2 網域功能等級的網域中的帳戶類別。 Kerberos AS 或 TGS 交換期間會強制執行驗證原則。 Active Directory 帳戶類別包括：

-   使用者

-   Computer

-   受管理的服務帳戶

-   群組受管理的服務帳戶

如需詳細資訊，請參閱[驗證原則和驗證原則定址接收器](authentication-policies-and-authentication-policy-silos.md)。

如需如何設定受保護帳戶的詳細資訊，請參閱[如何設定受保護帳戶](how-to-configure-protected-accounts.md)。

## <a name="see-also"></a>另請參閱
如需 LSA 與 LSASS 的詳細資訊，請參閱 [Windows Logon and Authentication Technical Overview](https://technet.microsoft.com/library/dn169029(v=ws.10).aspx)(Windows 登入及驗證技術概觀)。



