---
title: 開始您的適用於 Windows Server 2016 的一般資料保護法規 (GDPR) 之旅
description: 使用這份文件深入了解 GDPR 是什麼，以及 Microsoft 提供哪些產品來協助您符合法規。
ms.technology: techgroup-security
ms.topic: article
ms.date: 09/25/2017
ms.author: nirb
author: nirb-ms
ms.openlocfilehash: 506cd5cb44d93c9d7d221917505f76a2c5625baa
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870546"
---
# <a name="beginning-your-general-data-protection-regulation-gdpr-journey-for-windows-server"></a>開始您的 Windows Server 一般資料保護規定（GDPR）旅程 

>適用於：Windows Server (半年度管道)、Windows Server 2016

本文提供 GDPR 的相關資訊，包括它是什麼，以及 Microsoft 提供哪些產品來協助您符合法規。

## <a name="introduction"></a>簡介
在 2018 年 5 月 25 日，歐洲隱私權法預定生效，為隱私權、安全性與合規性設定新的全球標準。

一般資料保護法規 (或 GDPR) 將從根本上保護和實現個人的隱私權。 GDPR 樹立嚴格的全球隱私權需求，規範如何管理及保護個人資料，同時尊重個人選擇—不論傳送資料的位置、處理方式或儲存方式。

Microsoft 以及我們的客戶現在正朝向達成 GDPR 的隱私權目標邁進。 Microsoft 相信隱私權是基本權利，且我們認為 GDPR 是釐清和實現個人隱私權的重要步驟。 但我們也承認 GDPR 將要求全球各地的組織進行重大變革。

