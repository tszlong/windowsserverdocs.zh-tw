---
title: 'TLS (Schannel SSP) '
ms.topic: article
ms.assetid: ebd3c40c-b4c0-4f6d-a00c-f90eda4691df
manager: alanth
author: justinha
ms.date: 05/16/2018
ms.openlocfilehash: 92a61452d3c22c34bb3251ee2668679068f3a2ad
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87989494"
---
# <a name="tls-schannel-ssp-changes-in-windows-10-and-windows-server-2016"></a>TLS (Schannel SSP) Windows 10 和 Windows Server 2016 中的變更

>適用於：Windows 2016 (半年度管道)、Windows Server 2016 和 Windows 10

## <a name="cipher-suite-changes"></a>加密套件變更

Windows 10 版本1511和 Windows Server 2016 新增使用行動裝置管理 (MDM) 的加密套件順序設定支援。

如需加密套件優先順序順序的變更，請參閱[Schannel 中的加密套件](/windows/win32/secauthn/cipher-suites-in-schannel)。

已新增下列加密套件的支援：

- Windows 10 1507 版和 Windows Server 2016 中的 TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (RFC 5289) 
- Windows 10 1507 版和 Windows Server 2016 中的 TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (RFC 5289) 

下列加密套件的 DisabledByDefault 變更：

- Windows 10 版本1703中的 TLS_DHE_DSS_WITH_AES_256_CBC_SHA256 (RFC 5246) 
- Windows 10 版本1703中的 TLS_DHE_DSS_WITH_AES_128_CBC_SHA256 (RFC 5246) 
- Windows 10 版本1703中的 TLS_DHE_DSS_WITH_AES_256_CBC_SHA (RFC 5246) 
- Windows 10 版本1703中的 TLS_DHE_DSS_WITH_AES_128_CBC_SHA (RFC 5246) 
- Windows 10 版本1703中的 TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA (RFC 5246) 
- Windows 10 版本1709中的 TLS_RSA_WITH_RC4_128_SHA
- Windows 10 版本1709中的 TLS_RSA_WITH_RC4_128_MD5

從 Windows 10 版本1507和 Windows Server 2016 開始，預設會支援 SHA 512 憑證。

### <a name="rsa-key-changes"></a>RSA 金鑰變更

Windows 10 版本1507和 Windows Server 2016 會為用戶端 RSA 金鑰大小新增登錄設定選項。

