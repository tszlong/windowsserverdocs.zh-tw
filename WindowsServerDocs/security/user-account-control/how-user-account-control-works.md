---
title: 使用者帳戶控制的運作方式
description: 瞭解 (UAC) 的使用者帳戶控制，以及它如何協助防止惡意軟體損害電腦，以及協助組織部署更妥善管理的桌面。
ms.topic: article
ms.assetid: da83ddb2-6182-417c-aa8e-0b47b2e17d13
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: 948ef5cc3145f3ecdb35415f77f316ed265b44bf
ms.sourcegitcommit: decb6c8caf4851b13af271d926c650d010a6b9e9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/13/2021
ms.locfileid: "98177617"
---
# <a name="how-user-account-control-works"></a>使用者帳戶控制的運作方式

>適用於：Windows Server (半年度管道)、Windows Server 2016

使用者帳戶控制 (UAC) 有助於防止惡意程式 (又稱為惡意程式碼) 破壞電腦，還可以幫助組織部署最佳管理桌面。 使用 UAC，應用程式和作業一律在非系統管理員帳戶的資訊安全內容下執行，除非系統管理員特別授與系統的系統管理員層級存取權。 UAC 可以封鎖未經授權應用程式的自動安裝，以及防止意外變更系統設定。

## <a name="uac-process-and-interactions"></a>UAC 處理常式和互動
每一個需要系統管理員存取權杖的應用程式，必須向系統管理員顯示提示以取得同意。 其中一個例外狀況是父系處理程序和子處理程序間存在的關係。 子處理程序會繼承父系處理程序的使用者存取權杖。 不過，父系和子處理程序二者的整合層級必須相同。 Windows Server 2012 藉由標示其完整性層級來保護進程。 整合層級的衡量依據是信任。 「高」整合應用程式負責執行修改系統資料的工作，例如磁碟分割應用程式，「低」整合應用程式則負責執行可能破壞作業系統的工作，例如網頁瀏覽器。 整合層級較低的應用程式無法修改整合層級較高的應用程式資料。 當標準使用者嘗試執行的應用程式需要系統管理員存取權杖，UAC 會要求使用者提供有效的系統管理員認證。

為了進一步瞭解此程式的發生情形，請務必參閱 Windows Server 2012 登入程式的詳細資料。

### <a name="windows-server-2012-logon-process"></a>Windows Server 2012 登入程式
下圖說明系統管理員及標準使用者的登入程序有何不同：

![示範系統管理員的登入程式與標準使用者的登入程式有何不同的圖解](../media/How-User-Account-Control-Works/UACWindowsLogonProcess.gif)

根據預設值，標準使用者和系統管理員會在標準使用者的資訊安全內容中存取資源以及執行應用程式。 當使用者登入電腦後，系統會為使用者建立存取權杖。 存取權杖包含授與使用者的存取權層級相關資訊，其中包括特定安全性識別碼 (SID) 以及 Windows 權限。

當系統管理員登入後，會為使用者建立兩個獨立的存取權杖：一個是標準使用者存取權杖，另一個是系統管理員存取權杖。 標準使用者存取權杖和系統管理員存取權杖包含相同的使用者特定資訊，但不包含 Windows 系統管理權限和 SID。 標準使用者存取權杖是用來啟動那些不執行系統管理工作的應用程式 (標準使用者應用程式)。 標準使用者存取權杖之後會再用來顯示桌面 (Explorer.exe)。 Explorer.exe 是父系處理程序，所有其他使用者啟動的處理程序都會繼承其存取權杖。 因此，除非使用者同意或者提供認證來核准應用程式使用完整的系統管理存取權杖，否則所有應用程式會以標準使用者的身分執行。

如果使用者是 Administrators 群組的成員，則可以使用標準使用者存取權杖登入、瀏覽網頁以及閱讀電子郵件。 當系統管理員需要執行需要系統管理員存取權杖的工作時，Windows Server 2012 會自動提示使用者進行核准。 這個提示稱為提高權限提示，使用本機安全性原則嵌入式管理單元 (Secpol.msc) 或群組原則，即可設定其行為。

> [!NOTE]
> 「提高許可權」一詞是用來參考 Windows Server 2012 中的程式，該程式會提示使用者同意或使用完整系統管理員存取權杖的認證。

