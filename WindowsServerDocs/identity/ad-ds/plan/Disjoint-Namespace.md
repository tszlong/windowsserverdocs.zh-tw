---
ms.assetid: d92731f1-e4d8-4223-9b07-ca1f40bb0e1f
title: "分開命名空間"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ac231dbfbaaeafa39199e29a1744d84d5cec23e8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="disjoint-namespace"></a>分開命名空間

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

當一或多個網域成員電腦不符合的電腦的成員 Active Directory domain DNS 名稱主要網域名稱服務」(DNS) 尾碼時，就會發生分開命名空間。 例如，使用名 na.corp.fabrikam.com Active Directory domain corp.fabrikam.com 主要 DNS 尾碼成員電腦使用分開命名空間。  
  
分開命名空間是來管理、維護，以及疑難排解連續命名空間比更複雜。 主要 DNS 尾碼連續命名空間，符合 Active Directory 網域名稱。 網路應用程式所撰寫假設 Active Directory 命名空間是相同的所有成員網域的電腦的主要 DNS 尾碼分開命名空間中無法正常運作。  
  
## <a name="support-for-disjoint-namespaces"></a>支援分開命名空間  
網域成員電腦，包括網域控制站，可以分開命名空間中運作。 網域成員電腦可以登記其主機 (A) 的資源記錄和 IP 版本 (IPv6) 6 主機 (AAAA) 資源記錄分開的 DNS 名稱區中。 網域成員電腦登記資源，如此一來，當網域控制站繼續登記 Active Directory domain 名稱相同 DNS 區域中的全域和特定網站服務 (SRV) 資源記錄。  
  
例如，假設的網域控制站的名使用 corp.fabrikam.com 主要 DNS 尾碼 na.corp.fabrikam.com Active Directory domain 暫存器主機 (A) 和 IPv6 (AAAA) 主機的資源記錄 corp.fabrikam.com DNS 區域中。 網域控制站持續登記全域和特定網站服務 (SRV) 資源記錄 _msdcs。na.corp.fabrikam.com 和 na.corp.fabrikam.com DNS 區域中，這可以讓位置服務。  
  
> [!IMPORTANT]  
> Windows 作業系統可能支援分開命名空間，雖然寫入假設主要 DNS 尾碼 Active Directory domain 尾碼相同的應用程式可能無法運作這樣的環境中。 基於這個原因，您應該所有應用程式和他們各自的作業系統仔細部署之前測試分開命名空間。  
  
分開命名空間應該可以正常運作（和支援）下列情形：  
  
-   當使用多個 Active Directory 網域樹系使用單一 DNS 命名空間，也就是也稱為 DNS 區域  
  
    一個範例是使用區域網域名稱，例如 na.corp.fabrikam.com、sa.corp.fabrikam.com，以及 asia.corp.fabrikam.com 及使用單一 DNS 命名空間，例如 corp.fabrikam.com 的公司。  
  
-   在單一的 Active Directory domain 分成不同的 DNS 命名空間  
  
    一個範例是的使用 DNS 區域 hr.corp.contoso.com、production.corp.contoso.com，和 it.corp.contoso.com corp.contoso.com 的 Active Directory domain 的公司。  
  
分開命名空間無法正常運作（和不支援）下列情形：  
  
-   使用網域成員分開尾碼符合這個或其他樹系的 Active Directory 網域名稱。 這會中斷 Kerberos 名稱尾碼路由。  
  
-   另一個森林中使用的相同分開尾碼。 如此可防止之間的樹系唯一路由這些尾碼。  
  
-   當網域成員憑證授權時單位伺服器變更其會完全完整網域名稱 (FQDN)，，讓它不再使用的相同的主要 DNS 尾碼所使用的網域控制站的所屬的網域的 CA 伺服器。 若是如此，您可能需要驗證憑證的問題 CA 伺服器發行，而定 CRL Distribution 點中使用何種 DNS 名稱。 但是，如果您 CA 伺服器置於穩定分開命名空間，正確運作，以及支援。  
  
## <a name="considerations-for-disjoint-namespaces"></a>分開命名空間事項  
下列考量，可協助您選擇是否您應該使用分開命名空間。  
  
### <a name="application-compatibility"></a>應用程式的相容性  
如之前所述，分開命名空間可能會造成的任何應用程式與服務寫入假設電腦主要 DNS 尾碼是相同的名稱都是成員網域名稱的問題。 部署分開命名空間之前，您必須檢查應用程式的相容性問題。 此外，請務必查看您執行分析當您使用的所有應用程式的相容性。 這包括應用程式與 Microsoft 和其他軟體開發人員。  
  
### <a name="advantages-of-disjoint-namespaces"></a>分開命名空間的優點  
使用分開命名空間可以有下列優點：  
  
-   因為主要 DNS 尾碼的電腦，可以表示不同的資訊，您可以從 Active Directory 網域名稱分開管理 DNS 命名空間。  
  
