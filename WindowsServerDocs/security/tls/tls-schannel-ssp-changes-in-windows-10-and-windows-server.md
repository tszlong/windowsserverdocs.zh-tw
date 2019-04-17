---
title: TLS (Schannel SSP)
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: ebd3c40c-b4c0-4f6d-a00c-f90eda4691df
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: 6e1a229bfc7063597f8994c71bbaec5637d084a4
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="tls-schannel-ssp-changes-in-windows-10-and-windows-server-2016"></a>變更 Windows 10 與 Windows Server 2016 TLS (Schannel SSP)

>適用於：Windows Server 2016 和 Windows 10

## <a name="cipher-suite-changes"></a>密碼套件變更

Windows 10，版本 1511 年與 Windows Server 2016 新增密碼套件訂單使用行動裝置管理 (MDM) 設定支援。

套件優先順序訂單變更密碼，請查看[密碼套件 Schannel 在](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx)。

新增的支援下列加密套件：

- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (RFC 5289) Windows 10、1507 版和 Windows Server 2016 中
- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (RFC 5289) Windows 10、1507 版和 Windows Server 2016 中

變更下列加密套件 DisabledByDefault:

- TLS_DHE_DSS_WITH_AES_256_CBC_SHA256 (RFC 5246) 在 Windows 10，版本 1703 年
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA256 (RFC 5246) 在 Windows 10，版本 1703 年
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA (RFC 5246) 在 Windows 10，版本 1703 年
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA (RFC 5246) 在 Windows 10，版本 1703 年
- TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA (RFC 5246) 在 Windows 10，版本 1703 年
- 在 Windows 10，版本 1709 TLS_RSA_WITH_RC4_128_SHA
- 在 Windows 10，版本 1709 TLS_RSA_WITH_RC4_128_MD5

開始使用 Windows 10、1507 版和 Windows Server 2016、預設不支援 SHA 512 憑證。

### <a name="rsa-key-changes"></a>RSA 變更

Windows 10、1507 版和 Windows Server 2016 新增 client RSA 金鑰大小登錄設定選項。

