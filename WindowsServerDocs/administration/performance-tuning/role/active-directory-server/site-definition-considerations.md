---
title: 中的網站定義和網域控制站放置新增效能微調
description: Active Directory 效能微調中的網站定義和網域控制站放置考慮。
ms.topic: article
ms.author: timwi
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: fa051bd6637eff9f5a25cd8784d33a60095ccd54
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90077195"
---
# <a name="proper-placement-of-domain-controllers-and-site-considerations"></a>適當地放置網域控制站與站台考量

適當的網站定義對效能而言是不可或缺的。 從網站傳出的用戶端可能會遇到驗證和查詢的效能不佳。 此外，在用戶端上引進 IPv6 時，要求可以來自 IPv4 或 IPv6 位址，且 Active Directory 必須已正確定義 IPv6 的網站。 作業系統會偏好將 IPv6 設定為 IPv4。

從 Windows Server 2008 開始，網域控制站會嘗試使用名稱解析來進行反向查閱，以便判斷用戶端應該位於哪個網站。 這可能會導致 ATQ 執行緒集區耗盡，而導致網域控制站無法回應。 適當的解決方式是正確地定義 IPv6 的網站拓撲。 因應措施是，可以將名稱解析基礎結構優化，以快速回應網域控制站要求。 如需詳細資訊，請參閱 [Windows server 2008 或 Windows server 2008 R2 網域控制站延遲回應 LDAP 或 Kerberos 要求](https://support.microsoft.com/kb/2668820)。

另一個考慮區域是在使用 Rodc 的案例中尋找讀取/寫入 Dc。  某些作業需要存取可寫入的網域控制站，或在唯讀網域控制站足夠時，將目標設為可寫入的網域控制站。  將這些案例優化將需要兩個路徑：
-   當唯讀網域控制站足夠時，與可寫入的網域控制站聯繫。  這需要變更應用程式程式碼。
-   可能需要可寫入的網域控制站。  將讀寫網域控制站放在中央位置，以將延遲降至最低。

如需詳細資訊，請參閱：
-   [應用程式和 RODC 的相容性](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc772597(v=ws.10))
-   [Active Directory 服務介面 (ADSI) 和唯讀網域控制站 (RODC) –避免效能問題](/archive/blogs/fieldcoding/active-directory-service-interface-adsi-and-the-read-only-domain-controller-rodc-avoiding-performance-issues)

## <a name="optimize-for-referrals"></a>針對推薦優化

當網域控制站未裝載所查詢分割區的複本時，系統會使用參考來重新導向 LDAP 查詢。 傳回參考時，它會包含磁碟分割的分辨名稱、DNS 名稱和埠號碼。 用戶端會使用此資訊，在裝載分割區的伺服器上繼續查詢。 這是 DCLocator 案例，而且會維護所有建議的網站定義和網域控制站放置，但通常會忽略相依于參考的應用程式。 建議您確保 AD 拓撲（包括網站定義和網域控制站放置）適當地反映用戶端的需求。 此外，這可能包括在單一網站中擁有來自多個網域的網域控制站、調整 DNS 設定，或重新放置應用程式的網站。

## <a name="optimization-considerations-for-trusts"></a>信任的優化考慮

在樹系內部案例中，會根據下列網域階層來處理信任：父子網域- &gt; 子域- &gt; 樹系根域-子域-子域-子定義域 &gt; &gt; 。 這表示樹系根目錄和每個父系的安全通道可能會因為驗證要求的匯總傳輸信任階層中的 Dc 而多載。 當驗證也必須傳輸高度潛在的連結來影響上述流程時，這也可能會導致大型地理散佈的 Active Directory 發生延遲。 在樹系和下層信任案例之間可能會發生多載。 下列建議適用于所有案例：

-   適當地調整 MaxConcurrentAPI，以支援跨安全通道的負載。 如需詳細資訊，請參閱 [如何使用 MaxConcurrentApi 設定進行 NTLM 驗證的效能微調](https://support.microsoft.com/kb/2688798/EN-US)。

-   根據負載建立適當的快捷方式信任。

-   確定網域中的每個網域控制站都能夠執行名稱解析，並與受信任網域中的網域控制站通訊。

-   確定已將位置考慮納入信任。

-   盡可能啟用 Kerberos，並盡可能減少使用安全通道的風險，以降低遇到 MaxConcurrentAPI 瓶頸的風險。

跨網域信任案例是對許多客戶一直都有困難點的領域。 名稱解析和連線問題（通常是因為防火牆）會導致信任網域控制站上的資源耗盡，並影響所有用戶端。 此外，經常被忽略的案例是將受信任網域控制站的存取優化。 確定這項作業可正常運作的關鍵區域如下所示：

-   確認信任網域控制站所使用的 DNS 和 WINS 名稱解析，可以解析受信任網域的正確網域控制站清單。

    -   靜態加入的記錄往往會在一段時間內成為過時的問題，並重新產生連線問題。 DNS 轉送、動態 DNS 以及合併 WINS/DNS 基礎結構在長時間執行時更容易維護。

    -   針對用戶端可能需要存取的環境中的每個資源，請務必適當地設定轉寄和反向對應區域的轉寄站、條件轉寄和次要複本。 同樣地，這需要手動維護，而且往往會過時。 基礎結構的匯總很理想。

-   信任網域中的網域控制站會先嘗試在受信任網域中尋找位於相同網站的網域控制站，然後再容錯回復至一般定位器。

    -   如需 DCLocator 運作方式的詳細資訊，請參閱在 [最接近的網站中尋找網域控制站](/previous-versions/windows/it-pro/windows-2000-server/cc978016(v=technet.10))。

    -   將受信任和信任網域之間的網站名稱收斂，以反映相同位置中的網域控制站。 確定子網和 IP 位址對應已正確連結到兩個樹系中的網站。 如需詳細資訊，請參閱 [跨樹系信任的網域定位器](/archive/blogs/askds/domain-locator-across-a-forest-trust)。

    -   根據 DCLocator 需求，確定已針對網域控制站位置開啟埠。 如果網域之間有防火牆存在，請確定已針對所有信任正確設定防火牆。 如果未開啟防火牆，信任網域控制站仍會嘗試存取受信任的網域。 如果通訊因任何原因而失敗，則信任的網域控制站最終會將要求的時間提供給受信任的網域控制站。 不過，這些超時時間可能需要數秒鐘的時間，而且如果連入要求的數量很高，可能會耗盡信任網域控制站上的網路埠。 用戶端可能會在網域控制站上遇到等待等候超時的執行緒，如果應用程式在前景執行緒) 中執行要求，則可能會轉譯為無回應的應用程式 (。 如需詳細資訊，請參閱 [如何設定網域和信任的防火牆](https://support.microsoft.com/kb/179442)。

    -   使用 DnsAvoidRegisterRecords 來消除效能不佳或高延遲的網域控制站（例如，附屬網站中的網域控制站），從廣告到一般定位器。 如需詳細資訊，請參閱 [如何優化位於用戶端網站外部的網域控制站或通用類別目錄的位置](https://support.microsoft.com/kb/306602)。

        > [!NOTE]
        > 用戶端可以取用的網域控制站數目有大約50的實際限制。 這些應該是最適合網站的最高容量網域控制站。


    -  考慮將來自信任和信任網域的網域控制站放在相同的實體位置。

針對所有信任案例，會根據驗證要求中所指定的網域來路由認證。 這也適用于對 LookupAccountName 和 LsaLookupNames (以及其他專案的查詢，這些只是最常使用的) Api。 當這些 Api 的網域參數傳遞 Null 值時，網域控制站會嘗試找出每個可用的受信任網域中指定的帳號名稱。

-   當指定 Null 網域時，停用檢查所有可用的信任。 [如何使用 LsaLookupRestrictIsolatedNameLevel 登錄專案限制在外部受信任網域中的隔離名稱查閱](https://support.microsoft.com/kb/818024)

-   停用傳遞驗證要求，並在所有可用的信任之間指定 Null 網域。 [如果您在 Active Directory 網域控制站上有許多外部信任，Lsass.exe 進程可能會停止回應](https://support.microsoft.com/kb/923241/EN-US)

## <a name="additional-references"></a>其他參考資料
- [Active Directory 伺服器的效能調整](index.md)
- [硬體考量](hardware-considerations.md)
- [LDAP 考量](ldap-considerations.md)
- [針對 ADDS 效能問題進行疑難排解](troubleshoot.md)
- [Active Directory Domain Services 的容量規劃](https://go.microsoft.com/fwlink/?LinkId=324566)