### <a name="the-uac-user-experience"></a>UAC 使用者體驗
啟用 UAC 之後，標準使用者與管理員核准模式下的系統管理員，會有不同的體驗。 執行 Windows Server 2012 的建議且更安全的方法是將您的主要使用者帳戶設為標準使用者帳戶。 以標準使用者身分執行有助於大幅提升受管理環境的安全性。 使用內建的 UAC 提高權限元件，標準使用者可以輸入本機 Administrator 帳戶的有效認證，輕鬆執行系統管理工作。 標準使用者的預設內建 UAC 提高權限元件，就是指認證提示。

如果不想使用標準使用者身分執行，可以管理員核准模式下的系統管理員身分執行。 使用內建的 UAC 提高權限元件，本機 Administrators 群組的成員只要提供核准，就可以輕鬆執行系統管理工作。 管理員核准模式下，系統管理員的預設內建 UAC 提高權限元件，稱為同意提示。 使用本機安全性原則嵌入式管理單元 (Secpol.msc) 或群組原則，可以設定 UAC 提高權限提示行為。

**同意和認證提示**

啟用 UAC 之後，Windows Server 2012 會提示您同意，或在啟動需要完整系統管理員存取權杖的程式或工作之前，提示您輸入有效的本機系統管理員帳戶的認證。 此提示可以確保惡意軟體無法進行無訊息安裝。

**同意提示**

當使用者嘗試執行需要使用者系統管理存取權杖的工作時，就會顯示同意提示。 以下是 UAC 同意提示的螢幕擷取畫面。

![UAC 同意提示的螢幕擷取畫面](../media/How-User-Account-Control-Works/UACConsentPrompt.gif)

**認證提示**

當標準使用者嘗試執行需要使用者系統管理存取權杖的工作時，就會顯示認證提示。 您可以使用本機安全性原則嵌入式管理單元 (Secpol.msc) 或群組原則，設定此標準使用者預設提示行為。 系統管理員也需要將 [使用者帳戶控制: 在管理員核准模式，系統管理員之提高權限提示的行為]  原則設定值設定成 [提示認證]，借此提供他們的認證。

下列螢幕擷取畫面是 UAC 認證提示的範例。

![顯示 UAC 認證提示範例的螢幕擷取畫面](../media/How-User-Account-Control-Works/UACCredentialPrompt.gif)

**UAC 提高權限提示**

UAC 提高權限提示會依應用程式標示不同顏色，讓您可以立即識別應用程式的潛在安全性風險。 當應用程式嘗試以系統管理員的完整存取權杖執行時，Windows Server 2012 會先分析可執行檔，以判斷它的發行者。 應用程式會先根據可執行檔的發行者區分為三個類別： Windows Server 2012、發行者已驗證 (簽署) ，以及發行者未驗證 (不帶正負號的) 。 下圖說明 Windows Server 2012 如何判斷要向使用者呈現何種色彩提高許可權提示。

以下是提高權限提示的色彩編碼：

-   紅色背景加上紅色保護盾圖示：群組原則封鎖了應用程式或者應用程式出自被封鎖的發行者。

-   藍色背景加上藍色和金色盾牌圖示：應用程式是 Windows Server 2012 系統管理應用程式，例如主控台專案。

-   藍色背景加藍色保護盾圖示：使用 Authenticode 簽署的應用程式，而且受到本機電腦信任。

-   黃色背景加黃色保護盾圖示：未簽署或已簽署的應用程式，但不受本機電腦信任。

**保護盾圖示**

部分控制台項目，如 [日期和時間內容]，包含系統管理員和標準使用者作業的結合。 標準使用者可以查看時鐘以及變更時區，不過要想變更本機系統時間，就需要完整系統管理員存取權杖。 以下是 [ **日期和時間] 屬性** 主控台專案的螢幕擷取畫面。

![顯示 * * 日期和時間屬性 * * 主控台專案的螢幕擷取畫面](../media/How-User-Account-Control-Works/UACShieldIcon.gif)

[變更日期和時間] 按鈕上的保護盾圖示代表該處理程序需要完整的系統管理員存取權杖，而且會顯示 UAC 提高權限提示。

