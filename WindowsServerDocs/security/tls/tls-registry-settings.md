---
title: 傳輸層安全性（TLS）登錄設定
description: Windows Server 安全性
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
author: justinha
ms.author: justinha
manager: brianlic-msft
ms.date: 02/28/2019
ms.openlocfilehash: 60202e537093bd21515043ba56f70f3895c91d42
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/13/2020
ms.locfileid: "79322400"
---
# <a name="transport-layer-security-tls-registry-settings"></a>傳輸層安全性（TLS）登錄設定

>適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows 10

此適用于 IT 專業人員的參考主題包含透過 Schannel 安全性支援的傳輸層安全性（TLS）通訊協定和安全通訊端層（SSL）通訊協定之 Windows 執行支援的登錄設定資訊提供者（SSP）。 本主題所涵蓋的登錄子機碼和專案可協助您管理和疑難排解安全通道 SSP，特別是 TLS 和 SSL 通訊協定。 

> [!CAUTION]
> 這項資訊供您在進行疑難排解或確認必要的設定是否套用時做為參考之用。
> 建議您不要直接編輯登錄，除非已沒有其他替代方案。
> 登錄的修改在套用之前，登錄編輯程式或 Windows 作業系統並不會加以驗證。
> 因此可能會儲存不正確的值，而這將導致系統發生無法復原的錯誤。
> 可能的話，請不要直接編輯登錄，而應使用群組原則或像是 Microsoft Management Console (MMC) 的其他 Windows 工具來完成工作。
> 如果您必須編輯登錄，必須非常小心。

## <a name="certificatemappingmethods"></a>CertificateMappingMethods 

此項目依預設不存在於登錄中。 預設值是以下所列的四個憑證對應方法全都會受支援。

當伺服器應用程式要求用戶端驗證時，安全通道會自動嘗試將用戶端電腦所提供的憑證對應至使用者帳戶。 您可以建立對應，將憑證資訊與 Windows 使用者帳戶建立關係，來驗證使用用戶端憑證登入的使用者。 建立並啟用憑證對應之後，每當用戶端出示用戶端憑證時，您的伺服器應用程式便會自動將該名使用者與適當的 Windows 使用者帳戶產生關聯。

在大部分情況下，憑證會透過兩種方式之一對應至使用者帳戶： 

- 單一憑證對應至單一使用者帳戶 (一對一對應)。
- 多個憑證對應至一個使用者帳戶 (多對一對應)。

根據預設，安全通道提供者會使用下列四個憑證對應方法 (依慣用程度列出)：

1. Kerberos service-for-user (S4U) 憑證對應
2. 使用者主體名稱對應
3. 一對一對應 (也稱為主體/簽發者對應)
4. 多對一對應

適用版本：如本主題開頭的 [**適用**于] 清單中所指定。

登錄路徑： HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="ciphers"></a>加密

