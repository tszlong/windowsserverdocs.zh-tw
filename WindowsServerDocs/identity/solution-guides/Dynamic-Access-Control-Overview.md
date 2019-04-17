---
ms.assetid: 9ee8a6cb-7550-46e2-9c11-78d0545c3a97
title: "動態存取控制概觀"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 5cf74042c9b511abb1fbeb88224dea0c7f2c8706
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="dynamic-access-control-overview"></a>動態存取控制概觀

>適用於：Windows Server 2012 R2、Windows Server 2012

適用於 IT 專業人員此概觀主題描述動態存取控制與它已項目，這是在 Windows Server 2012 和 Windows 8 中。  
  
網域型動態存取控制可讓存取權的控制權限，根據模糊規則可以包含的資源，工作或使用者的角色，以及存取下列資源來使用裝置的設定敏感度限制適用於系統管理員。  
  
例如，使用者存取時的資源從他們的 office 電腦與筆記型電腦會使用透過 virtual 私人網路時，可能有不同的權限。 或存取可能會允許的裝置才符合安全性需求的網路系統管理員所定義。 動態存取控制使用時，使用者的權限變更動態介入額外的系統管理員使用者的工作或角色變更（導致對使用者的 account 屬性 AD ds）。  
  
不支援動態存取控制在之前的 Windows Server 2012 和 Windows 8 的 Windows 作業系統。 當動態存取控制設定環境的支援，不受支援的 Windows 版本中時，只有支援的版本會實作所做的變更。  
  
功能與動態存取控制相關聯的概念包括：  
  
