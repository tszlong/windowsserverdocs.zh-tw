---
title: 軟體限制原則技術概觀
description: Windows Server 安全性
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc7013b0-0efd-40fd-bd6d-75128adbd0b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: d007d55ced9c6a18581eaedb4edb66db9eeccab9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830849"
---
# <a name="software-restriction-policies-technical-overview"></a>軟體限制原則技術概觀

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

本主題說明軟體限制原則，何時以及如何使用功能，哪些變更已在過去的版本中，實作，並提供可協助您建立及部署以 Windows 為開頭的軟體限制原則的其他資源連結Server 2008 和 Windows Vista。

## <a name="introduction"></a>簡介
軟體限制原則提供系統管理員使用群組原則導向的機制，用以識別軟體，並控制其本機電腦上執行的能力。 這些原則可以用來保護電腦對抗已知的衝突執行 Microsoft Windows 作業系統 （從 Windows Server 2003 和 Windows XP Professional），並保護的電腦遭受安全性威脅，例如惡意的病毒和特洛伊木馬程式。 您也可以使用軟體限制原則，來建立高限制性的電腦設定，您可以只允許執行特別識別出的應用程式。 軟體限制原則已與 Microsoft Active Directory 和群組原則整合。 您也可以在獨立電腦上建立軟體限制原則。

軟體限制原則是信任原則，這些原則是由系統管理員設定的規範，可以限制未受到完全信任的指令碼和其他程式碼的執行。 軟體限制原則延伸到本機群組原則編輯器 」 中提供單一使用者介面，可以透過管理限制使用的應用程式的設定，在本機電腦上或整個網域。

## <a name="procedures"></a>程序

