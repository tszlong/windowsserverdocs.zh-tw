---
title: Kerberos 驗證最新消息
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: 7b046490c606cdf9e1436f503bf46a9cd4280ea9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831089"
---
# <a name="whats-new-in-kerberos-authentication"></a>What's New in Kerberos Authentication

>適用於：Windows Server 2016 和 Windows 10

## <a name="kdc-support-for-public-key-trust-based-client-authentication"></a>公開信任的金鑰為基礎的用戶端驗證的 KDC 支援

從 Windows Server 2016 開始，Kdc 會支援一種公用金鑰對應。 如果帳戶佈建的公開金鑰，KDC 會支援 Kerberos PKInit 明確地使用該金鑰。 因為沒有任何憑證驗證，支援自我簽署的憑證，並不支援驗證機制保證。

金鑰信任設定時，慣用的帳戶，無論 UseSubjectAltName 設定為何。

## <a name="kerberos-client-and-kdc-support-for-rfc-8070-pkinit-freshness-extension"></a>Kerberos 用戶端和 KDC 支援 RFC 8070 PKInit 有效期限延伸模組

從 Windows 10，版本 1607年和 Windows Server 2016 開始，Kerberos 用戶端嘗試[RFC 8070 PKInit 有效期限延伸](https://datatracker.ietf.org/doc/draft-ietf-kitten-pkinit-freshness/)對於公開金鑰為基礎的登入。 

從 Windows Server 2016 開始，Kdc 可以支援 PKInit 有效期限延伸模組。 根據預設，Kdc 不提供 PKInit 有效期限延伸模組。 若要啟用它，請使用新的 KDC 支援 PKInit 有效期限延伸 KDC 系統管理範本原則設定，網域中的所有網域控制站上。 設定時，Windows Server 2016 網域功能等級 (DFL) 網域時，將支援下列選項：

- **已停用**：KDC 會永遠不會提供 PKInit 有效期限延伸模組，並接受有效的驗證要求，但不要檢查最新狀態。 使用者將不會收到最新公開的索引鍵識別的 SID。
- **支援**:PKInit 有效期限延伸模組支援要求。 PKInit 有效期限延伸模組已成功驗證的 Kerberos 用戶端會收到最新公開的索引鍵識別的 SID。
- **必要**：PKInit 有效期限延伸模組，才能驗證成功。 使用公用金鑰的認證時，不支援 PKInit 有效期限延伸模組的 Kerberos 用戶端將一律失敗。

## <a name="domain-joined-device-support-for-authentication-using-public-key"></a>使用公開金鑰進行驗證的已加入網域的裝置支援

從 Windows 10 1507年版與 Windows Server 2016 中，如果已加入網域的裝置可註冊其繫結的公開金鑰與 Windows Server 2016 網域控制站 (DC)，然後裝置可以向使用 Kerberos 驗證的公開金鑰Windows Server 2016 DC。 如需詳細資訊，請參閱[已加入網域的裝置公用金鑰驗證](Domain-joined-Device-Public-Key-Authentication.md)

## <a name="kerberos-clients-allow-ipv4-and-ipv6-address-hostnames-in-service-principal-names-spns"></a>Kerberos 用戶端在服務主體名稱 (Spn) 允許 IPv4 和 IPv6 位址的主機名稱

從 Windows 10 1507年版和 Windows Server 2016 開始，Kerberos 用戶端可以設定為在 Spn 中支援 IPv4 和 IPv6 的主機名稱。 

登錄路徑：

HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\Kerberos\Parameters

若要設定 Spn 的 IP 位址的主機名稱的支援，建立 TryIPSPN 項目。 此項目依預設不存在於登錄中。 您已建立的項目之後，請將 DWORD 值變更為 1。 如果未設定，並不會嘗試 IP 位址的主機名稱。

如果在 Active Directory 中登錄 SPN，驗證就會成功使用 Kerberos。 

## <a name="kdc-support-for-key-trust-account-mapping"></a>金鑰信任帳戶對應的 KDC 支援

從 Windows Server 2016 開始，網域控制站具有金鑰信任帳戶對應，以及後援 SAN 行為現有 AltSecID 和使用者主體名稱 (UPN) 的支援。 當 UseSubjectAltName 設定為：

- 0:需要明確的對應。 然後必須是：
    - 金鑰信任 （新增搭配 Windows Server 2016)
    - ExplicitAltSecID
- 1：隱含的對應為允許 （預設值）：
    1. 如果金鑰信任帳戶設定，則它會使用對應 （Windows Server 2016 的新顯示）。
    2. 如果 SAN 中沒有 UPN，AltSecID 會嘗試進行對應。
    3. 如果 SAN 中沒有 UPN，UPN 會嘗試進行對應。

## <a name="see-also"></a>另請參閱

- [Kerberos 驗證概觀](kerberos-authentication-overview.md)
