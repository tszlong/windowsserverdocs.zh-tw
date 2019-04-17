---
title: "運送層安全性 (TLS) 登錄設定"
description: "Windows Server 安全性"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
author: justinha
ms.author: justinha
manager: brianlic-msft
ms.date: 08/07/2017
ms.openlocfilehash: 8ccfacc367a5d32438bebf22798479a07f6cbdfc
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="transport-layer-security-tls-registry-settings"></a>運送層安全性 (TLS) 登錄設定

>適用於：Windows Server（以每年次通道）、Windows Server 2016、Windows Server 2008 R2、Windows Server 2008、Windows 10、Windows 8.1、Windows 8、Windows 7、Windows Vista

本參考主題適用於 IT 專業人員包含支援的登錄設定 Windows 實作傳輸層級的安全性 (TLS) 通訊協定和資訊的安全通訊端層 (SSL) 通訊協定透過 Schannel 安全性支援提供者 (SSP)。 此主題協助您管理和疑難排解 Schannel SSP，所涵蓋的項目與登錄子專門 TLS 和 SSL 通訊協定。 

>[!Caution]
>為您進行疑難排解或驗證需要的設定的套用時使用的參考提供這項資訊。 我們建議您執行不直接編輯登錄除非另有任何其他另一種方式。
>變更登錄無法驗證它們套用之前的作業系統，或 Windows 作業系統。 如此一來，不正確的值可以儲存，並將導致處於無法復原錯誤系統中。 可能的話，而不是直接，編輯登錄使用群組原則」或其他 Windows 工具例如 Microsoft 管理 Console (MMC) 完成工作。 如果您必須編輯登錄，小心謹慎。 

## <a name="certificatemappingmethods"></a>CertificateMappingMethods 

此項目不存在於登錄預設。 預設值是所有的四個憑證對應方法列在下方的支援。

伺服器應用程式需要 client 驗證時, Schannel 會自動嘗試憑證帳號 client 電腦所提供的地圖。 您可以進行驗證使用者建立對應的相關資訊的 Windows 使用者帳號，憑證登入以 client 憑證。 您建立以及憑證對應之後，client 提出 client 憑證，每次您的伺服器應用程式會自動關聯使用者適當的 Windows 使用者 account。

在大部分案例中，憑證對應至帳號，在其中一種方式： 

- 單一憑證對應至單一使用者 account（一一對應）。
- 多個憑證對應至一的使用者 account（多一對應）。

根據預設，Schannel 提供者將會使用下列四個憑證對應，所列的方法順序的喜好設定：

1. Kerberos 服務的使用者 (S4U) 憑證對應
2. 使用者主體名稱對應
3. 一一對應（也稱為主旨日發行者對應）
4. 多一對應

適用版本：中指定為**適用於**清單中開頭本主題。

登錄路徑：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="ciphers"></a>加密

