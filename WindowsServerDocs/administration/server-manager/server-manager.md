---
title: 伺服器管理員
description: 瞭解 Windows Server 中的管理主控台，協助 IT 專業人員從桌面布建及管理本機與遠端 Windows 伺服器，而不需要實際存取伺服器，或啟用遠端桌面通訊協定 (rdP) 連接到每部伺服器。
ms.topic: conceptual
ms.assetid: d996ef40-8bcc-42b0-b6ae-806b828223f6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7c645698614f78520ed3540e154669a72e659bc1
ms.sourcegitcommit: de207e887575757f3389ccf940c2e0ad2dc70bd3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/13/2020
ms.locfileid: "94617208"
---
# <a name="server-manager"></a>伺服器管理員

> 適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

伺服器管理員是 Windows Server 中的管理主控台，可協助 IT 專業人員從桌面布建及管理本機與遠端 Windows 伺服器，而不需要實際存取伺服器，或啟用遠端桌面通訊協定， (rdP) 連線到每部伺服器。 雖然 Windows Server 2008 R2 和 Windows Server 2008 中有提供伺服器管理員，但伺服器管理員已在 Windows Server 2012 中更新，以支援遠端、多伺服器管理，並協助增加系統管理員可管理的伺服器數目。

在我們的測試中，Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 中的伺服器管理員可以用來管理多達100伺服器，視伺服器執行的工作負載而定。 使用單一 [伺服器管理員] 主控台所能管理的伺服器數目，依據您向受管理伺服器要求的資料量，以及可供執行伺服器管理員之電腦使用的硬體和網路資源，可能會有所不同。 當您要顯示的資料量接近該電腦的資源容量時，可能會發生伺服器管理員回應變慢，以及延遲完成重新整理的情形。 為了增加可以使用伺服器管理員所管理的伺服器數目，建議您使用 **\[設定事件資料\]** 對話方塊中的設定，限制伺服器管理員從受管理伺服器取得的事件資料。 [設定事件資料] 可以從 [事件] 磚中的 [工作] 功能表開啟。 如果您需要管理組織中的企業級伺服器數目，建議您評估 [Microsoft System Center 套件](https://go.microsoft.com/fwlink/p/?LinkId=239437)中的產品。

本主題及其子主題提供如何在伺服器管理員主控台中使用功能的相關資訊。 本主題包含下列各節。

- [檢查初始考量及系統需求](#review-initial-considerations-and-system-requirements)

- [可以在伺服器管理員中執行的工作](#tasks-that-you-can-perform-in-server-manager)

- [開始伺服器管理員](#start-server-manager)

- [重新啟動遠端伺服器](#restart-remote-servers)

- [將伺服器管理員設定匯出至其他電腦](#export-server-manager-settings-to-other-computers)

## <a name="review-initial-considerations-and-system-requirements"></a>檢查初始考量及系統需求

下列各節列出一些您需要檢查的初始考慮，以及伺服器管理員的硬體和軟體需求。

### <a name="hardware-requirements"></a>硬體需求

預設會在所有版本的 Windows Server 2016 上安裝伺服器管理員。 伺服器管理員不會有任何額外的硬體需求。

### <a name="software-and-configuration-requirements"></a>軟體和設定需求

預設會在所有版本的 Windows Server 2016 上安裝伺服器管理員。 您可以使用 Windows Server 2016 中的伺服器管理員來管理在遠端電腦上執行之 Windows Server 2016、Windows Server 2012 和 Windows Server 2008 R2 的 [Server Core 安裝選項](https://go.microsoft.com/fwlink/p/?LinkID=241573) 。 伺服器管理員是在 Windows Server 2016 的 Server Core 安裝選項上執行。

伺服器管理員在基本伺服器圖形化介面中執行;也就是未安裝伺服器圖形化 Shell 功能時。 Windows Server 2016 上預設不會安裝伺服器圖形化 Shell 功能。 如果您不是執行伺服器圖形化介面，伺服器管理員主控台會執行，但無法使用主控台提供的部分應用程式或工具。 網際網路瀏覽器無法在沒有伺服器圖形化 Shell 的情況下執行，因此網頁和應用程式（例如 HTML 說明） (mmc F1 說明，例如) 無法開啟。 未安裝伺服器圖形化介面時，您無法開啟設定 Windows 自動更新與意見反應的對話方塊;在伺服器管理員主控台中開啟這些對話方塊的命令會被重新導向為執行 **sconfig .cmd** 。

若要管理執行早于 Windows Server 2016 之 Windows Server 版本的伺服器，請安裝下列軟體和更新，以使用 Windows Server 2016 中的伺服器管理員來管理舊版 Windows Server。

|作業系統|必要軟體|
|----------|-----------|
| Windows Server 2012 R2 或 Windows Server 2012 |-   [.NET Framework 4。6](https://www.microsoft.com/download/details.aspx?id=45497)<br />-   [Windows Management Framework 5.0](https://go.microsoft.com/fwlink/?LinkID=395058)。 Windows Management Framework 5.0 下載套件更新 Windows Management Instrumentation Windows Server 2012 R2 和 Windows Server 2012 (WMI) 提供者。 更新的 WMI 提供者可讓伺服器管理員收集受管理伺服器上安裝的角色和功能的相關資訊。 在套用更新之前，執行 Windows Server 2012 R2 或 Windows Server 2012 的伺服器的管理性狀態為 [ **無法存取** ]。<br />-執行 Windows Server 2012 R2 或 Windows Server 2012 的伺服器不再需要與 [知識庫文章 2682011](https://go.microsoft.com/fwlink/p/?LinkID=245487) 相關聯的效能更新。|
| Windows Server 2008 R2 |-   [.NET Framework 4。5](https://www.microsoft.com/download/details.aspx?id=30653)<br />-   [Windows Management Framework 4.0](https://go.microsoft.com/fwlink/?LinkId=293881)。 Windows Management Framework 4.0 下載套件更新 Windows Management Instrumentation Windows Server 2008 R2 (WMI) 提供者。 更新的 WMI 提供者可讓伺服器管理員收集受管理伺服器上安裝的角色和功能的相關資訊。 在套用更新之前，執行 Windows Server 2008 R2 的伺服器具有 **無法存取** 的管理性狀態。<br />-與 [知識庫文章 2682011](https://go.microsoft.com/fwlink/p/?LinkID=245487) 相關聯的效能更新可讓伺服器管理員從 Windows Server 2008 R2 收集效能資料。|
| Windows Server 2008 |-   [.NET Framework 4](https://www.microsoft.com/download/en/details.aspx?id=17718)<br />-   [Windows Management Framework 3.0](https://go.microsoft.com/fwlink/p/?LinkID=229019) Windows Management Framework 3.0 下載套件更新 Windows Management Instrumentation Windows Server 2008 上 (WMI) 提供者。 更新的 WMI 提供者可讓伺服器管理員收集受管理伺服器上安裝的角色和功能的相關資訊。 在套用更新之前，執行 Windows Server 2008 的伺服器的管理性狀態為 [ **無法存取-請確認舊版執行 Windows Management Framework 3.0** ]。<br />-與 [知識庫文章 2682011](https://go.microsoft.com/fwlink/p/?LinkID=245487) 相關聯的效能更新可讓伺服器管理員收集 Windows Server 2008 的效能資料。|

#### <a name="manage-remote-computers-from-a-client-computer"></a>從用戶端電腦管理遠端電腦

伺服器管理員主控台隨附于 Windows 10 的 [遠端伺服器管理工具](https://go.microsoft.com/fwlink/?LinkID=404281) 。 請注意，在用戶端電腦上安裝遠端伺服器管理工具時，您無法使用伺服器管理員來管理本機電腦。伺服器管理員不能用來管理正在執行 Windows 用戶端作業系統的電腦或裝置。 您只能使用伺服器管理員管理以 Windows 為基礎的伺服器。

| 伺服器管理員來源作業系統 | 以 Windows Server 2016 為目標 | 以 Windows Server 2012 R2 為目標 | 以 Windows Server 2012 為目標 | 以 Windows Server 2008 R2 或 Windows Server 2008 為目標 | 目標為 Windows Server 2003 | 適用于 Windows 虛擬桌面 Windows 10 企業版多會話的目標 (WVD)  |
| ----- | ----- | ----- | ----- | ----- | ----- | ----- |
| Windows 10 或 Windows Server 2016 | 完整支援 | 完整支援 | 完整支援 | 滿足[軟體和設定需求](#software-and-configuration-requirements)之後，就可以執行大部分的管理工作，但不能安裝或解除安裝角色或功能 | 不受支援 | 不受支援 |
| Windows 8.1 或 Windows Server 2012 R2 | 不受支援 | 完整支援| 完整支援 | 滿足[軟體和設定需求](#software-and-configuration-requirements)之後，就可以執行大部分的管理工作，但不能安裝或解除安裝角色或功能 | 有限支援；僅線上或離線狀態 | 不受支援 |
| Windows 8 或 Windows Server 2012 | 不受支援 | 不受支援 | 完整支援 | 滿足[軟體和設定需求](#software-and-configuration-requirements)之後，就可以執行大部分的管理工作，但不能安裝或解除安裝角色或功能 | 有限支援；僅線上或離線狀態 | 不受支援 |

##### <a name="to-start-server-manager-on-a-client-computer"></a>在用戶端電腦上啟動伺服器管理員

1.  遵循 [遠端伺服器管理工具](../../remote/remote-server-administration-tools.md) 中的指示來安裝 Windows 10 的遠端伺服器管理工具。

2.  在 [ **開始** ] 畫面上，按一下 [ **伺服器管理員** ]。 安裝遠端伺服器管理工具後，就可以使用 [伺服器管理員] 磚。

3.  如果在安裝遠端伺服器管理工具之後，[ **開始** ] 畫面上沒有 [系統 **管理工具** ] 和 [ **伺服器管理員** ] 磚，在 [ **開始** ] 畫面上搜尋伺服器管理員不會顯示結果，請確認 [ **顯示系統管理工具** ] 設定已開啟。 若要查看此設定，請將滑鼠游標停留在 [ **開始** ] 畫面右上角，然後按一下 [ **設定** ]。 如果 [顯示系統管理工具] 為關閉狀態，請將設定開啟，以顯示隨遠端伺服器管理工具一起安裝的工具。

如需有關執行 Windows 10 管理遠端伺服器遠端伺服器管理工具的詳細資訊，請參閱 TechNet Wiki 上的 [遠端伺服器管理工具](https://go.microsoft.com/fwlink/?LinkID=221055) 。

#### <a name="configure-remote-management-on-servers-that-you-want-to-manage"></a>在您要管理的伺服器上設定遠端管理

> [!IMPORTANT]
> 預設會在 Windows Server 2016 中啟用伺服器管理員和 Windows PowerShell 遠端系統管理。

若要使用伺服器管理員在遠端伺服器上執行管理工作，您要管理的遠端伺服器必須設定為允許使用伺服器管理員和 Windows PowerShell 進行遠端系統管理。 如果 Windows Server 2012 R2 或 Windows Server 2012 上的遠端系統管理已停用，而您想要再次啟用它，請執行下列步驟。

##### <a name="to-configure-server-manager-remote-management-on--windows-server-2012-r2--or--windows-server-2012--by-using-the-windows-interface"></a>使用 Windows 介面設定 Windows Server 2012 R2 或 Windows Server 2012 上的伺服器管理員遠端系統管理

1.  > [!NOTE]
    > [ **設定遠端系統管理** ] 對話方塊所控制的設定，不會影響使用 DCOM 進行遠端通訊的伺服器管理員部分。

    執行下列其中一項，開啟伺服器管理員（如果尚未開啟）。

    -   在 Windows 工作列上，按一下 [伺服器管理員] 按鈕。

    -   在 [ **開始** ] 畫面上，按一下 [ **伺服器管理員** ]。

2.  在 [ **本機伺服器** ] 頁面的 [ **屬性** ] 區域中，按一下 [ **遠端系統管理** ] 屬性的超連結值。

3.  執行下列其中一項動作，然後按一下 [確定] 。

    -   若要防止這部電腦使用伺服器管理員來從遠端系統管理 (或 Windows PowerShell 安裝) ，請清除 [ **啟用從其他電腦遠端系統管理這部伺服器** ] 核取方塊。

    -   若要讓這部電腦使用伺服器管理員或 Windows PowerShell 從遠端系統管理，請選取 [ **啟用從其他電腦遠端系統管理這部伺服器** ]。

##### <a name="to-enable-server-manager-remote-management-on--windows-server-2012-r2--or--windows-server-2012--by-using-windows-powershell"></a>使用 Windows PowerShell 在 Windows Server 2012 R2 或 Windows Server 2012 上啟用伺服器管理員遠端系統管理

1.  執行下列其中一項動作。

    -   若要從 [ **開始** ] 畫面以系統管理員身分執行 Windows PowerShell，請以滑鼠右鍵按一下 **Windows PowerShell** 磚，然後按一下 [以 **系統管理員身分執行** ]。

    -   若要從桌面以系統管理員身分執行 Windows PowerShell，請在工作列中的 **Windows PowerShell** 快捷方式上按一下滑鼠右鍵，然後按一下 [以 **系統管理員身分執行** ]。

2.  輸入下列各項，然後按 **enter** 鍵，以啟用所有必要的防火牆規則例外狀況。

    **Configure-SMremoting.exe-啟用**

    > [!NOTE]
    > 這個命令在已經使用提高的使用者權限 (即 [以系統管理員身分執行]) 開啟的命令提示字元中也可以運作。

    如果啟用遠端系統管理失敗，請參閱 Microsoft TechNet 上的 [about_remote_Troubleshooting](https://go.microsoft.com/fwlink/p/?LinkID=135188) ，以取得疑難排解秘訣和最佳作法。

##### <a name="to-enable-server-manager-and-windows-powershell-remote-management-on-older-operating-systems"></a>在舊版作業系統上啟用伺服器管理員與 Windows PowerShell 遠端管理

-   執行下列其中一項動作。

    -   若要在執行 Windows Server 2008 R2 的伺服器上啟用遠端系統管理，請參閱 Windows Server 2008 R2 說明中的 [伺服器管理員的遠端系統管理](https://go.microsoft.com/fwlink/?LinkID=137378) 。

    -   若要在執行 Windows Server 2008 的伺服器上啟用遠端系統管理，請參閱 [Windows PowerShell 中的啟用和使用遠端命令](https://go.microsoft.com/fwlink/p/?LinkId=242565)。

## <a name="tasks-that-you-can-perform-in-server-manager"></a>可以在伺服器管理員中執行的工作

伺服器管理員可讓系統管理員使用單一工具執行下表中的工作，讓伺服器管理更有效率。 在 Windows Server 2012 R2 和 Windows Server 2012 中，伺服器的標準使用者和 Administrators 群組的成員都可以在伺服器管理員中執行管理工作，但根據預設，標準使用者無法執行某些工作，如下表所示。

系統管理員可以在伺服器管理員 Cmdlet 模組中使用兩個 Windows PowerShell Cmdlet， [disable-servermanagerstandarduserremoting](https://technet.microsoft.com/library/jj205470.aspx) 和 [disable-servermanagerstandarduserremoting](https://technet.microsoft.com/library/jj205468.aspx)，以進一步控制標準使用者存取某些額外的資料。 **Disable-servermanagerstandarduserremoting** Cmdlet 可提供一或多個標準、非系統管理員的使用者存取事件、服務、效能計數器，以及角色和功能清查資料。

> [!IMPORTANT]
> 伺服器管理員無法用來管理較新版本的 Windows Server 作業系統。 在 Windows Server 2012 或 Windows 8 上執行的伺服器管理員不能用來管理執行 Windows Server 2012 R2 的伺服器。

|工作描述|系統管理員 (內建的 Administrator 帳戶)|標準伺服器使用者|
|----------|----------------------------------|-------------|
|將遠端伺服器新增至伺服器管理員可以用來管理的伺服器集區。|是|否|
|建立和編輯服務器的自訂群組，例如位於特定地理位置的伺服器，或提供特定用途。|是|是|
|在執行 Windows Server 2012 R2 或 Windows Server 2012 的本機或遠端伺服器上安裝或卸載角色、角色服務和功能。 如需角色、角色服務和功能的定義，請參閱 [角色、角色服務和功能](https://go.microsoft.com/fwlink/p/?LinkId=239558)。|是|否|
|檢視和變更安裝在本機或遠端伺服器上的伺服器角色與功能。 **注意：** 在伺服器管理員中，角色和功能資料會以系統的基礎語言顯示，也稱為系統預設的 GUI 語言，或在安裝作業系統期間選取的語言。|是|標準使用者可以檢視及管理角色和功能，並執行檢視角色事件這類工作，但無法新增或移除角色服務。|
|啟動管理工具，例如 Windows PowerShell 或 mmc 嵌入式管理單元。您可以用滑鼠右鍵按一下 [ **伺服器** ] 磚中的伺服器，然後按一下 [ **Windows PowerShell** ]，啟動以遠端伺服器為目標的 Windows PowerShell 會話。 您可以從伺服器管理員主控台的 [ **工具** ] 功能表啟動 mmc 嵌入式管理單元，然後在嵌入式管理單元開啟之後將 mmc 指向遠端電腦。|是|是|
|以滑鼠右鍵按一下 [伺服器] 磚中的伺服器，然後按一下 [管理身分]，就可以用不同的認證管理遠端伺服器。 您可以使用 [管理身分] 進行一般伺服器及檔案和存放服務的管理工作。|是|否|
|執行與伺服器操作生命週期相關聯的管理工作，例如啟動或停止服務。並啟動其他工具，讓您設定伺服器的網路設定、使用者和群組，以及遠端桌面連線。|是|標準使用者無法啟動或停止服務。 他們可以變更本機伺服器的名稱、工作組或網域成員資格和遠端桌面設定，但使用者帳戶控制會提示您提供系統管理員認證，才能完成這些工作。 他們無法變更遠端管理設定。|
|執行與伺服器上安裝之角色的操作週期相關的管理工作，包括掃描角色是否符合最佳做法。|是|標準使用者無法執行最佳做法分析程式掃描。|
|判定伺服器狀態、識別重大事件，以及分析和疑難排解設定問題或失敗。|是|是|
|自訂事件、效能資料、服務，以及您想要在伺服器管理員儀表板上收到警示的最佳做法分析程式結果。|是|是|
|重新啟動伺服器。|是|否|
|重新整理伺服器管理員主控台中顯示的受管理伺服器的資料。|是|否|

> [!NOTE]
> 伺服器管理員不能用來將角色和功能新增至執行 Windows Server 2008 R2 或 Windows Server 2008 的伺服器。

## <a name="start-server-manager"></a>啟動伺服器管理員

當 Administrators 群組的成員登入伺服器時，預設會在執行 Windows Server 2016 的伺服器上自動啟動伺服器管理員。 如果您關閉伺服器管理員，請以下列其中一種方式重新開機它。 本節也包含變更預設行為，以及防止伺服器管理員自動啟動的步驟。

### <a name="to-start-server-manager-from-the-start-screen"></a>從開始畫面開始伺服器管理員

-   在 Windows [ **開始** ] 畫面上，按一下 [ **伺服器管理員** ] 磚。

### <a name="to-start-server-manager-from-the-windows-desktop"></a>從 Windows 桌面啟動伺服器管理員

-   在 Windows 工作列上按一下 [伺服器管理員]。

### <a name="to-prevent-server-manager-from-starting-automatically"></a>防止伺服器管理員自動啟動

1.  在伺服器管理員主控台的 [ **管理** ] 功能表上，按一下 [ **伺服器管理員屬性** ]。

2.  在 [伺服器管理員屬性] 對話方塊中，核取 [登入時不要自動啟動伺服器管理員] 核取方塊。 按一下 [確定]。

3.  或者，您可以啟用群組原則設定來防止伺服器管理員自動啟動， **請勿在登入時自動啟動伺服器管理員** 。 在本機群組原則編輯器主控台中，此原則設定的路徑為 [電腦設定 \ 系統管理員] Templates\System\Server 管理員。

## <a name="restart-remote-servers"></a>重新啟動遠端伺服器
您可以從伺服器管理員的角色或群組頁面的 [ **伺服器** ] 磚重新開機遠端伺服器。

> [!IMPORTANT]
> 重新啟動遠端伺服器會強制伺服器重新啟動，即使使用者仍登入遠端伺服器，或含有未儲存資料的程式仍開啟。 這個行為與關閉或重新啟動本機電腦不同，關閉或重新啟動會提示您儲存未儲存的程式資料，並確認您要強制登出已登入的使用者。 請確定您能夠將其他使用者強制登出遠端伺服器，而且您可以捨棄在遠端伺服器執行之程式所未儲存的資料。
>
> 如果在受管理的伺服器關閉和重新開機時，伺服器管理員發生自動重新整理，受管理的伺服器可能會發生重新整理和管理性狀態錯誤，因為伺服器管理員無法連線到遠端伺服器，直到它完成重新開機為止。

### <a name="to-restart-remote-servers-in-server-manager"></a>在伺服器管理員中重新啟動遠端伺服器

1.  在伺服器管理員中開啟角色或伺服器群組首頁。

2.  選取您已新增至伺服器管理員的一或多部遠端伺服器。 當您選取多個伺服器時按住 **Ctrl** ，就可以一次選取多個伺服器。 如需有關如何將伺服器新增到伺服器管理員伺服器集區的詳細資訊，請參閱 [將伺服器新增至伺服器管理員](add-servers-to-server-manager.md)。

3.  在選取的伺服器上按一下滑鼠右鍵，然後按一下 [重新啟動伺服器]。

## <a name="export-server-manager-settings-to-other-computers"></a>將伺服器管理員設定匯出至其他電腦

在伺服器管理員中，受管理伺服器的清單、伺服器管理員主控台設定的變更，以及您所建立的自訂群組，都會儲存在下列兩個檔案中。 您可以在其他電腦上重複使用這些設定，這些電腦執行相同版本的伺服器管理員 (或 Windows 10 遠端伺服器管理工具安裝的) 。 遠端伺服器管理工具必須在 Windows 用戶端電腦上執行，才能將伺服器管理員設定匯出至這些電腦。

-   %*appdata* % \Microsoft\Windows\ServerManager\Serverlist.xml

-   %*appdata* % \Local\Microsoft_Corporation\ServerManager.exe_StrongName_ *GUID* \6.2.0.0\user.config

> [!NOTE]
> -   伺服器集區中伺服器的 [管理身分] (或備用) 認證不會儲存在漫遊設定檔中。 伺服器管理員使用者必須將這些認證新增到他們要管理的每一部電腦。
> -   網路共用漫遊設定檔要在使用者初次登入網路再登出後，才會建立。 這時會建立 **Serverlist.xml** 檔案。

您可以使用下列兩種方式之一，匯出伺服器管理員設定、讓伺服器管理員設定可攜，或在其他電腦上使用它們。

-   若要將設定匯出到另一個已加入網域的電腦，請將伺服器管理員使用者設定為在 [active directory 使用者和電腦] 中擁有漫遊設定檔。 您必須是網域系統管理員，才能變更 [active directory 使用者和電腦] 中的使用者屬性。

-   若要將設定匯出至工作組中的另一部電腦，請使用伺服器管理員將上述兩個檔案複製到您要管理的電腦上的相同位置。

### <a name="to-export-server-manager-settings-to-other-domain-joined-computers"></a>將伺服器管理員設定匯出至其他加入網域的電腦

1.  **在 [active** directory 使用者和電腦] 中，開啟伺服器管理員使用者的 [內容] 對話方塊。

2.  在 [ **設定檔** ] 索引標籤上，新增網路共用的路徑以儲存使用者的設定檔。

3.  執行下列其中一項動作。

    -   在美國英文 (en-us) 組建中， **Serverlist.xml** 檔案的變更會自動儲存到設定檔。 繼續下一個步驟。

    -   在其他組建中，將下列兩個檔案從執行伺服器管理員的電腦複製到屬於使用者漫遊設定檔一部分的網路共用。

        -   %*appdata* % \Microsoft\Windows\ServerManager\Serverlist.xml

        -   %*localappdata* % \Microsoft_Corporation\ServerManager.exe_StrongName_ *GUID* \6.2.0.0\user.config

4.  按一下 [確定] 儲存變更，並關閉 [屬性] 對話方塊。

### <a name="to-export-server-manager-settings-to-computers-in-workgroups"></a>將伺服器管理員設定匯出至工作群組中的電腦

-   在您要管理遠端伺服器的電腦上，使用另一部執行伺服器管理員之電腦的相同檔案覆寫下列兩個檔案，其中包含您想要的設定。

    -   %*appdata* % \Microsoft\Windows\ServerManager\Serverlist.xml

    -   %*localappdata* % \Microsoft_Corporation\ServerManager.exe_StrongName_ *GUID* \6.2.0.0\user.config