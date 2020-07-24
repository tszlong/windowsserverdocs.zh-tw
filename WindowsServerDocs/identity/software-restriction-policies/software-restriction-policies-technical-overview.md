---
title: 軟體限制原則技術概觀
description: Windows Server 安全性
ms.prod: windows-server
ms.technology: security-software-restriction-policies
ms.topic: article
ms.assetid: dc7013b0-0efd-40fd-bd6d-75128adbd0b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 9a9a44cd4ab1f4c8ee1c4dadf5cc09de2208b46a
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966160"
---
# <a name="software-restriction-policies-technical-overview"></a>軟體限制原則技術概觀

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題描述軟體限制原則、使用此功能的時機與方式、過去版本中已實行的變更，並提供其他資源的連結，協助您建立及部署從 Windows Server 2008 和 Windows Vista 開始的軟體限制原則。

## <a name="introduction"></a>簡介
軟體限制原則可為系統管理員提供群組原則導向的機制，以識別軟體並控制其在本機電腦上執行的能力。 這些原則可以用來保護執行 Microsoft Windows 作業系統的電腦（從 Windows Server 2003 和 Windows XP Professional 開始），以防範已知的衝突，並保護電腦免于遭受安全性威脅，例如惡意病毒和特洛伊木馬程式。 您也可以使用軟體限制原則，來建立高限制性的電腦設定，您可以只允許執行特別識別出的應用程式。 軟體限制原則已與 Microsoft Active Directory 和群組原則整合。 您也可以在獨立電腦上建立軟體限制原則。

軟體限制原則是信任原則，這些原則是由系統管理員設定的規範，可以限制未受到完全信任的指令碼和其他程式碼的執行。 本機群組原則編輯器的軟體限制原則延伸提供單一使用者介面，可讓您在本機電腦或整個網域中管理用來限制應用程式使用的設定。

## <a name="procedures"></a>程序

