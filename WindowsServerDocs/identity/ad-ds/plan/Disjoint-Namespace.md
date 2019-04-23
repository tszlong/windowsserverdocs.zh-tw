---
ms.assetid: d92731f1-e4d8-4223-9b07-ca1f40bb0e1f
title: 斷續命名空間
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0a2e911e889343b05a515c94e615d3648289f2df
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883569"
---
# <a name="disjoint-namespace"></a>斷續命名空間

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

當一或多個網域成員電腦擁有主要網域名稱服務 (DNS) 尾碼與 Active Directory 網域的成員電腦的 DNS 名稱不相符時，就會發生脫離的命名空間。 例如，使用名為 na.corp.fabrikam.com 的 Active Directory 網域中的主要 DNS 尾碼 corp.fabrikam.com 的成員電腦使用脫離的命名空間。  
  
脫離的命名空間是更複雜的管理、 維護和疑難排解比連續的命名空間。 在連續的命名空間中，主要 DNS 尾碼會符合 Active Directory 網域名稱。 在脫離的命名空間中，撰寫以假設的 Active Directory 命名空間是所有網域成員電腦的主要 DNS 尾碼相同的網路應用程式無法正確運作。  
  
## <a name="support-for-disjoint-namespaces"></a>脫離的命名空間的支援  
網域成員電腦，包括網域控制站，可以在脫離的命名空間中運作。 網域成員電腦可以註冊其主機 (A) 資源記錄和 IP 第 6 版 (IPv6) 主機 (AAAA) 資源記錄，脫離 DNS 命名空間中的。 網域成員電腦註冊其資源記錄，如此一來，當網域控制站會繼續向等同於 Active Directory 網域名稱的 DNS 區域中的全域和站台特定的服務 (SRV) 資源記錄。  
  
例如，假設名為使用主要 DNS 尾碼 corp.fabrikam.com 的 na.corp.fabrikam.com 的 Active Directory 網域的網域控制站註冊主機 (A) 與 IPv6 主機 (AAAA) 資源記錄 corp.fabrikam.com DNS 區域。 網域控制站會繼續全域和站台特定的服務 (SRV) 資源記錄登錄在 _msdcs.na.corp.fabrikam.com na.corp.fabrikam.com DNS 區域，這可讓產生服務的位置。  
  
> [!IMPORTANT]  
> 雖然 Windows 作業系統可以支援脫離的命名空間，寫入假設主要 DNS 尾碼與 Active Directory 網域尾碼相同的應用程式可能無法運作這類環境。 基於這個理由，您應該測試所有的應用程式和其各自的作業系統仔細再部署脫離的命名空間。  
  
脫離的命名空間應該會運作 （而且受到） 在下列情況：  
  
-   當具有多個 Active Directory 網域的樹系使用單一的 DNS 命名空間，也就是所謂的 DNS 區域  
  
    其中一個範例就是公司的名稱，例如 na.corp.fabrikam.com、 sa.corp.fabrikam.com 和 asia.corp.fabrikam.com 使用地區網域，並使用單一的 DNS 命名空間，例如 corp.fabrikam.com。  
  
-   當單一 Active Directory 網域會分割成個別的 DNS 命名空間  
  
    這個範例是使用 DNS 區域，例如 hr.corp.contoso.com、 production.corp.contoso.com 和 it.corp.contoso.com corp.contoso.com 的 Active Directory 網域的公司。  
  
脫離的命名空間無法正常運作 （和不支援） 在下列情況：  
  
-   脫離的後置詞使用的網域成員符合這個或另一個樹系的 Active Directory 網域名稱。 這會中斷 Kerberos 名稱尾碼路由。  
  
-   在另一個樹系中，會使用相同的脫離尾碼。 這可防止樹系之間的唯一路由傳送這些尾碼。  
  
-   當網域成員憑證授權單位 (CA) 伺服器變更其完整的網域名稱 (FQDN)，使它無法再使用相同的主要 DNS 尾碼，可由在 CA 伺服器是成員的網域的網域控制站。 在此情況下，您可能無法驗證憑證發出，取決於哪些 DNS 名稱用於 CRL 發佈點在 CA 伺服器。 但是，如果您將 CA 伺服器放在穩定脫離的命名空間時，它可正常運作，並支援。  
  
## <a name="considerations-for-disjoint-namespaces"></a>脫離的命名空間考量  
下列考量可協助您決定是否應該使用脫離的命名空間。  
  
### <a name="application-compatibility"></a>應用程式相容性  
如先前所述，脫離的命名空間可能會造成問題的任何應用程式 」 和 「 寫入假設電腦主要 DNS 尾碼與它為成員的網域名稱的名稱相同的服務。 在部署脫離的命名空間之前，您必須檢查應用程式的相容性問題。 此外，請務必檢查當您執行您的分析，您使用的所有應用程式相容性。 這包括應用程式從 Microsoft 和其他軟體開發人員。  
  
### <a name="advantages-of-disjoint-namespaces"></a>脫離的命名空間的優點  
使用脫離的命名空間可以有下列優點：  
  
-   因為主要 DNS 尾碼的電腦可能表示不同的資訊，您可以從 Active Directory 網域名稱分開管理的 DNS 命名空間。  
  
