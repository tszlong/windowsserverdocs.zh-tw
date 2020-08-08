---
ms.assetid: 9ee8a6cb-7550-46e2-9c11-78d0545c3a97
title: 動態存取控制概觀
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: ef1c5686bb692f2fbe18e2f4bf7b2d0bd6efe52d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87952758"
---
# <a name="dynamic-access-control-overview"></a>動態存取控制概觀

>適用目標︰Windows Server 2012 R2、Windows Server 2012

這個適用於 IT 專業人員的概觀主題將說明動態存取控制及其相關聯的項目，這些項目已於 Windows Server 2012 和 Windows 8 中推出。

網域型動態存取控制讓系統管理員能夠根據明確定義的規則套用存取控制權限與限制，這些規則可以包含資源的敏感度、使用者的工作或角色，以及用來存取這些資源的裝置設定。

例如，使用者從辦公室電腦存取資源和透過虛擬私人網路使用可攜式電腦時，可能會有不同的權限。 或者，只有在裝置符合網路系統管理員所定義的安全性需求時才允許存取。 使用動態存取控制時，如果使用者的工作或角色變更時，使用者的許可權會動態變更，而不需要額外的系統管理員介入， (導致 AD DS) 中使用者帳戶屬性的變更。

Windows Server 2012 與 Windows 8 之前的 Windows 作業系統不支援動態存取控制。 在使用支援和不支援的 Windows 版本的環境中設定動態存取控制時，只有支援的版本會實作變更。

與動態存取控制相關聯的功能和概念包含：

