---
ms.assetid: d92731f1-e4d8-4223-9b07-ca1f40bb0e1f
title: 斷續命名空間
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 5abe67c89ce4c2f4b5056f6197242b5db8db340e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408854"
---
# <a name="disjoint-namespace"></a>斷續命名空間

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

當一或多個網域成員電腦的主功能變數名稱稱服務（DNS）尾碼不符合電腦所屬之 Active Directory 網域的 DNS 名稱時，就會發生脫離的命名空間。 例如，在名為 na.corp.fabrikam.com 的 Active Directory 網域中使用主要 DNS 尾碼 corp.fabrikam.com 的成員電腦，會使用脫離的命名空間。  
  
相較于連續的命名空間，脫離的命名空間比管理、維護和疑難排解更為複雜。 在連續的命名空間中，主要 DNS 尾碼會符合 Active Directory 功能變數名稱。 寫入的網路應用程式假設 Active Directory 命名空間與所有網域成員電腦的主要 DNS 尾碼相同，無法在脫離的命名空間中正常運作。  
  
## <a name="support-for-disjoint-namespaces"></a>支援脫離的命名空間  
網域成員電腦（包括網域控制站）可以在脫離的命名空間中運作。 網域成員電腦可以在脫離的 DNS 命名空間中註冊其主機（A）資源記錄和 IP 第6版（IPv6）主機（AAAA）資源記錄。 當網域成員電腦以這種方式註冊其資源記錄時，網域控制站會繼續在 DNS 區域中註冊與 Active Directory 功能變數名稱相同的全域和網站特定服務（SRV）資源記錄。  
  
例如，假設名為 na.corp.fabrikam.com 且使用主要 DNS 尾碼 corp.fabrikam.com 的域 Active Directory 控制器，會在 corp.fabrikam.com DNS 區域中註冊主機（A）和 IPv6 主機（AAAA）資源記錄。 網域控制站會繼續在 _msdcs 和 na.corp.fabrikam.com DNS 區域中註冊全域和網站特定服務（SRV）資源記錄，讓服務位置得以實現。  
  
> [!IMPORTANT]  
> 雖然 Windows 作業系統可能支援脫離的命名空間，但撰寫的應用程式假設主要 DNS 尾碼與 Active Directory 網域尾碼在這類環境中可能無法運作。 基於這個理由，您應該在部署脫離的命名空間之前，仔細測試所有應用程式及其各自的作業系統。  
  
在下列情況下，脫離的命名空間應可正常執行（並受到支援）：  
  
-   當具有多個 Active Directory 網域的樹系使用單一 DNS 命名空間（也稱為 DNS 區域）時  
  
    例如，使用 na.corp.fabrikam.com、sa.corp.fabrikam.com 和 asia.corp.fabrikam.com 等名稱的地區網域，並使用單一 DNS 命名空間（例如 corp.fabrikam.com）的公司。  
  
-   將單一 Active Directory 網域分割為個別的 DNS 命名空間時  
  
    其中一個範例是使用 DNS 區域（例如 hr.corp.contoso.com、production.corp.contoso.com 和 it.corp.contoso.com）的 Active Directory 網域 corp.contoso.com 的公司。  
  
在下列情況下，脫離的命名空間無法正常運作（而且不受支援）：  
  
-   網域成員所使用的不相鄰尾碼符合此或另一個樹系中的 Active Directory 功能變數名稱。 這會中斷 Kerberos 名稱尾碼路由。  
  
-   同一個不連續的尾碼會用於另一個樹系中。 這可避免在樹系之間唯一路由這些尾碼。  
  
-   網域成員憑證授權單位單位（CA）伺服器變更其完整功能變數名稱（FQDN），使其不再使用 CA 伺服器所屬網域的網域控制站所使用的相同主要 DNS 尾碼。 在此情況下，您可能會在驗證 CA 伺服器所發行的憑證時遇到問題，視 CRL 發佈點中使用的 DNS 名稱而定。 但是，如果您將 CA 伺服器放在穩定的脫離命名空間中，它會正常運作並受到支援。  
  
## <a name="considerations-for-disjoint-namespaces"></a>脫離的命名空間考慮  
下列考慮可協助您決定是否應該使用脫離的命名空間。  
  
### <a name="application-compatibility"></a>應用程式相容性  
如先前所述，脫離的命名空間可能會對寫入的任何應用程式和服務造成問題，假設電腦的主要 DNS 尾碼與它所屬的功能變數名稱名稱相同。 在您部署脫離的命名空間之前，您必須檢查應用程式是否有相容性問題。 此外，請務必檢查您在執行分析時所使用之所有應用程式的相容性。 這包括來自 Microsoft 和其他軟體發展人員的應用程式。  
  
### <a name="advantages-of-disjoint-namespaces"></a>脫離的命名空間的優點  
使用脫離的命名空間可能會有下列優點：  
  
-   由於電腦的主要 DNS 尾碼可能會指出不同的資訊，因此您可以將 DNS 命名空間與 Active Directory 功能變數名稱分開管理。  
  
-   您可以根據商業結構或地理位置來分隔 DNS 命名空間。 例如，您可以根據業務單位名稱或實體位置（例如大陸、國家/地區或建築物）來分隔命名空間。  
  