**保護提高權限提示**

將提示導向安全桌面，可以進一步保護提高權限處理程序。 同意和認證提示會在 Windows Server 2012 中預設顯示在安全桌面上。 只有 Windows 處理程序可以存取安全桌面。 若要取得更高的安全性層級，我們建議您繼續啟用 [使用者帳戶控制: 提示提升權限時切換到安全桌面] 原則設定。

可執行檔要求提高權限時，互動式桌面 (又稱為使用者桌面) 會切換至安全桌面。 安全桌面會將使用者桌面變暗，並顯示提高權限提示，您必須先回應該提示，才能繼續後續動作。 當使用者按一下 [是] 或 [否]，桌面就會切換回使用者桌面。

惡意程式碼會模仿安全桌面，不過將 [使用者帳戶控制: 在管理員核准模式，系統管理員之提升權限提示的行為] 原則設定設成 [提示要求同意] 後，即使使用者在模仿的安全桌面上按一下 [是]，惡意程式碼也不會被提高權限。 如果原則設定被設定成 [提示要求認證]，模仿認證提示的惡意程式碼或許可以從使用者蒐集認證。 不過，惡意程式碼不會得到提高後的權限而且系統會採取其他保護措施，讓惡意程式碼即使使用蒐集到的密碼也無法控制使用者介面。

雖然惡意程式碼會模仿安全桌面，不過除非使用者以前將惡意程式碼安裝到電腦上，否則不會發生這種問題。 由於啟用 UAC 之後，需要系統管理員存取權杖的處理程序就無法進行無訊息安裝，因此使用者必須按一下 [是]，或提供系統管理員認證，明確表示同意安裝該處理程序。 UAC 提高權限提示的具體行為，取決於群組原則的設定。

## <a name="uac-architecture"></a>UAC 架構
下圖提供 UAC 架構的詳細資訊。

![詳細說明 UAC 架構的圖表](../media/How-User-Account-Control-Works/UACArchitecture.gif)

若要進一步瞭解每個元件，請參閱下表：