-   [集中存取規則](#BKMK_Rules)

-   [集中存取原則](#BKMK_Policies)

-   [宣告](#BKMK_Claims)

-   [運算式](#BKMK_Expressions2)

-   [建議的權限](#BKMK_Permissions2)

### <a name="central-access-rules"></a><a name="BKMK_Rules"></a>集中存取規則
集中存取規則是授權規則的運算式，可以包含一或多個涉及使用者群組、使用者宣告、裝置宣告及資源內容的條件。 多個集中存取規則可以組成一個集中存取原則。

如果已針對網域定義了一或多個集中存取規則，檔案共用系統管理員便可將特定的規則與特定的資源和業務需求進行比對。

### <a name="central-access-policies"></a><a name="BKMK_Policies"></a>集中存取原則
集中存取原則是包含條件運算式的授權原則。 例如，假設組織有商務需求，將個人識別資訊的存取許可權制為只有檔案擁有者和 (人力資源) 部門的成員可以查看 PII 資訊， (PII) 檔案。 這代表要套用至 PII檔案的全組織原則，而不論它們位於組織中檔案伺服器上的何處。 若要實作這個原則，組織需要能夠：

-   識別和標記包含 PII 的檔案。

-   識別允許檢視 PII 資訊的 HR 成員群組。

-   將集中存取原則新增至集中存取規則，然後將集中存取規則套用至包含 PII 的所有檔案，而不論它們位於組織中檔案伺服器上的何處。

集中存取原則就像是安全性保護傘，組織可以跨伺服器套用這個保護傘。 這些是套用到檔案和資料夾的本機存取原則或判別存取控制清單 (DACL) 以外 (但不取代) 的原則。

### <a name="claims"></a><a name="BKMK_Claims"></a>退款
宣告是由網域控制站所發佈的使用者、裝置或資源的相關資訊中一個獨特的部分。 使用者的標題、檔案的部門分類，或電腦的健全狀況狀態，都是宣告的有效範例。 實體可以包含一個以上的宣告，而且所有的宣告組合都可用來授權資源的存取。 下列宣告類型可以在支援的 Windows 版本中使用：

-   **使用者宣告** 與特定的使用者相關聯的 Active Directory 屬性。

-   **裝置宣告** 與特定電腦物件相關聯的 Active Directory 屬性。

-   **資源屬性** 全域資源內容，標記為用於授權決策，並在 Active Directory 中發佈。

宣告讓系統管理員能夠精確做出有關使用者、裝置及資源的整個組織或整個企業的聲明，並可合併到運算式、規則及原則中。

### <a name="expressions"></a><a name="BKMK_Expressions2"></a>運算式
條件運算式是存取控制管理的增強功能，只有在符合特定條件 (例如，群組成員資格、位置或裝置的安全性狀態) 時，才允許或拒絕存取資源。 運算式是透過 Active Directory 管理中心 (ADAC) 中 ACL 編輯器或集中存取規則編輯器的 [進階安全性設定] 對話方塊來管理。

運算式可以協助系統管理員，在日漸複雜的商業環境中，利用彈性化條件來管理敏感性資源的存取。

### <a name="proposed-permissions"></a><a name="BKMK_Permissions2"></a>建議的權限
建議的權限讓系統管理員能夠更準確地針對存取控制設定的潛在變更所造成的影響塑造模型，而不需實際變更它們。

預測資源的有效存取權，可以協助您在實作這些變更之前，先規劃和設定這些資源的權限。

## <a name="additional-changes"></a>其他變更
在支援的 Windows 版本中，支援動態存取控制的其他增強功能包含：

### <a name="support-in-the-kerberos-authentication-protocol-to-reliably-provide-user-claims-device-claims-and-device-groups"></a>支援 Kerberos 驗證通訊協定，可靠的提供使用者宣告、裝置宣告及裝置群組。
根據預設，執行任何支援之 Windows 版本的裝置能夠處理與動態存取控制相關的 Kerberos 票證，其中包含複合驗證所需的資料。 網域控制站能夠利用與複合驗證相關的資訊，發出與回應 Kerberos 票證。 將網域設定為可辨識動態存取控制時，裝置會在初始驗證期間，收到來自網域控制站的宣告，然後在提交服務票證要求時收到複合驗證票證。 複合驗證會產生存取權杖，其中包含資源上可辨識動態存取控制的使用者和裝置的身分識別。

### <a name="support-for-using-the-key-distribution-center-kdc-group-policy-setting-to-enable-dynamic-access-control-for-a-domain"></a>支援使用金鑰發佈中心 (KDC) 群組原則設定，以啟用網域的動態存取控制。
每個網域控制站都需要具備相同的系統管理範本原則設定，這個設定位於 [電腦設定\原則\系統管理範本\系統\KDC\支援動態存取控制和 Kerberos 保護]**** 中。

### <a name="support-for-using-the-key-distribution-center-kdc-group-policy-setting-to-enable-dynamic-access-control-for-a-domain"></a>支援使用金鑰發佈中心 (KDC) 群組原則設定，以啟用網域的動態存取控制。
每個網域控制站都需要具備相同的系統管理範本原則設定，這個設定位於 [電腦設定\原則\系統管理範本\系統\KDC\支援動態存取控制和 Kerberos 保護]**** 中。

### <a name="support-in-active-directory-to-store-user-and-device-claims-resource-properties-and-central-access-policy-objects"></a>支援 Active Directory，以儲存使用者和裝置宣告、資源內容和集中存取原則物件。

### <a name="support-for-using-group-policy-to-deploy-central-access-policy-objects"></a>支援使用群組原則來部署集中存取原則物件。
以下的群組原則設定讓您能夠將集中存取原則物件部署到組織中的檔案伺服器：**電腦設定\原則\ Windows 設定\安全性設定\檔案系統\集中存取原則**。

### <a name="support-for-claims-based-file-authorization-and-auditing-for-file-systems-by-using-group-policy-and-global-object-access-auditing"></a>使用群組原則和全域物件存取稽核，支援宣告型檔案授權並對檔案系統進行稽核
您必須啟用分段集中存取原則稽核，使用建議的權限來稽核集中存取原則的有效存取權。 您可以在群組原則物件 (GPO) 的 [安全性設定]**** 中，在 [進階稽核原則設定]**** 下方設定這個電腦設定。 當您在 GPO 中設定安全性原則設定之後，就可以將 GPO 部署到網路中的電腦上。

### <a name="support-for-transforming-or-filtering-claim-policy-objects-that-traverse-active-directory-forest-trusts"></a>支援轉換或篩選周遊 Active Directory 樹系信任的宣告原則物件
您可以篩選或轉換周遊樹系信任的連入與連出宣告。 篩選和轉換宣告有下列三種基本案例：

-   **以值為基礎的篩選** 篩選能夠以宣告的值為基礎。 這允許受信任的樹系防止將含有特定值的宣告傳送到信任樹系。 信任樹系中的網域控制站可以藉由篩選來自受信任的樹系中含有特定值的連入宣告，使用以值為基礎的篩選來防止權限提高攻擊。

-   **以宣告類型為基礎的篩選** 根據宣告的類型而非宣告的值來篩選。 您可以依宣告的名稱來識別宣告類型。 您可以在受信任的樹系上使用以宣告類型為基礎的篩選，它可防止 Windows 將透露資訊的宣告傳送到信任樹系。

-   **以宣告類型為基礎的轉換** 在將宣告傳送給預期的目標之前操控宣告。 您在受信任的樹系中使用以宣告類型為基礎的轉換，將包含特定資訊的已知宣告一般化。 您可以使用轉換，將宣告類型、宣告值或兩者一般化。

## <a name="software-requirements"></a>軟體需求
由於動態存取控制的宣告和複合驗證需要 Kerberos 驗證延伸模組，因此，任何支援動態存取控制的網域都必須有足夠數目且執行支援的 Windows 版本的網域控制站，以支援來自動態存取控制感知 Kerberos 用戶端的驗證。 根據預設，裝置必須使用其他網站中的網域控制站。 如果沒有這類網域控制站可供使用，驗證將會失敗。 因此，您必須支援下列其中一個條件：

-   每個支援動態存取控制的網域都必須具有足夠數目且執行支援的 Windows Server 版本的網域控制站，以支援來自所有執行支援的 Windows 或 Windows Server 版本之裝置的驗證。

-   執行支援的 Windows 版本或者未使用宣告或複合身分識別來保護資源的裝置，應該停用動態存取控制的 Kerberos 通訊協定支援。

對於支援使用者宣告的網域，每個執行支援的 Windows Server 版本的網域控制站必須使用適當設定進行設定，以支援宣告、複合驗證，以及提供 Kerberos 防護。 在 KDC 系統管理範本原則中進行設定，如下：

-   **永遠提供宣告** 如果所有網域控制站都執行支援的 Windows Server 版本，請使用這個設定。 此外，將網域功能等級設定為 Windows Server 2012 或更高版本。

-   **支援** 當您使用這個設定時，請監視網域控制站，以確保執行支援的 Windows Server 版本的網域控制站數目滿足存取動態存取控制所保護的資源所需的用戶端電腦數目。

如果使用者網域和檔案伺服器網域位於不同的樹系，則檔案伺服器樹系根目錄中的所有網域控制站都必須設定為 Windows Server 2012 或更高的功能等級。

如果用戶端無法辨識動態存取控制，則兩個樹系之間必須具備雙向的信任關係。

如果宣告會在離開樹系時轉換，則使用者樹系根目錄中的所有網域控制站都必須設定為 Windows Server 2012 或更高的功能等級。

執行 Windows Server 2012 或 Windows Server 2012 R2 的檔案伺服器必須具有一個群組原則設定，指定它是否需要針對不含宣告的使用者權杖取得使用者宣告。 這個設定預設會設定為 [自動]****，如果有包含該檔案伺服器的使用者或裝置宣告的集中原則，則會將這個群組原則設定轉換為 [開啟]****。 如果檔案伺服器包含內含使用者宣告的判別 ACL，您需要將這個群組原則設定為 [開啟]****，如此便能讓伺服器知道要代表在存取伺服器時未提供宣告的使用者要求宣告。

## <a name="additional-resource"></a>其他資源
如需根據這項技術來執行解決方案的詳細資訊，請參閱[Dynamic 存取控制：案例總覽](Dynamic-Access-Control--Scenario-Overview.md)。