### <a name="disadvantages-of-disjoint-namespaces"></a>脫離命名空間的缺點  
使用脫離的命名空間可能會有下列缺點：  
  
-   您必須為樹系中的每個 Active Directory 網域，建立和管理不同的 DNS 區域，而該網域中的成員電腦會使用脫離的命名空間。 （也就是說，它需要額外且更複雜的設定）。  
  
-   您必須執行手動步驟來修改和管理允許網域成員使用指定的主要 DNS 尾碼的 Active Directory 屬性。  
  
-   若要優化名稱解析，您必須執行手動步驟來修改和維護群組原則，以設定具有替代主要 DNS 尾碼的成員電腦。  
  
    > [!NOTE]  
    > 藉由解析單一標籤名稱，可以使用 Windows 網際網路名稱服務（WINS）來抵銷此缺點。 如需 WINS 的詳細資訊，請參閱 WINS 技術參考（[https://go.microsoft.com/fwlink/?LinkId=102303](https://go.microsoft.com/fwlink/?LinkId=102303)）。  
  
-   當您的環境需要多個主要 DNS 尾碼時，您必須適當地設定樹系中所有 Active Directory 網域的 DNS 尾碼搜尋順序。  
  
    若要設定 DNS 尾碼搜尋順序，您可以使用群組原則物件或動態主機設定通訊協定（DHCP）伺服器服務參數。 您也可以修改登錄。  
  
-   您必須仔細測試所有應用程式的相容性問題。  
  
如需您可以採取的步驟來解決這些缺點的詳細資訊，請參閱建立脫離的命名空間（[https://go.microsoft.com/fwlink/?LinkId=106638](https://go.microsoft.com/fwlink/?LinkId=106638)）。  
  
### <a name="planning-a-namespace-transition"></a>規劃命名空間轉換  
在修改命名空間之前，請先參閱下列考慮，這適用于從連續命名空間轉換為脫離的命名空間（或反向）：  
  
-   在命名空間變更之後，手動設定的服務主體名稱（Spn）可能不再符合 DNS 名稱。 這可能會導致驗證失敗。  
  
    如需詳細資訊，請參閱服務登入因設定不正確的 Spn 而失敗（[https://go.microsoft.com/fwlink/?LinkId=102304](https://go.microsoft.com/fwlink/?LinkId=102304)）。  
  
    -   如果您使用具有限制委派的 Windows Server 2003 電腦，這些電腦可能需要額外的設定來變更 Spn。 如需詳細資訊，請參閱 Microsoft 知識庫中的文章936628（[https://go.microsoft.com/fwlink/?LinkId=102306](https://go.microsoft.com/fwlink/?LinkId=102306)）。  
  
    -   如果您想要委派許可權來修改次級系統管理員的 Spn，請參閱委派授權以修改 Spn （[https://go.microsoft.com/fwlink/?LinkId=106639](https://go.microsoft.com/fwlink/?LinkId=106639)）。  
  
-   如果您在部署中使用輕量型目錄存取協定（LDAP） over 安全通訊端層（SSL）（也稱為 LDAPS）搭配使用在脫離的命名空間中設定之網域控制站的 CA，則必須使用適當的 Active Directory 功能變數名稱和主要 DNS 尾碼（當您設定 LDAPS 憑證時）。  
  
    如需網域控制站憑證需求的詳細資訊，請參閱 Microsoft 知識庫中的文章321051（[https://go.microsoft.com/fwlink/?LinkId=102307](https://go.microsoft.com/fwlink/?LinkId=102307)）。  
  
    > [!NOTE]  
    > 使用 LDAPS 憑證的網域控制站可能會要求您重新部署其憑證。 當您這麼做時，網域控制站可能不會選取適當的憑證，直到重新開機為止。 如需有關 LDAPS 驗證和 Windows Server 2003 相關更新的詳細資訊，請參閱 Microsoft 知識庫中的文章932834（[https://go.microsoft.com/fwlink/?LinkId=102308](https://go.microsoft.com/fwlink/?LinkId=102308)）。  
  
### <a name="planning-for-disjoint-namespace-deployments"></a>規劃脫離的命名空間部署  
如果您在具有脫離的命名空間的環境中部署電腦，請採取下列預防措施：  
  
1.  通知您業務的所有軟體廠商，他們必須測試並支援脫離的命名空間。 要求他們確認其在使用脫離命名空間的環境中支援其應用程式。  
  
2.  在脫離的命名空間實驗室環境中測試所有版本的作業系統和應用程式。 當您這麼做時，請遵循下列建議：  
  
    1.  請先解決所有軟體問題，再將軟體部署到您的環境中。  
  
    2.  可能的話，請參與您打算在脫離的命名空間中部署之作業系統和應用程式的 Beta 測試。  
  
3.  確定系統管理員和技術服務人員都知道脫離的命名空間及其影響。  
  
4.  建立一個方案，讓您可以視需要從脫離的命名空間轉換成連續的命名空間。  
  
5.  宣導與作業系統和應用程式提供者之間脫離的命名空間支援的重要性。  
  