-   您可以將根據商務結構或地理位置的 DNS 命名空間。 例如，您可以分開根據業務單位名稱或實體位置，例如大陸、 國家/地區或建立的命名空間。  
  
### <a name="disadvantages-of-disjoint-namespaces"></a>脫離的命名空間的缺點  
使用脫離的命名空間可以有下列缺點：  
  
-   您必須建立和管理個別的 DNS 區域，可使用脫離的命名空間的成員電腦樹系中每個 Active Directory 網域。 （也就是它需要額外和更複雜的組態）。  
  
-   您必須執行手動步驟，以修改及管理 Active Directory 屬性，可讓以使用指定的主要 DNS 尾碼的網域成員。  
  
-   若要最佳化的名稱解析，您必須執行手動步驟，以修改及維護群組原則來設定成員電腦使用替代的主要 DNS 尾碼。  
  
    > [!NOTE]  
    > Windows 網際網路名稱服務 (WINS) 可用來位移這個缺點解析單一標籤名稱。 如需 WINS 的詳細資訊，請參閱 WINS 的技術參考 ([https://go.microsoft.com/fwlink/?LinkId=102303](https://go.microsoft.com/fwlink/?LinkId=102303))。  
  
-   當您的環境需要多個主要的 DNS 尾碼時，您必須在樹系中正確設定所有的 Active Directory 網域的 DNS 尾碼搜尋順序。  
  
    若要設定的 DNS 尾碼搜尋順序，您可以使用群組原則物件 」 或 「 動態主機設定通訊協定 (DHCP) 伺服器服務參數。 您也可以修改登錄。  
  
-   您必須仔細測試所有的應用程式的相容性問題。  
  
如需有關您可以採取的步驟來解決這些缺點，請參閱 < 建立脫離的命名空間 ([https://go.microsoft.com/fwlink/?LinkId=106638](https://go.microsoft.com/fwlink/?LinkId=106638))。  
  
### <a name="planning-a-namespace-transition"></a>規劃命名空間轉換  
修改命名空間之前，請檢閱下列考量適用於從連續的命名空間，以脫離的命名空間 （或相反） 的轉換：  
  
-   以手動方式設定服務主體名稱 (Spn) 可能不再符合 DNS 名稱的命名空間變更後。 這會導致驗證失敗。  
  
    如需詳細資訊，請參閱服務的登入失敗，因為未正確設定的 Spn ([https://go.microsoft.com/fwlink/?LinkId=102304](https://go.microsoft.com/fwlink/?LinkId=102304))。  
  
    -   如果您使用 Windows Server 2003 為基礎的電腦使用限制委派時，這些電腦可能需要額外的設定，來變更 Spn。 如需詳細資訊，請參閱知識庫文件 936628 Microsoft ([https://go.microsoft.com/fwlink/?LinkId=102306](https://go.microsoft.com/fwlink/?LinkId=102306))。  
  
    -   如果您想要委派權限可以修改系統管理員的次級的 Spn，請參閱 < 修改 spn 委派的權限 ([https://go.microsoft.com/fwlink/?LinkId=106639](https://go.microsoft.com/fwlink/?LinkId=106639))。  
  
-   如果您使用輕量型目錄存取通訊協定 (LDAP) over Secure Sockets Layer (SSL) （又稱為 LDAPS） 中具有脫離的命名空間中所設定的網域控制站的部署的 CA，您必須使用適當的 Active Directory 網域名稱和主要 DNS 尾碼設定 LDAPS 憑證時。  
  
    如需有關網域控制站憑證需求的詳細資訊，請參閱 321051 在 Microsoft 知識庫文件 ([https://go.microsoft.com/fwlink/?LinkId=102307](https://go.microsoft.com/fwlink/?LinkId=102307))。  
  
    > [!NOTE]  
    > 使用 LDAPS 憑證的網域控制站可能需要您重新部署其憑證。 當您這樣做時，網域控制站可能未選取適當的憑證之前重新啟動。 多個 Windows Server 2003 LDAPS 驗證和相關的更新的詳細資訊，請參閱知識庫文件 932834 Microsoft ([https://go.microsoft.com/fwlink/?LinkId=102308](https://go.microsoft.com/fwlink/?LinkId=102308))。  
  
### <a name="planning-for-disjoint-namespace-deployments"></a>脫離的命名空間的部署規劃  
如果您將部署在具有脫離的命名空間的環境中的電腦，請採取下列預防措施：  
  
1.  通知與業務執行，它們必須測試，而支援脫離的命名空間的所有軟體廠商。 要求他們確認其支援使用脫離的命名空間的環境中的應用程式。  
  
2.  在脫離的命名空間的實驗室環境中測試所有版本的作業系統和應用程式。 當您這樣做時，請遵循下列建議：  
  
    1.  在環境中部署軟體之前，請解決所有的軟體問題。  
  
    2.  可能的話，請加入 beta 版測試的作業系統和您打算在脫離的命名空間中部署的應用程式。  
  
3.  請確定系統管理員和技術服務人員會留意脫離的命名空間和其影響。  
  
4.  建立方案，可讓您轉換從脫離的命名空間至連續的命名空間，如有必要。  
  
5.  宣揚脫離的命名空間支援與作業系統和應用程式提供者的重要性。  
  


