---
title: TLS (安全通道 SSP)
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: ebd3c40c-b4c0-4f6d-a00c-f90eda4691df
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 05/16/2018
ms.openlocfilehash: 030fd81e0c6ba0423f1fa73e680006766cf2b180
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890769"
---
# <a name="tls-schannel-ssp-changes-in-windows-10-and-windows-server-2016"></a>Windows 10 和 Windows Server 2016 中的 TLS (安全通道 SSP) 變更

>適用於：Windows Server （半年通道）、 Windows Server 2016 和 Windows 10

## <a name="cipher-suite-changes"></a>加密套件變更

Windows 10 1511年版，Windows Server 2016 新增組態支援的加密套件順序使用行動裝置管理 (MDM)。

加密套件優先權順序變更，請參閱[安全通道中的加密套件](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx)。

已新增的對下列加密套件的詳細資訊：

- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (RFC 5289) 在 Windows 10 1507年版，Windows Server 2016
- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (RFC 5289) 在 Windows 10 1507年版，Windows Server 2016

DisabledByDefault 變更下列加密套件：

- TLS_DHE_DSS_WITH_AES_256_CBC_SHA256 (RFC 5246) 在 Windows 10 版本 1703年
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA256 (RFC 5246) 在 Windows 10 版本 1703年
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA (RFC 5246) 在 Windows 10 版本 1703年
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA (RFC 5246) 在 Windows 10 版本 1703年
- TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA (RFC 5246) 在 Windows 10 版本 1703年
- TLS_RSA_WITH_RC4_128_SHA 於 Windows 10 版本 1709
- 在 Windows 10 版本 1709 TLS_RSA_WITH_RC4_128_MD5

從 Windows 10，1507年版和 Windows Server 2016 開始，依預設支援 SHA 512 憑證。

### <a name="rsa-key-changes"></a>RSA 金鑰變更

Windows 10 1507年版，Windows Server 2016 新增用戶端 RSA 金鑰大小的登錄組態選項。

