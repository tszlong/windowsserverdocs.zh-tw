---
title: 網站定義和網域控制站的建置在 ADDS 效能微調
description: 網站定義和網域控制站放置考量事項中 Active Directory 效能微調。
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: TimWi; ChrisRob; HerbertM; KenBrumf;  MLeary; ShawnRab
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: e1652e45f51500ceeb0026b8892fbe9c54ff38f3
ms.sourcegitcommit: d84dc3d037911ad698f5e3e84348b867c5f46ed8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/28/2019
ms.locfileid: "66266641"
---
# <a name="proper-placement-of-domain-controllers-and-site-considerations"></a>獲得正確位置的網域控制站和站台考量

適當的站台定義效能很重要。 站台外的用戶端可能會遇到驗證和查詢的效能不佳。 此外，隨著 IPv6 的用戶端上，要求可以來自 IPv4 或 IPv6 位址和 Active Directory 必須能夠正確地定義適用於 IPv6 的網站。 同時設定時，作業系統會偏好 IPv6 ipv4。

從 Windows Server 2008 中，網域控制站嘗試使用名稱解析來進行反向的對應，以判斷站台用戶端應該位於中。 這會造成耗盡 ATQ 執行緒集區，並且會導致網域控制站沒有回應。 適當的解決方式，這個是 ipv6 定義適當的站台拓撲。 因應措施，其中可以最佳化的名稱解析基礎結構，以快速回應要網域控制站要求。 如需詳細資訊，請參閱[Windows Server 2008 或 Windows Server 2008 R2 網域控制站的延遲回應 LDAP 或 Kerberos 要求](https://support.microsoft.com/kb/2668820)。

需要考量的另一個區域尋找讀取/寫入網域控制站的 Rodc 正在使用中的案例。  某些作業需要存取可寫入的網域控制站或唯讀網域控制站即已足夠時，目標可寫入的網域控制站。  最佳化這些案例會採用兩個路徑：
-   當唯讀網域控制站即已足夠，請連絡可寫入網域控制站。  這需要應用程式程式碼變更。
-   其中可能需要可寫入的網域控制站。  進行讀寫網域控制站在中央位置以將延遲降至最低。

如需進一步資訊參考：
-   [應用程式與 rodc 之間的相容性](https://technet.microsoft.com/library/cc772597.aspx)
-   [Active Directory 服務介面 (ADSI) 和讀取只有網域控制站 (RODC) – 避免效能問題](https://blogs.technet.microsoft.com/fieldcoding/2012/06/24/active-directory-service-interface-adsi-and-the-read-only-domain-controller-rodc-avoiding-performance-issues/)

## <a name="optimize-for-referrals"></a>最佳化的轉介

轉介是如何 LDAP 查詢會重新導向網域控制站未裝載一份分割區查詢。 傳回一個轉介時，它會包含資料分割的辨別的名稱、 DNS 名稱和連接埠號碼。 用戶端會使用此資訊以繼續在裝載資料分割伺服器上的查詢。 這是 DCLocator 案例的所有建議網站定義和設置網域控制站會保留下來，但通常會忽略應用程式相依於轉介。 建議您確保 AD 拓撲，包括網站定義和設置網域控制站會正確反映在用戶端的需求。 此外，這可能包括從多個調整 DNS 設定，或重新放置應用程式的站台的單一站台中的網域中有網域控制站。

## <a name="optimization-considerations-for-trusts"></a>信任的最佳化考量

在樹系內的案例中，信任會根據下列網域階層處理：直屬子網域-&gt;子網域-&gt;樹系根網域-&gt;子網域-&gt;直屬子網域。 安全通道的樹系根目錄中和每個父代，這表示可以成為多載，因為傳輸網域控制站信任階層中的驗證要求的彙總。 驗證也會對傳輸高度隱蔽的連結，以影響上述流程時，這也可能會遇到延遲，大型的地理散佈情況之 Active Directory 中。 多載可能會發生在跨樹系和下層信任案例中。 下列建議適用於所有案例：

-   適當地調整 MaxConcurrentAPI，若要透過安全的通道支援的負載。 如需詳細資訊，請參閱 <<c0> [ 如何使用 MaxConcurrentApi 設定 NTLM 驗證的效能微調](https://support.microsoft.com/kb/2688798/EN-US)。

-   建立捷徑信任，視需要根據負載。

-   請確定網域中的每個網域控制站可執行名稱解析，並與受信任網域的網域控制站進行通訊。

-   請確定位置考量會納入為信任的帳戶。

-   啟用 Kerberos，可能的話，並使用安全通道來降低風險的 MaxConcurrentAPI 瓶頸時遇到的最小化。

跨網域信任的案例包括已一致許多客戶的痛苦點的區域。 名稱解析與連線問題，通常都是因為防火牆，導致資源耗盡，信任的網域控制站上，並會影響所有的用戶端。 此外，經常被忽略的案例最佳化的受信任的網域控制站的存取。 以確保其正常運作的主要區域如下所示：

-   請確定使用信任的網域控制站的 DNS 和 WINS 名稱解析可以解析正確的受信任網域的網域控制站清單。

    -   以靜態方式新增的記錄都有容易變成過時，而且經過一段時間研討連線問題。 DNS 轉送，動態 DNS，並合併 WINS/DNS 基礎結構是長期來說更容易維護。

    -   請確定正確設定轉寄站，條件式轉送，並在用戶端可能需要存取的環境中的每個資源的兩個正向和反向對應區域的次要複本。 同樣地，這需要手動維護，大多傾向於變成過時。 彙總基礎結構的是最理想的。

-   信任網域中的網域控制站會嘗試尋找網域控制站受信任的網域中，位於相同站台第一次，然後容錯回復到一般的定位器。

    -   如需 DCLocator 的運作方式的詳細資訊，請參閱[尋找最接近的站台中的網域控制站](https://technet.microsoft.com/library/cc978016.aspx)。

    -   收斂的受信任且信任的網域，以反映相同的位置中的網域控制站之間的站台名稱。 請確定子網路和 IP 位址對應正確地連結至這兩個樹系中的站台。 如需詳細資訊，請參閱 <<c0> [ 網域定位器跨樹系信任](http://blogs.technet.com/b/askds/archive/2008/09/24/domain-locator-across-a-forest-trust.aspx)。

    -   根據 DCLocator 的需求，網域控制站的位置，請確定已開啟，連接埠。 如果網域之間，有防火牆，請確定防火牆已正確設定所有信任的。 無法開啟防火牆時，信任的網域控制站將仍嘗試存取受信任的網域。 如果通訊失敗，因為任何原因，信任的網域控制站會最後逾受信任的網域控制站的要求。 不過，這些逾時可能需要幾秒鐘，每個要求，而且可以耗盡信任的網域控制站上的網路連接埠，如果連入要求數量很高。 用戶端可能會遇到逾時，網域控制站，等待視為無回應的執行緒，無法轉譯為無回應的應用程式中，（如果應用程式在前景執行緒中執行要求）。 如需詳細資訊，請參閱 <<c0> [ 如何設定防火牆供網域及信任](https://support.microsoft.com/kb/179442)。

    -   您可以使用 DnsAvoidRegisterRecords 來排除效能不佳執行或高延遲的網域控制站，例如衛星站台，從一般的定位器的廣告中。 如需詳細資訊，請參閱 <<c0> [ 如何來最佳化網域控制站或通用類別目錄存在於用戶端的站台外的位置](https://support.microsoft.com/kb/306602)。

        > [!Note]   沒有實際限制為大約 50%到用戶端可以取用的網域控制站數目。 這些應該是最大站台最佳且最高的容量的網域控制站。

         

    -   請考慮將網域控制站放在相同的實體位置中的受信任且信任網域。

所有信任的情況下，認證會路由都傳送根據驗證要求中指定的網域。 這也適用於查詢的 LookupAccountName 和 LsaLookupNames （以及其他項目，這些都只是最常使用的） Api。 當這些 Api 的網域參數傳遞 NULL 值時，網域控制站會嘗試尋找每個受信任的網域中指定的帳戶名稱。

-   停用時已指定 NULL 的網域，請檢查所有可用的信任。 [如何使用 LsaLookupRestrictIsolatedNameLevel 登錄項目來限制在外部信任的網域隔離名稱查閱](https://support.microsoft.com/kb/818024)

-   停用傳遞驗證要求與 NULL 指定跨所有可用的信任的網域。 [Lsass.exe 程序可能會停止回應，如果您的 Active Directory 網域控制站上有許多外部信任](https://support.microsoft.com/kb/923241/EN-US)

## <a name="see-also"></a>另請參閱
- [效能微調 Active Directory 伺服器](index.md)
- [硬體考量](hardware-considerations.md)
- [LDAP 考量](ldap-considerations.md)
- [針對 ADDS 效能問題進行疑難排解](troubleshoot.md) 
- [Active Directory Domain Services 的容量規劃](https://go.microsoft.com/fwlink/?LinkId=324566)