藉由設定密碼套件訂單應該控制 TLS 日 SSL 加密。 如需詳細資訊，請查看[設定 TLS 密碼套件訂單](manage-tls.md#configuring-tls-cipher-suite-order)。

如預設加密套件順序 Schannel SSP 所使用的相關資訊，請查看[在 TLS SSL (Schannel SSP) 的編碼器套件](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx)。 

##<a name="ciphersuites"></a>CipherSuites

設定 TLS 日 SSL 加密套件，應該要使用群組原則、MDM 或 PowerShell，查看[設定 TLS 密碼套件訂單](manage-tls.md#configuring-tls-cipher-suite-order)如需詳細資訊。

如預設加密套件順序 Schannel SSP 所使用的相關資訊，請查看[在 TLS SSL (Schannel SSP) 的編碼器套件](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx)。 


## <a name="clientcachetime"></a>ClientCacheTime

此項目控制量作業系統所需時間（毫秒）到期 client 端快取的項目。 設定為 0 關閉安全連接快取。 此項目不存在於登錄預設。 

第一次 client 連接到透過 Schannel SSP，完整伺服器 TLS 日 SSL 交換執行。 當您完成時，主要密碼、密碼套件及憑證儲存各自 client 伺服器上的快取工作階段中。

開始使用 Windows Server 2008 和 Windows Vista，預設 client 快取時間是 10 小時。

登錄路徑：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

預設 client 快取的時間

## <a name="fipsalgorithmpolicy"></a>FIPSAlgorithmPolicy

此項目控制聯邦資訊處理 (FIPS) 相容。 預設為 0。

適用版本：中指定為**適用於**清單中開頭本主題。 

登錄路徑：HKLM SYSTEM\CurrentControlSet\Control\LSA

Windows Server FIPS 密碼套件：查看[支援密碼套件和通訊協定 Schannel SSP 在](https://technet.microsoft.com/library/dn786419.aspx)。

## <a name="hashes"></a>Hashes

藉由設定密碼套件訂單應該控制 TLS 日 SSL hash 演算法。 查看[設定 TLS 密碼套件訂單](manage-tls.md#configuring-tls-cipher-suite-order)如需詳細資訊。

## <a name="issuercachesize"></a>IssuerCacheSize

此項目控制發行者快取的大小，並可搭配發行者對應。 地圖中 client 的憑證鏈結發行者的所有嘗試 Schannel SSP-不僅直接 client 憑證的發行者。 伺服器時不會發行者對應到帳號，一般，則可能會嘗試地圖相同發行者名稱重複數百種秒的時間。 

若要避免這個問題，伺服器有錯誤的快取，讓快取項如果過去未對應發行者的名稱，新增到快取，並 Schannel SSP 地圖的發行者名稱，再試一次之前不會到期。 這個登錄指定的快取的大小。 此項目不存在於登錄預設。 預設值為 100。 

適用版本：中指定為**適用於**清單中開頭本主題。

登錄路徑：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="issuercachetime"></a>IssuerCacheTime

此項目控制的快取逾時間隔（毫秒）。 地圖中 client 的憑證鏈結發行者的所有嘗試 Schannel SSP-不僅直接 client 憑證的發行者。 在何處發行者不會對應帳號，是常見原因，如此伺服器可能會嘗試地圖相同發行者名稱重複數百種秒的時間。

若要避免這個問題，伺服器有錯誤的快取，讓快取項如果過去未對應發行者的名稱，新增到快取，並 Schannel SSP 地圖的發行者名稱，再試一次之前不會到期。 此快取保留的效能，以便系統不會繼續嘗試地圖相同發行者。 此項目不存在於登錄預設。 預設值為 10 分鐘。

適用版本：中指定為**適用於**清單中開頭本主題。

登錄路徑：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="keyexchangealgorithm---client-rsa-key-sizes"></a>KeyExchangeAlgorithm-Client RSA 鍵大小

此項目控制 client RSA 金鑰大小。 

使用金鑰交換演算法應該控制設定密碼套件訂單。

Windows 10、1507 版和 Windows Server 2016 中新增了。

登錄路徑：HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\KeyExchangeAlgorithms\PKCS

若要指定 TLS client 最低支援各種不同的 RSA 按鍵的位元長度，建立**ClientMinKeyBitLength**的項目。 此項目不存在於登錄預設。 您所建立的項目之後，您想要的位元長度變更 DWORD 值。 如果未設定，1024 位元將最小值。 

若要指定 TLS client 最大支援各種不同的 RSA 按鍵的位元長度，建立**ClientMaxKeyBitLength**的項目。 此項目不存在於登錄預設。 您所建立的項目之後，您想要的位元長度變更 DWORD 值。 如果未設定，然後不執行最大值。

## <a name="keyexchangealgorithm---diffie-hellman-key-sizes"></a>KeyExchangeAlgorithm-時間時金鑰大小

此項目控制的時間時金鑰大小。 

使用金鑰交換演算法應該控制設定密碼套件訂單。

Windows 10、1507 版和 Windows Server 2016 中新增了。

登錄路徑：HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\KeyExchangeAlgorithms\Diffie 時

若要指定 TLS client 最低支援各種不同的時間-Helman 按鍵的位元長度，建立**ClientMinKeyBitLength**的項目。 此項目不存在於登錄預設。 您所建立的項目之後，您想要的位元長度變更 DWORD 值。 如果未設定，1024 位元將最小值。 
 
若要指定 TLS client 的最大支援各種不同的時間-Helman 金鑰元長度，建立**ClientMaxKeyBitLength**的項目。 此項目不存在於登錄預設。 您所建立的項目之後，您想要的位元長度變更 DWORD 值。 如果未設定，然後不執行最大值。 
 
若要指定時間-Helman 金鑰元長度 TLS 伺服器的預設值，建立**ServerMinKeyBitLength**的項目。 此項目不存在於登錄預設。 您所建立的項目之後，您想要的位元長度變更 DWORD 值。 如果未設定，2048 位元將會預設值。 

## <a name="maximumcachesize"></a>MaximumCacheSize

此項目控制最大的快取的項目。 設定為 0 MaximumCacheSize 停用伺服器端工作階段快取，並會防止重新。 增加 MaximumCacheSize 上方的預設值，導致 Lsass.exe 消耗額外的記憶體。 每個工作階段快取的項目通常需要 2 至 4 KB 的記憶體。 此項目不存在於登錄預設。 預設值是 20000 項目。 

適用版本：中指定為**適用於**清單中開頭本主題。

登錄路徑：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="messaging--fragment-parsing"></a>訊息中心 – 片段剖析

________________________________________
此項目控制將接受分散 TLS 交換簡訊的大小上限。 將不會接受大於允許的大小，並 TLS 交換將會失敗。 這些項目不存在於登錄預設。 

將值設定為 [0x0，分散的郵件不會處理，會導致 TLS 交換失敗。 這樣可 TLS 戶端或伺服器上目前的電腦不相容的 TLS Rfc。 

允許的最大大小可以增加最多 2 ^24-1 位元組。 Client 或伺服器朗讀大量的驗證資料與網路，並允許，最好立刻並不會消耗額外的記憶體的每個安全性操作。 

加入 Windows 7 和 Windows Server 2008 R2。
有可用的更新，可讓 Internet Explorer Windows XP、Windows Vista 或 Windows Server 2008 剖析分散的 TLS 日 SSL 交換訊息。

登錄路徑：HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Messaging

若要指定 TLS client 將接受分散 TLS 交換簡訊的大小上限，請建立**MessageLimitClient**的項目。 您所建立的項目之後，您想要的位元長度變更 DWORD 值。 如果未設定，預設值會 0x8000 位元組。 

若要指定 TLS 伺服器不 client 驗證時，將接受分散 TLS 交換簡訊的大小上限，請建立**MessageLimitServer**的項目。 您所建立的項目之後，您想要的位元長度變更 DWORD 值。 如果未設定，預設值會 0x4000 位元組。 

若要指定 TLS 伺服器 client 驗證時，將接受分散 TLS 交換簡訊的大小上限，請建立**MessageLimitServerClientAuth**的項目。 您所建立的項目之後，您想要的位元長度變更 DWORD 值。 如果未設定，預設值會 0x8000 位元組。 

## <a name="sendtrustedissuerlist"></a>SendTrustedIssuerList

此項目控制旗標信任的發行者清單會在傳送時使用。 在信任的憑證授權單位 client 驗證數百種的伺服器，有太多發行者伺服器，才能將它們傳送所有到 client 電腦要求 client 驗證時。 在這種情形時，可以設定此登錄金鑰，請而不傳送部分清單，Schannel SSP 將不會傳送任何清單 client。

未傳送一份信任的發行者可能會影響項目 client 傳送要求 client 憑證。 例如，當 Internet Explorer 收到的驗證 client 要求時，它只會顯示 client 的憑證鏈結其中一個最多的伺服器來傳送的憑證授權單位。 如果伺服器並未傳送清單，Internet Explorer 會顯示的所有 client 憑證 client 上所安裝的。 

可能需要此行為。 例如時 PKI 環境包含跨憑證，, client 和伺服器的憑證並不會相同 ca;因此，Internet Explorer 無法選擇將最多的其中一個 Ca 伺服器的憑證。 藉由設定不會傳送給受信任的發行者清單伺服器，Internet Explorer 將會傳送所有的憑證。

此項目不存在於登錄預設。

預設傳送信任的發行者清單行為

| Windows 版本 | 時間 |
|-----------------|------|
| Windows Server 2012 和 Windows 8 及更新版本 | FALSE |
| Windows Server 2008 R2 和 Windows 7 與更早 | 為 TRUE |

適用版本：中指定為**適用於**清單中開頭本主題。

登錄路徑：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="servercachetime"></a>ServerCacheTime

此項目控制的時間（毫秒），作業系統會到期伺服器端快取的項目。 設定為 0 停用伺服器端工作階段快取，並會防止重新。 增加 ServerCacheTime 上方的預設值，導致 Lsass.exe 消耗額外的記憶體。 每個工作階段快取的項目通常需要 2 至 4 KB 的記憶體。 此項目不存在於登錄預設。 

適用版本：中指定為**適用於**清單中開頭本主題。

登錄路徑：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

快取預設伺服器的時間：10 小時

## <a name="ssl-20"></a>SSL 2.0

此子控制 SSL 2.0 的使用。

開始使用 Windows 10，版本 1607 年和 Windows Server 2016、SSL 2.0 已經移除，已不再支援。
SSL 2.0 的預設設定，請查看[中 TLS 日 SSL (Schannel SSP) 通訊協定](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx)。 

登錄路徑：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

若要讓 SSL 2.0 通訊協定，建立**啟用**中適當子項目。 此項目不存在於登錄預設。 您所建立的項目後，變更 1 DWORD 值。 若要停用通訊協定，0 變更 DWORD 值。

SSL 2.0 子表格

| 子 | 描述 |
|--------|-------------|
| Client | 控制 SSL 2.0 SSL client 上的使用。 |
| 伺服器 | 控制 SSL 2.0 使用 SSL 伺服器上。 |
| DisabledByDefault | 停用 SSL 2.0 旗標。 |

## <a name="ssl-30"></a>SSL 3.0

此子控制 SSL 3.0 使用。

開始使用 Windows 10，版本 1607 年和 Windows Server 2016、SSL 3.0 已被停用預設值。 SSL 3.0 預設設定，請查看[中 TLS 日 SSL (Schannel SSP) 通訊協定](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx)。 

登錄路徑：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

若要讓 SSL 3.0 通訊協定，建立 Enabled 項目中適當子。 此項目不存在於登錄預設。 您所建立的項目後，變更 1 DWORD 值。 若要停用通訊協定，0 變更 DWORD 值。

SSL 3.0 子表格

| 子 | 描述 |
|--------|-------------|
| Client | 控制 SSL 3.0 SSL client 上的使用。 |
| 伺服器 | 控制 SSL 3.0 使用 SSL 伺服器上。 |
| DisabledByDefault | 停用 SSL 3.0 旗標。 |

## <a name="tls-10"></a>TLS 1.0

此子控制 TLS 1.0 使用。

TLS 1.0 預設設定，請查看[中 TLS 日 SSL (Schannel SSP) 通訊協定](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx)。

登錄路徑：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

若要停用 TLS 1.0 通訊協定，建立**啟用**中適當子項目。 此項目不存在於登錄預設。 您所建立的項目之後，0 變更 DWORD 值。 若要讓通訊協定，變更 1 DWORD 值。

TLS 1.0 子表格

| 子 | 描述 |
|--------|-------------|
| Client | 控制 TLS 1.0 TLS client 上的使用。 |
| 伺服器 | 控制 TLS 1.0 使用 TLS 伺服器上。 |
| DisabledByDefault | 停用 TLS 1.0 旗標。 |

## <a name="tls-11"></a>TLS 1.1

此子控制 TLS 1.1 使用。

TLS 1.1 預設設定，請查看[中 TLS 日 SSL (Schannel SSP) 通訊協定](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx)。

>[!Note] 
>您必須建立 TLS 1.1 支援和交涉伺服器執行 Windows Server 2008 R2 上的**DisabledByDefault**適當子（Client，伺服器）中的項目並將它設為「0」。 登錄中看不到的項目，它會預設為 [1]。

登錄路徑：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

若要停用 TLS 1.1 通訊協定，建立**啟用**中適當子項目。 此項目不存在於登錄預設。 您所建立的項目之後，0 變更 DWORD 值。 若要讓通訊協定，變更 1 DWORD 值。

TLS 1.1 子表格

| 子 | 描述 |
|--------|-------------|
| Client | 控制 TLS 1.1 TLS client 上的使用。 |
| 伺服器 | 控制 TLS 1.1 使用 TLS 伺服器上。 |
| DisabledByDefault | 停用 TLS 1.1 旗標。 |

## <a name="tls-12"></a>TLS 1.2

此子控制 TLS 1.2 使用。

TLS 1.2 預設設定，請查看[中 TLS 日 SSL (Schannel SSP) 通訊協定](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx)。

>[!Note] 
>您必須建立 TLS 1.2 支援和交涉伺服器執行 Windows Server 2008 R2 上的**DisabledByDefault**（Client，伺服器）適當子中的項目並將它設為「0」。 登錄中看不到的項目，它會預設為 [1]。

登錄路徑：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

若要停用 TLS 1.2 通訊協定，建立**啟用**中適當子項目。 此項目不存在於登錄預設。 您所建立的項目之後，0 變更 DWORD 值。 若要讓通訊協定，變更 1 DWORD 值。

TLS 1.2 子表格

| 子 | 描述 |
|--------|-------------|
| Client | 控制 TLS 1.2 TLS client 上的使用。 |
| 伺服器 | 控制 TLS 1.2 使用 TLS 伺服器上。 |
| DisabledByDefault | 停用 TLS 1.2 旗標。 |

## <a name="dtls-10"></a>DTLS 1.0

此子控制 DTLS 1.0 使用。

DTLS 1.0 預設設定，請查看[中 TLS 日 SSL (Schannel SSP) 通訊協定](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx)。

登錄路徑：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

若要停用 DTLS 1.0 通訊協定，建立**啟用**中適當子項目。 此項目不存在於登錄預設。 您所建立的項目之後，0 變更 DWORD 值。 若要讓通訊協定，變更 1 DWORD 值。

DTLS 1.0 子表格

| 子 | 描述 |
|--------|-------------|
| Client | 控制 DTLS 1.0 DTLS client 上的使用。 |
| 伺服器 | 控制 DTLS 1.0 DTLS 伺服器上的使用。 |
| DisabledByDefault | 停用 DTLS 1.0 旗標。 |

## <a name="dtls-12"></a>DTLS 1.2

此子控制 DTLS 1.2 使用。

DTLS 1.2 預設設定，請查看[中 TLS 日 SSL (Schannel SSP) 通訊協定](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx)。

登錄路徑：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

若要停用 DTLS 1.2 通訊協定，建立**啟用**中適當子項目。 此項目不存在於登錄預設。 您所建立的項目之後，0 變更 DWORD 值。 若要讓通訊協定，變更 1 DWORD 值。

DTLS 1.2 子表格

| 子 | 描述 |
|--------|-------------|
| Client | 控制 DTLS 1.2 DTLS client 上的使用。 |
| 伺服器 | 控制 DTLS 1.2 DTLS 伺服器上的使用。 |
| DisabledByDefault | 停用 DTLS 1.2 旗標。 |