-   [管理軟體限制原則](administer-software-restriction-policies.md)

    -   [判斷軟體限制原則的允許拒絕清單和應用程式清查](determine-allow-deny-list-and-application-inventory-for-software-restriction-policies.md)

    -   [使用軟體限制原則規則](work-with-software-restriction-policies-rules.md)

    -   [使用軟體限制原則來協助保護您的電腦免于電子郵件病毒](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

-   [為軟體限制原則進行疑難排解](troubleshoot-software-restriction-policies.md)

## <a name="software-restriction-policy-usage-scenarios"></a>軟體限制原則使用案例
商務使用者使用電子郵件、立即訊息和對等應用程式共同作業。 隨著這些共同作業的增加，特別是在商務運算中使用網際網路時，就會面臨惡意程式碼的威脅，例如蠕蟲、病毒和惡意使用者或攻擊者的威脅。

使用者可能會從原生 Windows 可執行檔（.exe 檔），到檔（例如 .doc 檔案）中的宏，到腳本（例如 .vbs 檔案）中，收到有害的程式碼。 惡意使用者或攻擊者通常會使用社交工程方法，讓使用者執行包含病毒和蠕蟲的程式碼。 （社交工程是一種用來誘騙使用者洩漏其密碼或某種形式的安全性資訊）的詞彙。如果啟用這類程式碼，它可能會在網路上產生阻絕服務攻擊、將敏感或私人資料傳送至網際網路、將電腦的安全性放在風險中，或損毀硬碟的內容。

IT 組織和使用者必須能夠判斷哪些軟體可以安全執行，哪些則否。 有了惡意程式碼可以採用的大量和表單，這就變得很棘手。

為了協助保護其網路電腦免于惡意程式碼和不明或不受支援的軟體，組織可以在其整體安全性策略中實行軟體限制原則。

系統管理員可以針對下列工作使用軟體限制原則：

-   定義什麼是受信任的程式碼

-   針對規範指令碼、可執行檔，以及 ActiveX 控制項設計彈性的群組原則。

軟體限制原則是透過遵循軟體限制原則的作業系統和應用程式 (例如，指令碼應用程式) 強制執行的。

具體而言，系統管理員可以基於下列目的使用軟體限制原則：

-   指定可在用戶端電腦上執行的軟體（可執行檔）

-   防止使用者在共用電腦上執行特定的程式

-   指定可以將受信任的發行者新增至用戶端電腦的人員

-   設定軟體限制原則的範圍（指定原則是否會影響用戶端電腦上的所有使用者或使用者子集）

-   防止可執行檔在本機電腦、組織單位 (OU)、站台或網域上執行。 當您未使用軟體限制原則來處理惡意使用者的潛在問題時，這個狀況便適用此做法。

## <a name="differences-and-changes-in-functionality"></a><a name="BKMK_Diffs_Changes"></a>功能的差異和變更
Windows Server 2012 和 Windows 8 的 SRP 功能不會有任何變更。

**支援版本**

軟體限制原則只能在執行至少 Windows Server 2003 的電腦上設定，包括 Windows Server 2012，以及至少 Windows XP （包括 Windows 8）。

> [!NOTE]
> 從 Windows Vista 開始的特定 Windows 用戶端作業系統版本沒有軟體限制原則。 群組原則無法在網域中管理的電腦可能不會收到分散式原則。

**比較軟體限制原則和 AppLocker 中的應用程式控制功能**

下表比較軟體限制原則（SRP）功能和 AppLocker 的功能和功能。

|應用程式控制函式|SRP|AppLocker|
|----------------|----|-------|
|影響範圍|SRP 原則可以套用到從 Windows XP 和 Windows Server 2003 開始的所有 Windows 作業系統。|AppLocker 原則僅適用于 Windows Server 2008 R2、Windows Server 2012、Windows 7 和 Windows 8。|
|原則建立|SRP 原則會透過群組原則進行維護，而且只有 GPO 的系統管理員可以更新 SRP 原則。 本機電腦上的系統管理員可以修改本機 GPO 中定義的 SRP 原則。|AppLocker 原則會透過群組原則維護，只有 GPO 的系統管理員可以更新原則。 本機電腦上的系統管理員可以修改本機 GPO 中定義的 AppLocker 原則。<p>AppLocker 允許自訂錯誤訊息，將使用者導向至網頁尋求協助。|
|原則維護|SRP 原則必須使用本機安全性原則嵌入式管理單元（如果原則是在本機建立）或群組原則管理主控台（GPMC）來更新。|您可以使用本機安全性原則嵌入式管理單元（如果原則是在本機建立）、GPMC 或 Windows PowerShell AppLocker Cmdlet 來更新 AppLocker 原則。|
|原則應用程式|SRP 原則會透過群組原則散發。|AppLocker 原則會透過群組原則散發。|
|強制模式|SRP 以「拒絕清單模式」運作，其中系統管理員可以針對不想要在此企業中允許的檔案建立規則，而其餘的檔案則預設為可執行。<p>SRP 也可以在「允許清單模式」中設定，如此一來，預設會封鎖所有檔案，而且系統管理員必須為想要允許的檔案建立允許規則。|根據預設，AppLocker 會在「允許清單模式」中運作，其中只有那些檔案允許執行，其中有相符的允許規則。|
|可控制的檔案類型|SRP 可以控制下列檔案類型：<p>-可執行檔<br />-Dll<br />-腳本<br />-Windows 安裝程式<p>SRP 無法分別控制每個檔案類型。 所有 SRP 規則都在單一規則集合中。|AppLocker 可以控制下列檔案類型：<p>-可執行檔<br />-Dll<br />-腳本<br />-Windows 安裝程式<br />-已封裝的應用程式和安裝程式（Windows Server 2012 和 Windows 8）<p>AppLocker 會針對五種檔案類型分別維護一個規則集合。|
|指定的檔案類型|SRP 支援已被視為可執行檔案類型的可擴充清單。 系統管理員可以為應該視為可執行檔的檔案新增擴充功能。|AppLocker 不支援此功能。 AppLocker 目前支援下列副檔名：<p>-可執行檔（.exe、.com）<br />-Dll （.ocx、.dll）<br />-腳本（.vbs、.js、ps1、.cmd、.bat）<br />-Windows 安裝程式（.msi、.mst、.msp）<br />-已封裝的應用程式安裝程式（.appx）|
|規則類型|SRP 支援四種類型的規則：<p>-Hash<br />-Path<br />-Signature<br />-網際網路區域|AppLocker 支援三種類型的規則：<p>-Hash<br />-Path<br />-發行者|
|編輯雜湊值|SRP 可讓系統管理員提供自訂的雜湊值。|AppLocker 會計算雜湊值本身。 在內部，它會針對可移植的可執行檔（Exe 和 Dll）和 Windows 安裝程式使用 SHA1 Authenticode 雜湊，並針對其餘部分使用 SHA1 一般檔案雜湊。|
|支援不同的安全性層級|透過 SRP 系統管理員，可以指定應用程式可以執行的許可權。 因此，系統管理員可以設定規則，讓「記事本」一律以限制許可權執行，而不使用管理許可權。<p>Windows Vista 和更早版本上的 SRP 支援多個安全性層級。 在 Windows 7 上，清單僅限於兩個層級：不允許和不受限制（基本使用者會轉譯為不允許）。|AppLocker 不支援安全性層級。|
|管理已封裝的應用程式和已封裝的應用程式安裝程式|無法|.appx 是 AppLocker 可以管理的有效檔案類型。|
|將規則的目標設為使用者或使用者群組|SRP 規則適用于特定電腦上的所有使用者。|AppLocker 規則可以鎖定特定使用者或使用者群組。|
|支援規則例外狀況|SRP 不支援規則例外狀況|AppLocker 規則可能有例外狀況，可讓系統管理員建立規則，例如「允許來自 Windows 的所有專案，但 Regedit.exe 除外」。|
|支援 audit 模式|SRP 不支援 audit 模式。 測試 SRP 原則的唯一方式是設定測試環境，並執行一些實驗。|AppLocker 支援 audit 模式，可讓系統管理員在實際生產環境中測試其原則的效果，而不會影響使用者體驗。 當您對結果感到滿意之後，就可以開始強制執行原則。|
|支援匯出和匯入原則|SRP 不支援原則匯入/匯出。|AppLocker 支援匯入和匯出原則。 這可讓您在範例電腦上建立 AppLocker 原則、加以測試，然後匯出該原則，然後將它匯入回所需的 GPO。|
|強制執行規則|就內部而言，SRP 規則強制會以較不安全的使用者模式執行。|就內部而言，Exe 和 Dll 的 AppLocker 規則會在核心模式中強制執行，這比在使用者模式中強制執行它們更為安全。|

## <a name="system-requirements"></a>系統需求
軟體限制原則只能在至少執行 Windows Server 2003 和 Windows XP 的電腦上設定和套用。 需要群組原則，才能散發包含軟體限制原則的群組原則物件。

## <a name="software-restriction-policies-components-and-architecture"></a>軟體限制原則元件和架構
軟體限制原則提供一種機制，讓作業系統和應用程式符合軟體限制原則，以限制軟體程式的執行時間執行。

概括而言，軟體限制原則是由下列元件所組成：

-   軟體限制原則 API。 應用程式開發介面（Api）是用來建立及設定構成軟體限制原則的規則。 還有一些軟體限制原則 Api 可用於查詢、處理及強制執行軟體限制原則。

-   軟體限制原則管理工具。 這是由**本機群組原則物件編輯器**嵌入式管理單元的**軟體限制原則**延伸所組成，系統管理員會使用此擴充功能來建立和編輯軟體限制原則。

-   一組作業系統 Api 和應用程式，可呼叫軟體限制原則 Api，在執行時間提供強制執行軟體限制原則。

-   Active Directory 和群組原則。 軟體限制原則取決於群組原則基礎結構，將軟體限制原則從 Active Directory 傳播到適當的用戶端，以及將這些原則的應用程式設定為適當的目的電腦。

-   Authenticode 和 WinVerify 信任 Api，可用來處理已簽署的可執行檔。

-   事件檢視器。 軟體限制原則用來將事件記錄到事件檢視器記錄的功能。

-   原則結果組（RSoP），可協助診斷將套用至用戶端的有效原則。

如需 SRP 架構的詳細資訊，SRP 如何管理規則、處理常式和互動，請參閱 Windows Server 2003 技術文件庫中的[軟體限制原則的使用方式](/previous-versions/windows/it-pro/windows-server-2003/cc786941(v=ws.10))。

## <a name="best-practices"></a><a name="BKMK_Best_Practices"></a>最佳作法

### <a name="do-not-modify-the-default-domain-policy"></a>請勿修改預設網域原則。

-   如果您沒有編輯預設網域原則，當您的自訂網域原則發生問題時，一律可以選擇重新套用預設網域原則。

### <a name="create-a-separate-group-policy-object-for-software-restriction-policies"></a>為軟體限制原則建立個別的群組原則物件。

-   如果您為軟體限制原則建立個別的群組原則物件（GPO），您可以在緊急情況下停用軟體限制原則，而不需要停用其餘的網域原則。

### <a name="if-you-experience-problems-with-applied-policy-settings-restart-windows-in-safe-mode"></a>如果您遇到套用原則設定的問題，請在安全模式中重新開機 Windows。

-   當 Windows 以安全模式啟動時，不適用軟體限制原則。 如果您不小心鎖定具有軟體限制原則的工作站，請以安全模式重新開機電腦、以本機系統管理員的身分登入、修改原則、執行**gpupdate**、重新開機電腦，然後以一般方式登入。

### <a name="use-caution-when-defining-a-default-setting-of-disallowed"></a>將預設設定定義為 [不允許] 時，請務必小心。

-   當您將預設設定定義為 [不**允許**] 時，除了已明確允許的軟體以外，不允許所有軟體。 您想要開啟的任何檔案都必須具有可讓它開啟的軟體限制原則規則。

-   若要保護系統管理員不受系統鎖定，當預設的安全性等級設為 [不**允許**] 時，會自動建立四個登錄路徑規則。 您可以刪除或修改這些登錄路徑規則;不過，不建議您這樣做。

### <a name="for-best-security-use-access-control-lists-in-conjunction-with-software-restriction-policies"></a>為了獲得最佳安全性，請使用存取控制清單搭配軟體限制原則。

-   使用者可能會嘗試藉由重新命名或移動不允許的檔案，或覆寫不受限制的檔案來規避軟體限制原則。 因此，建議您使用存取控制清單（Acl）來拒絕使用者執行這些工作所需的存取權。

### <a name="test-new-policy-settings-thoroughly-in-test-environments-before-applying-the-policy-settings-to-your-domain"></a>請先在測試環境中徹底測試新的原則設定，再將原則設定套用至您的網域。

-   新原則設定的行為可能會與原本預期的不同。 測試可減少在網路上部署原則設定時遇到問題的機會。

-   您可以設定一個測試網域，與您組織的網域分開，以測試新的原則設定。 您也可以藉由建立測試 GPO 並將其連結至測試組織單位，來測試原則設定。 當您使用測試使用者徹底測試過原則設定時，您可以將測試 GPO 連結到您的網域。

-   請勿在未測試的情況下，將程式或檔案設定為不**允許**，以查看可能的效果。 某些檔案的限制可能會嚴重影響電腦或網路的操作。

-   輸入錯誤或輸入錯誤的資訊，可能會導致未如預期般執行的原則設定。 在套用新原則設定之前進行測試，可能會導致非預期的行為。

### <a name="filter-user-policy-settings-based-on-membership-in-security-groups"></a>根據安全性群組中的成員資格來篩選使用者原則設定。

-   您可以在 GPO 的 [內容] 對話方塊的 [**安全性**] 索引標籤上，清除 [套用**群組原則**] 和 [**讀取**] 核取方塊，以指定不想套用原則設定的使用者或群組。

-   拒絕讀取權限時，電腦不會下載原則設定。 因此，下載不必要的原則設定會耗用較少的頻寬，讓網路的運作速度更快。 若要拒絕讀取權限，請在 GPO 的 [內容] 對話方塊的 [**安全性**] 索引標籤上，選取 [**讀取**] 核取方塊的 [**拒絕**]。

### <a name="do-not-link-to-a-gpo-in-another-domain-or-site"></a>請勿連結至另一個網域或網站中的 GPO。

-   連結到另一個網域或網站中的 GPO 可能會導致效能不佳。

## <a name="additional-resources"></a>其他資源

|內容類型|參考|
|--------|-------|
|**規劃**|[軟體限制原則技術參考](/previous-versions/windows/it-pro/windows-server-2003/cc728085(v=ws.10))|
|**作業**|[管理軟體限制原則](administer-software-restriction-policies.md)|
|**疑難排解**|[軟體限制原則疑難排解（2003）](/previous-versions/windows/it-pro/windows-server-2003/cc737011(v=ws.10))|
|**安全性**|[軟體限制原則的威脅和因應對策（2008）](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349795(v=ws.10))<p>[軟體限制原則的威脅和因應對策（2008 R2）](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/hh125926(v=ws.10))|
|**工具及設定**|[軟體限制原則工具和設定（2003）](/previous-versions/windows/it-pro/windows-server-2003/cc782454(v=ws.10))|
|**社群資源**|[使用軟體限制原則鎖定應用程式](/previous-versions/technet-magazine/cc510322(v=msdn.10)?pr=blog)|
