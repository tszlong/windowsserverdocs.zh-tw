---
title: What's New in Kerberos Authentication
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: a0916abf1076b5f791a856f0c85f54ad17f6d64c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403481"
---
# <a name="whats-new-in-kerberos-authentication"></a>What's New in Kerberos Authentication

>適用於：Windows Server 2016 和 Windows 10

## <a name="kdc-support-for-public-key-trust-based-client-authentication"></a>以公開金鑰信任為基礎之用戶端驗證的 KDC 支援

從 Windows Server 2016 開始，Kdc 支援一種公開金鑰對應方式。 如果為帳戶布建公開金鑰，則 KDC 會使用該金鑰明確支援 Kerberos PKInit。 由於沒有憑證驗證，因此支援自我簽署憑證，且不支援驗證機制保證。

設定帳戶時，無論 UseSubjectAltName 設定為何，都建議使用金鑰信任。

## <a name="kerberos-client-and-kdc-support-for-rfc-8070-pkinit-freshness-extension"></a>RFC 8070 PKInit Freshness Extension 的 Kerberos 用戶端和 KDC 支援

從 Windows 10 版本1607和 Windows Server 2016 開始，Kerberos 用戶端會針對公開金鑰型的登入嘗試[RFC 8070 PKInit](https://datatracker.ietf.org/doc/draft-ietf-kitten-pkinit-freshness/)有效期限延伸模組。 

從 Windows Server 2016 開始，Kdc 可以支援 PKInit freshness extension。 根據預設，Kdc 不提供 PKInit freshness extension。 若要啟用它，請在網域中的所有 Dc 上，使用 [PKInit 有效擴充功能的新 KDC 支援] KDC 系統管理範本原則設定。 設定時，當網域是 Windows Server 2016 網域功能等級（DFL）時，會支援下列選項：

- **已停用**：KDC 永遠不會提供 PKInit Freshness Extension，並接受有效的驗證要求，而不會檢查時效性。 使用者永遠不會收到全新的公開金鑰識別 SID。
- **支援**：要求支援 PKInit 時效性延伸模組。 Kerberos 用戶端成功向 PKInit Freshness Extension 進行驗證時，會收到全新的公開金鑰識別 SID。
- **必要**：成功驗證需要 PKInit 的有效延伸模組。 當使用公開金鑰認證時，不支援 PKInit freshness Extension 的 Kerberos 用戶端一律會失敗。

## <a name="domain-joined-device-support-for-authentication-using-public-key"></a>已加入網域的裝置支援使用公開金鑰進行驗證

從 Windows 10 版本1507和 Windows Server 2016 開始，如果已加入網域的裝置能夠向 Windows Server 2016 網域控制站（DC）註冊其系結的公開金鑰，則裝置可以使用 Kerberos 驗證來驗證公開金鑰Windows Server 2016 DC。 如需詳細資訊，請參閱已[加入網域的裝置公用金鑰驗證](Domain-joined-Device-Public-Key-Authentication.md)

## <a name="kerberos-clients-allow-ipv4-and-ipv6-address-hostnames-in-service-principal-names-spns"></a>Kerberos 用戶端允許服務主體名稱（Spn）中的 IPv4 和 IPv6 位址主機名稱

從 Windows 10 版本1507和 Windows Server 2016 開始，可以將 Kerberos 用戶端設定為支援 Spn 中的 IPv4 和 IPv6 主機名稱。 

登錄路徑：

HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\Kerberos\Parameters

若要設定 Spn 中 IP 位址主機名稱的支援，請建立 TryIPSPN 專案。 此項目依預設不存在於登錄中。 建立專案之後，請將 DWORD 值變更為1。 如果未設定，則不會嘗試 IP 位址主機名稱。

如果 SPN 已在 Active Directory 中註冊，則驗證會以 Kerberos 成功。 

如需詳細資訊，請參閱設定[IP 位址的 Kerberos](configuring-kerberos-over-ip.md)檔。

## <a name="kdc-support-for-key-trust-account-mapping"></a>金鑰信任帳戶對應的 KDC 支援

從 Windows Server 2016 開始，網域控制站支援金鑰信任帳戶對應，以及在 SAN 行為中回復到現有的 AltSecID 和使用者主體名稱（UPN）。 當 UseSubjectAltName 設定為時：

- 0明確對應是必要的。 然後必須有下列其中一項：
    - 金鑰信任（Windows Server 2016 的新密碼）
    - ExplicitAltSecID
- 1：允許隱含對應（預設值）：
    1. 如果已針對 account 設定金鑰信任，則會使用它來進行對應（Windows Server 2016 的新程式）。
    2. 如果 SAN 中沒有 UPN，則會嘗試進行對應的 AltSecID。
    3. 如果 SAN 中有 UPN，則會嘗試使用 UPN 進行對應。

## <a name="see-also"></a>另請參閱

- [Kerberos 驗證總覽](kerberos-authentication-overview.md)