-   您可以將 DNS 命名空間商務結構或地理位置為基礎。 例如，您可以分開根據商務單位名稱或所在位置，例如出這片大陸、國家/地區或建置命名空間。  
  
### <a name="disadvantages-of-disjoint-namespaces"></a>分開命名空間的缺點  
使用分開命名空間可能會有下列缺點：  
  
-   您必須建立及管理針對每個成員電腦使用分開命名空間的樹系的 Active Directory 網域不同 DNS 區域。 （也就是，需要其他及複雜的設定。）  
  
-   您必須執行手動修改和管理允許使用指定、主要 DNS 尾碼網域成員 Active Directory 屬性。  
  
-   若要最佳化的名稱解析，您必須執行來修改及維護群組原則設定成員電腦與其他主要 DNS 尾碼手動。  
  
    > [!NOTE]  
    > 此缺點位移解析單一標籤名稱，可 Windows 網際網路名稱服務」(WINS)。 如需 WINS，查看 WINS 技術參考 ([https://go.microsoft.com/fwlink/?LinkId=102303](https://go.microsoft.com/fwlink/?LinkId=102303))。  
  
-   當您的環境需要多個主要 DNS 尾碼時，您必須設定 DNS 尾碼搜尋的所有 Active Directory 網域順序森林中正確回應。  
  
    若要設定 DNS 尾碼搜尋順序，您可以使用群組原則物件」或「動態主機設定通訊協定」(DHCP) 伺服器服務參數。 您也可以修改登錄。  
  
-   您必須仔細測試所有應用程式的相容性問題。  
  
如需詳細資訊，有關步驟，您可能需要地這些缺點，查看建立分開命名空間 ([https://go.microsoft.com/fwlink/?LinkId=106638](https://go.microsoft.com/fwlink/?LinkId=106638))。  
  
### <a name="planning-a-namespace-transition"></a>規劃命名空間轉換  
您修改命名空間之前，先檢視的下列考量，可套用到轉換連續命名空間斷續命名空間（反之亦然）中：  
  
-   手動設定服務主體名稱 (Spn) 可能無法再符合 DNS 名稱命名空間變更之後。 這可能會造成驗證失敗。  
  
    如需詳細資訊，請服務登入失敗，因為不正確設定 Spn ([https://go.microsoft.com/fwlink/?LinkId=102304](https://go.microsoft.com/fwlink/?LinkId=102304))。  
  
    -   如果您使用 Windows Server 2003 電腦限制委派，這些電腦需要額外的設定變更 Spn。 如需詳細資訊，請查看 936628 中 Microsoft 知識庫 ([https://go.microsoft.com/fwlink/?LinkId=102306](https://go.microsoft.com/fwlink/?LinkId=102306))。  
  
    -   如果您想要委派修改 Spn 到附屬系統管理員權限，查看委派授權修改 Spn ([https://go.microsoft.com/fwlink/?LinkId=106639](https://go.microsoft.com/fwlink/?LinkId=106639))。  
  
-   如果您透過安全通訊端層 (SSL)（稱為 LDAPS）中部署具有分開命名空間中所設定的網域控制站 ca 使用輕量型 Directory 存取通訊協定 (LDAP)，您必須使用主要 DNS 尾碼與適當 Active Directory domain 名稱，當您設定的 LDAPS 憑證。  
  
    如需網域控制站憑證需求，查看 321051 中 Microsoft 知識庫 ([https://go.microsoft.com/fwlink/?LinkId=102307](https://go.microsoft.com/fwlink/?LinkId=102307))。  
  
    > [!NOTE]  
    > 使用 LDAPS 憑證的網域控制站可能需要重新憑證部署。 當您這樣做時，網域控制站可能選取適當的憑證直到時都重新啟動。 如需 Windows Server 2003 LDAPS 驗證和相關的更新的詳細資訊，請查看 932834 中 Microsoft 知識庫 ([https://go.microsoft.com/fwlink/?LinkId=102308](https://go.microsoft.com/fwlink/?LinkId=102308))。  
  
### <a name="planning-for-disjoint-namespace-deployments"></a>規劃分開命名空間部署  
如果您要部署的分開命名空間環境中的電腦，請執行下列預防措施：  
  
1.  通知與您進行商務他們必須測試，並支援分開命名空間的所有軟體廠商。 請他們檢查它們的環境中使用分開命名空間支援自己的應用程式。  
  
2.  測試分開命名空間 lab 環境作業系統和應用程式的所有版本。 當您這樣做時，請遵循這些建議：  
  
    1.  您到您的環境中部署軟體之前，請解析所有的軟體問題。  
  
    2.  可能的話，參與作業系統與要部署分開命名空間中的應用程式測試。  
  
3.  確保系統管理員及服務支援人員知道分開命名空間，以及它的影響。  
  
4.  建立可讓您轉換到命名空間，連續分開命名空間必要的計劃。  
  
5.  宣傳分開命名空間支援的作業系統和應用程式提供者的重要性。  
  