如需詳細資訊，請參閱 < [KeyExchangeAlgorithm-用戶端的 RSA 金鑰大小](tls-registry-settings.md#keyexchangealgorithm---client-rsa-key-sizes)。

### <a name="diffie-hellman-key-changes"></a>Diffie-hellman 金鑰變更

Windows 10 1507年版，Windows Server 2016 新增 Diffie-hellman 金鑰大小的登錄組態選項。

如需詳細資訊，請參閱 < [KeyExchangeAlgorithm-Diffie-hellman 金鑰大小](tls-registry-settings.md#keyexchangealgorithm---diffie-hellman-key-sizes)。

### <a name="schusestrongcrypto-option-changes"></a>SCH_USE_STRONG_CRYPTO 選項變更

Windows 10、 1507年版與 Windows Server 2016、windows [SCH_USE_STRONG_CRYPTO](https://msdn.microsoft.com/library/windows/desktop/aa379810.aspx)選項現在會停用 NULL、 MD5、 DES、，然後匯出的密碼。

## <a name="elliptical-curve-changes"></a>橢圓曲線變更

Windows 10 1507年版，Windows Server 2016 新增的電腦設定 下的橢圓曲線群組原則組態 > 系統管理範本 > 網路 > SSL 組態設定。 ECC 曲線訂單清單指定橢圓曲線會偏好的順序，以及可讓支援的曲線不會啟用。 
 
已新增的對下列橢圓曲線的詳細資訊：

- BrainpoolP256r1 (RFC 7027) 在 Windows 10 1507年版，Windows Server 2016
- BrainpoolP384r1 (RFC 7027) 在 Windows 10 1507年版，Windows Server 2016 
- BrainpoolP512r1 (RFC 7027) 在 Windows 10 1507年版，Windows Server 2016
- Curve25519 (RFC 草稿-ietf-tls-curve25519) 在 Windows 10 1607年版，Windows Server 2016

## <a name="dispatch-level-support-for-sealmessage--unsealmessage"></a>分派 SealMessage & UnsealMessage 的層級支援

Windows 10 1507年版，Windows Server 2016 新增對 SealMessage/UnsealMessage 分派層級。

## <a name="dtls-12"></a>DTLS 1.2

Windows 10 1607年版，Windows Server 2016 新增對支援 DTLS 1.2 (RFC 6347)。

## <a name="httpsys-thread-pool"></a>HTTP。SYS 執行緒集區 

Windows 10 1607年版，Windows Server 2016 新增登錄設定，用來處理 HTTP TLS 交握的執行緒集區的大小。SYS。

登錄路徑： 

HKLM\SYSTEM\CurrentControlSet\Control\LSA

若要指定每個 CPU 核心的最大執行緒集區大小，建立**MaxAsyncWorkerThreadsPerCpu**項目。 此項目依預設不存在於登錄中。 您已建立的項目之後，請將 DWORD 值變更為所需的大小。 如果未設定，最大值是每個 CPU 核心 2 個執行緒。

## <a name="next-protocol-negotiation-npn-support"></a>下一步 通訊協定交涉 (NPN) 支援

從 Windows 10 1703年版開始下, 一步 通訊協定交涉 (NPN) 已經移除，而且不受支援。

## <a name="pre-shared-key-psk"></a>預先共用的金鑰 (PSK)

Windows 10 1607年版，Windows Server 2016 新增對支援 PSK 金鑰交換演算法 (RFC 4279)。

已新增的對下列 PSK 加密套件的詳細資訊：

- TLS_PSK_WITH_AES_128_CBC_SHA256 (RFC 5487) 在 Windows 10 1607年版，Windows Server 2016
- 在 Windows 10 1607年版，Windows Server 2016 的 TLS_PSK_WITH_AES_256_CBC_SHA384(RFC 5487)
- TLS_PSK_WITH_NULL_SHA256 (RFC 5487) 在 Windows 10 1607年版，Windows Server 2016
- TLS_PSK_WITH_NULL_SHA384 (RFC 5487) 在 Windows 10 1607年版，Windows Server 2016
- TLS_PSK_WITH_AES_128_GCM_SHA256 (RFC 5487) 在 Windows 10 1607年版，Windows Server 2016
- TLS_PSK_WITH_AES_256_GCM_SHA384 (RFC 5487) 在 Windows 10 1607年版，Windows Server 2016

## <a name="session-resumption-without-server-side-state-server-side-performance-improvements"></a>工作階段繼續，而不需要伺服器端狀態伺服器端效能增強功能

Windows 10 1507年版，Windows Server 2016 提供每秒 30%以上的工作階段繼續相較於 Windows Server 2012 的工作階段票證。

## <a name="session-hash-and-extended-master-secret-extension"></a>工作階段的雜湊和擴充的主要祕密延伸模組

Windows 10 1507年版，Windows Server 2016 新增對 RFC 7627 支援：傳輸層安全性 (TLS) 工作階段的雜湊，並擴充主要祕密延伸模組。

這項變更，因為 Windows 10 和 Windows Server 2016 需要第 3 方[CNG SSL 提供者](https://msdn.microsoft.com/library/windows/desktop/ff468652.aspx)更新以支援 NCRYPT_SSL_INTERFACE_VERSION_3，並說明這個新介面。


## <a name="ssl-support"></a>SSL 支援

從 Windows 10，版本 1607年和 Windows Server 2016 開始，在 TLS 用戶端和伺服器 SSL 3.0 預設會停用。 這表示除非應用程式或服務特別要求 SSL 3.0 透過 SSPI，用戶端將永遠不會提供或接受 SSL 3.0，伺服器會永遠不會選取 SSL 3.0。

從 Windows 10 版本 1607年和 Windows Server 2016 開始，SSL 2.0 已移除，並不受支援。

## <a name="changes-to-windows-tls-adherence-to-tls-12-requirements-for-connections-with-non-compliant-tls-clients"></a>變更 Windows TLS 遵循與不符合規範的 TLS 用戶端連線的 TLS 1.2 需求

用戶端會使用 TLS 1.2 ["signature_algorithms 」 延伸模組](https://tools.ietf.org/html/rfc5246#section-7.4.1.4.1)來向伺服器表示哪個簽章/雜湊演算法配對可用於數位簽章 （亦即，伺服器憑證和伺服器金鑰交換）。 TLS 1.2 RFC 也需要伺服器憑證訊息接受"signature_algorithms 」 延伸模組：

「 如果用戶端提供的 「 signature_algorithms 」 延伸模組，然後由伺服器所提供的所有憑證必須都經過會出現在該延伸模組的雜湊/簽章演算法配對。 」

在實務上，有些協力廠商 TLS 用戶端不符合要包含所有簽章和雜湊演算法組它們願意接受"signature_algorithms 」 延伸模組中的 TLS 1.2 RFC 和失敗或完全省略分機 （後者表示伺服器的用戶端只支援 SHA1 與 RSA、 DSA 或 ECDSA）。

TLS 伺服器通常只有一個憑證，設定每個端點，這表示伺服器不能永遠提供符合用戶端的需求的憑證。

Windows 10 和 Windows Server 2016 中，Windows 的 TLS 堆疊之前恪守 TLS 1.2 的 RFC 需求，導致連線失敗與 RFC 不符合規範的 TLS 用戶端及互通性問題。 在 Windows 10 和 Windows Server 2016 中，比較不嚴謹的條件約束和伺服器可以傳送憑證不符合使用 TLS 1.2 RFC，如果是伺服器的唯一選項。 用戶端可能會再繼續或終止交握。

當驗證伺服器和用戶端憑證時，Windows TLS 堆疊會嚴格遵守 TLS 1.2 RFC，並只允許在伺服器和用戶端憑證中的 交涉的簽章和雜湊演算法。


