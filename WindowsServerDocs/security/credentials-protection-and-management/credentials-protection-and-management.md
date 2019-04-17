---
title: "認證保護與管理"
description: "Windows Server 安全性"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="credentials-protection-and-management"></a>認證保護與管理

>適用於：Windows Server（以每年次管道）、Windows Server 2016

本主題適用於 IT 專業人員討論功能與 Windows Server 2012 R2 和 Windows 8.1 認證保護和網域驗證控制項，以減少認證竊取導入了的方法。

## <a name="BKMK_CredentialsProtectionManagement"></a>
### <a name="restricted-admin-mode-for-remote-desktop-connection"></a>遠端桌面連接限制的管理模式
限制的管理模式提供的互動方式登入至主機的遠端伺服器不傳輸到伺服器認證的方法。 如此可防止認證如果伺服器洩漏此初始連接程序期間所收集。

遠端桌面 client 以系統管理員認證使用此模式，請嘗試互動方式登入的主機，也不會傳送認證支援此模式。 主機驗證使用者 account 連接具有系統管理員權限，以及支援限制的管理模式，當連接成功。 否則，連接失敗。 不在任何點傳送一般或認證遠端電腦的其他重複使用形式，會限制的管理模式。

### <a name="lsa-protection"></a>LSA 保護
本機安全性授權單位 (LSA)，位於在本機安全性授權單位安全性服務 (LSASS) 處理程序，驗證使用者的本機和遠端登入增益集，並執行本機安全性原則。 Windows 8.1 作業系統提供額外的保護的程式碼插入避免由未受保護的處理程序 LSA。 新增的安全性提供的認證 LSA 儲存和管理。 LSA 程序受保護的設定可以設定 Windows 8.1 中，但在 Windows RT 8.1 中的預設和無法變更。

適用於設定 LSA 保護的相關資訊，請查看[設定額外的 LSA 保護](configuring-additional-lsa-protection.md)。

### <a name="protected-users-security-group"></a>使用者安全性群組受保護狀態
這個新的網域的全域群組觸發主機電腦執行的 Windows Server 2012 R2 和 Windows 8.1 的裝置上新的非可設定保護。 保護 Users 群組可讓您的網域控制站和 Windows Server 2012 R2 網域中的網域額外的保護。 使用者登入電腦在網路上的非洩漏電腦時，此大幅降低了認證可用的類型。

有限制的受保護的 Users 群組成員進一步的驗證下列方法：

-   受保護的 Users 群組成員只可以使用 Kerberos 通訊協定來登入。 Account 無法使用 NTLM、摘要驗證或 CredSSP 進行驗證。 在執行 Windows 8.1 的裝置上的密碼不快取，讓您的裝置使用其中一種這些安全性支援提供者（層），將無法驗證網域 account 時的受保護的使用者群組成員。

-   Kerberos 通訊協定不會預先驗證程序使用較弱 DES 或 RC4 加密類型。 這表示支援至少好一段 cypher 套件，必須設定的網域。

-   無法使用 Kerberos 限制或受限制地委派帳號委派。 這表示如果使用者群組成員的受保護的使用者先前連接到其他系統可能會失敗。

-   四小時的預設 Kerberos 票證授與門票 (Tgt) 期間設定已可使用驗證原則和筒倉存取透過 Active Directory 系統管理員中心 (ADAC) 設定。 這表示當超過四小時的時間，使用者必須驗證再試一次。

> [!WARNING]
> 帳號服務和電腦不應該保護 Users 群組成員。 此群組的密碼或憑證都可在主機上，因為不提供任何本機的保護。 將會失敗，驗證，發生錯誤」的使用者名稱或密碼不正確「適用於任何服務或新增至受保護的使用者群組的電腦。

如需此群組的詳細資訊，請查看[受保護的使用者安全性群組](protected-users-security-group.md)。

### <a name="authentication-policy-and-authentication-policy-silos"></a>驗證原則和驗證原則筒倉
為基礎的樹系的 Active Directory 原則的推出，並可用於帳號和 Windows Server 2012 R2 網域功能層級網域中。 這些驗證原則可以控制要主機使用者可以用來登入。 保護使用者安全性群組搭配運作，系統管理員可以將帳號套用驗證存取控制項條件。 這些驗證原則隔離相關的帳號，將網路的範圍。

新 Active Directory 物件課程，驗證原則，可讓您與 Windows Server 2012 R2 網域功能等級網域中 account 類別適用於驗證設定。 驗證原則的 Kerberos 為或 TGS 期間執行換貨。 Active Directory account 類別︰

-   使用者

-   電腦

-   管理 Account 服務

-   群組多媒體受管理的服務 Account

如需詳細資訊，請查看[驗證原則和驗證原則筒倉](authentication-policies-and-authentication-policy-silos.md)。

如需如何設定保護帳號，查看[設定保護帳號如何](how-to-configure-protected-accounts.md)。

## <a name="see-also"></a>也了
如需有關 LSA LSASS，請查看[Windows 登入和技術驗證的概觀](https://technet.microsoft.com/library/dn169029(v=ws.10).aspx)。