-   [管理軟體限制原則](administer-software-restriction-policies.md)

    -   [決定軟體限制原則的允許拒絕清單和應用程式清查](determine-allow-deny-list-and-application-inventory-for-software-restriction-policies.md)

    -   [使用軟體限制原則規則](work-with-software-restriction-policies-rules.md)

    -   [使用軟體限制原則來協助保護電腦免於電子郵件病毒](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

-   [疑難排解軟體限制原則](troubleshoot-software-restriction-policies.md)

## <a name="software-restriction-policy-usage-scenarios"></a>軟體限制原則使用方式案例
商務使用者共同作業使用的電子郵件、 立即訊息和對等應用程式。 這些共同作業的增加，尤其是，利用網際網路商務運算，因此執行從惡意程式碼，例如蠕蟲、 病毒和惡意使用者或攻擊者的威脅的威脅。

使用者可能會收到許多形式，範圍從原生的 Windows 可執行檔 （.exe 檔），（例如.doc 檔案） 的文件中的巨集到的惡意程式碼 （例如.vbs 檔案） 的指令碼。 惡意使用者或攻擊者通常使用社交工程方法來取得使用者可執行程式碼包含病毒和蠕蟲。 （社交工程是誘騙使用者洩露密碼或某種形式的安全性資訊的詞彙）。如果這類程式碼已啟動，它可以產生網路上的阻絕服務攻擊、 將敏感或私人資料傳送到網際網路、 放置電腦的安全性風險的情況下，或損害硬碟上的內容。

IT 組織和使用者必須能夠判斷哪些軟體是安全地執行而不是。 大型的數字和惡意程式碼可採取的形式，這變得困難的工作。

為了協助保護他們的網路電腦的惡意程式碼和不明或不受支援的軟體，組織可以實作為其整體安全性策略的一部分的軟體限制原則。

系統管理員可以針對下列工作使用軟體限制原則：

-   定義什麼是受信任的程式碼

-   針對規範指令碼、可執行檔，以及 ActiveX 控制項設計彈性的群組原則。

軟體限制原則是透過遵循軟體限制原則的作業系統和應用程式 (例如，指令碼應用程式) 強制執行的。

具體而言，系統管理員可以基於下列目的使用軟體限制原則：

-   指定用戶端電腦上執行的軟體 （也就是可執行檔）

-   防止使用者在共用電腦上執行特定的程式

-   指定誰可以將受信任的發行者新增至用戶端電腦

-   設定 （指定原則是否會影響所有使用者，或用戶端電腦上的使用者子集） 的軟體限制原則的範圍

-   防止可執行檔在本機電腦、組織單位 (OU)、站台或網域上執行。 當您未使用軟體限制原則來處理惡意使用者的潛在問題時，這個狀況便適用此做法。

## <a name="BKMK_Diffs_Changes"></a>差異與功能的變更
SRP 適用於 Windows Server 2012 和 Windows 8 中的功能沒有任何變更。

**支援的版本**

軟體限制原則可以只上設定並套用到電腦至少執行 Windows Server 2003，包括 Windows Server 2012，且至少 Windows XP 中，包括 Windows 8。

> [!NOTE]
> 特定版本的 Windows 用戶端作業系統開始，Windows vista 中並沒有軟體限制原則。 不由群組原則管理的網域中的電腦可能不會收到分散式的原則。

**比較軟體限制原則與 AppLocker 中的應用程式控制功能**

下表比較軟體限制原則 (SRP) 功能與 AppLocker 的功能。

|應用程式控制函式|SRP|AppLocker|
|----------------|----|-------|
|領域|SRP 原則可以套用到從 Windows XP 和 Windows Server 2003 開始的所有 Windows 作業系統。|AppLocker 原則僅適用於 Windows Server 2008 R2、 Windows Server 2012、 Windows 7 和 Windows 8。|
|建立原則|SRP 原則會維護透過群組原則，並只 GPO 的系統管理員可以更新 SRP 原則。 在本機電腦上的系統管理員可以修改定義本機 GPO 中 SRP 原則。|AppLocker 原則透過群組原則維護和只有 GPO 的系統管理員可以更新原則。 在本機電腦上的系統管理員可以修改本機 GPO 中定義 AppLocker 原則。<br /><br />AppLocker 允許自訂錯誤訊息，將使用者導向至網頁尋求協助。|
|原則維護|SRP 原則，請使用 本機安全性原則嵌入式管理單元 （如果原則在本機建立） 或群組原則管理主控台 (GPMC) 必須更新。|使用本機安全性原則嵌入式管理單元 （如果原則在本機建立），或在 GPMC 中或 Windows PowerShell 的 AppLocker cmdlet，可以更新 AppLocker 原則。|
|原則的應用程式|SRP 原則是透過群組原則散發。|AppLocker 原則是透過群組原則散發。|
|強制模式|SRP 適用於 「 拒絕清單模式 」 系統管理員可以在其中建立規則，它們不會想要允許企業中，而其餘的檔案都可以執行預設的檔案。<br /><br />SRP 也可以設定 「 允許清單模式 」 中，依預設會封鎖所有檔案和系統管理員必須建立允許規則，他們想要允許的檔案。|依預設可在 「 允許清單模式 」 只需將這些檔案都可以執行，那里為相對應的允許規則的 AppLocker。|
|您可以控制的檔案類型|SRP 可以控制下列檔案類型：<br /><br />可執行檔<br />-Dll<br />-指令碼<br />-Windows 安裝程式<br /><br />SRP 無法分別控制每個檔案類型。 所有的 SRP 規則都以單一規則集合。|AppLocker 可以控制下列檔案類型：<br /><br />可執行檔<br />-Dll<br />-指令碼<br />-Windows 安裝程式<br />包裝應用程式和安裝程式 （Windows Server 2012 和 Windows 8）<br /><br />AppLocker 會維護的五個檔案類型的每個不同的規則集合。|
|指定的檔案類型|SRP 支援的可延伸的清單，會被視為可執行檔的檔案類型。 系統管理員可以新增擴充功能應該視為可執行檔的檔案。|AppLocker 不支援這個。 AppLocker 目前支援下列副檔名：<br /><br />可執行檔 （.exe、.com）<br />-Dll （.ocx、.dll）<br />-指令碼 （.vbs、.js、.ps1、.cmd、.bat）<br />-Windows 安裝程式 （.msi、.mst、.msp）<br />-已封裝應用程式安裝程式 (.appx)|
|規則類型|SRP 支援四種規則類型：<br /><br />雜湊<br />-   Path<br />簽章<br />網際網路區域|AppLocker 支援三種規則類型：<br /><br />雜湊<br />-   Path<br />-   Publisher|
|編輯的雜湊值|SRP 可讓系統管理員提供自訂的雜湊值。|AppLocker 會計算雜湊值本身。 在內部它會使用 SHA1 Authenticode 雜湊可攜式執行檔 （Exe 和 Dll） 與 Windows 安裝程式和 rest 的 SHA1 一般檔案雜湊值。|
|不同的安全性層級的支援|藉助於 SRP 的系統管理員可以指定應用程式可以執行的權限。 因此，系統管理員可以設定這類的規則以限制權限，並永遠不會使用系統管理權限，一定會執行該 「 記事本 」。<br /><br />在 Windows Vista 及更早版本的 SRP 支援多個安全性層級。 在 Windows 7 上該清單已限制為只有兩個層級：不允許和不受限制的 （基本的使用者會轉譯為不允許）。|AppLocker 不支援的安全性層級。|
|管理已封裝應用程式和已封裝應用程式安裝程式|無法|.appx 是有效的檔案類型，可以管理 AppLocker。|
|目標使用者或使用者群組的規則|SRP 規則套用到特定電腦上的所有使用者。|AppLocker 規則可以鎖定特定使用者或一群使用者。|
|規則的例外狀況的支援|SRP 不支援規則例外狀況|AppLocker 規則可以有例外狀況，允許系統管理員建立規則，例如 「 允許所有項目從 Windows 除了 Regedit.exe"。|
|稽核模式的支援|SRP 不支援稽核模式。 若要設定測試環境，並執行一些實驗，是測試 SRP 原則的唯一方式。|AppLocker 支援稽核模式可讓系統管理員在實際執行環境中測試其原則的效果，而不會影響使用者體驗。 一旦您滿意結果時，您可以開始強制執行原則。|
|匯出和匯入原則支援|SRP 不支援原則匯入/匯出。|AppLocker 支援匯入和匯出的原則。 這可讓您建立 AppLocker 原則範例的電腦上，測試它然後將該原則匯出並匯回所需的 GPO。|
|規則強制執行|就內部而言，SRP 規則強制執行，就會在使用者模式也就是較不安全。|就內部而言，在核心模式也就是比強制使用者模式中執行的方式更安全的 Exe 和 Dll 的 AppLocker 規則會強制執行。|

## <a name="system-requirements"></a>系統需求
軟體限制原則可以只上設定並套用到電腦至少執行 Windows Server 2003，且至少 Windows XP。 群組原則，才能發佈包含軟體限制原則的群組原則物件。

## <a name="software-restriction-policies-components-and-architecture"></a>軟體限制原則元件和架構
軟體限制原則提供的作業系統和應用程式遵守軟體限制原則來限制執行階段執行的軟體程式的機制。

概括而言，軟體限制原則包含下列元件：

-   軟體限制原則的 API。 應用程式發展介面 (Api) 用來建立及設定構成軟體限制原則的規則。 此外，還有一些軟體限制原則適用於查詢、 處理、 Api 及強制執行的軟體限制原則。

-   軟體限制原則的管理工具。 這包含**軟體限制原則**的延伸模組**本機群組原則物件編輯器**嵌入式管理單元，來建立和編輯的軟體限制原則的哪些系統管理員使用。

-   一組作業系統 Api 和應用程式，就會呼叫 Api 用於提供軟體限制原則，在執行階段強制執行的軟體限制原則。

-   Active Directory 和群組原則。 軟體限制原則取決於傳播的軟體限制原則從 Active Directory 到適當的用戶端，以及範圍和篩選這些原則套用至適當的群組原則基礎結構目標電腦。

-   Authenticode 和 WinVerify 信任應用程式開發介面用來處理已簽署的可執行檔。

-   事件檢視器。 軟體限制原則事件檢視器記錄檔的記錄檔事件所使用的函式。

-   產生原則組 (RSoP)，這可以協助有效原則將套用到用戶端的診斷。

如需 SRP 的架構中，如何 SRP 管理規則、 處理程序和互動，請參閱[軟體限制原則的運作方式](https://technet.microsoft.com/library/cc786941(v=WS.10).aspx)在 Windows Server 2003 技術文件庫中。

## <a name="BKMK_Best_Practices"></a>最佳作法

### <a name="do-not-modify-the-default-domain-policy"></a>請勿修改預設網域原則。

-   如果您不要編輯預設網域原則，您一律可以選擇重新套用預設網域原則，如果您的自訂的網域原則發生錯誤。

### <a name="create-a-separate-group-policy-object-for-software-restriction-policies"></a>建立軟體限制原則的個別群組原則物件。

-   如果您建立個別群組原則物件 (GPO) 軟體限制原則時，您可以停在發生緊急狀況的軟體限制原則而不需要停用您的網域原則的其餘部分。

### <a name="if-you-experience-problems-with-applied-policy-settings-restart-windows-in-safe-mode"></a>如果您遇到已套用的原則設定的問題，Windows 安全模式重新啟動。

-   以安全模式啟動 Windows 時，不會套用軟體限制原則。 如果您不小心鎖定的工作站使用軟體限制原則，以安全模式重新啟動電腦、 本機系統管理員身分登入、 修改原則、 執行**gpupdate**、 重新啟動電腦，並正常登入。

### <a name="use-caution-when-defining-a-default-setting-of-disallowed"></a>定義預設值為不允許時，請務必小心。

-   當您定義的預設設定為**不允許**，除了已明確允許的軟體不被允許的所有軟體。 任何您想要開啟的檔案必須要有軟體限制原則規則，好讓它開啟。

-   若要防止系統管理員將自己鎖定在系統中，當預設的安全性等級設定為**不允許**、 四個會自動建立登錄路徑規則。 您可以刪除或修改這些登錄路徑規則中;不過，這不建議。

### <a name="for-best-security-use-access-control-lists-in-conjunction-with-software-restriction-policies"></a>如需最佳安全性，軟體限制原則搭配使用存取控制清單。

-   使用者可能會嘗試規避軟體限制原則，藉由重新命名或移動不允許的檔案或覆寫不受限制的檔案。 如此一來，建議您使用存取控制清單 (Acl)，以拒絕使用者執行這些工作所需的存取權。

### <a name="test-new-policy-settings-thoroughly-in-test-environments-before-applying-the-policy-settings-to-your-domain"></a>將原則設定套用至您的網域之前在測試環境中徹底測試新的原則設定。

-   新的原則設定的行為可能原本預期的不同。 進行測試可減少在您網路上部署原則設定時遇到問題的機會。

-   您可以設定測試的網域，為分開貴組織的網域中，在用來測試新的原則設定。 您也可以藉由建立測試 GPO，並將它連結至測試的組織單位測試的原則設定。 當您已經徹底測試過的測試使用者的原則設定時，可以將測試 GPO 連結到您的網域。

-   未設定程式或檔案，因此**不允許**未經測試以查看可能會有效果。 限制某些檔案可能會嚴重影響您的電腦或網路的作業。

-   輸入不正確的資訊，或輸入錯誤可能會導致一個原則設定，並不會如預期般執行。 在套用之前先測試新的原則設定可避免非預期的行為。

### <a name="filter-user-policy-settings-based-on-membership-in-security-groups"></a>篩選使用者原則設定，根據安全性群組的成員資格。

-   您可以指定使用者或群組，您不想藉由清除套用的原則設定**套用群組原則**並**讀取**核取方塊，都位於**安全性** 索引標籤的 GPO。

-   當讀取權限遭拒時，原則設定不會下載電腦。 如此一來，較少的頻寬被消耗下載不必要的原則設定可讓網路更快速地運作。 若要拒絕的讀取權限，請選取**拒絕**如**讀取**核取方塊，呾儂奻**安全性** 索引標籤的 GPO。

### <a name="do-not-link-to-a-gpo-in-another-domain-or-site"></a>不要連結至另一個網域或網站中的 GPO。

-   連結至另一個網域或網站中的 GPO，可能會導致效能不佳。

## <a name="additional-resources"></a>其他資源

|內容類型|參考|
|--------|-------|
|**規劃**|[軟體限制原則技術參考](https://technet.microsoft.com/library/cc728085(v=WS.10).aspx)|
|**操作**|[管理軟體限制原則](administer-software-restriction-policies.md)|
|**疑難排解**|[軟體限制原則疑難排解 (2003)](https://technet.microsoft.com/library/cc737011(v=WS.10).aspx)|
|**安全性**|[威脅與對策的軟體限制原則 (2008)](https://technet.microsoft.com/library/dd349795(v=WS.10).aspx)<br /><br />[威脅與對策的軟體限制原則 (2008 R2)](https://technet.microsoft.com/library/hh125926(v=WS.10).aspx)|
|**工具及設定**|[軟體限制原則工具及設定 (2003)](https://technet.microsoft.com/library/cc782454(v=WS.10).aspx)|
|**社群資源**|[使用軟體限制原則的應用程式鎖定](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|