-   [中央存取規則](#BKMK_Rules)  
  
-   [中央存取原則](#BKMK_Policies)  
  
-   [宣告](#BKMK_Claims)  
  
-   [運算式](#BKMK_Expressions2)  
  
-   [建議的權限](#BKMK_Permissions2)  
  
### <a name="BKMK_Rules"></a>中央存取規則  
中央存取規則是授權規則，可能包括一或多個條件，包括使用者群組、使用者宣告、裝置宣告和資源屬性。 多個中央存取規則可以結合成的中央存取原則。  
  
如果有一或多個中央存取規則定義網域，檔案共用系統管理員可以符合特定資源與企業需求的特定規則。  
  
### <a name="BKMK_Policies"></a>中央存取原則  
中央存取原則是包含條件運算式授權原則。 例如，我們顯示組織只檔案擁有者和人們資源（小時又）部門的成員獲准檢視 PII 資訊中有個人資訊 (PII) 上限制存取商務用需求。 這表示全組織的原則，適用於 PII 檔案，只要它們跨組織位於檔案伺服器上。 若要執行這項原則，組織必須無法：  
  
-   找出並標記包含 PII 的檔案。  
  
-   找出小時又成員獲准檢視 PII 資訊的群組。  
  
-   中央存取原則新增到中央存取規則，並套用的中央存取規則至檔案中包含 PII，它們的跨組織所在之間檔案伺服器的地方。  
  
中央存取原則做為安全性 umbrellas 組織套用其伺服器上。 這些原則的除此之外（但不是會取代）任意存取控制清單 (Dacl)，適用於的檔案和資料夾的存取本機原則。  
  
### <a name="BKMK_Claims"></a>宣告  
理賠要求是獨特的使用者，裝置或度網域控制站的資源的相關資訊。 使用者的標題、的檔案或電腦的健康狀態部門分類是有效範例理賠要求。 實體可能需要多個宣告，請及任何主張的組合，可用於授權的存取權的資源。 支援的 Windows 版本提供宣告下列類型：  
  
-   **使用者宣告**的特定使用者的 Active Directory 屬性。  
  
-   **裝置宣告**的特定電腦物件相關聯的 Active Directory 屬性。  
  
-   **資源屬性**標示為決策授權使用及發行 Active Directory 中的全域資源屬性。  
  
宣告讓系統管理員讓精確組織或企業版-全聲明使用者、裝置和資源，就可以加入運算式、規則和原則。  
  
### <a name="BKMK_Expressions2"></a>運算式  
條件運算式是 enhancement 存取控制管理允許或拒絕符合某些條件時才資源，例如群組成員資格、位置或裝置的安全狀態。 管理運算式透過進階安全性設定] 對話方塊 ACL 編輯器的中央存取規則編輯器在 Active Directory 系統管理員中心 (ADAC)。  
  
運算式幫助系統管理員，管理存取敏感的資源彈性條件越來越複雜的企業環境中使用。  
  
### <a name="BKMK_Permissions2"></a>建議的權限  
建議的權限讓系統管理員可以更精準地模型潛在的變更，而不需要實際變更它們存取控制設定的影響。  
  
預測生效存取資源協助您規劃和之前實作變更這些設定的權限的資源。  
  
## <a name="additional-changes"></a>其他的變更  
支援的支援動態存取控制 Windows 版本中的其他改進包括：  
  
### <a name="support-in-the-kerberos-authentication-protocol-to-reliably-provide-user-claims-device-claims-and-device-groups"></a>支援 Kerberos 驗證通訊協定可靠地提供使用者宣告、裝置宣告，與裝置群組中。  
根據預設，執行下列任何支援的 Windows 版本的裝置都能處理動態存取控制與 Kerberos 門票，包括複合驗證所需資料。 網域控制站的問題，以及回應 Kerberos 門票複合驗證相關資訊。 網域辨識動態存取控制設定之後，裝置收到宣告網域控制站初始在驗證期間，並在提交服務票證要求時，它們會接收複合驗證票證。 包含的資源，辨識動態存取控制裝置的使用者身分存取權杖會導致複合驗證。  
  
### <a name="support-for-using-the-key-distribution-center-kdc-group-policy-setting-to-enable-dynamic-access-control-for-a-domain"></a>使用網域讓動態存取控制鍵 Distribution 中心 (KDC) 群組原則設定支援。  
每個網域控制站需要有相同的系統管理範本原則設定，這是位於**電腦設定 \ 原則 Templates\System\KDC\Support 動態存取控制以及 Kerberos 保護 \**。  
  
### <a name="support-for-using-the-key-distribution-center-kdc-group-policy-setting-to-enable-dynamic-access-control-for-a-domain"></a>使用網域讓動態存取控制鍵 Distribution 中心 (KDC) 群組原則設定支援。  
每個網域控制站需要有相同的系統管理範本原則設定，這是位於**電腦設定 \ 原則 Templates\System\KDC\Support 動態存取控制以及 Kerberos 保護 \**。  
  
### <a name="support-in-active-directory-to-store-user-and-device-claims-resource-properties-and-central-access-policy-objects"></a>在 Active Directory 儲存使用者和裝置宣告、資源屬性和中央存取原則物件的支援。  
  
### <a name="support-for-using-group-policy-to-deploy-central-access-policy-objects"></a>使用群組原則部署中央存取原則物件的支援。  
下列群組原則設定可讓您將檔案伺服器您在組織中部署中央存取原則物件：**電腦 Configuration\Policies\ Windows 安全性設定後者 System\Central 存取原則**。  
  
### <a name="support-for-claims-based-file-authorization-and-auditing-for-file-systems-by-using-group-policy-and-global-object-access-auditing"></a>授權宣告為基礎的檔案和檔案系統稽核使用群組原則和全球物件存取稽核支援  
您必須支援階段的中央存取原則稽核使用建議的權限稽核中央存取原則有效的存取。 在電腦這個設定**進階稽核原則設定**在**的安全性設定**群組原則物件 (GPO)。 [安全性] 設定中 GPO 設定之後，您可以在您的網路 GPO 部署至電腦。  
  
### <a name="support-for-transforming-or-filtering-claim-policy-objects-that-traverse-active-directory-forest-trusts"></a>轉換或篩選往返樹系的 Active Directory 信任的理賠要求原則物件的支援  
您可以篩選或轉換往返信任的樹系的傳入的和傳出宣告。 您有篩選與轉換宣告三個基本案例：  
  
-   **值型篩選**篩選器可以為基礎的值理賠要求。 這可讓受信任的樹系，以避免傳送到信任的樹系的某些值主張。 信任的樹系的網域控制站可以使用的值為基礎篩選防範篩選傳入宣告特定值的受信任的樹系的權限提高權限的攻擊。  
  
-   **取得型篩選**篩選根據類型理賠要求，而非宣告的值。 您找出宣告類型宣告的名稱。 您使用型篩選受信任的樹系，理賠要求，它就會防止視窗傳送宣告公開信任的樹系資訊。  
  
-   **取得型轉換**操作傳送到預期的目標之前理賠要求。 您使用宣告型轉換在受信任的樹系来將已知理賠要求含有特定資訊。 您可以使用轉換来將宣告類型、宣告值，或兩者。  
  
## <a name="software-requirements"></a>軟體需求  
宣告」和「動態存取控制複合驗證要求 Kerberos 驗證擴充功能，因為任何支援動態存取控制網域必須不足，無法網域控制站執行支援支援從動態存取控制感知 Kerberos 驗證的 Windows 版本。 根據預設，裝置必須使用網域控制站在其他網站。 如果這類網域控制站可供使用，將會失敗驗證。 因此，您必須支援一項下列條件：  
  
-   每個網域支援動態存取控制必須執行 Windows server 支援所有的裝置執行的 Windows 或 Windows Server 支援的版本中的驗證支援的版本不足，無法網域控制站。  
  
-   裝置執行的 Windows 或是可支援的版本不使用宣告或複合身分保護資源，應該停用動態存取控制 Kerberos 通訊協定的支援。  
  
支援宣告使用者網域中，每個執行支援的版本的 Windows server 的網域控制站必須設定與支援宣告和複合驗證以及提供 Kerberos 保護 \ 適當的設定。 設定的管理範本] \ [KDC 原則中，如下所示：  
  
-   **永遠提供宣告**使用此設定，如果所有網域控制站都執行支援的 Windows Server 版本。 此外，在 Windows Server 2012 或更高版本設定的網域功能層級。  
  
-   **支援的**當您使用此設定時，監視網域控制站確保足以 client 的電腦需要存取受動態存取控制資源數目執行支援的版本的 Windows Server 網域控制站的數目。  
  
如果使用者網域和檔案伺服器網域中不同的樹系，必須設定檔案伺服器的樹系根中的所有網域控制站的 Windows Server 2012 或更高版本功能層級。  
  
如果戶端無法辨識動態存取控制，之間兩個樹系必須是雙向信任關係。  
  
如果轉換宣告他們會離開樹系時，必須設定所有使用者的樹系根網域控制站在 Windows Server 2012 或更高版本正常運作的層級。  
  
執行 Windows Server 2012 或 Windows Server 2012 R2 檔案伺服器必須指定是否需要將無法執行宣告的使用者權杖使用者宣告群組原則設定。 此設定預設為**自動**，而導致已此群組原則設定**在**是否中央包含裝置的使用者或宣告該檔案伺服器的原則。 該檔案伺服器包含包含使用者宣告任意 Acl，如果您需要將這個群組原則設定為**的**，伺服器便知道要求宣告代表不提供宣告存取伺服器時的使用者。  
  
## <a name="additional-resource"></a>其他資源  
適用於執行方案這個技術為基礎的相關資訊，請查看[動態存取控制：案例概觀](Dynamic-Access-Control--Scenario-Overview.md)。  
  


