---
title: What's New in Kerberos Authentication
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
author: justinha
ms.author: Justinha
ms.date: 11/09/2016
ms.openlocfilehash: eac9e1abd2891b21e818eac1d69b0eac231a02b4
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078535"
---
# <a name="whats-new-in-kerberos-authentication"></a>What's New in Kerberos Authentication

>適用于： Windows Server 2016 和 Windows 10

## <a name="kdc-support-for-public-key-trust-based-client-authentication"></a>以公開金鑰信任為基礎的用戶端驗證的 KDC 支援

從 Windows Server 2016 開始，Kdc 支援公開金鑰對應的方式。
如果為帳戶布建公開金鑰，則 KDC 會使用該金鑰明確支援 Kerberos PKInit。
由於沒有憑證驗證，因此支援自我簽署憑證，且不支援驗證機制保證。

無論 UseSubjectAltName 設定為何，在設定帳戶時，偏好使用金鑰信任。

## <a name="kerberos-client-and-kdc-support-for-rfc-8070-pkinit-freshness-extension"></a>RFC 8070 PKInit 有效期限擴充功能的 Kerberos 用戶端和 KDC 支援

從 Windows 10，1607版和 Windows Server 2016 開始，Kerberos 用戶端會針對公開金鑰型的登入嘗試 [RFC 8070 PKInit](https://datatracker.ietf.org/doc/draft-ietf-kitten-pkinit-freshness/) 有效期限延伸模組。

從 Windows Server 2016 開始，Kdc 可支援 PKInit 有效期限延伸模組。
根據預設，Kdc 不提供 PKInit 有效期限延伸模組。 若要啟用此功能，請在網域中的所有網域控制站上，使用 [對 PKInit 有效延伸的 kdc 系統管理範本的新 KDC] 管理範本原則設定。
設定時，當網域為 Windows Server 2016 網域功能等級 (DFL) 時，支援下列選項：

- **Disabled**： KDC 永遠不會提供 PKInit 有效期限延伸模組，並且接受有效的驗證要求，而不檢查有效性。 使用者將永遠不會收到新的公開金鑰識別 SID。
- **支援**：要求時支援 PKInit 有效期限延伸模組。 Kerberos 用戶端成功向 PKInit 有效期限延伸驗證，會收到新的公開金鑰識別 SID。
- **必要**：成功驗證需要 PKInit 有效期限延伸模組。 使用公開金鑰認證時，不支援 PKInit 有效期限擴充的 Kerberos 用戶端一律會失敗。

## <a name="domain-joined-device-support-for-authentication-using-public-key"></a>已加入網域的裝置支援使用公開金鑰進行驗證

從 Windows 10 1507 版和 Windows Server 2016 開始，如果已加入網域的裝置能夠將其系結的公開金鑰註冊到 Windows Server 2016 網域控制站 (DC) ，則裝置可以使用 Kerberos 驗證對 Windows Server 2016 DC 進行驗證。 如需詳細資訊，請參閱已 [加入網域的裝置公開金鑰驗證](Domain-joined-Device-Public-Key-Authentication.md)

## <a name="kerberos-clients-allow-ipv4-and-ipv6-address-hostnames-in-service-principal-names-spns"></a>Kerberos 用戶端允許服務主體名稱中的 IPv4 和 IPv6 位址主機名稱 (Spn) 

從 Windows 10 1507 版和 Windows Server 2016 開始，可以將 Kerberos 用戶端設定為支援 Spn 中的 IPv4 和 IPv6 主機名稱。

登錄路徑︰

HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\Kerberos\Parameters

若要設定 Spn 中 IP 位址主機名稱的支援，請建立 TryIPSPN 專案。
此項目依預設不存在於登錄中。
在您建立專案之後，請將 DWORD 值變更為1。
如果未設定，則不會嘗試 IP 位址主機名稱。

如果 SPN 已在 Active Directory 中註冊，則會使用 Kerberos 成功驗證。

如需詳細資訊，請參閱設定 [Kerberos 以取得 IP 位址](configuring-kerberos-over-ip.md)的檔。

## <a name="kdc-support-for-key-trust-account-mapping"></a>Key Trust 帳戶對應的 KDC 支援

從 Windows Server 2016 開始，網域控制站支援金鑰信任帳戶對應，以及回復至現有的 AltSecID 和使用者主體名稱 (UPN) 在 SAN 行為中。 當 UseSubjectAltName 設定為：

- 0：需要明確對應。 然後必須有下列其中一個：
    - Windows Server 2016 的新 (新) 的金鑰信任
    - ExplicitAltSecID
- 1：允許隱含對應 (預設) ：
    1. 如果已針對帳戶設定金鑰信任，則會使用它來對應 (新的 Windows Server 2016) 。
    2. 如果 SAN 中沒有 UPN，則會嘗試進行對應的 AltSecID。
    3. 如果 SAN 中有 UPN，則會嘗試 UPN 以進行對應。

## <a name="see-also"></a>另請參閱

- [Kerberos Authentication Overview](kerberos-authentication-overview.md)