|元件|描述|
|-------|--------|
|**使用者**||
|使用者執行需要權限的作業|如果作業變更了檔案系統或登錄，則會呼叫  Virtualization 。 所有其他作業會呼叫 ShellExecute。|
|ShellExecute|ShellExecute 會呼叫 CreateProcess。 ShellExecute 會從 CreateProcess 尋找 ERROR_ELEVATION_REQUIRED 錯誤。 如果收到錯誤，ShellExecute 便會呼叫應用程式資訊服務，試著藉由提高權限提示來執行要求的工作。|
|CreateProcess|如果應用程式需要提高權限，CreateProcess 會使用  ERROR_ELEVATION_REQUIRED 拒絕要求。|
|**系統**||
|應用程式資訊服務|一種系統服務，可以協助啟動那些需要一或多個提高的權限或使用者權限才能執行的應用程式，例如本機系統管理作業，以及需要更高整合層級的應用程式。 當需要提高權限且使用者同意 (取決於群組原則) 啟動這類應用程式後，應用程式資訊服務會使用系統管理使用者完整存取權杖為應用程式建立新的處理程序，幫助啟動應用程式。|
|提升 ActiveX 安裝|如果未安裝 ActiveX，系統會檢查 UAC 滑桿等級。 如果已安裝 ActiveX，就會選取 [使用者帳戶控制: 提示提升權限時切換到安全桌面] 群組原則。|
|檢查 UAC 滑桿等級|UAC 現在有 4 個通知等級可以選擇，還有一個用於選擇通知等級的滑桿：<p><ul><li>高<p>    如果滑桿設定為 [一律通知]，系統會檢查是否已經啟用安全桌面。</li><li>中<p>    如果滑桿設定成 [預設 - 只在程式嘗試變更我的電腦時才通知我]，則會選取 [使用者帳戶控制: 僅針對已簽署與驗證過的可執行檔，提升其權限] 原則設定：<p><ul><li>如果啟用該原則設定，在允許執行指定的可執行檔之前，會先強制執行公開金鑰基礎架構 (PKI) 憑證路徑驗證。</li><li>如果沒有啟用該原則設定 (預設值)，在允許執行指定的可執行檔之前，不會強制執行 PKI 憑證路徑驗證。 系統會選取 [使用者帳戶控制: 提示提升權限時切換到安全桌面] 群組原則設定。</li></ul></li><li>低<p>    如果滑桿是設定成 [程式嘗試變更我的電腦時才通知我 (不要將桌面變暗)]，則會呼叫 CreateProcess。</li><li>永不通知<p>    如果滑杆設定為 [ **不要通知我**]，當程式嘗試安裝或嘗試在電腦上進行任何變更時，UAC 提示將永遠不會通知。 **重要事項：**     不建議使用此設定。 這項設定與設定 [ **使用者帳戶控制：系統管理員在管理核准模式中的提高許可權提示的行為]** 原則設定為 [提高許可權， **而不提示**]。</li></ul>|
|安全桌面已啟用|系統會選取 [使用者帳戶控制: 提示提升權限時切換到安全桌面] 群組原則設定。<p>-如果啟用安全桌面，則不論系統管理員和標準使用者的提示行為原則設定為何，所有提高許可權要求都會移至安全桌面。<br />-如果未啟用安全桌面，則所有提高許可權要求都會移至互動式使用者的桌面，並使用系統管理員和標準使用者的每個使用者設定。|
|CreateProcess|CreateProcess 會呼叫 AppCompat、融合以及安裝程式偵測，評估應用程式是否需要提高權限。 然後會檢查可執行檔，判斷它要求的執行層級 (儲存在可執行檔的應用程式資訊清單中)。 如果資訊清單中指定之要求的執行層級與存取權杖不符，CreateProcess 就會失敗，並將錯誤 (ERROR_ELEVATION_REQUIRED) 傳回 ShellExecute。 |
|AppCompat|AppCompat 資料庫儲存應用程式的相容性更正項目資訊。|
|融合|融合資料庫儲存描述應用程式之應用程式資訊清單的資訊。 系統會更新資訊清單架構描述，以增加新要求的執行層級欄位。|
|安裝程式偵測|安裝程式偵測會偵測安裝程式可執行檔，有助於防止在使用者不知情且未同意的情況下執行安裝。|
|**核心**||
|虛擬化|虛擬化技術可以確保不相容的應用程式不會無訊息失敗，或者在不明情況下失敗。 UAC 也會為要在受保護區域中寫入資料的應用程式，提供檔案和登錄模擬以及紀錄功能。|
|檔案系統和登錄|每個使用者檔案和登錄模擬都會將每部電腦登錄和檔案寫入要求，重新導向到對等的每個使用者位置。 讀取要求會先重新導向到虛擬化的每個使用者位置，然後再到每部電腦位置。|

舊版 Windows 中的 Windows Server 2012 UAC 有變更。 新的滑杆永遠不會完全關閉 UAC。 新的設定將會：

-   讓 UAC 服務保持執行狀態。

-   讓系統管理員所起始的所有提高許可權要求都自動核准，而不會顯示 UAC 提示。

-   自動拒絕標準使用者的所有提高許可權要求。

> [!IMPORTANT]
> 若要完全停用 UAC，您必須停 **用原則使用者帳戶控制：在管理員核准模式中執行所有系統管理員**。

> [!WARNING]
> 當 UAC 停用時，量身打造的應用程式將無法在 Windows Server 2012 上運作。

### <a name="virtualization"></a>虛擬化
企業環境中的系統管理員為了保護系統，所以很多特定業務 (LOB) 應用程式都設計為只能使用標準使用者存取權杖。 如此一來，在執行已啟用 UAC 的 Windows Server 2012 時，IT 系統管理員就不需要取代大部分的應用程式。

 Windows Server 2012 包含適用于不符合 UAC 規範之應用程式的檔案和登錄虛擬化技術，而且需要系統管理員存取權杖才能正確執行。 虛擬化可確保即使與 UAC 不相容的應用程式都與 Windows Server 2012 相容。 與 UAC 不相容的系統管理應用程式嘗試在受保護的目錄 (例如 Program Files) 寫入資料時，UAC 會針對要嘗試變更的資源，提供該應用程式專屬的虛擬化檢視。 虛擬化複本會保留在使用者設定檔中。 此策略會針對執行不相容之應用程式的每個使用者建立個別的虛擬化檔案複本。

