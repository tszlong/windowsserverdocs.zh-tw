---
title: 中的網站定義和網域控制站位置新增效能微調
description: Active Directory 效能微調中的網站定義和網域控制站放置考慮。
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: timwi; chrisrob; herbertm; kenbrumf;  mleary; shawnrab
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: aac7b8f37de2132778bd681d2f2e29ad0ad0810d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851871"
---
# <a name="proper-placement-of-domain-controllers-and-site-considerations"></a>適當位置的網域控制站和網站考慮

適當的網站定義對於效能很重要。 失去網站的用戶端可能會遇到效能不佳的驗證和查詢。 此外，在用戶端上引進 IPv6 之後，要求可能來自 IPv4 或 IPv6 位址，Active Directory 必須為 IPv6 正確定義網站。 當設定兩者時，作業系統偏好 IPv6 到 IPv4。

從 Windows Server 2008 開始，網域控制站會嘗試使用名稱解析來執行反向查詢，以判斷用戶端應該位於的網站。 這可能會導致 ATQ 執行緒集區耗盡，並導致網域控制站變成沒有回應。 適當的解決方法是正確定義 IPv6 的網站拓撲。 因應措施是，您可以將名稱解析基礎結構優化，以快速回應網域控制站要求。 如需詳細資訊，請參閱[Windows server 2008 或 Windows server 2008 R2 網域控制站延遲回應 LDAP 或 Kerberos 要求](https://support.microsoft.com/kb/2668820)。

另一個考慮區域是在使用 Rodc 的案例中尋找讀取/寫入 Dc。  某些作業需要存取可寫入的網域控制站，或在唯讀網域控制站夠好時，將目標設為可寫入的網域控制站。  將這些案例優化會採用兩個路徑：
-   當唯讀網域控制站足夠時，聯絡可寫入的網域控制站。  這需要變更應用程式代碼。
-   可能需要可寫入的網域控制站。  將讀寫網域控制站放在中央位置，以將延遲降到最低。

如需進一步資訊，請參閱：
-   [Rodc 的應用程式相容性](https://technet.microsoft.com/library/cc772597.aspx)
-   [Active Directory 服務介面（ADSI）和唯讀網域控制站（RODC）–避免效能問題](https://blogs.technet.microsoft.com/fieldcoding/2012/06/24/active-directory-service-interface-adsi-and-the-read-only-domain-controller-rodc-avoiding-performance-issues/)

## <a name="optimize-for-referrals"></a>針對參考優化

參照是當網域控制站未裝載所查詢的資料分割複本時，LDAP 查詢的重新導向方式。 傳回參考時，它會包含分割區的辨別名稱、DNS 名稱和埠號碼。 用戶端會使用此資訊在裝載資料分割的伺服器上繼續查詢。 這是 DCLocator 的案例，而且所有建議的網站定義和網域控制站放置都會維持不變，但相依于參照的應用程式通常會被忽略。 建議您確保 AD 拓撲（包括網站定義和網域控制站放置）正確反映用戶端的需求。 此外，這可能包括在單一網站中擁有來自多個網域的網域控制站、調整 DNS 設定，或重新放置應用程式的網站。

## <a name="optimization-considerations-for-trusts"></a>信任的優化考慮

在樹系內案例中，會根據下列網域階層來處理信任：&gt; 子域&gt; 樹系根域&gt; 子域&gt; 的單一子域。 這表示在樹系根節點和每個父系上的安全通道可能會因為驗證要求的匯總而超載，傳輸信任階層中的 Dc。 當驗證也必須傳輸高度潛在的連結來影響上述流程時，這也可能會導致大型地理散佈的 Active Directory 延遲。 多載可能發生在樹系和下層信任案例中。 下列建議適用于所有案例：

-   適當地調整 MaxConcurrentAPI，以支援跨安全通道的負載。 如需詳細資訊，請參閱[如何使用 MaxConcurrentApi 設定進行 NTLM 驗證的效能微調](https://support.microsoft.com/kb/2688798/EN-US)。

-   根據負載，適當地建立快捷方式信任。

-   請確定網域中的每個網域控制站都能夠執行名稱解析，並與受信任網域中的網域控制站進行通訊。

-   請確定已將位置考慮納入信任。

-   盡可能啟用 Kerberos，並將安全通道的使用降到最低，以降低遇到 MaxConcurrentAPI 瓶頸的風險。

跨網域信任案例是針對許多客戶一直都是痛點的領域。 名稱解析和連線問題通常是因為防火牆而造成信任網域控制站上的資源耗盡，並影響所有用戶端。 此外，經常被忽略的案例是將對受信任網域控制站的存取進行優化。 確保此作業正常運作的主要區域如下所示：

-   請確定信任的網域控制站所使用的 DNS 和 WINS 名稱解析，可以解析受信任網域的正確網域控制站清單。

    -   以靜態方式新增的記錄在一段時間後會變得過時並重新產生連接問題。 DNS 轉送、動態 DNS 和合併 WINS/DNS 基礎結構在長期執行中比較容易維護。

    -   針對用戶端可能需要存取的環境中的每個資源，請務必適當地設定轉寄和反向對應區域的轉寄站、條件式轉寄和次要複本。 同樣地，這需要手動維護，而且往往會變得過時。 基礎結構的匯總非常理想。

-   信任網域中的網域控制站會先嘗試在位於相同網站的受信任網域中尋找網域控制站，然後再容錯回復到一般定位器。

    -   如需 DCLocator 運作方式的詳細資訊，請參閱在[最接近的網站中尋找網域控制站](https://technet.microsoft.com/library/cc978016.aspx)。

    -   在受信任網域與信任網域之間融合網站名稱，以反映相同位置中的網域控制站。 請確定子網和 IP 位址對應已適當地連結至兩個樹系中的網站。 如需詳細資訊，請參閱[跨樹系信任的網域定位器](https://blogs.technet.com/b/askds/archive/2008/09/24/domain-locator-across-a-forest-trust.aspx)。

    -   確定埠已根據 DCLocator 需求開啟，適用于網域控制站位置。 如果網域之間有防火牆存在，請確定已針對所有信任正確設定防火牆。 如果防火牆未開啟，信任的網域控制站仍然會嘗試存取受信任的網域。 如果通訊因任何原因而失敗，信任的網域控制站最終會將要求提供給受信任的網域控制站。 不過，這些時間輸出可能會花費數秒的每個要求，如果傳入要求的數量很高，則可能會耗盡信任網域控制站上的網路埠。 用戶端可能會在網域控制站上遇到等待時間超時的擱置執行緒，這可能會轉譯成無回應的應用程式（如果應用程式在前景執行緒中執行要求）。 如需詳細資訊，請參閱[如何設定網域和信任的防火牆](https://support.microsoft.com/kb/179442)。

    -   使用 DnsAvoidRegisterRecords 來消除效能不佳或高延遲的網域控制站，例如在衛星網站中，從廣告到一般定位器。 如需詳細資訊，請參閱[如何優化位於用戶端網站外部之網域控制站或通用類別目錄的位置](https://support.microsoft.com/kb/306602)。

        > [!NOTE]
        > 用戶端可以使用的網域控制站數目有大約50的實際限制。 這些應該是最理想的網站和最高容量網域控制站。

    
    -  請考慮將網域控制站放在同一個實體位置中受信任和信任的網域。

在所有信任案例中，認證會根據驗證要求中指定的網域來路由傳送。 這也適用于 LookupAccountName 和 LsaLookupNames 的查詢（以及其他專案，這些只是最常使用的） Api。 當這些 Api 的網域參數傳遞 Null 值時，網域控制站會嘗試尋找每個可用信任網域中指定的帳號名稱。

-   當指定 Null 網域時，停用檢查所有可用的信任。 [如何使用 LsaLookupRestrictIsolatedNameLevel 登錄專案來限制外部受信任網域中的隔離名稱查閱](https://support.microsoft.com/kb/818024)

-   停用在所有可用的信任上指定 Null 網域的傳遞驗證要求。 [如果 Active Directory 網域控制站上有許多外部信任，Lsass.exe 進程可能會停止回應](https://support.microsoft.com/kb/923241/EN-US)

## <a name="see-also"></a>另請參閱
- [Active Directory 伺服器的效能微調](index.md)
- [硬體考量](hardware-considerations.md)
- [LDAP 考量](ldap-considerations.md)
- [針對 ADDS 效能問題進行疑難排解](troubleshoot.md) 
- [Active Directory Domain Services 的容量規劃](https://go.microsoft.com/fwlink/?LinkId=324566)