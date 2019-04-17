---
title: "軟體限制原則技術概觀"
description: "Windows Server 安全性"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="software-restriction-policies-technical-overview"></a>軟體限制原則技術概觀

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題描述軟體限制原則的時機，以及如何使用此功能，變更已實作在過去版本中，並提供額外的資源，協助您建立和部署軟體限制原則與 Windows Server 2008 和 Windows Vista 開頭的連結。

## <a name="introduction"></a>簡介
軟體限制原則提供系統管理員使用群組原則導向機制來找出軟體和控制能力的本機電腦上執行。 這些原則，可用來保護電腦免於已知衝突執行 Microsoft 的 Windows 作業系統 （與 Windows Server 2003 及 Windows XP Professional 開頭），並保護電腦免於惡意病毒和特洛伊木馬程式安全性威脅。 您也可以使用軟體限制原則來建立高度限制的電腦，您可讓只專門辨識應用程式執行設定。 整合軟體限制原則與 Microsoft Active Directory 群組原則。 您也可以在獨立的電腦上建立的軟體限制原則。

軟體限制原則是信任原則的系統管理員的身分為限制指令碼和其他驗證碼不會完全無法執行受信任的規範。 軟體限制原則延伸到本機群組原則編輯器提供可以來管理設定限制的應用程式使用的單一使用者介面本機電腦上或在網域。

## <a name="procedures"></a>程序