您應該藉由設定加密套件順序來控制 TLS/SSL 密碼。 如需詳細資訊，請參閱設定[TLS 加密套件順序](manage-tls.md#configuring-tls-cipher-suite-order)。

如需安全通道 SSP 所使用之預設加密套件順序的相關資訊，請參閱[TLS/SSL （安全通道 ssp）中的加密套件](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx)。 

## <a name="ciphersuites"></a>CipherSuites

必須使用群組原則、MDM 或 PowerShell 來設定 TLS/SSL 加密套件，如需詳細資訊，請參閱設定[Tls 加密套件順序](manage-tls.md#configuring-tls-cipher-suite-order)。

如需安全通道 SSP 所使用之預設加密套件順序的相關資訊，請參閱[TLS/SSL （安全通道 ssp）中的加密套件](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx)。 


## <a name="clientcachetime"></a>ClientCacheTime

此項目會控制作業系統使用戶端快取項目到期所需的時間量 (以毫秒為單位)。 若值為 0，將會關閉安全連線快取。 此項目依預設不存在於登錄中。 

用戶端第一次透過安全通道 SSP 連線到伺服器時，會執行完整 TLS/SSL 信號交換。 此動作完成時，主要密碼、加密套件和憑證會分別儲存在用戶端和伺服器的工作階段快取中。

從 Windows Server 2008 和 Windows Vista 開始，預設用戶端快取時間為10小時。

登錄路徑： HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

預設用戶端快取時間

## <a name="enableocspstaplingforsni"></a>EnableOcspStaplingForSni

線上憑證狀態通訊協定（OCSP）裝訂可讓 web 伺服器（例如 Internet Information Services （IIS））在 TLS 信號交換期間，將伺服器憑證傳送到用戶端時，提供伺服器憑證的目前撤銷狀態。 這項功能可減少 OCSP 伺服器上的負載，因為 web 伺服器可以快取伺服器憑證的目前 OCSP 狀態，並將它傳送給多個 web 用戶端。 若沒有此功能，每個 web 用戶端都會嘗試從 OCSP 伺服器抓取伺服器憑證的目前 OCSP 狀態。 這會在該 OCSP 伺服器上產生高負載。 

除了 IIS 之外，透過 HTTP.sys 的 web 服務也可以受益于這項設定，包括 Active Directory 同盟服務（AD FS）和 Web 應用程式 Proxy （WAP）。 

根據預設，具有簡單安全（SSL/TLS）系結的 IIS 網站會啟用 OCSP 支援。 不過，如果 IIS 網站使用下列其中一種或兩種類型的安全（SSL/TLS）系結，則預設不會啟用此支援：
- 需要伺服器名稱指示
- 使用集中式憑證存放區

在此情況下，TLS 交握期間的伺服器 hello 回應預設不會包含 OCSP 裝訂狀態。 此行為可改善效能： Windows OCSP 裝訂的執行會調整為數百個伺服器憑證。 由於 SNI 和 CCS 可讓 IIS 擴充到數千個可能有數千個伺服器憑證的網站，因此設定此行為預設為啟用可能會導致效能問題。

適用的版本：從 Windows Server 2012 和 Windows 8 開始的所有版本。 

登錄路徑： [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL]

新增下列機碼：

"EnableOcspStaplingForSni" = dword：00000001

若要停用，請將 DWORD 值設為0：

"EnableOcspStaplingForSni" = dword：00000000

> [!NOTE] 
> 啟用此登錄機碼可能會對效能造成影響。

## <a name="fipsalgorithmpolicy"></a>FIPSAlgorithmPolicy

此項目會控制美國聯邦資訊處理 (FIPS) 相容。 預設為 0。

適用的版本：從 Windows Server 2012 和 Windows 8 開始的所有版本。 

登錄路徑： HKLM SYSTEM\CurrentControlSet\Control\LSA

Windows Server FIPS 加密套件：請參閱[SCHANNEL SSP 中支援的加密套件和通訊協定](https://technet.microsoft.com/library/dn786419.aspx)。

## <a name="hashes"></a>雜湊

TLS/SSL 雜湊演算法應該藉由設定加密套件順序來控制。 如需詳細資訊，請參閱設定[TLS 加密套件順序](manage-tls.md#configuring-tls-cipher-suite-order)。

## <a name="issuercachesize"></a>IssuerCacheSize

此項目會控制簽發者快取大小，會與簽發者對應搭配使用。 Schannel SSP 會嘗試對應用戶端憑證鏈中的所有簽發者，而不只是用戶端憑證的直接發行者。 當簽發者未對應至帳戶時 (一般會如此)，伺服器可能會嘗試重複對應相同的簽發者名稱，每秒達數百次。 

為避免此狀況，伺服器備有負快取，因此若簽發者名稱未對應至帳戶，該名稱將會新增至快取，安全通道 SSP 將不會再次嘗試對應簽發者名稱，直到快取項目到期為止。 此登錄項目會指定快取大小。 此項目依預設不存在於登錄中。 預設值為 100。 

適用的版本：從 Windows Server 2008 和 Windows Vista 開始的所有版本。

登錄路徑： HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="issuercachetime"></a>IssuerCacheTime

此項目會控制快取逾時間隔的長度 (以毫秒為單位)。 Schannel SSP 會嘗試對應用戶端憑證鏈中的所有簽發者，而不只是用戶端憑證的直接發行者。 若簽發者未對應至帳戶 (一般會如此)，伺服器可能會嘗試重複對應相同的簽發者名稱，每秒達數百次。

為避免此狀況，伺服器備有負快取，因此若簽發者名稱未對應至帳戶，該名稱將會新增至快取，安全通道 SSP 將不會再次嘗試對應簽發者名稱，直到快取項目到期為止。 此快取會基於效能考量而保留，使系統不會繼續嘗試對應相同的簽發者。 此項目依預設不存在於登錄中。 預設值為 [10] 分鐘。

適用的版本：從 Windows Server 2008 和 Windows Vista 開始的所有版本。

登錄路徑： HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="keyexchangealgorithm---client-rsa-key-sizes"></a>Asymmetricalgorithm.keyexchangealgorithm-用戶端 RSA 金鑰大小

此專案會控制用戶端的 RSA 金鑰大小。 

金鑰交換演算法的使用應由設定加密套件順序來控制。

已在 Windows 10 版本1507和 Windows Server 2016 中新增。

登錄路徑： HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\KeyExchangeAlgorithms\PKCS

若要針對 TLS 用戶端指定最小支援的 RSA 金鑰位長度範圍，請建立**ClientMinKeyBitLength**專案。 此項目依預設不存在於登錄中。 建立專案之後，請將 DWORD 值變更為所需的位長度。 如果未設定，1024位會是最小值。 

若要指定 TLS 用戶端的 RSA 金鑰位長度的最大支援範圍，請建立**ClientMaxKeyBitLength**專案。 此項目依預設不存在於登錄中。 建立專案之後，請將 DWORD 值變更為所需的位長度。 如果未設定，則不會強制執行最大值。

## <a name="keyexchangealgorithm---diffie-hellman-key-sizes"></a>Asymmetricalgorithm.keyexchangealgorithm-Diffie-hellman 金鑰大小

此專案會控制 Diffie-hellman 金鑰大小。 

金鑰交換演算法的使用應由設定加密套件順序來控制。

已在 Windows 10 版本1507和 Windows Server 2016 中新增。

登錄路徑： HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\KeyExchangeAlgorithms\Diffie-Hellman

若要針對 TLS 用戶端指定 Helman 金鑰位長度的最低支援範圍，請建立**ClientMinKeyBitLength**專案。 此項目依預設不存在於登錄中。 建立專案之後，請將 DWORD 值變更為所需的位長度。 如果未設定，1024位會是最小值。 
 
若要指定 TLS 用戶端的 Helman 金鑰位長度的最大支援範圍，請建立**ClientMaxKeyBitLength**專案。 此項目依預設不存在於登錄中。 建立專案之後，請將 DWORD 值變更為所需的位長度。 如果未設定，則不會強制執行最大值。 
 
若要指定 TLS 伺服器預設的 Diffie-hellman Helman 金鑰位長度，請建立**ServerMinKeyBitLength**專案。 此項目依預設不存在於登錄中。 建立專案之後，請將 DWORD 值變更為所需的位長度。 如果未設定，2048位將會是預設值。 

## <a name="maximumcachesize"></a>MaximumCacheSize

此項目會控制快取元素的數目上限。 將 MaximumCacheSize 設為 0，會停用伺服器端工作階段快取並防止重新連線。 將 MaximumCacheSize 增加至預設值以上，會導致 Lsass.exe 耗用更多記憶體。 每個會話快取元素通常需要2到 4 KB 的記憶體。 此項目依預設不存在於登錄中。 預設值為20000個元素。 

適用的版本：從 Windows Server 2008 和 Windows Vista 開始的所有版本。

登錄路徑： HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="messaging--fragment-parsing"></a>訊息-片段剖析

________________________________________
此專案會控制將接受的分散 TLS 交握訊息的最大允許大小。 將不會接受大於允許大小的訊息，且 TLS 交握將會失敗。 這些專案預設不存在於登錄中。 

當您將值設定為0x0 時，不會處理分散的訊息，而且會導致 TLS 交握失敗。 這會讓目前電腦上的 TLS 用戶端或伺服器不符合 TLS Rfc。 

允許的大小上限可以增加至 2 ^ 24-1 個位元組。 允許用戶端或伺服器從網路讀取並儲存大量未經驗證的資料並不是個好主意，而且會針對每個安全性內容取用額外的記憶體。 

已在 Windows 7 和 Windows Server 2008 R2 中新增。
可以使用 windows XP、Windows Vista 或 Windows Server 2008 中的 Internet Explorer 來剖析已分割的 TLS/SSL 交握訊息的更新。

登錄路徑： HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Messaging

若要指定 TLS 用戶端會接受的分散 TLS 交握訊息的最大允許大小，請建立**MessageLimitClient**專案。 建立專案之後，請將 DWORD 值變更為所需的位長度。 如果未設定，預設值會是0x8000 個位元組。 

若要指定在沒有用戶端驗證時，TLS 伺服器將接受的分散 TLS 交握訊息的最大允許大小，請建立**MessageLimitServer**專案。 建立專案之後，請將 DWORD 值變更為所需的位長度。 如果未設定，預設值將會是0x4000 個位元組。 

若要指定在用戶端驗證時，TLS 伺服器將接受的分散 TLS 交握訊息的最大允許大小，請建立**MessageLimitServerClientAuth**專案。 建立專案之後，請將 DWORD 值變更為所需的位長度。 如果未設定，預設值會是0x8000 個位元組。 

## <a name="sendtrustedissuerlist"></a>SendTrustedIssuerList

此項目會控制信任的簽發者清單傳送時所使用的旗標。 如果伺服器信任數百個憑證授權單位以進行用戶端驗證，在要求用戶端驗證時，伺服器將會有太多簽發者能夠將其全數傳送到用戶端電腦。 在此情況下，可以設定這個登錄機碼，安全通道 SSP 將不會傳送任何清單給用戶端，而不是傳送部分清單。

若未傳送信任的簽發者清單，可能會影響用戶端被要求提供用戶端憑證時所傳送的內容。 例如，當 Internet Explorer 收到用戶端驗證的要求時，它只會顯示鏈結至伺服器所傳送的其中一個憑證授權單位的用戶端憑證。 如果伺服器未傳送清單，Internet Explorer 將會顯示所有安裝在用戶端上的用戶端憑證。 

此行為可能不妥當。 例如，當 PKI 環境包含交叉憑證時，用戶端和伺服器憑證將不會有相同的根 CA;因此，Internet Explorer 無法選擇連結到其中一個伺服器 Ca 的憑證。 若將伺服器設定為不傳送信任的簽發者清單，Internet Explorer 將會傳送其所有的憑證。

此項目依預設不存在於登錄中。

預設傳送信任的簽發者清單行為

| Windows 版本 | 時間 |
|-----------------|------|
| Windows Server 2012 和 Windows 8 和更新版本 | FALSE |
| Windows Server 2008 R2 和 Windows 7 和更早版本 | TRUE |

適用的版本：從 Windows Server 2008 和 Windows Vista 開始的所有版本。

登錄路徑： HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="servercachetime"></a>ServerCacheTime

此項目會控制作業系統使伺服器端快取項目到期所需的時間量 (以毫秒為單位)。 若值為 0，將會停用伺服器端工作階段快取並防止重新連線。 將 ServerCacheTime 增加至預設值以上，會導致 Lsass.exe 耗用更多記憶體。 每個會話快取元素通常需要2到 4 KB 的記憶體。 此項目依預設不存在於登錄中。 

適用的版本：從 Windows Server 2008 和 Windows Vista 開始的所有版本。

登錄路徑： HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

預設伺服器快取時間：10小時

## <a name="ssl-20"></a>SSL 2.0

這個子機碼會控制 SSL 2.0 的使用。

從 Windows 10 版本1607和 Windows Server 2016 開始，已移除 SSL 2.0，而且不再支援。
如需 SSL 2.0 預設設定，請參閱[TLS/SSL （SCHANNEL SSP）中的通訊協定](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx)。 

登錄路徑： HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

若要啟用 SSL 2.0 通訊協定，請在 [用戶端] 或 [伺服器] 子機碼中建立**已啟用**的專案，如下表所述。 此項目依預設不存在於登錄中。 建立專案之後，請將 DWORD 值變更為1。 

SSL 2.0 子機碼資料表

| 子機碼 | 描述 |
|--------|-------------|
| 用戶端 | 控制 SSL 用戶端上的 SSL 2.0 使用。 |
| Server | 控制 SSL 伺服器上的 SSL 2.0 使用。 |

若要停用用戶端或伺服器的 SSL 2.0，請將 DWORD 值變更為0。 如果 SSPI 應用程式要求使用 SSL 2.0，則會遭到拒絕。 

若要預設停用 SSL 2.0，請建立**DisabledByDefault**專案，並將 DWORD 值變更為1。 如果 SSPI 應用程式確定要求使用 SSL 2.0，則可能會進行協商。 

下列範例顯示登錄中停用的 SSL 2.0：

![SSL 2.0 已停用](images/ssl-2-registry-setting.png)


## <a name="ssl-30"></a>SSL 3.0

這個子機碼會控制 SSL 3.0 的使用。

從 Windows 10 版本1607和 Windows Server 2016 開始，預設會停用 SSL 3.0。 如需 SSL 3.0 預設設定，請參閱[TLS/SSL （SCHANNEL SSP）中的通訊協定](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx)。 

登錄路徑： HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

若要啟用 SSL 3.0 通訊協定，請在 [用戶端] 或 [伺服器] 子機碼中建立**已啟用**的專案，如下表所述。  
此項目依預設不存在於登錄中。 建立專案之後，請將 DWORD 值變更為1。 

SSL 3.0 子機碼資料表

| 子機碼 | 描述 |
|--------|-------------|
| 用戶端 | 控制 SSL 用戶端上的 SSL 3.0 使用。 |
| Server | 控制 SSL 伺服器上的 SSL 3.0 使用。 |

若要停用用戶端或伺服器的 SSL 3.0，請將 DWORD 值變更為0。
如果 SSPI 應用程式要求使用 SSL 3.0，則會遭到拒絕。 

若要預設停用 SSL 3.0，請建立**DisabledByDefault**專案，並將 DWORD 值變更為1。 如果 SSPI 應用程式明確要求使用 SSL 3.0，則可能會進行協商。 

下列範例顯示登錄中停用的 SSL 3.0：

![SSL 3.0 已停用](images/ssl-3-registry-setting.png)

## <a name="tls-10"></a>TLS 1.0

這個子機碼會控制 TLS 1.0 的使用。

如需 TLS 1.0 預設設定，請參閱[tls/SSL （SCHANNEL SSP）中的通訊協定](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx)。

登錄路徑： HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

若要啟用 TLS 1.0 通訊協定，請在用戶端或伺服器子機碼中建立**已啟用**的專案，如下表所述。 此項目依預設不存在於登錄中。 建立專案之後，請將 DWORD 值變更為1。 

TLS 1.0 子機碼資料表

| 子機碼 | 描述 |
|--------|-------------|
| 用戶端 | 控制 TLS 用戶端上的 TLS 1.0 使用。 |
| Server | 控制 TLS 伺服器上的 TLS 1.0 使用。 |

若要停用用戶端或伺服器的 TLS 1.0，請將 DWORD 值變更為0。
如果 SSPI 應用程式要求使用 TLS 1.0，則會遭到拒絕。 

若要預設停用 TLS 1.0，請建立**DisabledByDefault**專案，並將 DWORD 值變更為1。 如果 SSPI 應用程式明確要求使用 TLS 1.0，則可能會進行協商。 

下列範例顯示在登錄中停用的 TLS 1.0：

![已停用 TLS 1。0](images/tls-registry-setting.png)

## <a name="tls-11"></a>TLS 1.1

這個子機碼會控制 TLS 1.1 的使用。

如需 TLS 1.1 預設設定，請參閱[tls/SSL （SCHANNEL SSP）中的通訊協定](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx)。

登錄路徑： HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

若要啟用 TLS 1.1 通訊協定，請在用戶端或伺服器子機碼中建立**已啟用**的專案，如下表所述。 此項目依預設不存在於登錄中。 建立專案之後，請將 DWORD 值變更為1。 

TLS 1.1 子機碼資料表

| 子機碼 | 描述 |
|--------|-------------|
| 用戶端 | 控制 TLS 用戶端上的 TLS 1.1 使用。 |
| Server | 控制 TLS 伺服器上的 TLS 1.1 使用。 |

若要停用用戶端或伺服器的 TLS 1.1，請將 DWORD 值變更為0。
如果 SSPI 應用程式要求使用 TLS 1.1，則會遭到拒絕。 

若要預設停用 TLS 1.1，請建立**DisabledByDefault**專案，並將 DWORD 值變更為1。 如果 SSPI 應用程式明確要求使用 TLS 1.1，則可能會進行協商。 

下列範例顯示在登錄中停用的 TLS 1.1：

![已停用 TLS 1。1](images/tls-11-registry-setting.png)

## <a name="tls-12"></a>TLS 1.2

這個子機碼會控制 TLS 1.2 的使用。

如需 TLS 1.2 預設設定，請參閱[tls/SSL （SCHANNEL SSP）中的通訊協定](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx)。

登錄路徑： HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

若要啟用 TLS 1.2 通訊協定，請在用戶端或伺服器子機碼中建立**已啟用**的專案，如下表所述。 此項目依預設不存在於登錄中。 建立專案之後，請將 DWORD 值變更為1。 

TLS 1.2 子機碼資料表

| 子機碼 | 描述 |
|--------|-------------|
| 用戶端 | 控制 TLS 用戶端上的 TLS 1.2 使用。 |
| Server | 控制 TLS 伺服器上的 TLS 1.2 使用。 |

若要停用用戶端或伺服器的 TLS 1.2，請將 DWORD 值變更為0。
如果 SSPI 應用程式要求使用 TLS 1.2，則會遭到拒絕。 

若要預設停用 TLS 1.2，請建立**DisabledByDefault**專案，並將 DWORD 值變更為1。 如果 SSPI 應用程式明確要求使用 TLS 1.2，則可能會進行協商。 

下列範例顯示在登錄中停用的 TLS 1.2：

![已停用 TLS 1。2](images/tls-12-registry-setting.png)

## <a name="dtls-10"></a>DTLS 1.0

這個子機碼會控制 DTLS 1.0 的使用。

如需 DTLS 1.0 預設設定，請參閱[TLS/SSL （SCHANNEL SSP）中的通訊協定](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx)。

登錄路徑： HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

若要啟用 DTLS 1.0 通訊協定，請在用戶端或伺服器子機碼中建立**已啟用**的專案，如下表所述。 此項目依預設不存在於登錄中。 建立專案之後，請將 DWORD 值變更為1。 

DTLS 1.0 子機碼資料表

| 子機碼 | 描述 |
|--------|-------------|
| 用戶端 | 控制在 DTLS 用戶端上使用 DTLS 1.0 的情況。 |
| Server | 控制 DTLS 伺服器上的 DTLS 1.0 使用。 |

若要停用用戶端或伺服器的 DTLS 1.0，請將 DWORD 值變更為0。
如果 SSPI 應用程式要求使用 DTLS 1.0，則會遭到拒絕。 

若要預設停用 DTLS 1.0，請建立**DisabledByDefault**專案，並將 DWORD 值變更為1。 如果 SSPI 應用程式明確要求使用 DTLS 1.0，則可能會進行協商。 

下列範例顯示登錄中停用的 DTLS 1.0：

![已停用 DTLS 1。0](images/dtls-10-registry-setting.png)

## <a name="dtls-12"></a>DTLS 1。2

這個子機碼會控制 DTLS 1.2 的使用。

如需 DTLS 1.2 預設設定，請參閱[TLS/SSL （SCHANNEL SSP）中的通訊協定](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx)。

登錄路徑： HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

若要啟用 DTLS 1.2 通訊協定，請在用戶端或伺服器子機碼中建立**已啟用**的專案，如下表所述。 此項目依預設不存在於登錄中。 建立專案之後，請將 DWORD 值變更為1。 

DTLS 1.2 子機碼資料表

| 子機碼 | 描述 |
|--------|-------------|
| 用戶端 | 控制在 DTLS 用戶端上使用 DTLS 1.2 的情況。 |
| Server | 控制 DTLS 伺服器上的 DTLS 1.2 使用。 |


若要停用用戶端或伺服器的 DTLS 1.2，請將 DWORD 值變更為0。
如果 SSPI 應用程式要求使用 DTLS 1.0，則會遭到拒絕。 

若要預設停用 DTLS 1.2，請建立**DisabledByDefault**專案，並將 DWORD 值變更為1。 如果 SSPI 應用程式明確要求使用 DTLS 1.2，則可能會進行協商。 

下列範例顯示登錄中停用的 DTLS 1.1：

![已停用 DTLS 1。1](images/dtls-11-registry-setting.png)


