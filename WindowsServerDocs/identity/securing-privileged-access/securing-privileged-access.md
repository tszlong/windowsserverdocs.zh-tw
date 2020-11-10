---
title: 保護特殊權限存取
description: 保護特殊權限存取的分段式方法
ms.topic: conceptual
ms.assetid: f5dec0c2-06fe-4c91-9bdc-67cc6a3ede60
ms.date: 02/25/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: mas
ms.openlocfilehash: 23b7322b76eb60c0ae19d3aa0e9e826998a92c77
ms.sourcegitcommit: 8c0a419ae5483159548eb0bc159f4b774d4c3d85
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/03/2020
ms.locfileid: "93235875"
---
# <a name="securing-privileged-access"></a>保護特殊權限存取

>適用於：Windows Server

保護特殊權限存取是針對現代組織中的企業資產建立安全性保證時重要的第一個步驟。 IT 組織中大部分或所有企業資產的安全性，都需仰賴用來管理及開發的特殊權限帳戶的完整性。 網路攻擊者常會以這些帳戶以及特殊權限存取的其他項目作為目標，使用認證竊取攻擊 (例如[傳遞雜湊與傳遞票證](https://www.microsoft.com/pth)) 取得取得資料與系統的存取權。

為保護特殊權限存取的安全以對抗這些積極的攻擊者，您必須採取完整且深思熟慮的方法來隔絕這些系統風險。

## <a name="what-are-privileged-accounts"></a>什麼是特殊權限帳戶？

在討論如何保護特殊權限帳戶之前，我們要先定義此帳戶。

特殊權限帳戶 (例如 Active Directory Domain Services 的系統管理員) 可直接或間接存取 IT 組織中大部分或所有的資產，而使這些帳戶的入侵成為高度商業風險。

## <a name="why-securing-privileged-access-is-important"></a>保護特殊權限存取為何如此重要？

網路攻擊者會重點攻擊系統的特殊權限存取 (例如 Active Directory (AD))，以快速取得所有組織目標資料的存取權。 傳統的安全性方法著重於以網路和防火牆作為主要安全性周邊，但網路安全性的有效性已因兩個趨勢而明顯降低︰

* 組織將資料和資源裝載於傳統網路界限以外，像是行動企業電腦、行動電話和平板電腦等裝置、雲端服務，以及自攜裝置 (BYOD)
* 攻擊者已透過網路釣魚和其他網路與電子郵件攻擊，取得網路界限內部工作站存取權，展現出一致且持續的攻擊能力。

基於這些因素，除了傳統的網路周邊策略外，還有必要建置以驗證和授權身分識別控制為基礎的現代化安全性周邊。 這裡的安全性周邊定義為資產之間的一組一致的控制項，以及它們面臨的威脅。 特殊權限帳戶可以有效地控制這個新的安全性周邊，因此保護特殊權限存取是非常重要的。

![此圖顯示組織的身分識別層級](../media/securing-privileged-access/PAW_LP_Fig2.JPG)

取得系統管理帳戶控制權的攻擊者可以使用這些權限，提高他們對目標組織的影響力，說明如下：

![此圖顯示取得系統管理帳戶控制權的攻擊者可以使用那些權限，靠犧牲目標組織的代價來追求其個人目的](../media/securing-privileged-access/PAW_LP_Fig3.JPG)

下圖說明兩個路徑：

* 一個「藍色」路徑，會使用標準使用者帳戶對電子郵件、網頁瀏覽等資源進行非特殊權限存取，以及完成日常工作。

   > [!NOTE]
   > 稍後說明的藍色路徑項目將指出超出系統管理帳戶範圍的廣泛環境保護。

* 一個「紅色」路徑，會在強化的裝置上進行特殊權限存取，以降低網路釣魚和其他網頁和電子郵件攻擊的風險。

![此圖顯示藍圖所建立，用於系統管理的個別「路徑」，以隔離特殊權限存取工作和高風險標準使用者工作 (例如網頁瀏覽與存取電子郵件)](../media/securing-privileged-access/PAW_LP_Fig4.JPG)

## <a name="securing-privileged-access-roadmap"></a>保護特殊權限存取的藍圖

此藍圖的設計目的是要充分運用您已部署的 Microsoft 技術、利用可強化安全性的技術，以及整合任何您可能已部署的第三方安全性工具。

Microsoft 建議的藍圖分成 3 個階段︰

* [階段 1：前 30 天](#phase-1-quick-wins-with-minimal-operational-complexity)
   * 以有意義的正面影響快速取得利基。
* [階段 2：90 天](#phase-2-significant-incremental-improvements)
   * 累積實質改善。
* [階段 3：進行中](#phase-3-security-improvement-and-sustainment)
   * 安全性改善和持續維護。

藍圖已根據我們對這些攻擊的經驗和解決方案的實作經驗，優先排定最有效、最快速的實作。

Microsoft 建議您遵循這份藍圖來保護特殊權限存取以對抗積極的攻擊者。 您可能需要調整這份藍圖以容納您現有的功能以及您組織中的特定需求。

> [!NOTE]
> 保護特殊權限存取需要更廣泛的項目，包括技術元件 (主機防禦、帳戶保護、身分識別管理等) 以及處理程序的變更，和系統管理實務與知識。 藍圖的時間表為估計值，並且依我們的經驗與客戶實作為根據。 根據您的環境與變更管理處理程序的複雜程度，組織中的持續時間會有所不同。

## <a name="phase-1-quick-wins-with-minimal-operational-complexity"></a>階段 1：以最低操作複雜性快速取得利基

階段 1 的藍圖著重在快速減輕受到最常用的認證竊取與不當使用攻擊技術攻擊的風險。 階段 1 設計為大約在 30 天內實作，如下圖所示：

![階段 1 圖表：1. 個別的系統管理員和使用者帳戶，2. Just in Time 本機系統管理員密碼，3. 系統管理員工作站階段 1，4. 身分識別攻擊偵測](../media/securing-privileged-access/PAW_LP_Fig6.JPG)

### <a name="1-separate-accounts"></a>1.個別帳戶

為了將網際網路風險 (網路釣魚攻擊、網頁瀏覽) 隔絕在特殊權限存取帳戶外，請為所有具備特殊權限存取權的人員建立專用帳戶。 系統管理員不應以具備高特殊權限的帳戶瀏覽網頁、查看其電子郵件，以及執行日常生產力工作。 如需詳細資訊，請參閱參考文件的[個別的系統管理帳戶](securing-privileged-access-reference-material.md#separate-administrative-accounts)一節。

請依照[在 Azure AD 中管理緊急存取帳戶](/azure/active-directory/users-groups-roles/directory-emergency-access)一文中的指引，在您的內部部署 AD 和 Azure AD 環境中建立至少兩個緊急存取帳戶，且都必須具備永久指派的系統管理員權限。 只有在傳統系統管理員帳戶無法執行必要的工作時 (例如，在發生災害的情況下)，才可使用這些帳戶。

### <a name="2-just-in-time-local-admin-passwords"></a>2.Just in Time 本機系統管理員密碼

為了降低攻擊者從本機 SAM 資料庫竊取本機系統管理員帳戶密碼雜湊，並濫用它來攻擊其他電腦的風險，組織應確保每部電腦都有唯一的本機系統管理員密碼。 本機系統管理員密碼解決方案 (LAPS) 工具可在每個工作站和伺服器上設定唯一的隨機密碼，並將其儲存在受到 ACL 保護的 Active Directory (AD) 中。 只有合格的授權使用者才可讀取或要求重設這些本機系統管理員帳戶密碼。 您可以從 [Microsoft 下載中心](https://aka.ms/LAPS)取得要在工作站和伺服器上使用的 LAPS。

如需使用 LAP 和 PAW 操作環境的其他指導方針，請參閱[以乾淨來源準則為基礎的操作標準](securing-privileged-access-reference-material.md#operational-standards-based-on-clean-source-principle)。

### <a name="3-administrative-workstations"></a>3.系統管理工作站

對於具有 Azure Active Directory 和傳統內部部署 Active Directory 系統管理權限的使用者，請確定他們使用以[高度安全 Windows 10 裝置的標準](/windows-hardware/design/device-experiences/oem-highly-secure)進行設定的 Windows 10 裝置，作為初始安全性機制。 具備特殊權限的系統管理員帳戶不應是系統管理工作站的本機系統管理員群組成員。  需要對工作站進行設定變更時，可以利用透過使用者存取控制 (UAC) 的權限提升。  此外也應將 Windows 10 安全性基準套用至工作站，以進一步強化裝置。

### <a name="4-identity-attack-detection"></a>4.身分識別攻擊偵測

[Azure 進階威脅防護 (ATP)](/azure-advanced-threat-protection/what-is-atp) 是一種雲端式安全性解決方案，可識別、偵測及協助您調查進階威脅、遭到入侵的身分識別，以及惡意內部人員在您的內部部署 Active Directory 環境內導入的動作。

## <a name="phase-2-significant-incremental-improvements"></a>階段 2：累積實質改善

階段 2 以階段 1 完成的工作為建置基礎，設計為大約在 90 天內完成。 此階段的步驟會在此圖表中說明︰

![階段 2 圖表：1. Windows Hello 企業版/MFA，2. PAW 首度發行，3. Just in Time 權限，4. Credential Guard，5. 認證外洩，6. 橫向移動弱點偵測](../media/securing-privileged-access/PAW_LP_Fig7.JPG)

### <a name="1-require-windows-hello-for-business-and-mfa"></a>1.需要 Windows Hello 企業版和 MFA

系統管理員可受益於 Windows Hello 企業版在使用上的相關便利性。 系統管理員可在其電腦上將其複雜的密碼取代為強式雙重要素驗證。 攻擊者必須同時擁有裝置和生物特徵辨識資訊或 PIN 碼，因此要在員工不知情的情況下取得存取權，難度會提高許多。 如需 Windows Hello 企業版和推出路徑的詳細資訊，請參閱 [Windows Hello 企業版概觀](/windows/security/identity-protection/hello-for-business/hello-overview)

使用 Azure MFA，為您在 Azure AD 中的系統管理員帳戶啟用多重要素驗證 (MFA)。 您至少應啟用[基準保護條件式存取原則](/azure/active-directory/conditional-access/baseline-protection#require-mfa-for-admins)。如需 Azure Multi-Factor Authentication 的詳細資訊，請參閱[部署雲端式 Azure Multi-Factor Authentication](/azure/active-directory/authentication/howto-mfa-getstarted) 一文

### <a name="2-deploy-paw-to-all-privileged-identity-access-account-holders"></a>2.將 PAW 部署至所有特殊權限身分識別存取的帳戶持有者

在繼續為特殊權限帳戶隔絕電子郵件、網頁瀏覽和其他非系統管理工作中潛藏的威脅時，您應為有權存取組織資訊系統的所有人員，實作專用的特殊權限存取工作站 (PAW)。 如需 PAW 部署的其他指引，請參閱[特殊權限存取工作站](privileged-access-workstations.md#paw-phased-implementation)一文。

### <a name="3-just-in-time-privileges"></a>3.Just in Time 權限

若要降低權限的公開時間並增加使用的可見性，請使用如下所示的其中一個適當解決方案或其他第三方解決方案，即時提供權限 (JIT)：

* 針對 Active Directory 網域服務 (AD DS)，請使用 Microsoft Identity Manager (MIM) 的[特殊權限存取管理員 (PAM)](/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services) 功能。
* 針對 Azure Active Directory，請使用 [Azure AD 特殊權限身分識別管理 (PIM)](/azure/active-directory/privileged-identity-management/pim-deployment-plan) 功能。

### <a name="4-enable-windows-defender-credential-guard"></a>4.啟用 Windows Defender Credential Guard

啟用 Credential Guard，將有助於保護 NTLM 密碼雜湊、Kerberos 票證授權票證，以及由應用程式儲存為網域認證的認證。 這項功能可提高在環境中使用遭竊的認證進行身分轉換的困難度，因而有助於防止認證竊取攻擊 (例如傳遞雜湊或傳遞票證)。 如需 Credential Guard 的運作方式和部署方式的相關資訊，請參閱[使用 Windows Defender Credential Guard 保護衍生的網域認證](/windows/security/identity-protection/credential-guard/credential-guard)一文。

### <a name="5-leaked-credentials-reporting"></a>5.認證外洩報告

「Microsoft 每一天會分析超過 6.5 兆個訊號，以找出新興的威脅並保護客戶」- [Microsoft 數據](https://news.microsoft.com/bythenumbers/cyber-attacks)

啟用 Microsoft Azure AD Identity Protection 可報告認證外洩的使用者，以便進行補救。 [Azure AD Identity Protection](/azure/active-directory/identity-protection/index) 可用來協助您的組織保護雲端和混合式環境免於遭受威脅。

### <a name="6-azure-atp-lateral-movement-paths"></a>6.Azure ATP 橫向移動路徑

請確定特殊權限存取的帳戶持有者僅將其 PAW 用於系統管理，讓遭入侵的非特殊權限帳戶無法透過認證竊取攻擊 (例如傳遞雜湊或傳遞票證) 取得特殊權限帳戶的存取權。 [Azure ATP 橫向移動路徑 (LMP)](/azure-advanced-threat-protection/use-case-lateral-movement-path) 可讓您輕鬆了解報告，以識別特殊權限帳戶在何處可能有漏洞會遭到入侵。

## <a name="phase-3-security-improvement-and-sustainment"></a>階段 3：安全性改善和持續維護

藍圖的階段 3 以階段 1 和 2 中採取的步驟為建置基礎，以加強您的安全性狀態。 下圖以視覺化方式說明階段 3︰

![階段 3：1. 檢閱 RBAC，2. 減少受攻擊面，3. 將記錄與 SEIM 整合，4. 認證外洩自動化](../media/securing-privileged-access/PAW_LP_Fig8.JPG)

這些功能將會以先前各個階段的步驟為建置基礎，並將您的防護提高到更積極主動的狀態。 這個階段沒有特定的時間軸，而且可能需要較長的時間才能根據您的個別組織實作。

### <a name="1-review-role-based-access-control"></a>1.檢閱角色型存取控制ss control

使用 [Active Directory 管理層模型](securing-privileged-access-reference-material.md)一文中所述的三層式模型，檢閱並確保較低層的系統管理員沒有較高層資源 (群組成員資格、使用者帳戶的 ACL 等) 的系統管理存取權。

### <a name="2-reduce-attack-surfaces"></a>2.減少受攻擊面

強化您的身分識別工作負載，包括網域、網域控制站、ADFS 和 Azure AD Connect，因為只要有其中一個系統遭到入侵，就可能導致組織中的其他系統遭到入侵。 [減少 Active Directory 的攻擊面](../ad-ds/plan/security-best-practices/reducing-the-active-directory-attack-surface.md)和[保護身分識別基礎結構的五個步驟](/azure/security/azure-ad-secure-steps)這兩篇文章提供保護內部部署和混合式身分識別環境的指引。

### <a name="3-integrate-logs-with-siem"></a>3.整合記錄與 SIEM

將記錄整合到集中式 SIEM 工具中，可協助您的組織分析、偵測及回應安全性事件。 [監視 Active Directory 遭到危害的徵兆](../ad-ds/plan/security-best-practices/monitoring-active-directory-for-signs-of-compromise.md)和[附錄 L：要監視的事件](../ad-ds/plan/appendix-l--events-to-monitor.md)這兩篇文章提供您在環境中應監視哪些事件的指引。

這屬於計畫以外的部分，因為要在安全性資訊及事件管理 (SIEM) 中彙總、建立和調整警示，需仰賴具經驗的分析人員 (不同於 30 天計畫中的 Azure ATP，其中包含現成可用的警示)

### <a name="4-leaked-credentials---force-password-reset"></a>4.認證外洩 - 強制密碼重設

藉由啟用 Azure AD Identity Protection 以在密碼有洩露的疑慮時自動強制密碼重設，繼續增強您的安全性狀態。 [使用風險事件觸發 Multi-Factor Authentication 和密碼變更](/azure/active-directory/authentication/tutorial-risk-based-sspr-mfa)一文中的指引說明如何使用條件式存取原則來啟用此功能。

## <a name="am-i-done"></a>我完成了嗎？

簡單的說，答案是「不可以」。

惡意人士絕不會停手，因此您也不能鬆懈。 此藍圖可協助您的組織抵禦目前已知的威脅，但攻擊者也會持續不斷演進和變異。 我們建議您將安全性視為一項持續的過程，並將重點放在提高攻擊者以您的環境做為攻擊目標時須付出的成本並降低其攻擊成功率。

雖然這並不是組織安全性計畫的全部，但保護特殊權限存取的確是安全性策略的要素。