大部分應用程式工作都可以使用虛擬化功能正常運作。 雖然虛擬化可讓大部分應用程式順利執行，但這只是短期的修復，而非長期解決之道。 應用程式開發人員應該儘快修改其應用程式，使其符合 Windows Server 2012 標誌程式的規範，而不是依賴檔案、資料夾和登錄虛擬化。

虛擬化不適用於下列案例：

1.  虛擬化不適用於以完整系統管理存取權杖提升權限並執行的應用程式。

2.  虛擬化僅支援 32 位元的應用程式。 未提高權限的 64 位元應用程式嘗試取得 Windows 物件的控制碼 (唯一識別碼) 時，只會收到拒絕存取訊息。 原生 Windows 64 位元應用程式必須與 UAC 相容並將資料寫入正確位置。

3.  如果應用程式包含具有要求之執行層級屬性的應用程式資訊清單，則會停用應用程式的虛擬化。

### <a name="request-execution-levels"></a>要求執行層級
所謂應用程式資訊清單，就是說明並識別共用和私用並存組件的 XML 檔，這些並存組件是應用程式要在執行時間加以繫結的組件。 在 Windows Server 2012 中，應用程式資訊清單包含 UAC 應用程式相容性用途的專案。 在應用程式資訊清單中包含項目的系統管理應用程式，會提示使用者提供存取使用者存取權杖的權限。 雖然大部分系統管理應用程式在應用程式資訊清單中沒有相關的項目，但是還是可以使用應用程式相容性修正執行，且不需進行修改。 應用程式相容性修正是資料庫專案，可讓與 UAC 不相容的應用程式正常運作于 Windows Server 2012。

所有與 UAC 相容的應用程式都應該在應用程式資訊清單中加入要求的執行層級。 如果應用程式需要系統的系統管理存取權，將應用程式要求的執行層級標示為「需要系統管理員」，可以確保系統將此程式識別為系統管理應用程式，然後執行必要的提高權限步驟。 要求的執行層級會指定應用程式所需的權限。

### <a name="installer-detection-technology"></a>安裝程式偵測技術
安裝程式是為了部署軟體而設計的應用程式。 大部分的安裝程式會寫入系統目錄以及登錄機碼。 在安裝程式偵測技術中，這些受保護的系統位置通常只有系統管理員才能寫入資料，這表示標準使用者沒有足夠的存取權可以安裝程式。 Windows Server 2012 啟發式地會偵測安裝程式，並要求系統管理員使用者提供系統管理員認證或核准，才能以存取權限執行。 Windows Server 2012 也會啟發式地偵測卸載應用程式的更新和程式。 UAC 的設計目標之一就是防止在使用者不知情和未同意的情況下執行安裝，因為安裝程式會在檔案系統與登錄的受保護區域寫入資料。

安裝程式偵測只適用於下列各項：

-   32 位元可執行檔。

-   不具備要求的執行層級屬性的應用程式。

-   以標準使用者身分執行且啟用 UAC 的互動式處理程序。

建立 32 位元處理程序之前，會檢查下列屬性以判斷是否為安裝程式：

-   檔案名稱包含如 install、setup 或 update 等關鍵字。

-   [版本設定資源] 欄位包含下列關鍵字：Vendor、Company Name、Product Name、File Description、Original Filename、Internal Name 以及 Export Name。

-   內嵌到可執行檔的並排資訊清單關鍵字。

-   可執行檔連結的特定 StringTable 項目關鍵字。

-   可執行檔連結的資源指令碼資料機碼屬性。

-   可執行檔中有目標位元組序列。

> [!NOTE]
> 關鍵字和位元組序列是從各種安裝程式技術觀察到的一般特性衍生而來。

> [!NOTE]
> [使用者帳戶控制: 偵測應用程式安裝，並提示提升權限] 原則設定必須啟用，安裝程式偵測才能偵測安裝程式。 這項設定預設為啟用，且可以使用本機安全性原則嵌入式管理單元 (Secpol.msc) 在本機進行設定，或使用群組原則 (Gpedit.msc) 為網域、OU 或特定群組進行設定。


