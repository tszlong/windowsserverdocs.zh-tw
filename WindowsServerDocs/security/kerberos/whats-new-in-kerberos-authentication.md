---
title: "F:kerberos 驗證中的新功能"
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: 7b046490c606cdf9e1436f503bf46a9cd4280ea9
ms.sourcegitcommit: 2782a80a916f8432c030af76e860a72f08481899
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/13/2018
---
# <a name="whats-new-in-kerberos-authentication"></a>F:kerberos 驗證中的新功能

>適用於：Windows Server 2016 和 Windows 10

## <a name="kdc-support-for-public-key-trust-based-client-authentication"></a>公開鍵信任型 client 驗證 \ [KDC 支援

開始使用 Windows Server 2016、 Kdc 支援公用按鍵對應的方式。 如果公用鍵提供帳號，\ [KDC 支援 Kerberos PKInit 明確使用該按鍵。 因為有任何憑證驗證，支援自動簽署的憑證，並驗證機制保證不受支援。

信任鍵時慣用帳號，無論 UseSubjectAltName 設定的設定。

## <a name="kerberos-client-and-kdc-support-for-rfc-8070-pkinit-freshness-extension"></a>Kerberos client 和 \ [KDC 支援 RFC 8070 PKInit 有效期限擴充功能

開始使用 Windows 10，版本 1607 年和 Windows Server 2016、Kerberos 戶端嘗試[RFC 8070 PKInit 有效期限擴充功能](https://datatracker.ietf.org/doc/draft-ietf-kitten-pkinit-freshness/)以公用鍵為主的登入的附加元件。 

開始使用 Windows Server 2016、 Kdc 可支援 PKInit 有效期限擴充功能。 根據預設，Kdc 不提供 PKInit 有效期限擴充功能。 若要讓它，使用新的 \ [KDC 支援 PKInit 有效期限擴充功能 KDC 系統管理範本原則設定的網域中的所有網域控制站在。 設定時，Windows Server 2016 網域功能等級 (DFL) 網域時支援下列選項：

- **停用**: \ [KDC 永遠不會提供 PKInit 有效期限擴充功能，並檢查有效期限不接受要求有效的驗證。 使用者將不會收到新公開金鑰身分 SID。
- **支援的**: PKInit 有效期限延伸支援要求。 成功驗證 PKInit 有效期限副檔名 Kerberos 戶端收到全新公開金鑰身分 SID。
- **需要**: PKInit 有效期限擴充功能是必要的成功驗證。 使用公用按鍵認證，一定會失敗 Kerberos 戶端不支援 PKInit 有效期限擴充功能。

## <a name="domain-joined-device-support-for-authentication-using-public-key"></a>使用公用鍵驗證加入網域的裝置支援

從 Windows 10 版本 1507年與 Windows Server 2016 加入網域的裝置是否可以與 Windows Server 2016 網域控制站 DC 登記其結合公用鍵開始，然後裝置可以使用驗證使用 Windows Server 2016 DC Kerberos 驗證公開鍵。 如需詳細資訊，請查看[加入網域的裝置公開鍵驗證](Domain-joined-Device-Public-Key-Authentication.md)

## <a name="kerberos-clients-allow-ipv4-and-ipv6-address-hostnames-in-service-principal-names-spns"></a>Kerberos 戶端允許服務主體名稱 (Spn) 中的 [IPv4 和 IPv6 位址主機

開始使用 Windows 10 1507 版和 Windows Server 2016、 Kerberos 戶端可以設定為支援 Spn IPv4 和 IPv6 主機。 

登錄路徑：

HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\Kerberos\Parameters

若要設定 Spn IP 位址主機的支援，請建立 TryIPSPN 項目。 此項目不存在於登錄預設。 您所建立的項目後，變更 1 DWORD 值。 如果未設定，並不會嘗試 IP 位址主機。

如果在 Active Directory 中為登記 SPN、驗證成功 Kerberos 使用。 

## <a name="kdc-support-for-key-trust-account-mapping"></a>\ [KDC 支援鍵信任 account 對應

開始使用 Windows Server 2016、網域控制站有鍵信任 account 對應，以及後援舊行為現有 AltSecID 和使用者主體名稱 (UPN) 的支援。 當 UseSubjectAltName 設為︰

- 0： 明確對應需要。 然後必須是：
    - 按鍵信任 （新的 Windows Server 2016)
    - ExplicitAltSecID
- 1： 隱含對應允許 （預設值）：
    1. 如果信任鍵設定為帳號，則它會使用對應 （新的 Windows Server 2016）。
    2. 如果舊中有不 UPN AltSecID 會嘗試對應。
    3. 如果在舊 UPN，UPN 會嘗試對應。

## <a name="see-also"></a>也了

- [Kerberos 驗證的概觀](kerberos-authentication-overview.md)