-   [管理軟體限制原則](administer-software-restriction-policies.md)

    -   [軟體限制原則判斷拒絕允許清單與應用程式清單](determine-allow-deny-list-and-application-inventory-for-software-restriction-policies.md)

    -   [使用軟體限制原則規則](work-with-software-restriction-policies-rules.md)

    -   [為了協助保護您的電腦免受電子郵件病毒使用軟體限制原則](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

-   [疑難排解軟體限制原則](troubleshoot-software-restriction-policies.md)

## <a name="software-restriction-policy-usage-scenarios"></a>軟體限制原則使用量案例
商務使用者合作，使用電子郵件、 立即訊息、 和對等應用程式。 增加這些合作，尤其是藉由使用網際網路在公司電腦中，因此執行的惡意程式碼，例如蠕蟲、 病毒和惡意的使用者或攻擊者威脅的威脅。

使用者可能會收到許多形式，原生 Windows 可執行檔 （.exe 檔案） 從巨集 （例如.doc 檔案）、 文件中的惡意程式碼指令碼 （例如.vbs 檔案）。 使用者惡意或攻擊者經常使用取得執行程式碼包含病毒、 蠕蟲使用者社交方法。 （社交是到洩露他們的密碼或某種類型的安全性資訊欺騙連絡人的詞彙。）如果便會觸動這類程式碼，它可以產生阻服務攻擊網路上的、 敏感或私密資料傳送至網際網路、 讓電腦的安全性風險，或損壞到硬碟。

IT 組織和使用者必須能判斷安全地執行的軟體，但不。 大量和惡意程式碼可以進行表單，這將會變成困難的工作。

為了協助保護其電腦的未知或不支援的軟體和惡意程式碼，組織可以實作軟體限制原則他們整體安全性策略的一部分。

系統管理員可以使用軟體限制原則下列任務：

-   定義信任的程式碼

-   設計彈性的群組原則的規範指令碼，可執行檔和 ActiveX 控制項

作業系統和應用程式 （例如指令碼處理的應用程式） 符合使用軟體限制原則，會執行軟體限制原則。

具體而言，系統管理員可以使用軟體限制原則下列目的：

-   指定 （可執行檔） 的軟體可以 client 電腦上執行

-   防止共用的電腦上執行特定程式

-   指定誰可以 client 電腦新增受信任的發行者

-   設定的軟體限制原則 （指定原則會影響所有使用者是否或 client 電腦的使用者子集） 範圍

-   防止可執行檔的本機電腦，單位 （組織單位）、 網站或網域上執行。 您不具有惡意的使用者使用軟體限制原則潛在問題時，這是適用於案例。

## <a name="BKMK_Diffs_Changes"></a>不同和變更的功能
SRP for Windows Server 2012 和 Windows 8 中的功能有任何變更。

**支援的版本**

軟體限制原則可只在與套用到電腦，最少執行 Windows Server 2003，包括 Windows Server 2012，而至少 Windows XP 中，包括 Windows 8。

> [!NOTE]
> 某些版本的 Windows client 作業系統開始在 Windows vista 不需要軟體限制原則。 不透過群組原則管理網域中的電腦可能會不會收到分散式的原則。

**比較軟體限制原則與 AppLocker 應用程式控制項函式**

下表比較軟體限制原則 (SRP) 的功能與 AppLocker 函式的功能。

|應用程式控制功能|SRP|AppLocker|
|----------------|----|-------|
|範圍|可以 SRP 原則套用到所有 Windows 作業系統開始使用 Windows XP 和 Windows Server 2003。|AppLocker 原則僅適用於 Windows Server 2008 R2、 Windows Server 2012、 Windows 7 和 Windows 8。|
|建立原則|SRP 原則會保留透過群組原則和只有 GPO 的系統管理員可以更新 SRP 原則。 在本機電腦上的系統管理員可以修改在本機 GPO 定義 SRP 原則。|透過群組原則維護 AppLocker 原則，只有 GPO 的系統管理員可以更新的原則。 在本機電腦上的系統管理員可以在本機 GPO 定義 AppLocker 原則來修改。<br /><br />AppLocker 允許錯誤訊息，以直接網頁以取得協助使用者的自訂項的目。|
|原則維護|SRP 原則必須使用本機安全性原則嵌入式管理單元 （如果原則建立本機） 或群組原則管理主控台 (GPMC) 來更新。|AppLocker 原則可以使用 [本機安全性原則嵌入式管理單元 （如果原則建立本機），或在 GPMC 中或 Windows PowerShell AppLocker cmdlet 來更新。|
|原則應用程式|透過群組原則分散 SRP 原則。|透過群組原則分散 AppLocker 原則。|
|執法模式|SRP 適用於「拒絕清單模式」的系統管理員，可以建立規則不允許在企業中的其餘部分檔案的預設允許執行而想要的檔案。<br /><br />您也可以在 [允許清單模式」設定 SRP 的預設會封鎖所有的檔案，系統管理員需要建立允許的檔案，他們希望規則。|AppLocker 的預設適用於「允許清單模式」只那些檔案的允許執行的那里是相對應的允許規則。|
|檔案類型，就可以控制|SRP 可以控制下列檔案類型：<br /><br />-的可執行檔<br />-Dll<br />-指令碼<br />Windows 安裝程式<br /><br />SRP 分開無法控制每個檔案類型。 所有 SRP 規則都的單一規則集合中。|AppLocker 可以控制下列檔案類型：<br /><br />-的可執行檔<br />-Dll<br />-指令碼<br />Windows 安裝程式<br />-\ [已封裝的應用程式並安裝程式 （Windows Server 2012 和 Windows 8）<br /><br />AppLocker 的五個檔案類型的每個維護不同規則的收藏。|
|指定的檔案類型|SRP 支援檔案類型被認為是可執行檔延伸的清單。 系統管理員可以將新增檔案，都被視為可執行檔的擴充的功能。|AppLocker 不支援此。 AppLocker 目前支援下列檔案擴充功能：<br /><br />-的可執行檔 （.exe、.com）<br />-Dll （.ocx、.dll）<br />-指令碼 （.vbs、.js、.ps1、.cmd、.bat）<br />.msi、.mst （.msp） 的 Windows 安裝程式<br />-已封裝應用程式安裝程式 (.appx)|
|規則類型|SRP 支援四種類型的規則：<br /><br />-Hash<br />路徑<br />簽章<br />網際網路區域|AppLocker 支援三種類型的規則：<br /><br />-Hash<br />路徑<br />-發行者|
|編輯 hash 值。|SRP 可讓系統管理員提供自訂 hash 值。|AppLocker 計算本身 hash 值。 內部可移植可執行檔 （執行檔和 Dll） 及 Windows 安裝程式與其他 SHA1 一般檔案 hash 使用湊 SHA1 驗證碼。|
|不同的安全性等級的支援|使用 SRP 系統管理員可以指定的應用程式可以執行的權限。 因此，系統管理員可以設定此類規則該記事本一律會執行的受限權限和從未使用系統管理員權限。<br /><br />Windows Vista 或更早版本 SRP 支援多安全性層級。 Windows 7 該清單是限於只兩種層級： 允許並不受限制 （基本的使用者轉換到不允許）。|AppLocker 不支援的安全性層級。|
|管理 Packaged 應用程式及 Packaged 應用程式安裝程式|無法|.appx 是有效的檔案類型，可以管理 AppLocker。|
|目標使用者或群組中的使用者規則|SRP 規則適用於特定電腦上所有使用者。|AppLocker 規則可以鎖定特定的使用者或群組中的使用者。|
|支援規則例外|SRP 不支援規則例外|AppLocker 規則可能例外讓系統管理員建立規則，例如 [允許所有項目從 Windows 除了 Regedit.exe」。|
|稽核模式的支援|SRP 不支援稽核模式。 測試 SRP 原則的唯一方式是設定測試環境，以及執行一些實驗。|AppLocker 支援稽核模式可讓系統管理員實際執行環境中測試他們原則的影響，而不影響的使用者體驗。 一旦您滿意結果，就可以開始執行的原則。|
|支援匯出與匯入原則|SRP 不支援原則匯入匯出。|AppLocker 支援匯入及匯出的原則。 這可讓您建立 AppLocker 原則範例在電腦上，測試然後匯出該原則和匯入想要 GPO 回。|
|執法規則|內部，SRP 規則執法交貨使用者-模式中的較不安全。|內部，AppLocker Exe 和 Dll 規則的執行核心模式，比使用者模式中執行這些更安全。|

## <a name="system-requirements"></a>系統需求
軟體限制原則可只在與套用到電腦，最少執行 Windows Server 2003，而至少 Windows XP。 群組原則，才能散發包含軟體限制原則的群組原則物件。

## <a name="software-restriction-policies-components-and-architecture"></a>軟體限制原則元件和架構
軟體限制原則提供機制作業系統和應用程式的軟體限制原則與相容限制執行階段的執行的軟體程式。

高階，軟體限制原則包含下列元件：

-   軟體限制原則 API。 應用程式介面 (Api) 用來建立和設定的軟體限制原則構成規則。 也有軟體限制原則 Api 的查詢、 處理，並執行的軟體限制原則。

-   軟體限制原則管理工具。 這包含**軟體限制原則**的擴充功能**本機群組原則物件編輯器**嵌入式管理單元，建立和編輯軟體限制原則的系統管理員使用。

-   一組作業系統的 Api 和應用程式，將軟體限制原則提供執法軟體限制原則執行階段 Api。

-   Active Directory，群組原則。 傳播軟體限制原則的 Active Directory 戶端適當，以及範圍及篩選這些原則的應用程式的適當的目標電腦的群組原則基礎結構軟體限制原則而定。

-   驗證碼與 WinVerify 信任 Api 可用來處理簽署的可執行檔。

-   事件檢視器。 使用軟體限制原則登入事件的事件檢視器登的功能。

-   結果設定的原則 (RSoP)，可協助有效的原則，將會套用到 client 的診斷。

如需有關 SRP 架構方式 SRP 管理規則處理程序，互動，查看[如何軟體限制原則運作](https://technet.microsoft.com/library/cc786941(v=WS.10).aspx)Windows Server 2003 技術的媒體櫃中。

## <a name="BKMK_Best_Practices"></a>最佳做法

### <a name="do-not-modify-the-default-domain-policy"></a>請勿修改預設網域原則。

-   如果您未進行編輯預設網域原則，您隨時可以選擇若發生使用您的自訂的網域原則套用預設網域原則。

### <a name="create-a-separate-group-policy-object-for-software-restriction-policies"></a>建立不同的群組原則物件的軟體限制原則。

-   如果您建立不同群組原則物件 (GPO) 軟體限制原則，您可以停用您的網域原則的其餘部分不來停用緊急軟體限制的原則。

### <a name="if-you-experience-problems-with-applied-policy-settings-restart-windows-in-safe-mode"></a>如果您遇到套用的原則設定的問題時，「 安全模式重新開機。

-   Windows 開始使用 「 安全模式時，不會套用軟體限制原則。 如果您不小心鎖定工作站使用軟體限制原則，電腦重新開機到 「 安全模式、 在本機系統管理員身分登入，修改原則、 執行**gpupdate**、 重新開機，以及通常登入。

### <a name="use-caution-when-defining-a-default-setting-of-disallowed"></a>定義不允許的預設設定時，請務必小心。

-   當您定義的預設設定**不允許**，除了明確允許的軟體不被允許所有標示的軟體。 有軟體限制原則規則，讓它開放對任何您想要開放的檔案。

-   若要防止退出系統鎖定時的預設安全性層級設定為系統管理員**不允許**、 四個會自動建立登錄路徑規則。 您可以 delete 或修改這些登錄的路徑規則。不過，建議您不要。

### <a name="for-best-security-use-access-control-lists-in-conjunction-with-software-restriction-policies"></a>最佳的安全性，以搭配使用軟體限制原則與使用存取控制清單。

-   使用者可能會嘗試重新命名或移動不允許的檔案或覆寫檔案不受限制避開軟體限制原則。 如此一來，建議您即可授權使用者所需執行這些工作，使用存取控制清單 (Acl)。

### <a name="test-new-policy-settings-thoroughly-in-test-environments-before-applying-the-policy-settings-to-your-domain"></a>先套用到您的網域原則設定測試環境中完全測試新原則設定。

-   新的原則設定可能會做原始預期的不同。 測試減少您在網路上部署原則設定時遇到問題的機會。

-   您可以設定測試網域中，您組織的網域中的測試新原則設定的不同。 您也可以建立測試 GPO 並連結到測試組織單位測試原則設定。 當您擁有完全測試測試使用者的原則設定時，您可以將測試 GPO 連結到您的網域。

-   程式集或檔案，因此不設定**不允許**以查看哪些可能會影響測試。 限制某些檔案可以嚴重會影響您的電腦或網路的作業。

-   輸入不正確的資訊或輸入錯誤，會導致並不會如預期般運作執行的原則設定。 先套用測試新原則設定可防止未預期的行為。

### <a name="filter-user-policy-settings-based-on-membership-in-security-groups"></a>篩選依據安全性群組成員資格使用者原則設定。

-   您可以指定的使用者或群組，您不想要清除套用原則設定**適用於群組原則**和**朗讀**核取方塊，這位於**安全性**] 索引標籤的 [GPO。

-   當遭拒朗讀權限時，原則設定不是電腦所下載。 如此一來，較少的頻寬耗用下載不必要原則設定可讓網路更快速地運作。 若要拒絕朗讀權限，選取 [**拒絕**的**朗讀**核取方塊，這位於**安全性**索引標籤的 [GPO。

### <a name="do-not-link-to-a-gpo-in-another-domain-or-site"></a>不要到另一個網域或網站 GPO 連結。

-   在另一部網域或網站 GPO 連結，會導致效能不佳。

## <a name="additional-resources"></a>其他資源

|內容類型|資訊尋找參考資料|
|--------|-------|
|**規劃**|[軟體限制原則技術參考](https://technet.microsoft.com/library/cc728085(v=WS.10).aspx)|
|**作業**|[管理軟體限制原則](administer-software-restriction-policies.md)|
|**疑難排解**|[疑難排解 (2003) 軟體限制原則](https://technet.microsoft.com/library/cc737011(v=WS.10).aspx)|
|**安全性**|[威脅和措施的軟體限制原則 (2008)](https://technet.microsoft.com/library/dd349795(v=WS.10).aspx)<br /><br />[威脅和措施的軟體限制原則 (2008 R2)](https://technet.microsoft.com/library/hh125926(v=WS.10).aspx)|
|**工具和設定**|[軟體限制原則工具和設定 (2003)](https://technet.microsoft.com/library/cc782454(v=WS.10).aspx)|
|**社群資源**|[應用程式鎖定使用軟體限制原則](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|


