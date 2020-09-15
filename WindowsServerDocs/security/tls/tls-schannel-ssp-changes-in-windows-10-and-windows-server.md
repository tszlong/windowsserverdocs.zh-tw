---
title: 'TLS (Schannel SSP) '
ms.topic: article
ms.assetid: ebd3c40c-b4c0-4f6d-a00c-f90eda4691df
author: justinha
ms.author: Justinha
ms.date: 05/16/2018
ms.openlocfilehash: 389a5a009320f7a19f5cbf942fe7c86f08f573ac
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078525"
---
# <a name="tls-schannel-ssp-changes-in-windows-10-and-windows-server-2016"></a>TLS (Schannel SSP) Windows 10 和 Windows Server 2016 中的變更

>適用於：Windows 2016 (半年度管道)、Windows Server 2016 和 Windows 10

## <a name="cipher-suite-changes"></a>加密套件變更

Windows 10，1511和 Windows Server 2016 版新增了使用 Mobile 裝置管理 (MDM) 設定加密套件順序的支援。

如需加密套件優先順序的變更，請參閱 [Schannel 中的加密套件](/windows/win32/secauthn/cipher-suites-in-schannel)。

已新增下列加密套件的支援：

- Windows 10、1507版和 Windows Server 2016 TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (RFC 5289) 
- Windows 10、1507版和 Windows Server 2016 TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (RFC 5289) 

下列加密套件的 DisabledByDefault 變更：

- Windows 10 1703 版中的 TLS_DHE_DSS_WITH_AES_256_CBC_SHA256 (RFC 5246) 
- Windows 10 1703 版中的 TLS_DHE_DSS_WITH_AES_128_CBC_SHA256 (RFC 5246) 
- Windows 10 1703 版中的 TLS_DHE_DSS_WITH_AES_256_CBC_SHA (RFC 5246) 
- Windows 10 1703 版中的 TLS_DHE_DSS_WITH_AES_128_CBC_SHA (RFC 5246) 
- Windows 10 1703 版中的 TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA (RFC 5246) 
- TLS_RSA_WITH_RC4_128_SHA Windows 10，版本1709
- TLS_RSA_WITH_RC4_128_MD5 Windows 10，版本1709

從 Windows 10、1507版和 Windows Server 2016 開始，預設支援 SHA 512 憑證。

### <a name="rsa-key-changes"></a>RSA 金鑰變更

Windows 10，版本1507和 Windows Server 2016 新增用戶端 RSA 金鑰大小的登錄設定選項。

