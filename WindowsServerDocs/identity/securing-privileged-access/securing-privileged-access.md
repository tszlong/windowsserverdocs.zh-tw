---
title: 保護特殊權限存取
description: 保護特殊許可權存取的階段式方法
ms.prod: windows-server
ms.topic: conceptual
ms.assetid: f5dec0c2-06fe-4c91-9bdc-67cc6a3ede60
ms.date: 02/25/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: mas
ms.openlocfilehash: e6ff22d0563fa11aa633004966b2cd2648ba5877
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357699"
---
# <a name="securing-privileged-access"></a>保護特殊權限存取

>適用於：Windows Server

保護特殊權限存取是針對現代組織中的企業資產建立安全性保證時重要的第一個步驟。 IT 組織中大部分或所有商務資產的安全性，取決於用來管理、管理和開發之特殊許可權帳戶的完整性。 網路攻擊者通常會以這些帳戶和特殊許可權存取的其他元素為目標，以取得使用認證竊取攻擊（例如[傳遞雜湊和傳遞票證](https://www.microsoft.com/pth)）的資料和系統存取權。

保護已決定敵人的特殊許可權存取，需要您採取完整且謹慎的方法，將這些系統與風險隔離。

## <a name="what-are-privileged-accounts"></a>什麼是特殊許可權帳戶？

在討論如何保護它們之前，請先定義特殊許可權帳戶。

許可權帳戶（例如 Active Directory Domain Services 的系統管理員可直接或間接存取 IT 組織中的大部分或所有資產，因而危及這些帳戶的商業風險。

## <a name="why-securing-privileged-access-is-important"></a>為何要保護特殊許可權存取是很重要的？

網路攻擊者著重于 Active Directory （AD）等系統的特殊許可權存取，以快速存取所有以資料為目標的組織。 傳統的安全性方法將重點放在網路和防火牆作為主要的安全性周邊，但網路安全性的效益已大幅降低兩個趨勢：

* 組織會將資料和資源裝載在行動企業電腦上的傳統網路界限外，例如行動電話和平板電腦、雲端服務和攜帶您自己的裝置（BYOD）
* 攻擊者已透過網路釣魚和其他網路與電子郵件攻擊，取得網路界限內部工作站存取權，展現出一致且持續的攻擊能力。

除了傳統的網路周邊策略之外，這些因素還需要在驗證和授權身分識別控制之外，建立現代化的安全性周邊。 這裡的安全性周邊定義為資產之間的一組一致的控制項，以及它們的威脅。 特殊許可權帳戶可有效控制這個新的安全性周邊，因此保護特殊許可權存取是非常重要的。

![此圖顯示組織的身分識別層級](../media/securing-privileged-access/PAW_LP_Fig2.JPG)

取得系統管理帳戶控制權的攻擊者，可以使用這些許可權來增加對目標群組織的影響，如下所示：

![此圖顯示取得系統管理帳戶控制權的攻擊者可以使用那些權限，靠犧牲目標組織的代價來追求其個人目的](../media/securing-privileged-access/PAW_LP_Fig3.JPG)

下圖描述兩個路徑：

* 「藍色」路徑，其中標準使用者帳戶用於對資源的非特殊許可權存取，例如電子郵件、網頁流覽和每天工作完成。

   > [!NOTE]
   > 後面所述的藍色路徑專案指出超出系統管理帳戶的廣泛環境保護。

* 「紅色」路徑，其中會在強化的裝置上進行特殊許可權存取，以降低網路釣魚和其他網路和電子郵件攻擊的風險。

![此圖顯示藍圖所建立的個別「路徑」，以隔離特殊許可權存取工作和高風險標準使用者工作，例如網頁流覽和存取電子郵件](../media/securing-privileged-access/PAW_LP_Fig4.JPG)

## <a name="securing-privileged-access-roadmap"></a>保護特殊許可權存取藍圖

該藍圖的設計目的是要充分利用您已部署的 Microsoft 技術，利用雲端技術來加強安全性，並整合您可能已經部署的任何協力廠商安全性工具。

Microsoft 建議的藍圖分成3個階段：

* [第1階段：前30天]()
   * 以有意義的正面影響快速獲勝。
* [第2階段：90天]()
   * 大幅增加的改善。
* [第3階段：漸進]()
   * 安全性改進和 sustainment。

藍圖已根據我們對這些攻擊的經驗和解決方案的實作經驗，優先排定最有效、最快速的實作。 

Microsoft 建議您遵循這份藍圖來保護特殊權限存取以對抗積極的攻擊者。 您可能需要調整這份藍圖以容納您現有的功能以及您組織中的特定需求。

> [!NOTE]
> 保護特殊權限存取需要更廣泛的項目，包括技術元件 (主機防禦、帳戶保護、身分識別管理等) 以及處理程序的變更，和系統管理實務與知識。 藍圖的時間表為估計值，並且依我們的經驗與客戶實作為根據。 根據您的環境與變更管理處理程序的複雜程度，組織中的持續時間會有所不同。

## <a name="phase-1-quick-wins-with-minimal-operational-complexity"></a>階段 1：最少操作複雜度的快速 wins

藍圖的第1階段著重在快速緩和認證竊取和濫用最常使用的攻擊技巧。 第1階段是設計為在大約30天內執行，並在此圖中說明：

![第1階段圖表：1. 區分系統管理員和使用者帳戶，2。 及時的本機系統管理員密碼，3。 系統管理工作站階段1、4。 身分識別攻擊偵測](../media/securing-privileged-access/PAW_LP_Fig6.JPG)

### <a name="1-separate-accounts"></a>1.個別帳戶

若要協助從特殊許可權存取帳戶來區分網際網路風險（網路釣魚攻擊、網頁流覽），請為具有特殊許可權存取權的所有人員建立專用帳戶。 系統管理員不應流覽網頁、檢查其電子郵件，以及使用高許可權帳戶來執行日常生產力工作。 如需詳細資訊，請參閱參考檔的[個別系統管理帳戶](securing-privileged-access-reference-material.md#separate-administrative-accounts)一節。

遵循在[Azure AD 中管理緊急存取帳戶](/azure/active-directory/users-groups-roles/directory-emergency-access)一文中的指導方針，在您的內部部署 AD 和 Azure AD 環境中建立至少兩個緊急存取帳戶，並具有永久指派的系統管理員許可權。 這些帳戶僅供傳統系統管理員帳戶無法執行必要工作時使用，例如在發生嚴重損壞的情況下。

### <a name="2-just-in-time-local-admin-passwords"></a>2.及時的本機系統管理員密碼

為了降低敵人從本機 SAM 資料庫竊取本機系統管理員帳戶密碼雜湊，並濫用它來攻擊其他電腦的風險，組織應確保每部電腦都有唯一的本機系統管理員密碼。 本機系統管理員密碼解決方案（LAPS）工具可以在每部工作站上設定唯一的隨機密碼，而伺服器則會將它們儲存在 ACL 所保護的 Active Directory （AD）中。 只有合格的授權使用者可以讀取或要求重設這些本機系統管理員帳戶密碼。 您可以從[Microsoft 下載中心](http://Aka.ms/LAPS)取得要在工作站和伺服器上使用的 LAPS。

如需使用 LAPS 和 Paw 操作環境的其他指引，請參閱[根據乾淨來源準則的操作標準](securing-privileged-access-reference-material.md#operational-standards-based-on-clean-source-principle)一節。

### <a name="3-administrative-workstations"></a>3.系統管理工作站

做為具有 Azure Active Directory 和傳統內部部署 Active Directory 系統管理許可權之使用者的初始安全性措施，請確定他們使用已設定為[高度安全 Windows 10 裝置之標準的 Windows 10 裝置](/windows-hardware/design/device-experiences/oem-highly-secure). 特殊許可權的系統管理員帳戶不應該是系統管理工作站的本機管理員群組成員。  需要對工作站進行設定變更時，可以利用透過使用者存取控制（UAC）提升許可權。  此外，Windows 10 安全性基準應套用至工作站，以進一步強化裝置。

### <a name="4-identity-attack-detection"></a>4.身分識別攻擊偵測

[Azure 進階威脅防護（ATP）](/azure-advanced-threat-protection/what-is-atp)是以雲端為基礎的安全性解決方案，可識別、偵測並協助您調查在內部部署引導的先進威脅、遭盜用的身分識別，以及惡意的內部人員動作 Active Directory環境.

## <a name="phase-2-significant-incremental-improvements"></a>第2階段:大幅增加的改良功能

第2階段是以第1階段完成的工作為基礎，設計為在大約90天內完成。 此階段的步驟會在此圖表中說明︰

![第2階段圖表：1. Windows Hello 企業版/MFA，2。 PAW 首度發行，3。 僅限時間許可權，4。 Credential Guard，5。 洩漏的認證，6。 橫向移動弱點偵測](../media/securing-privileged-access/PAW_LP_Fig7.JPG)

### <a name="1-require-windows-hello-for-business-and-mfa"></a>1.需要 Windows Hello 企業版和 MFA

系統管理員可以受益于與 Windows Hello 企業版相關聯的使用便利性。 系統管理員可以在其電腦上以強式雙因素驗證取代他們的複雜密碼。 攻擊者必須同時擁有裝置和生物識別資訊或 PIN，才能在沒有員工知識的情況下取得存取權。 如需 Windows Hello 企業版和要推出的路徑的詳細資訊，請參閱[Windows Hello 企業版總覽](/windows/security/identity-protection/hello-for-business/hello-overview)

在使用 Azure MFA 的 Azure AD 中，為您的系統管理員帳戶啟用多重要素驗證（MFA）。 若要深入瞭解[基準保護條件式存取原則](/azure/active-directory/conditional-access/baseline-protection#require-mfa-for-admins)，請參閱[部署雲端式 Azure 多因素驗證](/azure/active-directory/authentication/howto-mfa-getstarted)一文中的 azure 多因素驗證的詳細資訊。

### <a name="2-deploy-paw-to-all-privileged-identity-access-account-holders"></a>2.將 PAW 部署至所有特殊許可權身分識別存取帳戶持有者

繼續將特殊許可權帳戶與電子郵件、網頁流覽和其他非系統管理工作中所找到的威脅分開，您應該為所有具有特殊許可權存取權的人員，執行組織的資訊系統。 如需 PAW 部署的其他指引，請參閱特殊[許可權存取工作站](privileged-access-workstations.md#paw-phased-implementation)一文。

### <a name="3-just-in-time-privileges"></a>3.及時許可權

若要降低許可權的曝光時間，並增加其使用方式的可見度，請使用適當的解決方案（例如下列或其他協力廠商解決方案），以及時（JIT）提供許可權：

* 針對 Active Directory 網域服務 (AD DS)，請使用 Microsoft Identity Manager (MIM) 的[特殊權限存取管理員 (PAM)](/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services) 功能。
* 針對 Azure Active Directory，請使用 [Azure AD 特殊權限身分識別管理 (PIM)](/azure/active-directory/privileged-identity-management/pim-deployment-plan) 功能。

### <a name="4-enable-windows-defender-credential-guard"></a>4.啟用 Windows Defender Credential Guard

啟用 Credential Guard 有助於保護 NTLM 密碼雜湊、Kerberos 票證授權票證，以及應用程式儲存為網域認證的認證。 這項功能有助於防止認證竊取攻擊，例如傳遞雜湊或傳遞票證，方法是使用遭竊的認證來增加環境中的旋轉困難度。 有關 Credential Guard 如何運作以及如何部署的資訊，請參閱[使用 Windows Defender Credential Guard 保護衍生的網域認證](/windows/security/identity-protection/credential-guard/credential-guard)一文。

### <a name="5-leaked-credentials-reporting"></a>5.洩漏的認證報告

「每天，Microsoft 會分析6500000000000的信號，以識別新興威脅並保護客戶」- [Microsoft 依數位](https://news.microsoft.com/bythenumbers/cyber-attacks)

啟用 Microsoft Azure AD Identity Protection，以報告認證洩漏的使用者，讓您可以進行補救。 您可以利用[Azure AD Identity Protection](/azure/active-directory/identity-protection/index)來協助您的組織保護雲端和混合式環境免于遭受威脅。

### <a name="6-azure-atp-lateral-movement-paths"></a>6.Azure ATP 橫向移動路徑

請確定特殊許可權存取帳戶持有者僅使用其 PAW 進行系統管理，讓遭入侵的非特殊許可權帳戶無法透過認證竊取攻擊（例如傳遞雜湊或票證傳遞）取得特殊許可權帳戶的存取權。 [Azure ATP 橫向移動路徑（lmp）](/azure-advanced-threat-protection/use-case-lateral-movement-path)可讓您輕鬆瞭解報告，以識別特殊許可權帳戶可能會遭到入侵的地方。

## <a name="phase-3-security-improvement-and-sustainment"></a>第3階段:安全性改進和 sustainment

藍圖的第3階段是以階段1和2中所採取的步驟為基礎，以加強您的安全性狀態。 此圖以視覺化方式描述第3階段：

![第3階段:1. 回顧 RBAC，2。 減少受攻擊面，3。 將記錄與 SEIM （4）整合。 洩漏的認證自動化](../media/securing-privileged-access/PAW_LP_Fig8.JPG)

這些功能將根據先前階段中的步驟來建立，並將您的防禦措施移到更主動的狀態。 這個階段沒有特定的時間軸，而且可能需要較長的時間才能根據您的個別組織執行。

### <a name="1-review-role-based-access-control"></a>1.審查角色型存取控制

使用[Active Directory 系統管理層模型](securing-privileged-access-reference-material.md)一文中所述的三個階層式模型，檢查並確保較低層的系統管理員沒有較高層級資源（群組成員資格、使用者帳戶的 acl 等等）的系統管理存取權。

### <a name="2-reduce-attack-surfaces"></a>2.減少受攻擊面

強化您的身分識別工作負載（包括網域、網域控制站、ADFS 和 Azure AD Connect）會危害其中一個系統，可能會導致貴組織中的其他系統遭到入侵。 [減少 Active Directory 攻擊面](../ad-ds/plan/security-best-practices/reducing-the-active-directory-attack-surface.md)和[五個步驟來保護身分識別基礎結構](/azure/security/azure-ad-secure-steps)的文章提供保護內部部署和混合式身分識別環境的指導方針。

### <a name="3-integrate-logs-with-siem"></a>3.整合記錄與 SIEM

將登入整合到集中式 SIEM 工具，可協助您的組織分析、偵測及回應安全性事件。 [針對洩露](../ad-ds/plan/security-best-practices/monitoring-active-directory-for-signs-of-compromise.md)和[附錄 L Active Directory 監視的文章：監視](../ad-ds/plan/appendix-l--events-to-monitor.md)的事件會針對應該在您環境中監視的事件提供指引。

這是超越計畫的一部分，因為在安全性資訊和事件管理（SIEM）中匯總、建立和調整警示需要熟練的分析師（不同于30天計畫中的 Azure ATP，其中包含現成的警示）

### <a name="4-leaked-credentials---force-password-reset"></a>4.洩漏的認證-強制密碼重設

藉由讓 Azure AD Identity Protection 在懷疑密碼洩露時自動強制密碼重設，以繼續增強您的安全性狀態。 [使用風險事件來觸發多重要素驗證和密碼變更](/azure/active-directory/authentication/tutorial-risk-based-sspr-mfa)一文中的指導方針，說明如何使用條件式存取原則來啟用此功能。

## <a name="am-i-done"></a>我完成了嗎？

簡短的答案是「否」。

不良的人絕對不會停止，因此您也不能這麼做。 此藍圖可協助您的組織抵禦目前已知的威脅，因為攻擊者會不斷演進和轉移。 我們建議您將安全性視為持續性的程式，著重于提高成本，並降低以您的環境為目標之敵人的成功率。

雖然它不是貴組織安全性計畫的唯一部分，但保護特殊許可權存取是安全性策略的重要元件。
