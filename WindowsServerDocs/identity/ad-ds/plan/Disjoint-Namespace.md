---
ms.assetid: d92731f1-e4d8-4223-9b07-ca1f40bb0e1f
title: 斷續命名空間
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 76733e31497ab92ab54cb583b9db88b989400389
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93068730"
---
# <a name="disjoint-namespace"></a>斷續命名空間

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

當一或多個網域成員電腦具有主功能變數名稱稱服務 (DNS) 尾碼，而該名稱與電腦所屬 Active Directory 網域的 DNS 名稱不符時，就會出現脫離的命名空間。 例如，在名為 na.corp.fabrikam.com 的 Active Directory 網域中使用主要 DNS 尾碼 corp.fabrikam.com 的成員電腦，會使用脫離的命名空間。

相較于連續的命名空間，脫離的命名空間比管理、維護和疑難排解更為複雜。 在連續的命名空間中，主要 DNS 尾碼會與 Active Directory 的功能變數名稱相符。 寫入的網路應用程式假設 Active Directory 命名空間等同于所有網域成員電腦的主要 DNS 尾碼，在脫離的命名空間中無法正常運作。

## <a name="support-for-disjoint-namespaces"></a>支援斷續命名空間

網域成員電腦（包括網域控制站）可以在脫離的命名空間中運作。 網域成員電腦可以 () 資源記錄和 IP 第6版 (IPv6) 主機 (不連續的 DNS 命名空間中的主機來註冊其主機。 當網域成員電腦以這種方式註冊其資源記錄時，網域控制站會繼續在 DNS 區域中註冊全域和特定網站服務 (SRV) 資源記錄，該資源會與 Active Directory 功能變數名稱相同。

例如，假設 na.corp.fabrikam.com 網域的域 Active Directory 控制器使用 corp.fabrikam.com 的主要 DNS 尾碼來登錄主機 () 和 IPv6 主機 (AAAA) corp.fabrikam.com DNS 區域中的資源記錄。 網域控制站會繼續註冊全域和特定網站服務 (SRV) 資源記錄在 _msdcs 的 na.corp.fabrikam.com DNS 區域中，這可讓服務位置成為可能。

> [!IMPORTANT]
> 雖然 Windows 作業系統可能支援脫離的命名空間，但撰寫的應用程式假設主要 DNS 尾碼與 Active Directory 的網域尾碼在這類環境中可能無法運作。 基於這個理由，您應該先仔細測試所有應用程式及其各自的作業系統，再部署脫離的命名空間。

脫離的命名空間應該可 (，並在下列情況下受到支援) ：

- 當具有多個 Active Directory 網域的樹系使用單一 DNS 命名空間時，也稱為 DNS 區域

    其中一個範例是使用區域網域（具有 na.corp.fabrikam.com、sa.corp.fabrikam.com 和 asia.corp.fabrikam.com 等名稱），並使用單一 DNS 命名空間（例如 corp.fabrikam.com）的公司。

- 當單一 Active Directory 網域分割成不同的 DNS 命名空間時

    其中一個範例是具有 corp.contoso.com 網域的公司，Active Directory 其使用 DNS 區域，例如 hr.corp.contoso.com、production.corp.contoso.com 和 it.corp.contoso.com。

脫離的命名空間無法正常運作 (而且在下列情況下不支援) ：

- 網域成員所使用的脫離尾碼符合這個或其他樹系中的 Active Directory 功能變數名稱。 這會中斷 Kerberos 名稱尾碼路由。

- 另一個樹系中會使用相同的脫離尾碼。 這可防止在樹系之間唯一路由傳送這些尾碼。

- 當網域成員憑證授權單位單位 (CA) server 變更其完整功能變數名稱 (FQDN) ，如此就不會再使用 CA 伺服器所屬網域的網域控制站所使用的相同主要 DNS 尾碼。 在此情況下，您可能會在驗證 CA 伺服器所發出的憑證時遇到問題，這取決於 CRL 發佈點中使用的 DNS 名稱。 但是，如果您將 CA 伺服器放在穩定的脫離命名空間中，它就會正常運作並受到支援。

## <a name="considerations-for-disjoint-namespaces"></a>脫離的命名空間考慮

下列考慮可協助您決定是否應該使用脫離的命名空間。

### <a name="application-compatibility"></a>應用程式相容性

如先前所述，脫離的命名空間可能會導致任何撰寫的應用程式和服務發生問題，而這些應用程式和服務會假設電腦的主要 DNS 尾碼與它所屬功能變數名稱的名稱相同。 部署脫離的命名空間之前，您必須先檢查應用程式的相容性問題。 此外，請務必檢查您在執行分析時使用的所有應用程式的相容性。 這包括來自 Microsoft 和其他軟體發展人員的應用程式。

### <a name="advantages-of-disjoint-namespaces"></a>斷續命名空間的優點

使用脫離的命名空間可能有下列優點：

- 由於電腦的主要 DNS 尾碼可以指出不同的資訊，因此您可以從 Active Directory 功能變數名稱分開管理 DNS 命名空間。

- 您可以根據商務結構或地理位置來分隔 DNS 命名空間。 例如，您可以根據業務單位名稱或實體位置（例如大陸、國家/地區或建築物）來分隔命名空間。

### <a name="disadvantages-of-disjoint-namespaces"></a>斷續命名空間的缺點

使用脫離的命名空間可能有下列缺點：

- 您必須為樹系中每個具有使用脫離命名空間之成員電腦的 Active Directory 網域，建立及管理個別的 DNS 區域。  (也需要進行額外的複雜設定。 ) 

- 您必須執行手動步驟來修改和管理允許網域成員使用指定的主要 DNS 尾碼的 Active Directory 屬性。

- 若要優化名稱解析，您必須執行手動步驟來修改和維護群組原則，以設定具有替代主要 DNS 尾碼的成員電腦。

> [!NOTE]
> 您可以藉由解析單一標籤名稱，使用 Windows 網際網路名稱服務 (WINS) 來彌補此缺點。 如需 WINS 的詳細資訊，請參閱 [Wins 技術參考](/previous-versions/windows/it-pro/windows-server-2003/cc736411(v=ws.10))。

- 當您的環境需要多個主要 DNS 尾碼時，您必須適當地設定樹系中所有 Active Directory 網域的 DNS 尾碼搜尋順序。

    若要設定 DNS 尾碼搜尋順序，您可以使用群組原則的物件或動態主機設定通訊協定 (DHCP) Server 服務參數。 您也可以修改登錄。

- 您必須仔細測試所有應用程式的相容性問題。

如需您可以採取以解決這些缺點之步驟的詳細資訊，請參閱 [建立脫離的命名空間](/previous-versions/windows/it-pro/windows-server-2003/cc755926(v=ws.10))。

### <a name="planning-a-namespace-transition"></a>規劃命名空間轉換

在修改命名空間之前，請先參閱下列考慮，這適用于從連續的命名空間轉換為脫離的命名空間 (或反向) ：

- 手動設定的服務主體名稱 (Spn) 可能不再符合命名空間變更之後的 DNS 名稱。 這可能會導致驗證失敗。

    如需詳細資訊，請參閱服務登入 [失敗，因為 spn 的設定不正確](/previous-versions/windows/it-pro/windows-server-2003/cc772897(v=ws.10))。

    - 如果您使用具有限制委派的 Windows Server 2003 型電腦，則這些電腦可能需要額外的設定才能變更 Spn。 如需詳細資訊，請參閱 Microsoft 知識庫中的文章936628， [當您嘗試在執行 Windows Server 2003 (404) 的電腦上設定限制委派時，SPN 不會出現在可委派給帳戶的服務清單中](https://support.microsoft.com/help/936628) 。

    - 如果您想要委派許可權來修改次級系統管理員的 Spn，請參閱委派許可權來 [修改 spn](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770439(v=ws.10))。

- 如果您使用輕量型目錄存取協定 (LDAP) 超過安全通訊端層 (SSL)  (稱為 LDAPS) ，且部署中的 CA 具有脫離的命名空間中設定的網域控制站，則在設定 LDAPS 憑證時，您必須使用適當的 Active Directory 功能變數名稱和主要 DNS 尾碼。

    如需網域控制站憑證需求的詳細資訊，請參閱 Microsoft 知識庫中的文章321051： [如何使用協力廠商憑證授權單位單位啟用 LDAP OVER SSL](https://support.microsoft.com/help/321051/)。

    > [!NOTE]
    > 使用 LDAPS 憑證的網域控制站可能會要求您重新部署其憑證。 當您這樣做時，網域控制站在重新開機之前，可能不會選取適當的憑證。 如需有關輕量型目錄存取協定的詳細資訊 (LDAP) 透過安全通訊端層 (SSL) 適用于 Windows Server 2003 的 LDAPS (驗證，請參閱 Microsoft 知識庫中的文章938703、 [如何針對 LDAP OVER SSL 連線問題進行疑難排解](https://support.microsoft.com/help/938703/)。

### <a name="planning-for-disjoint-namespace-deployments"></a>規劃斷續命名空間部署

如果您在具有脫離的命名空間的環境中部署電腦，請採取下列預防措施：

1. 通知所有您經營業務的軟體廠商，他們必須測試並支援脫離的命名空間。 要求他們在使用脫離的命名空間的環境中，確認它們支援其應用程式。

2. 在脫離的命名空間實驗室環境中測試所有版本的作業系統和應用程式。 當您這樣做時，請遵循下列建議：

    1. 在您的環境中部署軟體之前，請先解決所有的軟體問題。

    2. 可能的話，請參與您計畫在脫離的命名空間中部署之作業系統和應用程式的 Beta 測試。

3. 確定系統管理員和技術支援人員都知道脫離的命名空間及其影響。

4. 如果有必要，您可以建立一個方案，讓您從脫離的命名空間轉換成連續的命名空間。

5. 宣導與作業系統和應用程式提供者的脫離命名空間支援的重要性。