我們已在以下文章中概述我們對 GDPR 的承諾，以及我們如何支援客戶：[透過 Microsoft Cloud 符合 GDPR](https://blogs.microsoft.com/on-the-issues/2017/02/15/get-gdpr-compliant-with-the-microsoft-cloud/#hv52B68OZTwhUj2c.99)部落格文章 (英文)，由我們的首席隱私官 [Brendon Lynch](https://blogs.microsoft.com/on-the-issues/author/brendonlynch/)撰寫；以及[透過一般資料保護法規履約承諾贏得您的信任](https://blogs.microsoft.com/on-the-issues/2017/04/17/earning-trust-contractual-commitments-general-data-protection-regulation/#6QbqoGWXCLavGM63.99)部落格文章 (英文)，由 Microsoft 公司副總裁兼副總法律顧問 [Rich Sauer](https://blogs.microsoft.com/on-the-issues/author/rsauer/) 撰寫。

雖然您的 GDPR 合規之旅看似困難重重，但我們會在這裡協助您。 如需 GDPR 特定資訊、我們的承諾，以及如何開始您的旅程，請造訪 [Microsoft 信任中心的 GDPR 部分](https://www.microsoft.com/en-us/trustcenter/privacy/gdpr)。

## <a name="gdpr-and-its-implications"></a>GDPR 和其影響
GDPR 是龐雜的法規，需要大幅改變您收集、使用及管理個人資料的方法。 Microsoft 長久以來一直協助客戶遵守複雜規範，現在要為 GDPR 做準備了，我們會是您的旅程最佳夥伴。

GDPR 針對向歐洲經濟共同體 (歐盟) 中的人民提供商品與服務，或是收集和分析歐盟居民相關資料的組織實施規則，不論這些企業的所在位置。 GDPR 的主要元素如下所示：

- **增強的個人隱私權權利。** 確保歐盟居民有權存取他們的個人資料、更正該資料中的錯誤、清除該資料、處理其個人資料以及移動資料，來加強歐盟居民的資料保護。

- **加強保護個人資料的責任。** 強化組織處理個人資料的責任，釐清責任歸屬以確保合規。

- **強制的個人資料缺口報告。** 控管個人資料的組織必須向其監管當局回報對個人權利和自由構成風險的個人資料洩露情形，不得無故拖延，且應盡可能在察覺洩漏情形的 72 小時以內回報。

正如你所預料，GDPR 可能對您的企業產生重大影響，可能會要求您更新隱私權原則、實施並加強資料保護控制和洩漏通知程序、部署高度透明的原則，並進一步投資 IT 和訓練。 Microsoft Windows 10 可協助您有效且有效率地處理其中的部分需求。

## <a name="personal-and-sensitive-data"></a>個人與機密資料
為了努力符合 GDPR，您必須了解法規如何定義個人和機密資料，以及這些定義如何關聯至您組織保有的資料。 根據該瞭解，您將能夠探索資料的建立、處理、管理及儲存位置。

GDPR 將個人資料認定為可關聯至已識別或可識別自然人的任何資訊。 這包括直接識別 (例如您的法定名稱) 和間接識別 (例如可確認資料提及的對象是您的特定資訊)。 GDPR 也釐清個人資料的概念，包含線上識別碼 (例如 IP 位址、行動裝置識別碼) 以及位置資料。

GDPR 引進了遺傳資料（例如，個人的 gene 序列）與生物特徵辨識資料的特定定義。 遺傳資料和生物識別資料，以及其他子類別的個人資料（個人資料會洩漏種族或種族來源、政治意見、宗教或哲學信念，或貿易等成員資格：關於健康狀態的資料，或有關的資料個人的性別生命或色情方向，會在 GDPR 下被視為機密的個人資料。 敏感性個人資料會受到加強的保護，而且通常需要個別的明確同意，以便處理這些資料。

### <a name="examples-of-info-relating-to-an-identified-or-identifiable-natural-person-data-subject"></a>已識別或可識別自然人的相關資訊範例 (資料主題)
這份清單提供 GDPR 規範的數個資訊類型範例。 並非完整的清單。

-   Name

-   身分證號碼 (例如 SSN)

-   位置資料 (例如住址)

-   線上識別碼 (例如電子郵件地址、螢幕名稱、IP 位址、裝置識別碼)

-   匿名資料 (例如使用金鑰識別個人身份)

-   基因資料 (例如來自個人的生物樣本)

-   生物特徵辨識資料 (例如指紋、臉部辨識)

## <a name="getting-started-on-the-journey-towards-gdpr-compliance"></a>開始 GDPR 合規之旅
由於要符合 GDPR 的待辦事項眾多，因此強烈建議您不要等到開始實施後才著手準備。 您應該現在就檢查您的隱私權和資料管理做法。 我們建議您將焦點放在四個主要步驟，來啟動 GDPR 相容性之旅：

-   **探索.** 找出您擁有哪些個人資料，以及存放位置。 

-   **管理.** 管理個人資料的使用與存取方式。

-   **禁止.** 建立安全性控制，以防止、偵測及因應弱點及資料洩漏。  

-   **彙報.** 對資料要求採取行動、回報資料洩漏，以及保留必要的文件。

    ![4 個主要 GDPR 步驟搭配運作的圖表](../media/GDPR-Windows-Server-Overview/gdpr-steps-diagram.png)

我們已經針對每個步驟，概述各種 Microsoft 解決方案提供的工具範例、資源和功能，可以用來協助您處理該步驟的需求。 雖然本文不是完整的「操作說明」指南，但我們已包含連結供您尋找更多詳細資料，而您可以在[Microsoft 信任中心的 GDPR 一節](https://www.microsoft.com/en-us/trustcenter/privacy/gdpr)中取得更多資訊。

## <a name="windows-server-security-and-privacy"></a>Windows Server 安全性與隱私權
GDPR 會要求您執行適當的技術和組織安全性措施，以保護個人資料和處理系統。 在 GDPR 的內容中，您的實體和虛擬伺服器環境可能會處理個人和敏感性資料。 處理可以表示任何作業或操作集，例如資料收集、儲存和抓取。

您滿足這項需求的能力，以及執行適當的技術安全性措施，必須反映出您在現今日益惡意的 IT 環境中所面臨的威脅。 現今的安全性威脅環境是一種積極且 tenacious 的威脅。 以往，惡意攻擊者主要將重點放在透過攻擊獲得社群認可，或者暫時使系統離線的快感。 從那時起，攻擊者的動機已轉移到金錢，包括保留裝置和資料 hostage，直到擁有者支付要求的 ransom 為止。

現代化攻擊越來越針對大規模竊取智慧財產權，以及會導致財務損失的目標系統效能降低，更遑論威脅全球個人、企業和國家/地區利益的網路恐怖主義。 這些攻擊者通常是訓練有素的個人和安全專家，其中有些人甚至受雇於擁有大量預算，以及似乎有無限制人力資源的民族國家/地區。 此類威脅，需要能夠應付這種挑戰的方法。

這些威脅不只是威脅到您掌控任何個人或機密資料的能力，也對您的整體企業造成重大風險。 請考慮 McKinsey、Ponemon 研究所、Verizon 和 Microsoft 的最新資料：

- GDPR 期待您回報的資料洩漏類型平均成本為 350 萬美元。

- 這些破壞事件的 63% 涉及弱式或遭竊密碼，GDPR 也期待您應能妥善處理。

- 每天都有超過 300,000 個新惡意程式碼樣本建立和散播，讓您的保護資料工作更加困難重重。

就像最近的勒索軟體攻擊一樣，一旦呼叫了網際網路的黑色災禍，攻擊者就會在更大的目標之後進行，以支付更多費用，而且可能會造成嚴重的後果。 GDPR 包含的是讓您的系統（包括桌上型和膝上型電腦）確實包含個人和機密資料的懲罰。

有兩個主要原則引導並繼續引導 Windows 的開發：

- **安全性。** 我們的軟體和服務存放區代表我們的客戶所提供的資料，應受到保護，避免遭到傷害，並且只能以適當的方式使用或修改。 開發人員可以輕鬆地瞭解安全性模型，並將其打造到其應用程式中。

- **隱私.** 使用者應該控制其資料的使用方式。 使用者應清楚瞭解資訊的使用原則。 使用者應可控制何時及何時收到可充分利用其時間的資訊。 使用者可以輕鬆地指定適當的資訊使用方式，包括控制他們傳送的電子郵件使用方式。

Microsoft 針對這些原則保持堅定不移，最近由 Microsoft 的 CEO，Satya Nadella， 

> 「_隨著世界持續改變和業務需求演進，有些東西是一致的：客戶的安全性和隱私權需求。_ 」

當您使用 GDPR 時，瞭解實體和虛擬伺服器在建立、存取、處理、儲存和管理資料時的角色，可能會在 GDPR 下符合個人和潛在敏感性資料的重要性。 Windows Server 提供的功能可協助您遵守 GDPR 需求，以執行適當的技術和組織安全性措施來保護個人資料。

Windows Server 2016 的安全性狀態不是一個螺栓，這是一種架構原則。 而且，在四個主體中最能瞭解：

- **禁止.** 預防性措施的持續焦點和創新;封鎖已知的攻擊和已知的惡意程式碼。

- **是否.** 完整的監視工具，可協助您更快找出異常狀況並回應攻擊。

- **作出.** 領先的回應和復原技術，以及深入的諮詢專長。

- **查出.** 隔離作業系統元件和資料秘密、限制系統管理員許可權，以及嚴格測量主機健全狀況。

在 Windows Server 中，您可以保護、偵測和防禦可能導致資料缺口的攻擊類型，大幅提升。 由於 GDPR 對於洩漏通知的嚴格需求，確保您的桌上型和膝上型電腦系統受到妥善保護，可以降低讓您面臨昂貴洩漏分析及通知的風險。

在接下來的章節中，您將瞭解 Windows Server 如何提供功能，使其符合 GDPR 合規性旅程的「保護」階段中的正好。 這些功能可分為三種保護案例：

- **保護您的認證並限制系統管理員許可權。** Windows Server 2016 可協助您執行這些變更，協助防止您的系統作為進一步入侵的啟動點。

- **保護作業系統以執行您的應用程式和基礎結構。** Windows Server 2016 提供保護層，協助防止外部攻擊者執行惡意軟體或利用弱點。

- **安全虛擬化。** Windows Server 2016 使用受防護的虛擬機器和受防護網狀架構，來啟用安全的虛擬化。 這可協助您在網狀架構中的受信任主機上加密和執行虛擬機器，並更有效地保護它們免于遭受惡意攻擊。

以下詳細說明的這些功能會以特定 GDPR 需求的參考為基礎，建立在先進的裝置保護之上，協助維護作業系統和資料的完整性與安全性。

GDPR 內的金鑰布建是依設計和預設的資料保護，並協助您符合此布建的能力是 Windows 10 中的功能，例如 BitLocker 裝置加密。 BitLocker 使用可信賴平臺模組（TPM）技術，其提供以硬體為基礎的安全性相關功能。 這個加密處理器晶片包含多個實體安全性機制，讓它能夠防篡改，而且惡意軟體無法篡改 TPM 的安全性功能。

此晶片包含多個實體安全性機制，使它具備防竄改功能，在 TPM 安全性功能的加持下，惡意程式碼軟體便無法進行竄改。 使用 TPM 技術的一些主要優點是您可以：

-   產生、儲存密碼編譯金鑰及限制此金鑰的使用。

-   使用 tpm 的唯一 RSA 金鑰來進行平臺裝置驗證，並將其燒錄到自己。

-   藉由進行和儲存安全性測量，協助確保平台完整性。

與作業中不導致資料洩漏的其他相關進階裝置保護包含 Windows 信任式開機，可確保惡意程式碼無法在系統防禦之前啟動，協助維護系統完整性。

## <a name="windows-server-supporting-your-gdpr-compliance-journey"></a>Windows Server：支援您的 GDPR 合規性旅程
Windows Server 內的主要功能可協助您有效率且有效地執行 GDPR 所需的安全性和隱私權機制，以符合規範。 雖然使用這些功能並不保證您的相容性，但它們會支援您的工作。

伺服器作業系統位於組織基礎結構的策略性層，又新的機會，從可能竊取資料並中斷業務的攻擊中，建立保護層。 在伺服器層級的 IT 基礎結構中，GDPR 的重要層面（例如隱私權）、資料保護和存取控制都必須在您的 IT 基礎結構內解決。

Windows Server 2016 致力於協助保護身分識別、作業系統和虛擬化層級，協助封鎖用來對系統造成非法存取的常見攻擊媒介：竊取的認證、惡意程式碼和遭入侵的虛擬化網狀架構。 除了降低業務風險之外，Windows Server 2016 內建的安全性元件也有助於滿足重要政府與業界安全性法規的合規性需求。 

這些身分識別、作業系統和虛擬化保護可讓您更有效地保護在任何雲端中執行 Windows Server 的資料中心，並限制攻擊者洩露認證、啟動惡意程式碼，並在您的網路. 同樣地，當部署為 Hyper-v 主機時，Windows Server 2016 會透過受防護的虛擬機器和分散式防火牆功能，為您的虛擬化環境提供安全性保證。 在 Windows Server 2016 中，伺服器作業系統會成為您資料中心安全性的積極參與者。

### <a name="protect-your-credentials-and-limit-administrator-privileges"></a>保護您的認證並限制系統管理員許可權 
控制個人資料的存取權，以及處理該資料的系統，是 GDPR 的區域，具有特定需求，包括系統管理員的存取權。 特殊許可權身分識別是任何具有較高許可權的帳戶，例如，屬於網域系統管理員、Enterprise Administrators、local Administrators 或甚至 Power Users 群組成員的使用者帳戶。 這類身分識別也可以包含直接被授與許可權的帳戶，例如執行備份、關閉系統，或在 [本機安全性原則] 主控台的 [使用者權限指派] 節點中所列的其他許可權。

以一般存取控制原則和 GDPR，您必須保護這些特殊許可權的身分識別，以免受到潛在攻擊者的危害。 首先，請務必瞭解身分識別遭到入侵的方式;然後您可以規劃防止攻擊者取得這些特殊許可權身分識別的存取權。

#### <a name="how-do-privileged-identities-get-compromised"></a>特殊許可權身分識別遭到入侵的方式為何？
當組織沒有保護它們的指導方針時，特殊許可權身分識別可能會遭到入侵。 範例如下所示：

- **比所需更多的許可權。** 其中一個最常見的問題是，使用者所擁有的許可權比執行其工作功能所需的還要多。 例如，管理 DNS 的使用者可能是 AD 系統管理員。 這通常是為了避免需要設定不同的系統管理層級而完成。 不過，如果這類帳戶遭到入侵，攻擊者會自動擁有較高的許可權。

- **以較高的許可權持續登入。** 另一個常見的問題是，具有較高許可權的使用者可以無限期地使用它。 這對使用特殊許可權帳戶登入桌上型電腦、保持登入，以及使用特殊許可權帳戶流覽 web 及使用電子郵件（一般 IT 工作職能）的 IT 專業人員而言，非常常見。 無限制的許可權帳戶持續時間會使帳戶更容易遭受攻擊，並增加帳戶遭到入侵的機率。

- **社交工程研究。** 大部分的認證威脅一開始先研究組織，然後透過社交工程進行。 例如，攻擊者可能會執行電子郵件網路釣魚攻擊，以洩露可存取組織網路的合法帳戶（但不一定是提高許可權的帳戶）。 然後，攻擊者會使用這些有效的帳戶在您的網路上執行額外的研究，以及識別可執行系統管理工作的特殊許可權帳戶。 

- **利用具有更高許可權的帳戶。** 即使是在網路中使用一般且未提高許可權的使用者帳戶，攻擊者還是可以存取具有較高許可權的帳戶。 其中一個較常見的方法，就是使用傳遞雜湊或傳遞權杖攻擊。 如需有關傳遞雜湊和其他認證竊取技術的詳細資訊，請參閱 <<c0>傳遞雜湊（PtH）頁面上的資源。

還有其他方法可供攻擊者用來識別及危害特殊許可權身分識別（每天都會建立新的方法）。 因此，您必須建立作法，讓使用者以最低許可權帳戶登入，以降低攻擊者取得特殊許可權身分識別的能力。 下列各節概述 Windows Server 可減輕這些風險的功能。

#### <a name="just-in-time-admin-jit-and-just-enough-admin-jea"></a>及時管理員（JIT）且只有足夠的系統管理員（JEA）
雖然保護傳遞雜湊或傳遞票證的攻擊很重要，但系統管理員認證仍然可以透過其他方式遭竊，包括社交工程、不滿的員工和暴力密碼破解。 因此，除了盡可能隔離認證以外，您也想要一種方法來限制系統管理員層級許可權的範圍，以免遭到入侵。

目前，太多系統管理員帳戶的許可權較高，即使他們只有一個責任區域。 例如，需要一組非常精簡的許可權來管理 DNS 伺服器的 DNS 系統管理員，通常會被授與網域系統管理員層級的許可權。 此外，由於這些認證會授與永久，因此可以使用的時間長度沒有限制。

具有不必要網域系統管理員層級許可權的每個帳戶，都會讓攻擊者暴露于尋求入侵認證的風險。 若要將攻擊的介面區最小化，您只想要提供系統管理員執行此作業所需的特定許可權集，而僅針對完成它所需的時間範圍。

只要使用足夠的系統管理和即時管理，系統管理員就可以在需要的確切時間範圍內，要求所需的特定許可權。 例如，DNS 系統管理員可以使用 PowerShell 來啟用足夠的系統管理，讓您建立一組可供 DNS 管理使用的有限命令。

如果 DNS 系統管理員需要更新其中一部伺服器，她會要求使用 Microsoft Identity Manager 2016 來管理 DNS 的存取權。 要求工作流程可以包含核准程式（例如雙因素驗證），這可能會在授與所要求的許可權之前，呼叫系統管理員的行動電話來確認她的身分識別。 授與之後，這些 DNS 許可權可讓您存取特定時間範圍內 DNS 的 PowerShell 角色。

如果 DNS 系統管理員的認證遭竊，請想像這種情況。 首先，由於認證沒有任何系統管理員許可權，因此攻擊者將無法存取 DNS 伺服器（或其他任何系統）來進行任何變更。 如果攻擊者嘗試要求 DNS 伺服器的許可權，則第二因素驗證會要求他們確認其身分識別。 由於攻擊者不可能有 DNS 系統管理員的行動電話，因此驗證會失敗。 這會鎖定系統的攻擊者，並警示 IT 組織該認證可能遭到入侵。

此外，許多組織會使用免費的[本機系統管理員密碼解決方案（LAPS）](http://aka.ms/laps) ，做為其伺服器和用戶端系統的簡單但功能強大的 JIT 管理機制。 LAPS 功能可讓您管理已加入網域之電腦的本機帳戶密碼。 密碼會儲存在 Active Directory （AD）中，並受到和存取控制清單（ACL）的保護，因此只有合格的使用者可以讀取或要求重設。

如《 [Windows 認證竊取緩和指南》](https://www.microsoft.com/en-us/download/confirmation.aspx?id=54095)所述， 

> 「犯罪罪犯用來執行認證竊取和重複使用攻擊的工具和技術，惡意攻擊者發現能夠更輕鬆地達成目標。_認證竊取通常依賴操作實務或使用者認證暴露，因此有效的緩和措施需要全面的方法來處理人員、程式和技術。此外，這些攻擊會依賴攻擊者竊取認證，然後再危害系統以擴充或保存存取權，因此組織必須藉由執行可防止攻擊者在遭入侵的網路。_ 」

Windows Server 的重要設計考慮是減輕認證竊取的風險，特別是衍生的認證。 Credential Guard 針對衍生的認證竊取和重複使用提供大幅改良的安全性，方法是在 Windows 中執行重大的架構變更，其設計目的是為了協助排除硬體隔離攻擊，而不只是嘗試防禦它們。

使用 Windows Defender Credential Guard、NTLM 和 Kerberos 衍生認證時，會使用虛擬化安全性來保護，而用於許多目標攻擊的認證竊取攻擊技巧和工具會遭到封鎖。 具有系統管理權限，在作業系統中執行的惡意程式碼，無法擷取受虛擬式安全性保護的密碼。 雖然 Windows Defender Credential Guard 是一種強大的緩和措施，但持續的威脅攻擊可能會轉移到新的攻擊技巧，而且您也應該納入 Device Guard （如下所述），以及其他安全性策略和架構。

#### <a name="windows-defender-credential-guard"></a>Windows Defender Credential Guard
Windows Defender Credential Guard 使用以虛擬化為基礎的安全性來隔離認證資訊，防止密碼雜湊或 Kerberos 票證遭到攔截。 它會使用全新的隔離本地安全機構（LSA）程式，而不是其他作業系統的存取權。 隔離的 LSA 所使用的所有二進位檔都會使用在受保護的環境中啟動之前已驗證的憑證進行簽署，讓傳遞雜湊類型的攻擊完全無效。

Windows Defender Credential Guard 使用：

- 以虛擬化為基礎的安全性（必要）。 也需要：

    - 64 位元 CPU

    - CPU 虛擬化延伸模組，加上擴充頁面資料表

    - Windows Hypervisor

- 安全開機 (必要)

- 個別或韌體 TPM 2.0 (最好能提供硬體繫結)

您可以使用 Windows Defender Credential Guard 來保護 Windows Server 2016 上的認證和認證衍生，以協助保護特殊許可權身分識別。 如需 Windows Defender Credential Guard 需求的詳細資訊，請參閱[使用 Windows Defender Credential Guard 保護衍生的網域認證](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard)。

#### <a name="windows-defender-remote-credential-guard"></a>Windows Defender 遠端認證防護
Windows Server 2016 和 Windows 10 年度更新版的 windows Defender 遠端認證防護也有助於保護具有遠端桌面連線的使用者認證。 過去，使用遠端桌面服務的任何人都必須登入其本機電腦，然後在對目的電腦執行遠端連線時，需要再次登入。 第二個登入會將認證傳遞至目的電腦，並將其公開至傳遞雜湊或傳遞票證攻擊。

使用 Windows Defender 遠端認證防護，Windows Server 2016 會針對遠端桌面會話執行單一登入，而不需要重新輸入您的使用者名稱和密碼。 相反地，它會利用您已經用來登入本機電腦的認證。 若要使用 Windows Defender 遠端認證防護，遠端桌面用戶端和伺服器必須符合下列需求：

- 必須聯結到 Active Directory 網域，而且位於相同的網域或具有信任關係的網域中。

- 必須使用 Kerberos 驗證。

- 必須至少執行 Windows 10 1607 版或 Windows Server 2016。  

- 需要遠端桌面傳統 Windows 應用程式。 遠端桌面通用 Windows 平臺應用程式不支援 Windows Defender 遠端認證防護。

您可以使用遠端桌面伺服器上的登錄設定，以及遠端桌面用戶端上的群組原則或遠端桌面連線參數，啟用 Windows Defender 遠端 Credential Guard。 如需啟用 Windows Defender 遠端認證防護的詳細資訊，請參閱[使用 Windows Defender 遠端認證防護來保護遠端桌面認證](https://docs.microsoft.com/windows/access-protection/remote-credential-guard)。 如同 Windows Defender Credential Guard，您可以使用 Windows Defender 遠端 Credential Guard 協助保護 Windows Server 2016 上的特殊許可權身分識別。

### <a name="secure-the-operating-system-to-run-your-apps-and-infrastructure"></a>保護作業系統以執行您的應用程式和基礎結構
防止網路威脅也需要尋找及封鎖惡意程式碼和攻擊，藉由破壞基礎結構的標準操作作法來取得控制。 如果攻擊者可以讓作業系統或應用程式以非預先決定且不可行的方式執行，它們可能會使用該系統來執行惡意的動作。 Windows Server 2016 提供防止外部攻擊者執行惡意軟體或利用弱點的保護層。 作業系統會藉由警示系統管理員活動來保護基礎結構和應用程式的作用中角色，以指出系統已遭入侵。

#### <a name="windows-defender-device-guard"></a>Windows Defender Device Guard
Windows Server 2016 包含 Windows Defender Device Guard，以確保只有受信任的軟體可以在伺服器上執行。 使用虛擬化安全性，它可以根據組織的原則來限制可在系統上執行的二進位檔。 如果除了指定的二進位檔嘗試執行以外的任何專案，Windows Server 2016 會封鎖它並記錄失敗的嘗試，讓系統管理員可以看到可能發生的漏洞。 缺口通知是 GDPR 合規性需求的重要部分。

Windows Defender Device Guard 也會與 PowerShell 整合，讓您可以授權哪些腳本可在您的系統上執行。 在舊版的 Windows Server 中，系統管理員只要從程式碼檔案刪除原則，就可以略過程式碼完整性強制執行。 使用 Windows Server 2016 時，您可以設定由組織簽署的原則，讓只有擁有簽署原則之憑證存取權的人員可以變更原則。

#### <a name="control-flow-guard"></a>控制流程防護 
Windows Server 2016 也包含內建的保護，可防範某些類別的記憶體損毀攻擊。 修補您的伺服器很重要，但總是有可能為尚未識別的弱點開發惡意程式碼。 利用這些弱點的一些最常見方法，就是將異常或極端的資料提供給執行中的程式。 例如，攻擊者可以藉由提供比預期更多的程式輸入，並將程式所保留的區域溢出，以利用緩衝區溢位弱點來保存回應。 這可能會損毀可能保存函式指標的相鄰記憶體。

當程式呼叫此函式時，它就可以跳到攻擊者所指定的非預期位置。 這些攻擊也稱為跳躍式程式設計（JOP）攻擊。 控制流程防護會將嚴格限制放在可執行檔應用程式程式碼，以防止 JOP 攻擊，特別是間接呼叫指示。 它會新增輕量的安全性檢查，以識別應用程式中的函式集合，這些函式是間接呼叫的有效目標。 當應用程式執行時，它會驗證這些間接呼叫目標是否有效。

如果控制流程防護檢查在執行時間失敗，Windows Server 2016 會立即終止程式，並中斷任何嘗試間接呼叫無效位址的惡意探索。 控制流程防護可為裝置防護提供一層額外的保護。 如果白名單的應用程式已遭入侵，它就能夠由 Device Guard 取消核取，因為 Device Guard 檢測會看到應用程式已簽署並被視為受信任。

但是因為控制流程防護可以識別應用程式是否以非預先決定的非可行循序執行，所以攻擊會失敗，使遭入侵的應用程式無法執行。 這些保護結合在一起，讓攻擊者難以將惡意程式碼插入 Windows Server 2016 上執行的軟體。

我們鼓勵開發人員建立將處理個人資料的應用程式，以在其應用程式中啟用控制流程防護（CFG）。 這項功能會在 Microsoft Visual Studio 2015 中提供，並可在「CFG 感知」版本的 Windows 上執行，也就是適用于 Windows 10 和 Windows 8.1 Update （KB3000850）之桌面和伺服器的 x86 和 x64 版本。 您不需要為程式碼的每個部分啟用 CFG，因為已啟用 CFG 的混合，而且已啟用非 CFG 的程式碼將會正常執行。 但是無法為所有程式碼啟用 CFG，可以開啟保護中的間距。 此外，已啟用 CFG 的程式碼可在「CFG 感知」版本的 Windows 上正常運作，因此與它們完全相容。

#### <a name="windows-defender-antivirus"></a>Windows Defender 防毒軟體
Windows Server 2016 包含業界領先的 Windows Defender 主動式偵測功能，以封鎖已知的惡意程式碼。 Windows Defender 防毒軟體（AV）會與 Windows Defender Device Guard 和控制流程防護搭配運作，以防止任何種類的惡意程式碼安裝在您的伺服器上。 預設會開啟此功能–系統管理員不需要採取任何動作就能開始工作。 Windows Defender AV 也已優化，可支援 Windows Server 2016 中的各種伺服器角色。 在過去，攻擊者會使用 shell （例如 PowerShell）來啟動惡意的二進位代碼。 在 Windows Server 2016 中，PowerShell 現在已與 Windows Defender AV 整合，以便在啟動程式碼之前先掃描是否有惡意程式碼。

Windows Defender AV 是內建的反惡意程式碼解決方案，可為桌上型電腦、可攜式電腦和伺服器提供安全性和反惡意程式碼管理。 Windows Defender AV 已大幅改善，因為它是在 Windows 8 中引進。 Windows Server 中的 windows Defender 防毒軟體使用多種方式來改善反惡意程式碼：

- **雲端傳遞的保護**有助於在數秒內偵測及封鎖新的惡意程式碼，即使該惡意程式碼從未出現過。

- **豐富的本機內容**可改進辨識惡意程式碼的方式。 Windows Server 不僅會通知 Windows Defender AV 內容，像是檔案和進程，還有內容的來源、儲存位置等等。 

- **廣泛的全域感應器**可協助保持 WINDOWS Defender AV 的最新資訊，並瞭解甚至是最新的惡意程式碼。 這是透過兩種方式完成：從端點收集豐富的本機內容資料並集中分析該資料。

- **防篡改**檢查可協助保護 WINDOWS Defender AV 本身免于惡意程式碼的攻擊。 例如，Windows Defender AV 會使用受保護的處理常式，以防止未受信任的進程嘗試使用 Windows Defender AV 元件、其登錄機碼等來進行篡改。

- **企業層級功能**賦予 IT 專業人員讓 WINDOWS Defender AV 成為企業級反惡意程式碼解決方案所需的工具和設定選項。

#### <a name="enhanced-security-auditing"></a>增強的安全性審核 
Windows Server 2016 會主動警示系統管理員對潛在的入侵嘗試，並提供更詳細的資訊，可用於更快速的攻擊偵測和法庭分析。 它會將控制流程防護、Windows Defender 裝置防護及其他安全性功能中的事件記錄在一個位置，讓系統管理員更容易判斷哪些系統可能有風險。

新的事件類別目錄包括：

- **審查群組成員資格。** 可讓您在使用者的登入 token 中，對群組成員資格資訊進行審核。 在建立登入會話的電腦上列舉或查詢群組成員資格時，就會產生事件。 
 
- **Audit PnP 活動。** 可讓您在「隨插即用」偵測到外部裝置（可能包含惡意程式碼）時進行審核。 PnP 事件可以用來追蹤系統硬體中的變更。 事件中包含硬體廠商識別碼的清單。

Windows Server 2016 與安全性事件事件管理（SIEM）系統（例如 Microsoft Operations Management Suite （OMS））很容易整合，可將資訊併入可能違反的智慧報告中。 增強型審核所提供的資訊深度可讓安全性小組更快速且更有效率地識別及回應潛在的缺口。

### <a name="secure-virtualization"></a>安全虛擬化
現今的企業可以將它們從 SQL Server 到 SharePoint，Active Directory 網網域控制站都虛擬化。 虛擬機器（Vm）可讓您更輕鬆地部署、管理、服務和自動化您的基礎結構。 但在安全性方面，遭入侵的虛擬化網狀架構已經成為一個難以對抗的新攻擊向量–到目前為止。 從 GDPR 的觀點來看，您應該考慮保護 Vm，就像保護實體伺服器一樣，包括使用 VM TPM 技術。

Windows Server 2016 基本上改變了企業可以如何保護虛擬化的方式，包括多項技術，可讓您建立只在自己的網狀架構上執行的虛擬機器;協助保護其執行所在的儲存體、網路和主機裝置。

#### <a name="shielded-virtual-machines"></a>受防護的虛擬機器
讓虛擬機器更容易遷移、備份和複寫的情況，也可以讓它們更容易修改和複製。 虛擬機器只是一個檔案，因此不會在網路、儲存體、備份或其他地方受到保護。 另一個問題是，網狀架構系統管理員（不論是存放裝置管理員還是網路系統管理員）都可以存取所有虛擬機器。

在網狀架構上遭到入侵的系統管理員，很容易就能在虛擬機器上產生洩露的資料。 所有攻擊者都必須使用遭盜用的認證，將他們喜歡的任何 VM 檔案複製到 USB 磁片磁碟機上，並將其移出組織，讓這些 VM 檔案可以從任何其他系統存取。 例如，如果有任何一部遭竊的 Vm 是 Active Directory 的網域控制站，攻擊者就可以輕鬆地查看內容，並使用立即可用的暴力密碼破解技術來破解 Active Directory 資料庫中的密碼，最後提供他們存取權您的基礎結構中的所有其他專案。

Windows Server 2016 引進受防護的虛擬機器（受防護的 Vm），以協助防範如剛才所述的案例。 受防護的 Vm 包含虛擬 TPM 裝置，可讓組織將 BitLocker 加密套用到虛擬機器，並確保它們只在受信任的主機上執行，以協助防範遭入侵的存放裝置、網路和主機系統管理員。 受防護的 Vm 是使用第2代 Vm 所建立，其支援整合可延伸韌體介面（UEFI）固件，並具有虛擬 TPM。

#### <a name="host-guardian-service"></a>主機守護者服務
除了受防護的 Vm，主機守護者服務是建立安全虛擬化網狀架構的必要元件。 其工作是先證明 Hyper-v 主機的健康狀態，然後才允許受防護的 VM 開機或遷移至該主機。 它會保存受防護 Vm 的金鑰，而且在保證安全性健康狀態之前，將不會釋放它們。 有兩種方式可以要求 Hyper-v 主機向主機守護者服務證明。

第一個（且最安全）是硬體信任的證明。 此解決方案會要求您的受防護 Vm 在具有 TPM 2.0 晶片和 UEFI 2.3.1 的主機上執行。 需要此硬體才能提供主機守護者服務所需的測量開機和作業系統核心完整性資訊，以確保 Hyper-v 主機未遭到篡改。

IT 組織可以選擇使用系統管理員信任的證明，如果您的組織未使用 TPM 2.0 硬體，這可能是理想的做法。 此證明模型很容易部署，因為主機只會放入安全性群組中，而主機守護者服務會設定為允許受防護的 Vm 在屬於安全性群組成員的主機上執行。 使用此方法時，不會進行複雜的測量，以確保主機電腦不會遭到篡改。 不過，您可以消除未加密 Vm 在 USB 磁片磁碟機上的門，或 VM 將在未經授權的主機上執行的可能性。 這是因為 VM 檔案不會在指定群組中的任何電腦上執行。 如果您還沒有 TPM 2.0 硬體，可以從系統管理員信任的證明開始，並在硬體升級時切換到硬體信任的證明。

#### <a name="virtual-machine-trusted-platform-module"></a>虛擬機器信賴平臺模組
Windows Server 2016 支援 TPM 的虛擬機器，可讓您在虛擬機器中支援先進的安全性技術，例如 BitLocker®磁片磁碟機加密。 您可以使用 Hyper-v 管理員或 VMTPM Windows PowerShell Cmdlet，在任何第2代 Hyper-v 虛擬機器上啟用 TPM 支援。

您可以使用存放在主機上或存放在主機守護者服務中的本機加密金鑰，來保護虛擬 TPM （vTPM）。 因此，雖然主機守護者服務需要更多的基礎結構，但它也提供更多保護。

#### <a name="distributed-network-firewall-using-software-defined-networking"></a>使用軟體定義網路的分散式網路防火牆
在虛擬化環境中改善保護的其中一種方法，就是將網路劃分成可讓 Vm 只與運作所需的特定系統通訊的方式。 例如，如果您的應用程式不需要與網際網路連線，您可以將其分割，將這些系統排除為來自外部攻擊者的目標。 Windows Server 2016 中的軟體定義網路（SDN）包含分散式網路防火牆，可讓您以動態方式建立安全性原則，以保護您的應用程式免于來自網路內部或外部的攻擊。 此分散式網路防火牆可讓您隔離網路中的應用程式，以增加您的安全性層級。 原則可以套用至虛擬網路基礎結構的任何位置、隔離 VM 對 VM 流量、VM 對主機流量，或 VM 到網際網路流量（如有需要）–可能是因為個別系統遭到入侵或以程式設計方式跨多個子網。 Windows Server 2016 軟體定義的網路功能也可讓您路由傳送或鏡像非 Microsoft 虛擬裝置的連入流量。 例如，您可以選擇透過 Barracuda 虛擬裝置傳送所有的電子郵件流量，以提供額外的垃圾郵件篩選保護。 這可讓您輕鬆地將內部部署或雲端中的額外安全性分層。

### <a name="other-gdpr-considerations-for-servers"></a>伺服器的其他 GDPR 考慮
GDPR 包含入侵通知的明確需求，其中的個人資料缺口表示「_違反安全性，導致意外或非法的損毀、遺失、改變、未經授權的洩漏，或存取個人資料已傳送、儲存或以其他方式處理。_ 」  很明顯地，如果您一開始就無法偵測到缺口，就無法開始在72小時內繼續符合嚴格的 GDPR 通知需求。

如 Windows 資訊安全中心白皮書中所述， [Post 缺口：處理先進的威脅](http://wincom.blob.core.windows.net/documents/Post_Breach_Dealing_with_Advanced_Threats_Whitepaper.pdf)

> 「_與前置缺口不同的是，入侵後會假設已發生缺口–作為飛行記錄器和犯罪場景調查員（CSI）。入侵後，會為安全性小組提供識別、調查和回應攻擊所需的資訊和工具組，否則會保持無法偵測到的內容。_ 」

在本節中，我們將探討 Windows Server 如何協助您符合 GDPR 缺口通知義務。 這一開始會先瞭解 Microsoft 提供的基礎威脅資料，並針對您的權益進行收集和分析，而透過 Windows Defender Advanced 威脅防護（ATP），該資料對您而言可能非常重要。

#### <a name="insightful-security-diagnostic-data"></a>深入解析的安全性診斷資料
在將近兩年的期間，Microsoft 已將威脅轉變成有用的智慧，協助 fortify 其平臺並保護客戶。 現今，憑藉著雲端提供的巨大運算優勢，我們找到由威脅情報驅動的豐富分析引擎來保護客戶的新方式。

結合應用自動和手動流程、機器學習和人類專家，我們可以建立 Intelligent Security Graph，從其本身學習並即時進化，降低偵測和因應所有產品上新事件的整體時間。

![Microsoft 智慧安全性圖表](../media/GDPR-Windows-Server-Overview/gdpr-intelligent-security-graph.png)

Microsoft 的威脅情報範圍，實際上是數十億個資料點：35000000000每月、1000000000客戶在企業和取用者區段中掃描的訊息，存取200個以上的雲端服務，並14000000000每天執行的驗證。 這項資料會由 Microsoft 代表您一起提取，以建立 Intelligent Security Graph，協助您以動態方式保護您的 front 門，以保持安全、維持生產力，並符合 GDPR 的需求。

#### <a name="detecting-attacks-and-forensic-investigation"></a>偵測攻擊和鑑識調查
隨著網路攻擊越來越精巧也越來越針對性，即使是最好的端點防護最終也可能被破解。 有兩個功能可用來協助進行潛在的缺口偵測-Windows Defender Advanced 威脅防護（ATP）和 Microsoft Advanced 威脅分析（ATA）。

Windows Defender 進階威脅防護 (ATP) 可協助您偵測、調查及回應網路上的進階攻擊和資料漏洞。 GDPR 的資料缺口類型預期您會透過技術安全性措施來保護，以確保個人資料和處理系統的機密性、完整性和可用性。

Windows Defender ATP 的主要優點包括下列各項：

- **偵測無法偵測的。** 在作業系統核心、Windows 安全性專家，以及超過1000000000部機器的獨特光纖，以及所有 Microsoft 服務的信號之間建立的感應器。

- **內建，而不是置中。** 無代理程式，具有高效能且最小的影響，雲端技術;無部署的輕鬆管理。 

- **適用于 Windows 安全性的單一窗格。** 從 Windows Defender ATP、Windows Defender 防毒軟體和 Windows Defender Device Guard，探索6個月的豐富、機器時間軸、統一的安全性事件。

- **Microsoft graph 的威力。** 利用 Microsoft 智慧安全性圖表來整合偵測和探索與 Office 365 ATP 訂用帳戶，以追蹤和回應攻擊。

如需詳細資訊，請參閱[Windows DEFENDER ATP 建立者更新預覽的新功能](https://blogs.microsoft.com/microsoftsecure/2017/03/13/whats-new-in-the-windows-defender-atp-creators-update-preview/)。

ATA 是內部部署產品，可協助偵測組織中的身分識別洩漏。 ATA 可以針對驗證、授權和資訊收集通訊協定（例如 Kerberos、DNS、RPC、NTLM 和其他通訊協定），來捕捉和剖析網路流量。 ATA 會使用此資料來建立與網路上的使用者和其他實體相關的行為設定檔，讓它能夠偵測異常和已知的攻擊模式。 下表列出 ATA 偵測到的攻擊類型。


|攻擊類型 |描述 |
|---------|---------|
|惡意攻擊 |從已知的攻擊類型清單中尋找攻擊，就會偵測到這些攻擊，包括：<ul><li>傳遞票證（Platform ptt）</li><li>傳遞雜湊（PtH）</li><li>越過-雜湊</li><li>偽造的 PAC （MS14-068）</li><li>黃金票證</li><li>惡意的複寫</li><li>偵察</li><li>暴力密碼破解</li><li>遠端執行</li></ul>如需可偵測到之惡意攻擊的完整清單及其描述，請參閱[ATA 可以偵測哪些可疑的活動？](https://docs.microsoft.com/advanced-threat-analytics/understand-explore/ata-threats)。|
|異常行為 |這些攻擊會使用行為分析來偵測，並使用機器學習來識別可疑的活動，包括：<ul><li>異常登入</li><li>未知的威脅</li><li>密碼共用</li><li>橫向移動</li></ul>|
|安全性問題和風險 |藉由查看目前的網路和系統組態來偵測這些攻擊，包括：<ul><li>信任中斷</li><li>弱式通訊協定</li><li>已知的通訊協定弱點</li></ul>|

您可以使用 ATA 來協助偵測嘗試入侵特殊許可權身分識別的攻擊者。 如需有關部署 ATA 的詳細資訊，請參閱[Advanced 威脅分析檔](https://docs.microsoft.com/advanced-threat-analytics/)中的規劃、設計和部署主題。

## <a name="related-content-for-associated-windows-server-2016-solutions"></a>相關聯 Windows Server 2016 解決方案的相關內容

- **Windows Defender 防毒軟體：** https://www.youtube.com/watch?v=P1aNEy09NaI 和 https://docs.microsoft.com/windows/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10

- **Windows Defender Advanced 威脅防護：** https://www.youtube.com/watch?v=qxeGa3pxIwg 和 https://docs.microsoft.com/windows/threat-protection/windows-defender-atp/configure-server-endpoints-windows-defender-advanced-threat-protection

- **Windows Defender Device Guard：** https://www.youtube.com/watch?v=F-pTkesjkhI 和 https://docs.microsoft.com/windows/device-security/device-guard/device-guard-deployment-guide

- **Windows Defender Credential Guard：** https://www.youtube.com/watch?v=F-pTkesjkhI 和 https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard

- **控制流程防護：** https://msdn.microsoft.com/library/windows/desktop/mt637065(v=vs.85).aspx

- **安全性與保證：** https://docs.microsoft.com/windows-server/security/security-and-assurance

## <a name="disclaimer"></a>免責聲明
本文代表 Microsoft 於截至文件發行當日為止對於 GDPR 的探討觀點。 我們花了很多時間來 GDPR，並想想我們已經仔細瞭解其意圖和意義。 但 GDPR 的應用與具體事實高度相關，並非 GDPR 的所有面向與解讀都符合現況。

因此，本文僅供參考，不應做為法律建議或用來判斷 GDPR 會如何套用到您與您的組織。 我們鼓勵您與具具有法律資格的專業人員討論 GDPR、其如何具體套用至您的組織，以及如何最理想地確保合規性。

MICROSOFT 對於本文中的資訊不負任何明示、默示或法定擔保責任。 本文是以原本的形式提供。 本文中提供的資訊和檢視，包括URL 及其他網際網路網站參考資料，可能會依情況改變，恕不另行通知。

本文不為您提供對任何 Microsoft 產品中的任何智慧財產的法定權利。  您可在僅供內部參考的用途下複製本文。  

發行日期：2017 年 9 月<br>
版本 1.0<br>
© 2017 Microsoft. 著作權所有，並保留一切權利。