如需詳細資訊，請參閱 [Asymmetricalgorithm.keyexchangealgorithm 用戶端 RSA 金鑰大小](tls-registry-settings.md#keyexchangealgorithm---client-rsa-key-sizes)。

### <a name="diffie-hellman-key-changes"></a>Diffie-hellman 金鑰變更

Windows 10，版本1507和 Windows Server 2016 新增 Diffie-hellman 金鑰大小的登錄設定選項。

如需詳細資訊，請參閱 [asymmetricalgorithm.keyexchangealgorithm-diffie-hellman 金鑰大小](tls-registry-settings.md#keyexchangealgorithm---diffie-hellman-key-sizes)。

### <a name="sch_use_strong_crypto-option-changes"></a>SCH_USE_STRONG_CRYPTO 選項變更

使用 Windows 10 1507 和 Windows Server 2016 版時， [SCH_USE_STRONG_CRYPTO](/windows/win32/api/schannel/ns-schannel-schannel_cred) 選項現在會停用 Null、MD5、DES 和匯出密碼。

## <a name="elliptical-curve-changes"></a>橢圓形曲線變更

Windows 10，1507版和 Windows Server 2016 會在 [電腦設定] > 系統管理範本 > Network > SSL Configuration 設定下，新增橢圓形曲線的群組原則設定。
ECC 曲線順序清單會指定橢圓曲線的慣用順序，以及啟用未啟用的支援曲線。

已新增下列橢圓曲線的支援：

- BrainpoolP256r1 (RFC 7027) Windows 10、1507版及 Windows Server 2016
- BrainpoolP384r1 (RFC 7027) Windows 10、1507版及 Windows Server 2016
- BrainpoolP512r1 (RFC 7027) Windows 10、1507版及 Windows Server 2016
- Curve25519 (RFC draft-ietf-tls-Curve25519) ，版本1607和 Windows Server 2016 Windows 10

## <a name="dispatch-level-support-for-sealmessage--unsealmessage"></a>SealMessage & UnsealMessage 的分派層級支援

Windows 10，1507和 Windows Server 2016 版會在分派層級新增 SealMessage/UnsealMessage 支援。

## <a name="dtls-12"></a>DTLS 1。2

Windows 10，1607和 Windows Server 2016 版新增了 DTLS 1.2 (RFC 6347) 的支援。

## <a name="httpsys-thread-pool"></a>HTTP.SYS 執行緒集區

Windows 10，1607版和 Windows Server 2016 會新增執行緒集區大小的登錄設定，以用來處理 HTTP.SYS 的 TLS 交握。

登錄路徑︰

HKLM\SYSTEM\CurrentControlSet\Control\LSA

若要指定每個 CPU 核心的執行緒集區大小上限，請建立 **MaxAsyncWorkerThreadsPerCpu** 專案。
此項目依預設不存在於登錄中。
在您建立專案之後，請將 DWORD 值變更為所需的大小。
如果未設定，則最大值為每個 CPU 核心2個執行緒。

## <a name="next-protocol-negotiation-npn-support"></a>下一個通訊協定協商 (NPN) 支援

從 Windows 10 版本1703開始，下一個通訊協定協商 (NPN) 已移除，不再受到支援。

## <a name="pre-shared-key-psk"></a>預先共用金鑰 (PSK) 

Windows 10，版本1607和 Windows Server 2016 新增了 PSK 金鑰交換演算法的支援 (RFC 4279) 。

已新增下列 PSK 加密套件的支援：

- Windows 10、1607版和 Windows Server 2016 TLS_PSK_WITH_AES_128_CBC_SHA256 (RFC 5487) 
- Windows 10、1607版和 Windows Server 2016 TLS_PSK_WITH_AES_256_CBC_SHA384 (RFC 5487) 
- Windows 10、1607版和 Windows Server 2016 TLS_PSK_WITH_Null_SHA256 (RFC 5487) 
- Windows 10、1607版和 Windows Server 2016 TLS_PSK_WITH_Null_SHA384 (RFC 5487) 
- Windows 10、1607版和 Windows Server 2016 TLS_PSK_WITH_AES_128_GCM_SHA256 (RFC 5487) 
- Windows 10、1607版和 Windows Server 2016 TLS_PSK_WITH_AES_256_GCM_SHA384 (RFC 5487) 

## <a name="session-resumption-without-server-side-state-server-side-performance-improvements"></a>沒有伺服器端狀態伺服器端效能改進的會話繼續

相較于 Windows Server 2012，1507版和 Windows Server 2016 Windows 10 提供每秒30% 的會話繼續。

## <a name="session-hash-and-extended-master-secret-extension"></a>會話雜湊和延伸的主要秘密延伸模組

Windows 10，1507版和 Windows Server 2016 新增了 RFC 7627：傳輸層安全性 (TLS) 會話雜湊和延伸主要密碼延伸模組的支援。

由於這種變更的緣故，Windows 10 和 Windows Server 2016 需要協力廠商的 [CNG SSL 提供者](/windows/win32/seccng/cng-ssl-provider-functions) 更新以支援 NCRYPT_SSL_INTERFACE_VERSION_3，以及描述這個新介面。


## <a name="ssl-support"></a>SSL 支援

從 Windows 10、1607版和 Windows Server 2016 開始，預設會停用 TLS 用戶端和伺服器 SSL 3.0。
這表示，除非應用程式或服務特別透過 SSPI 要求 SSL 3.0，否則用戶端將永遠不會提供或接受 SSL 3.0，而且伺服器永遠不會選取 SSL 3.0。

從 Windows 10 版本1607和 Windows Server 2016 開始，已移除 SSL 2.0，且已不再支援。

## <a name="changes-to-windows-tls-adherence-to-tls-12-requirements-for-connections-with-non-compliant-tls-clients"></a>針對與不相容 TLS 用戶端的連線，Windows TLS 的變更遵守 TLS 1.2 需求

在 TLS 1.2 中，用戶端會使用 ["signature_algorithms" 延伸](https://tools.ietf.org/html/rfc5246#section-7.4.1.4.1) 模組向伺服器指出可在數位簽章中使用的簽章/雜湊演算法組 (亦即，伺服器憑證和伺服器金鑰交換) 。
TLS 1.2 RFC 也需要伺服器憑證訊息接受 "signature_algorithms" 延伸模組：

"如果用戶端提供「signature_algorithms」擴充功能，則伺服器提供的所有憑證都必須由該擴充功能中的雜湊/簽章演算法組簽署。」

在實務上，某些協力廠商 TLS 用戶端不符合 TLS 1.2 RFC，而且無法包含它們願意在 "signature_algorithms" 延伸模組中接受的所有簽章和雜湊演算法組，或完全省略延伸模組 (後者向伺服器指出用戶端僅支援搭配 RSA、DSA 或 ECDSA) SHA1。

TLS 伺服器通常只會針對每個端點設定一個憑證，這表示伺服器不一定能提供符合用戶端需求的憑證。

在 Windows 10 和 Windows Server 2016 之前，Windows TLS 堆疊會嚴格遵守 TLS 1.2 RFC 需求，導致與 RFC 不相容 TLS 用戶端和互通性問題的連線失敗。
在 Windows 10 和 Windows Server 2016 中，會放寬限制，而且伺服器可以傳送不符合 TLS 1.2 RFC 的憑證（如果這是伺服器的唯一選項）。
用戶端接著可以繼續或終止交握。

驗證服務器和用戶端憑證時，Windows TLS 堆疊會嚴格符合 TLS 1.2 RFC，而且只允許伺服器和用戶端憑證中的協商簽章和雜湊演算法。