如需詳細資訊，請參閱[asymmetricalgorithm.keyexchangealgorithm-用戶端 RSA 金鑰大小](tls-registry-settings.md#keyexchangealgorithm---client-rsa-key-sizes)。

### <a name="diffie-hellman-key-changes"></a>Diffie-hellman 金鑰變更

Windows 10 版本1507和 Windows Server 2016 新增 Diffie-hellman 金鑰大小的登錄設定選項。

如需詳細資訊，請參閱[asymmetricalgorithm.keyexchangealgorithm-diffie-hellman 金鑰大小](tls-registry-settings.md#keyexchangealgorithm---diffie-hellman-key-sizes)。

### <a name="sch_use_strong_crypto-option-changes"></a>SCH_USE_STRONG_CRYPTO 選項變更

使用 Windows 10 （版本1507和 Windows Server 2016）時， [SCH_USE_STRONG_CRYPTO](/windows/win32/api/schannel/ns-schannel-schannel_cred)選項現在會停用 Null、MD5、DES 和匯出密碼。

## <a name="elliptical-curve-changes"></a>橢圓形曲線變更

Windows 10 版本1507和 Windows Server 2016 新增 [電腦設定] 下的 [群組原則設定] > 系統管理範本 > 網路 > SSL 設定]。
[ECC 曲線順序] 清單會指定將橢圓曲線慣用的順序，以及啟用未啟用的支援曲線。

已新增下列橢圓形曲線的支援：

- Windows 10、1507版和 Windows Server 2016 中的 BrainpoolP256r1 (RFC 7027) 
- Windows 10、1507版和 Windows Server 2016 中的 BrainpoolP384r1 (RFC 7027) 
- Windows 10、1507版和 Windows Server 2016 中的 BrainpoolP512r1 (RFC 7027) 
- Curve25519 (RFC draft-ietf-tls-Curve25519) Windows 10，版本1607和 Windows Server 2016

## <a name="dispatch-level-support-for-sealmessage--unsealmessage"></a>SealMessage & UnsealMessage 的分派層級支援

Windows 10 版本1507和 Windows Server 2016 在分派層級新增對 SealMessage/UnsealMessage 的支援。

## <a name="dtls-12"></a>DTLS 1。2

Windows 10 版本1607和 Windows Server 2016 新增對 DTLS 1.2 (RFC 6347) 的支援。

## <a name="httpsys-thread-pool"></a>HTTP.SYS 執行緒集區

Windows 10 版本1607和 Windows Server 2016 會新增執行緒集區大小的登錄設定，用來處理 HTTP.SYS 的 TLS 交握。

登錄路徑︰

HKLM\SYSTEM\CurrentControlSet\Control\LSA

若要指定每個 CPU 核心的執行緒集區大小上限，請建立**MaxAsyncWorkerThreadsPerCpu**專案。
此項目依預設不存在於登錄中。
建立專案之後，請將 DWORD 值變更為所需的大小。
如果未設定，則最大值為每個 CPU 核心2個執行緒。

## <a name="next-protocol-negotiation-npn-support"></a>下一個通訊協定協商 (NPN) 支援

從 Windows 10 1703 版開始， (NPN) 的下一個通訊協定協商已移除，不再受到支援。

## <a name="pre-shared-key-psk"></a>預先共用金鑰 (PSK) 

Windows 10 版本1607和 Windows Server 2016 新增了 PSK 金鑰交換演算法的支援 (RFC 4279) 。

已新增下列 PSK 加密套件的支援：

- Windows 10 1607 版和 Windows Server 2016 中的 TLS_PSK_WITH_AES_128_CBC_SHA256 (RFC 5487) 
- Windows 10 1607 版和 Windows Server 2016 中的 TLS_PSK_WITH_AES_256_CBC_SHA384 (RFC 5487) 
- Windows 10 1607 版和 Windows Server 2016 中的 TLS_PSK_WITH_Null_SHA256 (RFC 5487) 
- Windows 10 1607 版和 Windows Server 2016 中的 TLS_PSK_WITH_Null_SHA384 (RFC 5487) 
- Windows 10 1607 版和 Windows Server 2016 中的 TLS_PSK_WITH_AES_128_GCM_SHA256 (RFC 5487) 
- Windows 10 1607 版和 Windows Server 2016 中的 TLS_PSK_WITH_AES_256_GCM_SHA384 (RFC 5487) 

## <a name="session-resumption-without-server-side-state-server-side-performance-improvements"></a>沒有伺服器端狀態伺服器端效能改進的會話繼續

相較于 Windows Server 2012，windows 10 版本1507和 Windows Server 2016 提供每秒30% 以上的會話繼續。

## <a name="session-hash-and-extended-master-secret-extension"></a>會話雜湊和延伸主要密碼延伸模組

Windows 10 版本1507和 Windows Server 2016 新增 RFC 7627 的支援：傳輸層安全性 (TLS) 會話雜湊和延伸主要密碼延伸模組。

由於這種變更，Windows 10 和 Windows Server 2016 需要協力廠商[CNG SSL 提供者](/windows/win32/seccng/cng-ssl-provider-functions)更新，以支援 NCRYPT_SSL_INTERFACE_VERSION_3，並說明這個新介面。


## <a name="ssl-support"></a>SSL 支援

從 Windows 10 版本1607和 Windows Server 2016 開始，預設會停用 TLS 用戶端和伺服器 SSL 3.0。
這表示除非應用程式或服務特別透過 SSPI 要求 SSL 3.0，否則用戶端永遠不會提供或接受 SSL 3.0，且伺服器永遠不會選取 SSL 3.0。

從 Windows 10 版本1607和 Windows Server 2016 開始，已移除 SSL 2.0，而且不再支援。

## <a name="changes-to-windows-tls-adherence-to-tls-12-requirements-for-connections-with-non-compliant-tls-clients"></a>對 Windows TLS 所做的變更遵循與不相容 TLS 用戶端連線的 TLS 1.2 需求

在 TLS 1.2 中，用戶端會使用「 [signature_algorithms」延伸](https://tools.ietf.org/html/rfc5246#section-7.4.1.4.1)模組，向伺服器表示可在數位簽章中使用的簽章/雜湊演算法組， (亦即伺服器憑證和伺服器金鑰交換) 。
TLS 1.2 RFC 也需要伺服器憑證訊息接受「signature_algorithms」延伸模組：

「如果用戶端提供了「signature_algorithms」延伸模組，則伺服器提供的所有憑證都必須以該延伸中出現的雜湊/簽章演算法配對來簽署。」

在實務上，有些協力廠商 TLS 用戶端不符合 TLS 1.2 RFC，而且無法包含它們在「signature_algorithms」延伸模組中願意接受的所有簽章和雜湊演算法組，或完全省略此延伸模組 (後者向伺服器表示用戶端僅支援與 RSA、DSA 或 ECDSA) SHA1。

TLS 伺服器通常只會有一個針對每個端點設定的憑證，這表示伺服器不一定會提供符合用戶端需求的憑證。

在 Windows 10 和 Windows Server 2016 之前，Windows TLS 堆疊會嚴格遵守 TLS 1.2 RFC 需求，而導致與 RFC 不相容的 TLS 用戶端和互通性問題的連接失敗。
在 Windows 10 和 Windows Server 2016 中，限制是寬鬆的，而且如果這是伺服器的唯一選項，伺服器就可以傳送不符合 TLS 1.2 RFC 的憑證。
用戶端接著可以繼續或終止交握。

在驗證服務器和用戶端憑證時，Windows TLS 堆疊會嚴格符合 TLS 1.2 RFC，而且只允許伺服器和用戶端憑證中的已協商簽章和雜湊演算法。