如需詳細資訊，請查看[KeyExchangeAlgorithm-Client RSA 金鑰大小](tls-registry-settings.md#keyexchangealgorithm---client-rsa-key-sizes)。

### <a name="diffie-hellman-key-changes"></a>時間時變更

Windows 10、1507 版和 Windows Server 2016 新增時間時金鑰大小登錄設定選項。

如需詳細資訊，請查看[KeyExchangeAlgorithm-時間時金鑰大小](tls-registry-settings.md#keyexchangealgorithm---diffie-hellman-key-sizes)。

### <a name="schusestrongcrypto-option-changes"></a>SCH_USE_STRONG_CRYPTO 選項變更

Windows 10、1507 版與 Windows Server 2016 [SCH_USE_STRONG_CRYPTO](https://msdn.microsoft.com/library/windows/desktop/aa379810.aspx)選項現在停用 NULL，MD5，DES，及匯出加密。

## <a name="elliptical-curve-changes"></a>變更橢圓形曲線

Windows 10、1507 版和 Windows Server 2016 增加電腦設定] 下的橢圓形這些群組原則設定 > 系統管理範本] > 網路 > SSL 設定。 ECC 曲線順序清單指定橢圓形曲線慣用的順序，以及可支援的曲線這不支援。 
 
新增下的橢圓形這些的支援：

- BrainpoolP256r1 (RFC 7027) Windows 10、1507 版和 Windows Server 2016 中
- BrainpoolP384r1 (RFC 7027) Windows 10、1507 版和 Windows Server 2016 中 
- BrainpoolP512r1 (RFC 7027) Windows 10、1507 版和 Windows Server 2016 中
- Curve25519 (RFC 草稿-ietf-tls-curve25519) Windows 10，版本 1607 年和 Windows Server 2016 中

## <a name="dispatch-level-support-for-sealmessage--unsealmessage"></a>適用於 SealMessage 與 UnsealMessage 發送層級支援

Windows 10、1507 版和 Windows Server 2016 新增支援 SealMessage 日 UnsealMessage 發送層級。

## <a name="dtls-12"></a>DTLS 1.2

Windows 10，版本 1607 年和 Windows Server 2016 新增 DTLS 1.2 (RFC 6347) 的支援。

## <a name="httpsys-thread-pool"></a>HTTP.SYS 執行緒集區 

Windows 10，版本 1607 年和 Windows Server 2016 新增登錄用來處理 TLS handshakes HTTP.SYS.

登錄路徑： 

HKLM\SYSTEM\CurrentControlSet\Control\LSA

若要指定每個 CPU 核心最大討論串中有集區大小，請建立**MaxAsyncWorkerThreadsPerCpu**的項目。 此項目不存在於登錄預設。 您所建立的項目後，變更所需的大小 DWORD 值。 如果未設定，最大值是每個 CPU 核心 2 執行緒。

## <a name="next-protocol-negotiation-npn-support"></a>下一步通訊協定交涉 (NPN) 的支援

開始使用 Windows 10 版本 1703 年下, 一步通訊協定交涉 (NPN) 已移除，已不再支援。

## <a name="pre-shared-key-psk"></a>預先共用的金鑰 (PSK)

Windows 10，版本 1607 年和 Windows Server 2016 新增 PSK 金鑰交換演算法 (RFC 4279) 的支援。

新增的支援下列 PSK 密碼套件：

- TLS_PSK_WITH_AES_128_CBC_SHA256 (RFC 5487) Windows 10，版本 1607 年和 Windows Server 2016 中
- Windows 10，版本 1607 年和 Windows Server 2016 中 TLS_PSK_WITH_AES_256_CBC_SHA384(RFC 5487)
- TLS_PSK_WITH_NULL_SHA256 (RFC 5487) Windows 10，版本 1607 年和 Windows Server 2016 中
- TLS_PSK_WITH_NULL_SHA384 (RFC 5487) Windows 10，版本 1607 年和 Windows Server 2016 中
- TLS_PSK_WITH_AES_128_GCM_SHA256 (RFC 5487) Windows 10，版本 1607 年和 Windows Server 2016 中
- TLS_PSK_WITH_AES_256_GCM_SHA384 (RFC 5487) Windows 10，版本 1607 年和 Windows Server 2016 中

## <a name="session-resumption-without-server-side-state-server-side-performance-improvements"></a>不伺服器端狀態伺服器端效能改進的工作階段回復

Windows 10、1507 版和 Windows Server 2016 提供活動門票相較於 Windows Server 2012 30%更多工作階段 resumptions 秒。

## <a name="session-hash-and-extended-master-secret-extension"></a>工作階段 Hash 和延伸的主要密碼擴充功能

Windows 10、1507 版和 Windows Server 2016 新增支援 RFC 7627：傳輸層級的安全性 (TLS) 工作階段 Hash 和延伸的主要密碼擴充功能。

由於這項變更，Windows 10 與 Windows Server 2016 需要 3 廠商[CNG SSL 提供者](https://msdn.microsoft.com/library/windows/desktop/ff468652.aspx)支援 NCRYPT_SSL_INTERFACE_VERSION_3，並描述這個新的介面的更新。


## <a name="ssl-support"></a>SSL 支援

開始使用 Windows 10，版本 1607 年和 Windows Server 2016、TLS client 和伺服器 SSL 3.0 預設停用。 這表示應用程式或服務專門要求 SSPI 透過 SSL 3.0，除非 client 將不提供或接受 SSL 3.0，伺服器一律不會選取 SSL 3.0。

開始使用 Windows 10 版本 1607 年和 Windows Server 2016、SSL 2.0 已經移除，已不再支援。

## <a name="changes-to-windows-tls-adherence-to-tls-12-requirements-for-connections-with-non-compliant-tls-clients"></a>變更 Windows TLS 遵守有不相容的 TLS 戶端 TLS 1.2 需求

在 TLS 1.2 client 使用[「signature_algorithms「擴充功能](https://tools.ietf.org/html/rfc5246#section-7.4.1.4.1)以指出伺服器可能會在數位簽章（亦即伺服器的憑證和 server 的關鍵換貨）中使用的簽章日 hash 演算法配對。 TLS 1.2 RFC 也需要伺服器的憑證郵件接受」signature_algorithms」擴充功能：

「Client 提供「signature_algorithms「擴充功能，如果然後伺服器提供所有憑證必須都登由該擴充中顯示的演算法對 hash 月簽章。」

實際上，某些第三方 TLS 戶端，請不要使用 TLS 1.2 RFC 和失敗包含所有的簽章和 hash 他們會接受在「signature_algorithms「擴充功能的演算法配對遵守或完全略過擴充功能（後者表示伺服器 client 僅支援 SHA1 RSA、DSA 或 ecdsa 來）。

TLS 伺服器通常只有設定端點，這表示伺服器永遠不能提供的憑證，以符合 client 的需求每一個憑證。

Windows 10 與 Windows Server 2016、Windows TLS 堆疊之前嚴格遵守 TLS 1.2 RFC 需求 RFC 不相容的 TLS 戶端與交互操作問題會導致連接失敗。 在 Windows 10 與 Windows Server 2016、限制的於輕鬆置於，以伺服器傳送 TLS 1.2 RFC，不符合憑證的是否伺服器的唯一的選項。 Client 然後可能繼續或結束交換。

驗證時 server 和 client 的憑證，Windows TLS 堆疊嚴格遵守 TLS 1.2 RFC，且只允許 server 和 client 憑證中的 [交涉簽章和 hash 演算法。


