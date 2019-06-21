---
title: 開始您的適用於 Windows Server 2016 的一般資料保護法規 (GDPR) 之旅
description: 使用這份文件深入了解 GDPR 是什麼，以及 Microsoft 提供哪些產品來協助您符合法規。
keywords: 隱私權, GDPR
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.technology: techgroup-security
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 09/25/2017
ms.author: nirb
ms.openlocfilehash: 1c374c00573e87594eeeab620face9ea9acaa531
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284207"
---
# <a name="beginning-your-general-data-protection-regulation-gdpr-journey-for-windows-server"></a>從您的一般資料保護規定 (GDPR) 之旅，適用於 Windows Server 

>適用於：Windows Server （半年通道），Windows Server 2016

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

- **增強個人隱私權權限。** 確保歐盟居民有權存取他們的個人資料、更正該資料中的錯誤、清除該資料、處理其個人資料以及移動資料，來加強歐盟居民的資料保護。

- **增加的待命中，以保護個人資料。** 強化組織處理個人資料的責任，釐清責任歸屬以確保合規。

- **必要的個人資料缺口報告。** 控管個人資料的組織必須向其監管當局回報對個人權利和自由構成風險的個人資料洩露情形，不得無故拖延，且應盡可能在察覺洩漏情形的 72 小時以內回報。

正如你所預料，GDPR 可能對您的企業產生重大影響，可能會要求您更新隱私權原則、實施並加強資料保護控制和洩漏通知程序、部署高度透明的原則，並進一步投資 IT 和訓練。 Microsoft Windows 10 可協助您有效且有效率地處理其中的部分需求。

## <a name="personal-and-sensitive-data"></a>個人與機密資料
為了努力符合 GDPR，您必須了解法規如何定義個人和機密資料，以及這些定義如何關聯至您組織保有的資料。 根據您將能夠探索建立該資料的位置，處理，了解管理和儲存。

GDPR 將個人資料認定為可關聯至已識別或可識別自然人的任何資訊。 這包括直接識別 (例如您的法定名稱) 和間接識別 (例如可確認資料提及的對象是您的特定資訊)。 GDPR 也釐清個人資料的概念，包含線上識別碼 (例如 IP 位址、行動裝置識別碼) 以及位置資料。

GDPR 引進基因資料 (例如，個人的基因序列) 和生物特徵辨識資料的特定定義。 基因資料、生物特徵辨識資料以及其他個人資料子類別 (會揭露種族、民族血統、政治觀點、宗教或哲學信仰或工會成員資格的個人資料：有關健康的資料，或有關一個人的性生活或性取向的資料) 在 GDPR 中都被視為機密個人資料。 機密個人資料必須給予強化保護，且通常需要個人明確同意這些資料的處理位置。

### <a name="examples-of-info-relating-to-an-identified-or-identifiable-natural-person-data-subject"></a>已識別或可識別自然人的相關資訊範例 (資料主題)
這份清單提供 GDPR 規範的數個資訊類型範例。 並非完整的清單。

-   名稱

-   身分證號碼 (例如 SSN)

-   位置資料 (例如住址)

-   線上識別碼 (例如電子郵件地址、螢幕名稱、IP 位址、裝置識別碼)

-   匿名資料 (例如使用金鑰識別個人身份)

-   基因資料 (例如來自個人的生物樣本)

-   生物特徵辨識資料 (例如指紋、臉部辨識)

## <a name="getting-started-on-the-journey-towards-gdpr-compliance"></a>開始 GDPR 合規之旅
由於要符合 GDPR 的待辦事項眾多，因此強烈建議您不要等到開始實施後才著手準備。 您應該現在就檢查您的隱私權和資料管理做法。 我們建議您將焦點放在四個主要步驟，來啟動 GDPR 相容性之旅：

-   **探索。** 找出您擁有哪些個人資料，以及存放位置。 

-   **管理。** 管理個人資料的使用與存取方式。

-   **保護。** 建立安全性控制，以防止、偵測及因應弱點及資料洩漏。  

-   **報表。** 對資料要求採取行動、回報資料洩漏，以及保留必要的文件。

    ![4 個主要 GDPR 步驟搭配運作的圖表](../media/GDPR-Windows-Server-Overview/gdpr-steps-diagram.png)

