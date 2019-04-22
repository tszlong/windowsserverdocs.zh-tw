---
title: 保護特殊權限的存取
description: 若要保護特殊權限的存取的分段式的方法
ms.prod: windows-server-threshold
ms.topic: conceptual
ms.assetid: f5dec0c2-06fe-4c91-9bdc-67cc6a3ede60
ms.date: 02/25/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: mas
ms.openlocfilehash: 0d54a94d51a4d1e0a1d28f78ec39bf16bc3d9100
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822009"
---
# <a name="securing-privileged-access"></a>保護特殊權限的存取

>適用於：Windows Server

保護特殊權限存取是針對現代組織中的企業資產建立安全性保證時重要的第一個步驟。 IT 組織中的大部分或所有企業資產的安全性取決於用來管理、 管理及開發的特殊權限帳戶的完整性。 網路攻擊者通常會以這些帳戶，才能存取資料和使用認證竊取攻擊，例如系統的特殊權限存取的其他項目[Pass-雜湊與傳遞票證](https://www.microsoft.com/pth)。

保護特殊權限的存取積極的攻擊者會要求您執行完整且深思熟慮的方法來隔絕這些系統風險。

## <a name="what-are-privileged-accounts"></a>特殊權限的帳戶有哪些？

討論如何保護其安全之前可讓定義特殊權限的帳戶。

特殊權限的帳戶，例如 Active Directory 網域服務的系統管理員會有直接或間接存取大部分或所有的資產在 IT 組織，使這些帳戶的洩漏大量商務風險。

## <a name="why-securing-privileged-access-is-important"></a>保護特殊權限的存取為何如此重要？

特殊權限的存取，例如 Active Directory (AD) 來快速存取所有組織目標資料的系統上的網路攻擊者焦點。 傳統的安全性方法著重於網路和防火牆作為主要安全性周邊，但網路安全性的效能明顯降低兩個趨勢：

* 組織要裝載資料，而且雲端服務，在行動裝置企業版裝置，例如行動電話和平板電腦的電腦上的傳統網路界限之外的資源，並攜帶您自己的裝置 (BYOD)
* 攻擊者已透過網路釣魚和其他網路與電子郵件攻擊，取得網路界限內部工作站存取權，展現出一致且持續的攻擊能力。

這些因素會使得建置出驗證和授權的最新的安全性周邊身分識別的控制項，加上的傳統網路周邊策略必須顧及。 的安全性周邊被定義為一組一致的資產之間的潛在威脅，這些控制項。 特殊權限的帳戶可有效控管的這個新的安全性周邊因此請務必保護特殊權限的存取。

![此圖顯示組織的身分識別層級](../media/securing-privileged-access/PAW_LP_Fig2.JPG)

取得系統管理帳戶控制權的攻擊者可以使用這些權限，來增加其影響的目標組織如下所述：

![此圖顯示取得系統管理帳戶控制權的攻擊者可以使用那些權限，靠犧牲目標組織的代價來追求其個人目的](../media/securing-privileged-access/PAW_LP_Fig3.JPG)

下圖說明兩個路徑：

* 完成 「 藍色 」 路徑的標準使用者帳戶用於資源，例如電子郵件和網頁瀏覽和每日工作的非特殊權限存取的位置。

   > [!NOTE]
   > 稍後所述的藍色路徑項目表示的系統管理帳戶以外的廣泛環境保護。

* 「 紅色 」 的路徑，特殊權限的存取發生強行寫入的裝置，以減少網路釣魚和其他網路與電子郵件的攻擊的風險。

![此圖顯示藍圖，以隔離特殊權限的存取工作從高風險標準使用者工作，例如網頁瀏覽與存取電子郵件所建立的個別 「 路徑 」 進行管理](../media/securing-privileged-access/PAW_LP_Fig4.JPG)

## <a name="securing-privileged-access-roadmap"></a>保護特殊權限的存取計劃

藍圖的設計目的，若要充分使用的 Microsoft 技術，您已經部署，充分利用雲端技術來加強安全性，並整合您可能已經部署任何第 3 方安全性工具。

Microsoft 建議的藍圖分成 3 個階段：

* [第 1 階段：前 30 天內]()
   * 使用有意義的正面影響的快速 wins。
* [第 2 階段：90 天]()
   * 重要的累加增強功能。
* [第 3 階段：進行中]()
   * 改進安全性和 sustainment。

藍圖已根據我們對這些攻擊的經驗和解決方案的實作經驗，優先排定最有效、最快速的實作。 

Microsoft 建議您遵循這份藍圖來保護特殊權限存取以對抗積極的攻擊者。 您可能需要調整這份藍圖以容納您現有的功能以及您組織中的特定需求。

> [!NOTE]
> 保護特殊權限存取需要更廣泛的項目，包括技術元件 (主機防禦、帳戶保護、身分識別管理等) 以及處理程序的變更，和系統管理實務與知識。 藍圖的時間表為估計值，並且依我們的經驗與客戶實作為根據。 根據您的環境與變更管理處理程序的複雜程度，組織中的持續時間會有所不同。

## <a name="phase-1-quick-wins-with-minimal-operational-complexity"></a>階段 1：最少的操作複雜性的快速 wins

計劃的階段 1 著重在快速減輕受到最常用的攻擊技術的認證竊取和濫用。 第 1 階段設計為在大約 30 天內實作，並會在此圖中所述：

![第 1 階段圖表：1. 另一個系統管理員和使用者帳戶、 2。 Just-in-time 3 次本機系統管理員密碼。 系統管理員工作站的階段 1，4。 身分識別攻擊偵測](../media/securing-privileged-access/PAW_LP_Fig6.JPG)

### <a name="1-separate-accounts"></a>1.不同的帳戶

為了杜絕網際網路風險 (網路釣魚攻擊、 網頁瀏覽) 從特殊權限存取帳戶，請為所有具備特殊權限存取的人員建立專用的帳戶。 系統管理員應該不會瀏覽網頁，檢查其電子郵件，以及進行日常的產能的工作，使用具有高度權限的帳戶。 詳細資訊，在此可以找到一節[分隔系統管理帳戶](securing-privileged-access-reference-material.md#separate-administrative-accounts)的參考文件。

請遵循本文中的指導方針[在 Azure AD 中管理緊急存取帳戶](/azure/active-directory/users-groups-roles/directory-emergency-access)建立至少兩個緊急存取帳戶，永久指派的系統管理員權限，在這兩個您內部部署 AD 與 Azure AD 環境. 這些帳戶是僅供使用，無法執行必要的工作例如，在傳統的系統管理員帳戶時的災害案例。

### <a name="2-just-in-time-local-admin-passwords"></a>2.Just-in-time 時間本機系統管理員密碼

若要減輕攻擊者竊取本機 SAM 資料庫中的本機系統管理員帳戶密碼雜湊，並濫用它攻擊其他電腦的風險，組織應該確定每台機器有唯一的本機系統管理員密碼。 本機系統管理員密碼解決方案 (LAPS) 工具可以在每一個工作站上設定唯一的隨機密碼，伺服器將其儲存在 Active Directory (AD) 保護的 ACL。 只有符合資格授權的使用者可讀取，或要求的本機系統管理員帳戶密碼重設。 您可以取得用於工作站和伺服器從 LAPS [Microsoft 下載中心取得](http://Aka.ms/LAPS)。

其他指導方針操作環境使用 LAP 和 Paw 可以找到一節[乾淨來源準則為基礎的操作標準](securing-privileged-access-reference-material.md#operational-standards-based-on-clean-source-principle)。

### <a name="3-administrative-workstations"></a>3.系統管理工作站

作為 Azure Active Directory 與傳統內部部署 Active Directory 系統管理員權限的使用者初始的安全性措施，請確定它們使用設定的 Windows 10 裝置[高度安全的 Windows 標準10 的裝置](/windows-hardware/design/device-experiences/oem-highly-secure)。 

### <a name="4-identity-attack-detection"></a>4.身分識別攻擊偵測

[Azure 進階威脅防護 (ATP)](/azure-advanced-threat-protection/what-is-atp)是雲端為基礎的安全性解決方案，以識別、 偵測到，並可協助您調查進階的威脅、 遭入侵的身分識別及導向您在內部使用的惡意內部人士動作Directory 環境。

## <a name="phase-2-significant-incremental-improvements"></a>第 2 階段：重要的漸進式增強

第 2 階段完成第 1 階段中的工作為基礎，而且是大約 90 天內完成。 此階段的步驟會在此圖表中說明︰

![第 2 階段圖表：1. Windows hello 企業版 / MFA，2。 PAW 首度發行、 3。 Just-in-time 4 的時間權限。 Credential Guard，5。 認證外洩，6。 橫向移動的弱點可能會偵測](../media/securing-privileged-access/PAW_LP_Fig7.JPG)

### <a name="1-require-windows-hello-for-business-and-mfa"></a>1.需要 Windows Hello 為商務和 MFA

系統管理員可以受益於與 Windows hello 企業版相關的使用便利性。 系統管理員可以取代他們的電腦上的增強式雙重要素驗證的複雜密碼。 攻擊者必須同時裝置和生物識別資訊或 PIN，才能存取該員工的不知情的情況下更為困難。 關於 Windows Hello 企業和首度發行的路徑的更多詳細資料可在發行項[Windows Hello for Business 概觀](/windows/security/identity-protection/hello-for-business/hello-overview)

使用 Azure MFA 的 Azure AD 中，為您的系統管理員帳戶啟用多重要素驗證 (MFA)。 在啟用最小[基準保護條件式存取原則](/azure/active-directory/conditional-access/baseline-protection#require-mfa-for-admins)可以找到 Azure Multi-factor Authentication 的詳細資訊，在本文中[部署雲端式 Azure Multi-factor Authentication](/azure/active-directory/authentication/howto-mfa-getstarted)

### <a name="2-deploy-paw-to-all-privileged-identity-access-account-holders"></a>2.將 PAW 部署到所有的特殊權限的身分識別存取帳戶持有者

繼續從電子郵件、 瀏覽網頁及其他非系統管理工作中找到的威脅將特殊權限的帳戶的程序，您應該實作專用的特殊權限存取工作站 (PAW) 具有特殊權限的存取權的所有人員的您組織的資訊系統。 PAW 部署的其他指引，請參閱文章[特殊權限存取工作站](privileged-access-workstations.md#paw-phased-implementation)。

### <a name="3-just-in-time-privileges"></a>3.只是在時間的權限

若要降低權限的曝光時間並增加使用的可見性，提供 just-in-time (JIT) 使用如下所示的其中一個適當解決方案或其他第三方解決方案的權限：

* 針對 Active Directory 網域服務 (AD DS)，請使用 Microsoft Identity Manager (MIM) 的[特殊權限存取管理員 (PAM)](/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services) 功能。
* 針對 Azure Active Directory，請使用 [Azure AD 特殊權限身分識別管理 (PIM)](/azure/active-directory/privileged-identity-management/pim-deployment-plan) 功能。

### <a name="4-enable-windows-defender-credential-guard"></a>4.啟用 Windows Defender Credential Guard

啟用 Credential Guard 可協助保護 NTLM 密碼雜湊、 Kerberos 票證授權票證，並做為網域認證的應用程式所儲存的認證。 這項功能有助於防止認證竊取攻擊，例如傳遞-雜湊或傳遞票證藉由增加在環境中使用遭竊的認證進行樞紐分析的困難度。 Credential Guard 的運作方式以及如何部署的詳細資訊可在發行項[保護衍生的網域認證，與 Windows Defender Credential Guard](/windows/security/identity-protection/credential-guard/credential-guard)。

### <a name="5-leaked-credentials-reporting"></a>5.報告的認證外洩

「 每一天，Microsoft 會分析超過 6.5 兆個訊號識別新興的威脅及保護客戶以 「- [Microsoft 的數字](https://news.microsoft.com/bythenumbers/cyber-attacks)

啟用 Microsoft Azure AD Identity Protection，以報告認證外洩的使用者，以便予以修復。 [Azure AD Identity Protection](/azure/active-directory/identity-protection/index)可以用來協助組織保護雲端和混合式環境的威脅。

### <a name="6-azure-atp-lateral-movement-paths"></a>6.Azure ATP 橫向移動路徑

請確定權限存取帳戶持有者會使用其 PAW 系統管理目的在於遭入侵的非特殊權限帳戶無法存取特殊權限的帳戶透過認證竊取攻擊，例如傳遞-雜湊或傳遞票證。 [Azure ATP 橫向移動路徑 (LMPs)](/azure-advanced-threat-protection/use-case-lateral-movement-path)提供容易了解報告來識別可能危及開啟特殊權限的帳戶。

## <a name="phase-3-security-improvement-and-sustainment"></a>第 3 階段：改進安全性和 sustainment

計劃的第 3 階段為基礎來鞏固安全性現況的階段 1 和 2 中所採取的步驟。 此圖表會以視覺化方式說明第 3 階段：

![第 3 階段：1. 檢閱 RBAC，2。 減少受攻擊面，3。 整合 SEIM，4 中的記錄檔。 認證外洩的自動化](../media/securing-privileged-access/PAW_LP_Fig8.JPG)

這些功能會建置在上一個階段的步驟，並將您的防禦措施移到更主動的防護。 這個階段中有任何特定的時間軸，而且可能需要較長的時間實作根據您針對個別組織。

### <a name="1-review-role-based-access-control"></a>1.檢閱以角色為基礎的存取控制

使用三個階層式的模型一文所述[Active Directory 系統管理層模型](securing-privileged-access-reference-material.md)、 檢閱，並確保較低層系統管理員不需要系統管理存取權較高層資源 （群組成員資格的 Acl使用者帳戶等.)。

### <a name="2-reduce-attack-surfaces"></a>2.減少受攻擊面

強化您的身分識別工作負載，包括網域、 網域控制站，ADFS，與 Azure AD Connect 為危害這些系統的其中一個可能會導致您的組織中的其他系統遭到入侵。 發行項[減少 Active Directory 的攻擊面](../ad-ds/plan/security-best-practices/reducing-the-active-directory-attack-surface.md)並[五個步驟，保護您的身分識別基礎結構](/azure/security/azure-ad-secure-steps)提供指導方針來保護您的內部部署和混合式身分識別環境。

### <a name="3-integrate-logs-with-siem"></a>3.將記錄與 SIEM 整合

將記錄整合到集中式的 SIEM 工具可協助您分析、 偵測及回應安全性事件的組織。 發行項[監視的洩露跡象的 Active Directory](../ad-ds/plan/security-best-practices/monitoring-active-directory-for-signs-of-compromise.md)和[附錄 l:要監視的事件](../ad-ds/plan/appendix-l--events-to-monitor.md)應該監視您的環境中的事件提供指引。

這是一部分的更多規劃彙總，因為建立，並調整警示中的安全性資訊及事件管理 (SIEM) 需要技術熟練的分析師 （不同於 30 天計劃包括從 方塊中的警示中的 Azure ATP)

### <a name="4-leaked-credentials---force-password-reset"></a>4.認證外洩-強制密碼重設

繼續增強您的安全性狀態，藉由啟用 Azure AD Identity Protection 到密碼有可能遭到入侵時，會自動強制執行密碼重設。 本指南的文章中找到[使用觸發程序的多重要素驗證和密碼變更的風險事件](/azure/active-directory/authentication/tutorial-risk-based-sspr-mfa)說明如何啟用此使用條件式存取原則。

## <a name="am-i-done"></a>我完成了嗎？

簡短的答案是沒有。

壞人永遠不會停止，因此都不可以。 這份藍圖可協助保護貴組織目前已知的威脅，因為攻擊者會不斷地發展並轉移。 我們建議您檢視為進行中的程序著重於引發成本，並降低其成功率的對手您環境的安全性。

雖然不是唯一的保護特殊權限的存取貴組織的安全性計畫部分是您的安全性策略的重要元件。