我們已經針對每個步驟，概述各種 Microsoft 解決方案提供的工具範例、資源和功能，可以用來協助您處理該步驟的需求。 雖然這篇文章不完整的 「 作法 」 指南，我們已加入連結，以了解詳細資訊，但的詳細資訊位於[GDPR 區段 Microsoft 信任中心](https://www.microsoft.com/en-us/trustcenter/privacy/gdpr)。

## <a name="windows-server-security-and-privacy"></a>Windows Server 安全性和隱私權
GDPR 會要求您實作適當的技術和組織安全性措施，以保護個人資料和處理系統。 在 GDPR 的內容中，實體和虛擬伺服器環境可能正在處理個人和機密資料。 處理可以表示任何作業或一組作業，例如資料收集、 儲存和擷取。

您為了符合此需求，以及實作適當的技術安全性措施的能力必須反映您面對現今越來越不友善的 IT 環境中的威脅。 今日的安全性威脅形勢比以往更具侵略性也更加頑強。 以往，惡意攻擊者主要將重點放在透過攻擊獲得社群認可，或者暫時使系統離線的快感。 之後，攻擊者的動機已轉向賺錢，包括挾持裝置和資料，逼迫擁有者支付贖金。

現代化攻擊越來越針對大規模竊取智慧財產權，以及會導致財務損失的目標系統效能降低，更遑論威脅全球個人、企業和國家/地區利益的網路恐怖主義。 這些攻擊者通常是訓練有素的個人和安全專家，其中有些人甚至受雇於擁有大量預算，以及似乎有無限制人力資源的民族國家/地區。 此類威脅，需要能夠應付這種挑戰的方法。

這些威脅不只是威脅到您掌控任何個人或機密資料的能力，也對您的整體企業造成重大風險。 作者： McKinsey，Ponemon Institute、 Verizon、 與 Microsoft 的最新資料，請考慮：

- GDPR 期待您回報的資料洩漏類型平均成本為 350 萬美元。

- 這些破壞事件的 63% 涉及弱式或遭竊密碼，GDPR 也期待您應能妥善處理。

- 每天都有超過 300,000 個新惡意程式碼樣本建立和散播，讓您的保護資料工作更加困難重重。

中所看到最近的勒索軟體攻擊，一次呼叫黑色的最頭痛的網際網路，攻擊者會更大的目標可以容忍付出更有可能發生的重大的後果後。 GDPR 包含讓您的系統，包括桌上型電腦和膝上型電腦，包含個人和機密資料的確實豐富目標的負面影響。

兩個索引鍵的原則有指引，並繼續引導您開發 Windows 的：

- **安全性。** 我們的軟體和服務代表我們的客戶所儲存的資料應該受到保護免於受到破壞和使用或修改適當的方式。 安全性模型應該了解，並在其應用程式中建置的開發人員更容易。

- **隱私權。** 使用者應該如何使用其資料的控制項中。 原則的資訊，請使用應使用者清楚了解。 使用者應該控制時，如果他們收到進行最佳運用工作時間的資訊。 它應該是資訊的以指定適當使用他們包括控制使用的電子郵件傳送的使用者輕鬆。

Microsoft 一直堅定不移，最近由 Microsoft 執行長 Satya Nadella 記下這些原則對 

> 「_世界持續變更，且商務需求的演變，一些是一致： 安全性和隱私權的客戶的需求。_ "

要符合 gdpr 的規範，了解在建立、 存取、 處理、 儲存和管理為個人可能符合資格的資料實體和虛擬伺服器的角色，且可能含有機密資料在 GDPR 下的也很重要。 Windows Server 提供功能可協助您遵守 GDPR 需求，以實作適當的技術和組織安全性措施來保護個人資料。

Windows Server 2016 的安全性狀態不是 bolt 在;就架構的原則。 此外，可以在四個主體最能理解：

- **保護。** 持續不斷地注意和預防措施; 上的創新封鎖已知的攻擊和已知的惡意程式碼。

- **偵測到。** 全方位監視工具，可協助您找出異常狀況及回應攻擊更快。

- **回應。** 導致回應和復原技術，加上諮詢專業知識的深度。

- **隔離。** 隔離的作業系統元件和資料的機密資料，限制系統管理員權限，並嚴格測量主機健全狀況。

使用 Windows Server，您能夠保護、 偵測及防範可能會導致資料外洩的攻擊類型的大幅提升。 由於 GDPR 對於洩漏通知的嚴格需求，確保您的桌上型和膝上型電腦系統受到妥善保護，可以降低讓您面臨昂貴洩漏分析及通知的風險。

在接下來區段中，您會看到如何利用 Windows Server 提供可以納入您的 GDPR 合規性旅程的 「 保護 」 階段的功能。 這些功能可分成三種保護案例：

- **保護您的認證，並限制系統管理員權限。** Windows Server 2016 可協助實作這些變更，以協助防止您的系統做為進一步入侵啟動點。

- **保護作業系統執行您的應用程式和基礎結構。** Windows Server 2016 提供層級的保護，可協助封鎖外部攻擊者執行惡意軟體或惡意探索的弱點。

- **安全的虛擬化。** Windows Server 2016 可讓安全虛擬化、 使用受防護的虛擬機器和受防護網狀架構。 這可協助您加密，並在 網狀架構，更好的保護它們免於惡意攻擊的信任主機上執行您的虛擬機器。

下面參考特定的 GDPR 需求的更詳細討論這些功能，是建置可協助維護的完整性與安全性的作業系統和資料的進階的裝置保護。

GDPR 中的金鑰佈建是資料保護所設計，並根據預設，而且可幫助您能夠符合這項佈建功能，例如 BitLocker 裝置加密的 Windows 10 中。 BitLocker 使用信賴平台模組 (TPM) 技術，可提供硬體式的安全性相關的函式。 此密碼編譯處理器的晶片包含多個的實體安全性機制，來防上，且惡意軟體無法竄改的 TPM 安全性功能。

此晶片包含多個實體安全性機制，使它具備防竄改功能，在 TPM 安全性功能的加持下，惡意程式碼軟體便無法進行竄改。 使用 TPM 技術的一些主要優點是您可以：

-   產生、儲存密碼編譯金鑰及限制此金鑰的使用。

-   藉由使用 TPM 的唯一 RSA 金鑰 (已燒錄至其本身)，使用 TPM 技術進行平台裝置驗證。

-   藉由進行和儲存安全性測量，協助確保平台完整性。

與作業中不導致資料洩漏的其他相關進階裝置保護包含 Windows 信任式開機，可確保惡意程式碼無法在系統防禦之前啟動，協助維護系統完整性。

## <a name="windows-server-supporting-your-gdpr-compliance-journey"></a>Windows Server:支援您的 GDPR 合規性旅程
Windows Server 內的主要功能可協助您有效率且有效地實作 GDPR 合規性所需要的安全性和隱私權的機制。 雖然使用這些功能將不保證您的合規性，它們將支援您這麼做的努力。

伺服器作業系統是保護的在組織的基礎結構，讓新的機會，來建立層級，免於遭受攻擊的可能竊取資料，並中斷您的業務策略層級。 重要的層面，例如設計、 資料保護和存取控制的隱私權 GDPR 的需要處理您的 IT 基礎結構，在伺服器層級內。

使用此選項，以協助保護身分識別、 作業系統和虛擬化層，Windows Server 2016 可以協助封鎖了其他人違法存取您的系統使用的常見攻擊誘因： 遭竊認證、 惡意程式碼和遭入侵的虛擬化網狀架構。 除了降低業務風險，安全性元件內建在 Windows Server 2016 說明位址索引鍵的政府和業界的合規性需求安全性法規。 

這些身分識別、 作業系統和虛擬化保護可讓您更進一步保護執行 Windows Server VM，以在任何雲端中，為您的資料中心，並限制洩漏認證、 啟動惡意程式碼，並維持偵測到的攻擊者的能力您網路。 同樣地，部署為 HYPER-V 主機時，Windows Server 2016 提供您透過受防護的虛擬機器和分散式的防火牆功能的虛擬化環境的安全性保證。 使用 Windows Server 2016 時，伺服器作業系統會變成您資料中心的安全性的積極參與者。

### <a name="protect-your-credentials-and-limit-administrator-privileges"></a>保護您的認證，並限制系統管理員權限 
控制存取權的個人資料，並處理該資料，系統是 gdpr 的規範可包括系統管理員的存取權的特定需求的區域。 特殊權限的身分識別是有更高的權限，例如網域系統管理員、 企業系統管理員、 本機系統管理員，或甚至 Power Users 群組成員的使用者帳戶的任何帳戶。 這類身分識別也可以包含已授與權限直接，例如執行備份，正在關閉系統或在 [本機安全性原則] 主控台中的 [使用者權限指派] 節點中列出的其他權限的帳戶。

為一般的存取控制原則和內嵌 gdpr 的規範，您需要從潛在攻擊者入侵保護這些特殊權限的身分識別。 首先，請務必了解如何遭到入侵的身分識別;然後您可以規劃以防止攻擊者存取這些特殊權限的身分識別。

#### <a name="how-do-privileged-identities-get-compromised"></a>如何特殊權限的身分識別取得遭到盜用？
組織的指導方針來保護它們沒有時，特殊權限的身分識別可以受到危害。 範例如下所示：

- **更多權限超出所需。** 其中一個最常見的問題是，使用者會具有比執行其作業職責所需的更多的權限。 比方說，管理 DNS 的使用者可能是 AD 系統管理員。 大多數情況下，這是為了避免需要設定不同的管理層級。 不過，如果這類帳戶遭到入侵，攻擊者會自動具有更高的權限。

- **持續登入提高的權限。** 另一個常見的問題是以較高權限的使用者可以將它用於無限制的時間。 這是很常見的 IT 專業人員登入桌上型電腦使用特殊權限的帳戶，保持登入，並使用特殊權限的帳戶瀏覽網頁，並使用電子郵件 (一般 IT 工作作業的函式)。 無限制的持續時間的特殊權限的帳戶可以讓帳戶更容易遭受攻擊，並增加帳戶將會遭到入侵的可能性。

- **社交工程研究。** 大部分認證威脅一開始先研究組織，然後進行透過社交工程。 比方說，攻擊者可能會執行組織的網路存取電子郵件網路釣魚攻擊來入侵合法的帳戶 （但不是一定是提高權限的帳戶）。 在您網路上執行其他參考資料，並識別可以執行管理工作的特殊權限的帳戶，攻擊者接著會使用這些有效的帳戶。 

- **利用以較高權限的帳戶。** 即使與網路中的一般、 非提高權限的使用者帳戶，攻擊者可以取得存取權以更高權限的帳戶。 其中一個較為常見的方法，這種做法因此是藉由傳遞-雜湊或傳遞權杖攻擊。 如需有關傳遞-雜湊和其他認證竊取技術的詳細資訊，請參閱資源上[Pass-雜湊 (PtH) 頁面](https://technet.microsoft.com/dn785092.aspx)。

當然，還有其他攻擊者可以使用來找出並危及 （以每一天建立新的方法） 的特殊權限的身分識別的方法。 因此是很重要，您會建立使用最低權限的帳戶，以降低攻擊者能夠存取具有特殊權限的身分識別登入使用者的做法。 下列各節概述其中 Windows Server 可以降低這些風險的功能。

#### <a name="just-in-time-admin-jit-and-just-enough-admin-jea"></a>在 Just-in-time 系統管理員 (JIT) 和 Just Enough Admin (JEA)
雖然防範 Pass-雜湊或傳遞票證攻擊中很重要，系統管理員認證的類型仍透過其他方式，包括社交工程、 不平的員工和暴力密碼破解遭竊的。 因此，除了隔離最大的認證，您也想要限制的系統管理員層級權限範圍，以防遭到入侵的方式。

現在，太多的系統管理員帳戶是責任的權限過大，即使只有一個區域。 例如，DNS 系統管理員，需要非常少的權限來管理 DNS 伺服器，通常授與網域系統管理員層級權限。 此外，因為這些認證會獲得免費提供，沒有任何限制時間使用。

每個具有不必要的網域系統管理員層級權限的帳戶會增加您暴露於攻擊者試圖入侵的認證。 若要降低攻擊的介面區，您要提供只會將特定集的權限系統管理員需要執行工作 – 而且僅適用於完成所需的時間視窗。

使用 Just Enough Administration 和 Just in Time Administration，系統管理員可以要求特定的權限所需的確切的視窗所需的時間。 DNS 系統管理員，例如，使用 PowerShell 來啟用 Just Enough Administration 可讓您建立一組有限的命令所提供的 DNS 管理。

如果 DNS 系統管理員需要對其中一個她伺服器進行更新，她會要求使用 Microsoft Identity Manager 2016 管理 DNS 的存取。 要求工作流程可以包含雙因素驗證，可以呼叫以確認她的身分識別，之後再授與要求的權限的系統管理員的行動電話等執行核准程序。 一旦授與，這些 DNS 權限提供權限的 PowerShell 角色 DNS 的特定時間範圍內。

如果 DNS 系統管理員的認證都被偷了，假設有此狀況。 首先，因為認證沒有附加到它們的系統管理員權限，攻擊者便無法存取 DNS 伺服器 – 或任何其他系統-進行任何變更。 如果攻擊者嘗試將要求的 DNS 伺服器的權限，第二要素驗證會要求他們確認其身分識別。 因為它不可能的攻擊者有 DNS 系統管理員的行動電話，驗證將會失敗。 這會鎖定攻擊者從系統中，並警示 IT 組織的認證可能會受到危害。

此外，許多組織使用免費[本機系統管理員密碼解決方案 (LAPS)](http://aka.ms/laps)做為簡單但功能強大 JIT 管理機制的伺服器和用戶端系統。 LAPS 功能提供管理加入網域的電腦的本機帳戶密碼。 密碼會儲存在 Active Directory (AD) 中，並受到存取控制清單 (ACL)，因此只有符合資格的使用者可以讀取它，或要求其重設。

如中所述[Windows 認證竊取風險降低指南](https://www.microsoft.com/en-us/download/confirmation.aspx?id=54095)， 

> 「_工具和技術的罪犯用來執行認證竊取和重複使用攻擊改善、 惡意攻擊都覺得您更輕鬆地達成其目標。認證竊取通常會仰賴操作實務或使用者認證外洩，所以有效的防護功能需要處理人員、 程序和技術有全面的方法。此外，這些攻擊依賴攻擊者竊取認證之後危害系統，以展開，或保存存取，以便組織必須快速遏制漏洞，藉由實作自由移動時，防止攻擊者，並在無法偵測到的策略遭到入侵的網路。_ "

重要的設計考量適用於 Windows Server 已減輕認證竊取風險 — 特別是，衍生的認證。 Credential Guard 的設計目的是協助消除硬體為基礎的隔離攻擊的 Windows 中實作重大的架構變更，而不是單純試圖提供大幅改善的安全性，以防止衍生的認證竊取和重複使用防禦它們。

雖然使用 Windows Defender Credential Guard、 NTLM 和 Kerberos 使用虛擬化型安全性保護衍生的認證，認證竊取攻擊技巧和工具用於許多目標的攻擊會遭到封鎖。 具有系統管理權限，在作業系統中執行的惡意程式碼，無法擷取受虛擬式安全性保護的密碼。 雖然 Windows Defender Credential Guard 是功能強大的防護，持續性威脅的攻擊的將可能的 shift 鍵，以新的攻擊技巧，而且您也應該併入 Device Guard，如下所述，以及其他安全性策略和架構。

#### <a name="windows-defender-credential-guard"></a>Windows Defender Credential Guard
Windows Defender Credential Guard 會使用虛擬化安全性隔離認證的詳細資訊，防止密碼雜湊或 Kerberos 票證被攔截。 它會使用全新隔離本機安全性授權 (LSA) 程序，不能存取作業系統的其餘部分。 隔離的 LSA 所使用的所有二進位檔會使用啟動它們在受保護的環境中，讓通過-雜湊類型的攻擊完全失效之前會驗證的憑證簽署。

使用 Windows Defender Credential Guard:

- （必要） 的虛擬化型安全性。 另外需要：

    - 64 位元 CPU

    - CPU 虛擬化延伸模組，再加上擴充的頁面資料表

    - Windows Hypervisor

- 安全開機 (必要)

- 個別或韌體 TPM 2.0 (最好能提供硬體繫結)

您可以使用 Windows Defender Credential Guard，以協助保護特殊權限的身分識別保護認證和 Windows Server 2016 上的認證衍生項目。 如需有關 Windows Defender Credential Guard 需求的詳細資訊，請參閱 <<c0> [ 保護衍生的網域認證，與 Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard)。

#### <a name="windows-defender-remote-credential-guard"></a>Windows Defender Credential Guard 遠端
Windows Defender 遠端 Credential Guard 在 Windows Server 2016 和 Windows 10 年度更新版也可協助保護使用遠端桌面連線的使用者認證。 過去，使用遠端桌面服務的任何人都必須登入其本機電腦，然後才能登入一次執行時其目標電腦的遠端連線。 此第二個登入會將認證傳遞到目標電腦上，將其公開至 Pass-雜湊或傳遞票證攻擊。

使用 Windows Defender 遠端 Credential Guard，Windows Server 2016 實作單一登入遠端桌面工作階段，因此不需要重新輸入您的使用者名稱和密碼。 相反地，它會利用您已經使用登入您的本機電腦的認證。 若要使用 Windows Defender 遠端 Credential Guard，在遠端桌面用戶端和伺服器必須符合下列需求：

- 必須加入 Active Directory 網域，而且位於相同網域或具有信任關係的網域。

- 必須使用 Kerberos 驗證。

- 必須至少執行 Windows 10 版本 1607年或 Windows Server 2016。  

- 需要遠端桌面傳統 Windows 應用程式。 遠端桌面通用 Windows 平台應用程式不支援 Windows Defender 遠端 Credential Guard。

您可以使用登錄設定的遠端桌面伺服器及群組原則或是遠端桌面用戶端上的遠端桌面連線參數，以啟用 Windows Defender 遠端 Credential Guard。 如需有關如何啟用 Windows Defender 和 Credential Guard 遠端的詳細資訊，請參閱[與 Windows Defender 遠端 Credential Guard 保護遠端桌面認證](https://docs.microsoft.com/windows/access-protection/remote-credential-guard)。 為與 Windows Defender Credential Guard，您可以使用 Windows Defender 和 Credential Guard 遠端協助保護 Windows Server 2016 上的特殊權限身分識別。

### <a name="secure-the-operating-system-to-run-your-apps-and-infrastructure"></a>安全的作業系統來執行您的應用程式和基礎結構
防止網路威脅也需要找出及封鎖惡意程式碼和設法破壞您的基礎結構的標準作業做法就獲得控制權的攻擊。 如果攻擊者可以取得作業系統或非預先定義、 非可行的方式執行的應用程式，他們可能使用該系統來採取惡意動作。 Windows Server 2016 提供封鎖外部攻擊者執行惡意軟體，或利用弱點的保護層級。 作業系統會主動保護基礎結構和應用程式的警示，表示已經違反服務等級的系統活動的系統管理員角色。

#### <a name="windows-defender-device-guard"></a>Windows Defender Device Guard
Windows Server 2016 包含 Windows Defender Device Guard，以確保只有受信任的軟體，可以在伺服器上執行。 使用虛擬化型安全性，它可以限制二進位檔案可以執行組織的原則為基礎的系統上。 如果指定的二進位檔以外的任何嘗試執行時，Windows Server 2016 會封鎖它，並記錄失敗的嘗試，讓系統管理員能夠查看已潛在的缺口。 缺口通知是不可或缺的一部分的 GDPR 合規性需求。

Windows Defender Device Guard 也會使用 PowerShell 整合，以便您可以在系統上執行的指令碼，可以對其進行授權。 在舊版的 Windows Server 中，系統管理員可以略過程式碼完整性強制執行，只要從程式碼檔案中刪除原則。 使用 Windows Server 2016，您可以設定，因此只有具有憑證簽署原則的存取權的人員可以變更原則經過貴組織的原則。

#### <a name="control-flow-guard"></a>控制流程防護 
Windows Server 2016 也包含內建保護，防範某些類別的記憶體損毀攻擊。 修補您的伺服器是很重要，但總是有可能無法針對尚未識別的弱點可能會開發該惡意程式碼。 利用這些弱點的最常見的方法有些異常或極端資料提供給執行中的程式。 比方說，攻擊者可以利用緩衝區溢位弱點可能會提供更多輸入才能程式非預期，而且超過保留的程式，以保存回應的區域。 這可能會損毀可能會佔據函式指標的相鄰記憶體。

當程式呼叫透過此函式時，它可以再跳到非預期的位置，攻擊者所指定。 這些攻擊也稱為是跳躍導向程式設計 (JOP) 攻擊。 控制流程防護會防止 JOP 攻擊，藉由將嚴格限制放在哪些應用程式程式碼可以執行 – 特別是間接呼叫的指示。 它會新增輕量級的安全性檢查，來識別應用程式中是有效的間接呼叫目標的函式的集合。 應用程式執行時，它會驗證這些間接呼叫目標有效。

如果控制流程防護檢查失敗，在執行階段，Windows Server 2016 立即終止程式，來間接呼叫了無效的位址會嘗試任何攻擊的重大。 控制流程防護 Device Guard 提供重要的額外的保護。 如果遭入侵的白名單的應用程式，它就是能夠執行未檢查的 Device Guard 的情況，因為檢測會看到應用程式已經過簽署，並會被視為 Device Guard 的受信任。

但因為是否應用程式正在執行非預先定義、 非可行的順序，可以識別控制流程防護，攻擊將會失敗，防止遭入侵的應用程式無法執行。 在一起，這些保護進行惡意程式碼插入 Windows Server 2016 上執行的軟體的攻擊者很難。

建置將會處理個人資料的應用程式的開發人員都在其應用程式啟用控制流程防護 (CFG)。 這項功能適用於 Microsoft Visual Studio 2015，而是 「 CFG 感知 」 版本的 Windows 上執行 — 用於桌面和伺服器的 Windows 10 和 Windows 8.1 Update (KB3000850) 的 x86 和 x64 版本。 您不必啟用 CFG 的程式碼中，每個組件，如已啟用 CFG 混用和非 CFG 啟用程式碼會正常執行。 不過，無法啟用 CFG 的所有程式碼可開啟保護中的間距。 此外，CFG 正常"CFG 感知 」 版本的 Windows 上啟用程式碼可以運作，因此與它們完全相容。

#### <a name="windows-defender-antivirus"></a>Windows Defender 防毒軟體
Windows Server 2016 包含業界領先業界、 作用中偵測的功能，Windows Defender 封鎖已知的惡意程式碼。 Windows Defender 防毒 (AV) 可以與 Windows Defender Device Guard 和控制流程防護，以防止在您的伺服器上安裝的任何類型的惡意程式碼一起運作。 依預設開啟，系統管理員不需要採取任何動作，才能開始使用。 Windows Defender AV 也最佳化，以支援 Windows Server 2016 中的各種伺服器角色。 在過去，攻擊者會使用 PowerShell 等的殼層啟動惡意二進位程式碼。 在 Windows Server 2016 中，PowerShell 現在會與 Windows Defender AV 之前啟動的程式碼掃描惡意程式碼整合。

Windows Defender AV 是內建反惡意程式碼解決方案，提供安全性和反惡意程式碼管理桌上型電腦、 可攜式電腦和伺服器。 因為它 Windows 8 中引進 Windows Defender AV 已經過大幅改良。 Windows Server 中的 Windows Defender 防毒軟體會使用多管齊來改善反惡意程式碼：

- **雲端傳遞的保護**有助於在數秒內偵測及封鎖新的惡意程式碼，即使該惡意程式碼從未出現過。

- **豐富的本機內容**可改進辨識惡意程式碼的方式。 Windows Server 會通知 Windows Defender AV 不僅相關的內容，如檔案和處理程序還內容來自何處，它有已儲存位置，和更多功能。 

- **廣泛的全域感應器**協助保護 Windows Defender AV，目前並注意即使是最新的惡意程式碼。 這是透過兩種方式完成：從端點收集豐富的本機內容資料並集中分析該資料。

- **竄改校訂**可協助保護 Windows Defender AV 本身攻擊，惡意程式碼。 例如，Windows Defender AV 會使用受保護的處理序可防止未受信任的處理序嘗試竄改 Windows Defender AV 元件，其登錄機碼，並依此類推。

- **企業級功能**提供 IT 專業人員的工具和製作 Windows Defender AV 企業級反惡意程式碼解決方案所需的組態選項。

#### <a name="enhanced-security-auditing"></a>增強的安全性稽核 
Windows Server 2016 時主動通知管理員使用增強式的安全性稽核可提供更詳細的資訊，可用來更快速的攻擊偵測和鑑識分析潛在的缺口嘗試。 從控制流程防護，Windows Defender Device Guard，以及在單一位置，其他安全性功能輕鬆判斷系統可能會有風險的系統管理員，它就會記錄事件。

新的事件類別目錄包括：

- **稽核的群組成員資格。** 可讓您稽核使用者的登入 token 中的群組成員資格資訊。 當您列舉或查詢上建立登入工作階段所在的電腦群組成員資格時，會產生事件。 
 
- **稽核 PnP 的活動。** 可讓您稽核時隨插即用會偵測到外部的裝置 – 可能包含惡意程式碼。 PnP 事件可用來追蹤系統硬體中的變更。 事件中包含的硬體廠商識別碼清單。

輕鬆地與安全性事件管理 (SIEM) 系統，例如 Microsoft Operations Management Suite (OMS)，它可以將資訊併入智慧報告潛在的缺口，整合 Windows Server 2016。 所提供的增強稽核資訊的深度可讓安全性小組找出並回應潛在的缺口，更快速且有效。

### <a name="secure-virtualization"></a>保護虛擬化
企業現今虛擬化他們也可以從 sharepoint 到 Active Directory 網域控制站的 SQL Server 的所有項目。 虛擬機器 (Vm) 只是讓您更輕鬆地部署、 管理服務，及自動化您的基礎結構。 說到安全性，遭入侵的虛擬化網狀架構得很難防禦 – 是新的攻擊媒介，但到目前為止。 GDPR 的觀點而言，您應該考慮您想保護包括 VM TPM 技術使用的實體伺服器保護的 Vm。

Windows Server 2016 徹底變更企業可以如何保護虛擬化，包括多項技術可讓您建立虛擬機器將只在您自己的網狀架構; 上執行協助保護從儲存體、 網路與主機裝置執行。

#### <a name="shielded-virtual-machines"></a>受防護的虛擬機器
將很容易就能移轉虛擬機器的相同事項，備份和複寫，也更輕鬆地修改和複製。 虛擬機器只是一個檔案，因此不在網路上，在儲存體、 備份，或其他位置中受到保護。 另一個問題是 – 無論是存放裝置系統管理員或網路系統管理員 – 的網狀架構系統管理員，擁有存取權的所有虛擬機器。

遭到入侵的系統管理員，在 網狀架構上輕鬆地造成入侵資料跨虛擬機器。 攻擊者必須做的只是使用遭入侵的認證複製到 USB 磁碟機想任何 VM 檔案，它走出公司，這些 VM 的檔案，可以從任何其他系統存取。 如果這些遭竊的任何的 Vm 一個 Active Directory 網域控制站，比方說，攻擊者可能輕易地檢視的內容並使用隨手可得暴力密碼破解技術來破解密碼在 Active Directory 資料庫中，最終讓他們能夠存取所有項目基礎結構內。

Windows Server 2016 引進類似先前所提到的受防護的虛擬機器 (受防護的 Vm) 可協助防範案例。 受防護的 Vm 包括虛擬的 TPM 裝置，如此可讓組織將 BitLocker 加密套用到虛擬機器，並確保它們只能在受信任的主機，以協助防止遭入侵的儲存體、 網路和主機系統管理員上執行。 使用第 2 代 Vm，支援整合可延伸韌體介面 (UEFI) 韌體，並具有虛擬 TPM，會建立受防護的 Vm。

#### <a name="host-guardian-service"></a>主機守護者服務
受防護的 Vm，以及主機守護者服務已建立安全的虛擬化網狀架構的重要元件。 其工作是將 HYPER-V 主機的健全狀況證明，才會允許受防護的 VM 開機，或者在移轉至該主控件。 它保存金鑰的受防護的 Vm，並將不會釋放它們，直到可確保安全性健康狀態。 有兩種方式，您可以要求證明主機守護者服務的 HYPER-V 主機。

第一個和最安全的是硬體信任的證明。 此解決方案會要求在有 TPM 2.0 晶片和 UEFI 2.3.1 的主機上執行受防護的 Vm。 這個硬體，才能提供嚴測的開機，且所需的主機守護者服務，以確保 HYPER-V 主機的作業系統核心完整性資訊未被竄改。

IT 組織可以選擇使用系統管理信任證明，這可能會想，如果 TPM 2.0 硬體不在貴組織使用。 這個證明模型很容易部署，因為主機只會將其放入安全群組和主機守護者服務已設定為允許安全性群組的成員主機上執行的受防護的 Vm。 使用此方法，沒有任何複雜的度量，以確保主機電腦未遭竄改。 不過，您不要排除未加密的 Vm 外的媒體櫃門 USB 磁碟機或 VM 會在未經授權的主機上執行。 這是因為 VM 檔案將不會執行指定的群組以外的任何電腦上。 如果您還沒有 TPM 2.0 硬體，可以開始使用系統管理信任證明，並切換至硬體信任證明，當您的硬體升級。

#### <a name="virtual-machine-trusted-platform-module"></a>虛擬機器信賴平台模組
Windows Server 2016 支援 TPM 對於虛擬機器，可讓您在虛擬機器中支援進階的安全性技術，例如 BitLocker® 磁碟機加密。 您可以使用 HYPER-V 管理員] 或 [啟用 VMTPM Windows PowerShell cmdlet，以啟用任何層代 2 HYPER-V 虛擬機器上的 TPM 支援。

您可以使用本機密碼編譯金鑰儲存在主機上或儲存在 「 主機守護者服務，以保護虛擬 TPM (vTPM)。 因此，雖然 「 主機守護者服務需要更多的基礎結構，它也提供更多的保護。

#### <a name="distributed-network-firewall-using-software-defined-networking"></a>使用軟體定義網路功能的分散式的網路防火牆
改善虛擬化環境中的保護的方法之一是可讓 Vm 僅與特定函式所需的系統與通訊的方式將網路分割。 例如，如果您的應用程式不需要網際網路連線，您可以分割它，消除這些系統為目標，從外部攻擊者。 軟體定義網路 (SDN) 在 Windows Server 2016 包含分散式的網路防火牆，可讓您以動態方式建立可以保護您的應用程式遭受來自內部或外部網路的安全性原則。 此分散式的網路防火牆將您的安全性，可讓您找出您網路中的應用程式層級。 原則可以套用的任何地方整個虛擬網路基礎結構，隔離 VM 到網際網路的流量，來裝載 VM 的流量或 VM 對 VM 流量，在必要時 – 適用於個別可能已遭入侵的系統，或以程式設計方式跨多個子網路。 Windows Server 2016 軟體定義網路功能也可讓您將路由傳送，或鏡像處理到非 Microsoft 的虛擬設備的連入流量。 例如，您可以選擇將您所有的電子郵件流量透過其他垃圾郵件篩選保護 Barracuda 虛擬應用裝置。 這可讓您輕鬆地層中額外的安全性同時對內部部署或雲端中。

### <a name="other-gdpr-considerations-for-servers"></a>伺服器的其他 GDPR 考量
GDPR 包含明確的需求，而是指個人資料缺口的缺口通知 」_導致意外或非法損毀、 遺失、 改變、 未經授權的揭露，或存取權、 個人的安全性缺口資料傳輸、 儲存或否則處理。_ "  很明顯地，您無法開始向前移到 72 小時內符合嚴格的 GDPR 通知要求，如果您一開始無法偵測到缺口。

Windows 資訊安全中心白皮書所述[Post 漏洞：因應進階威脅](http://wincom.blob.core.windows.net/documents/Post_Breach_Dealing_with_Advanced_Threats_Whitepaper.pdf)

> 「_不同於前的漏洞善後會假設缺口已經發生 – 做為飛行記錄器和犯罪場景調查員 (CSI)。善後安全性小組提供的資訊和工具組所需來識別、 調查和回應，否則會保持未偵測到的攻擊和以下版本雷達圖。_ "

在本節中，我們將探討 Windows Server 可以幫助您符合 GDPR 的缺口通知責任。 開始了解基礎威脅資料提供給 Microsoft，以收集及分析您的權益和做法，請透過 Windows Defender 進階威脅防護 (ATP)，該資料可以是您的關鍵。

#### <a name="insightful-security-diagnostic-data"></a>深入解析的安全性診斷資料
幾乎兩個數十年，Microsoft 有已停用威脅成實用商業智慧，可協助強化其平台和保護客戶。 現今，憑藉著雲端提供的巨大運算優勢，我們找到由威脅情報驅動的豐富分析引擎來保護客戶的新方式。

結合應用自動和手動流程、機器學習和人類專家，我們可以建立 Intelligent Security Graph，從其本身學習並即時進化，降低偵測和因應所有產品上新事件的整體時間。

![Microsoft 智慧安全性圖表](../media/GDPR-Windows-Server-Overview/gdpr-intelligent-security-graph.png)

Microsoft 威脅情報的範圍也就是跨越數十億個資料點：35 億個訊息每月掃描 1 十億的客戶，跨企業和取用者存取 200 個以上雲端服務和每日執行的 14 億筆驗證的區段。 這項資料是彙總了代替您由 Microsoft Intelligent Security Graph，可協助您保持安全、 維持生產力及符合 GDPR 需求以動態方式保護您的大門。

#### <a name="detecting-attacks-and-forensic-investigation"></a>偵測攻擊和鑑識調查
隨著網路攻擊越來越精巧也越來越針對性，即使是最好的端點防護最終也可能被破解。 有兩項功能可用來協助進行潛在的缺口偵測-Windows Defender 進階威脅防護 (ATP) 和 Microsoft Advanced Threat Analytics (ATA)。

Windows Defender 進階威脅防護 (ATP) 可協助您偵測、調查及回應網路上的進階攻擊和資料漏洞。 類型的資料缺口 GDPR 預期您防範透過技術的安全性措施，以確保持續的機密性、 完整性和可用性的個人資料和處理系統。

Windows Defender atp 的主要優點如下所示：

- **偵測不到。** 所有 Microsoft 服務的內都建深度在作業系統核心，Windows 安全性專家，並可從 1 億個機器和訊號的唯一光學感應器。

- **內建，不加諸上。** 高效能與影響降到最低，雲端架構; 無代理程式沒有部署的輕鬆管理。 

- **窗口的 Windows 安全性。** 瀏覽 6 個月的豐富的電腦時間表，從 Windows Defender ATP、 Windows Defender 防毒軟體和 Windows Defender Device Guard 統一的安全性事件。

- **Microsoft graph 的強大功能。** 運用 Microsoft 智慧安全性圖表，來整合偵測和瀏覽與 Office 365 ATP 訂用帳戶，來追蹤回及回應攻擊。

深入了解 [Windows Defender ATP Creators Update 預覽的新功能](https://blogs.microsoft.com/microsoftsecure/2017/03/13/whats-new-in-the-windows-defender-atp-creators-update-preview/)。

ATA 是內部部署產品，可協助偵測組織中的身分識別入侵。 ATA 可以擷取並剖析進行驗證、 授權和資訊收集 （例如 Kerberos、 DNS、 RPC、 NTLM 和其他通訊協定） 的通訊協定的網路流量。 ATA 會使用此資料來建置其行為的設定檔使用者和其他實體的相關網路上，以便它可以偵測到異常和已知的攻擊模式。 下表列出 ATA 所偵測到的攻擊類型。


|攻擊類型 |描述 |
|---------|---------|
|惡意的攻擊 |這些攻擊的偵測是透過尋找已知攻擊類型清單，包括從攻擊：<ul><li>傳遞票證 (PtT)</li><li>Pass-雜湊 (PtH)</li><li>-Overpass-the-hash</li><li>偽造的 PAC (MS14-068)</li><li>黃金票證</li><li>惡意的複寫</li><li>偵察</li><li>暴力密碼破解</li><li>遠端執行</li></ul>如需可以偵測到的惡意攻擊的完整清單及其描述中，請參閱[可疑活動 ATA 可以偵測哪些？](https://docs.microsoft.com/advanced-threat-analytics/understand-explore/ata-threats)。|
|異常行為 |這些攻擊會使用行為分析來偵測，並使用機器學習服務來找出可疑的活動，包括：<ul><li>異常登入</li><li>不明的威脅</li><li>共用密碼</li><li>橫向移動</li></ul>|
|安全性問題和風險 |藉由查看目前的網路和系統組態，偵測到這些攻擊包括：<ul><li>信任中斷</li><li>弱式通訊協定</li><li>已知的通訊協定弱點</li></ul>|

您可以使用 ATA，以協助偵測攻擊者嘗試侵入特殊權限的身分識別。 如需有關如何部署 ATA 的詳細資訊，請參閱中的計劃、 設計和部署主題[Advanced Threat Analytics 文件](https://docs.microsoft.com/advanced-threat-analytics/)。

## <a name="related-content-for-associated-windows-server-2016-solutions"></a>相關聯的 Windows Server 2016 解決方案的相關的內容

- **Windows Defender 防毒軟體：** https://www.youtube.com/watch?v=P1aNEy09NaI 和 https://docs.microsoft.com/windows/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10

- **Windows Defender 進階威脅防護：** https://www.youtube.com/watch?v=qxeGa3pxIwg 和 https://docs.microsoft.com/windows/threat-protection/windows-defender-atp/configure-server-endpoints-windows-defender-advanced-threat-protection

- **Windows Defender Device Guard:** https://www.youtube.com/watch?v=F-pTkesjkhI 和 https://docs.microsoft.com/windows/device-security/device-guard/device-guard-deployment-guide

- **Windows Defender Credential Guard:** https://www.youtube.com/watch?v=F-pTkesjkhI 和 https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard

- **控制流程防護：** https://msdn.microsoft.com/library/windows/desktop/mt637065(v=vs.85).aspx

- **安全性和保證：** https://docs.microsoft.com/windows-server/security/security-and-assurance

## <a name="disclaimer"></a>免責聲明
本文代表 Microsoft 於截至文件發行當日為止對於 GDPR 的探討觀點。 我們花費很多時間研究 GDPR，因此認為我們已充分理解其用途和意義。 但 GDPR 的應用與具體事實高度相關，並非 GDPR 的所有面向與解讀都符合現況。

因此，本文僅供參考，不應做為法律建議或用來判斷 GDPR 會如何套用到您與您的組織。 我們鼓勵您與具具有法律資格的專業人員討論 GDPR、其如何具體套用至您的組織，以及如何最理想地確保合規性。

MICROSOFT 對於本文中的資訊不負任何明示、默示或法定擔保責任。 本文是以原本的形式提供。 本文中提供的資訊和檢視，包括URL 及其他網際網路網站參考資料，可能會依情況改變，恕不另行通知。

本文不為您提供對任何 Microsoft 產品中的任何智慧財產的法定權利。  您可在僅供內部參考的用途下複製本文。  

發行日期：2017 年 9 月<br>
版本 1.0<br>
© 2017 Microsoft. 保留一切權利。


