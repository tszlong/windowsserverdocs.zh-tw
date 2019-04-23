---
title: 管理多部遠端伺服器使用伺服器管理員
description: 伺服器管理員
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3a17e686-e7f2-47e2-b7af-733777c38b5f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e4db3e486cec5cb065bfd202b7da2547be41a01a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847609"
---
# <a name="manage-multiple-remote-servers-with-server-manager"></a>管理多部遠端伺服器使用伺服器管理員

>適用於：Windows Server 2016

伺服器管理員是 Windows Server 2012 R2 和 Windows Server 2012 中，可協助 IT 專業人員佈建及管理本機和遠端 Windows 伺服器從其桌面，而不需要是實際操作伺服器，管理主控台或必須啟用與每部伺服器的遠端桌面通訊協定 (rdP) 連線。 雖然伺服器管理員是適用於 Windows Server 2008 R2 和 Windows Server 2008，伺服器管理員 已更新在 Windows Server 2012，以便支援遠端多伺服器管理，並且增加系統管理員可以管理的伺服器數目。

在我們的測試，在 Windows Server 2012 R2 和 Windows Server 2012 的伺服器管理員可用來管理多達 100 部負責處理一般工作負載的伺服器。 使用單一 [伺服器管理員] 主控台所能管理的伺服器數目，依據您向受管理伺服器要求的資料量，以及可供執行伺服器管理員之電腦使用的硬體和網路資源，可能會有所不同。 當您要顯示的資料量接近該電腦的資源容量時，可能會發生伺服器管理員回應變慢，以及延遲完成重新整理的情形。 為了增加可以使用伺服器管理員所管理的伺服器數目，建議您使用 **\[設定事件資料\]** 對話方塊中的設定，限制伺服器管理員從受管理伺服器取得的事件資料。 [設定事件資料] 可以從 [事件]  磚中的 [工作]  功能表開啟。 如果您需要管理組織中數量達企業等級的伺服器時，建議您評估 [Microsoft System Center 套件](https://go.microsoft.com/fwlink/p/?LinkId=239437)中的產品。

本主題及子主題提供如何使用伺服器管理員主控台中的功能的相關資訊。 本主題涵蓋下列各節。

-   [檢查初始考量及系統需求](#BKMK_1.1)

-   [您可以執行伺服器管理員 中的工作](#BKMK_tasks)

-   [啟動 伺服器管理員](#BKMK_start)

-   [重新啟動遠端伺服器](#BKMK_restart)

-   [將伺服器管理員設定匯出到其他電腦](#BKMK_export)

## <a name="BKMK_1.1"></a>檢查初始考量及系統需求
下列各節列出一些您需要檢閱，以及硬體和軟體需求的 伺服器管理員的初始考量。

### <a name="hardware-requirements"></a>硬體需求
伺服器管理員會根據預設，所有版本的 Windows Server 2012 R2 和 Windows Server 2012 安裝。 沒有其他硬體需求存在於伺服器管理員。

### <a name="BKMK_softconfig"></a>軟體和組態需求
伺服器管理員會根據預設，所有版本的 Windows Server 2012 安裝。 雖然您可以使用伺服器管理員來管理[Server Core 安裝選項](https://go.microsoft.com/fwlink/p/?LinkID=241573)的 Windows Server 2012 和 Windows Server 2008 R2 的遠端電腦上執行，伺服器管理員不會直接在 Server Core 安裝上執行選項。

若要完全管理遠端伺服器執行 Windows Server 2008 或 Windows Server 2008 R2，請在您想要管理，在顯示的順序在伺服器上安裝下列更新。

若要管理執行 Windows Server 2012 的伺服器，Windows Server 2008 R2 或 Windows Server 2012 r2 中使用伺服器管理員的 Windows Server 2008 會套用下列更新到較舊的作業系統。

-   [.NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)

-   [Windows Management Framework 4.0](https://go.microsoft.com/fwlink/?LinkId=293881)。 Windows Management Framework 4.0 下載套件會更新 Windows Server 2012、 Windows Server 2008 R2 和 Windows Server 2008 上的 Windows Management Instrumentation (WMI) 提供者。 更新的 WMI 提供者可讓伺服器管理員收集受管理的伺服器上已安裝的角色和功能的相關資訊。 等到套用更新之後，執行 Windows Server 2012、 Windows Server 2008 R2 或 Windows Server 2008 的伺服器會有的管理性狀態**無法存取**。

-   效能更新相關聯[知識庫文章 2682011](https://go.microsoft.com/fwlink/p/?LinkID=245487)允許伺服器管理員 來收集效能資料，從 Windows Server 2008 和 Windows Server 2008 R2。 此效能更新不需要在執行 Windows Server 2012 的伺服器上。

若要管理執行 Windows Server 2008 R2 的伺服器或 Windows Server 2008，套用下列更新至較舊的作業系統。

-   [.NET Framework 4](https://www.microsoft.com/download/en/details.aspx?id=17718)

-   [Windows Management Framework 3.0](https://go.microsoft.com/fwlink/p/?LinkID=229019) Windows Management Framework 3.0 下載套件會更新 Windows Server 2008 和 Windows Server 2008 R2 上的 Windows Management Instrumentation (WMI) 提供者。 更新的 WMI 提供者可讓伺服器管理員收集受管理的伺服器上已安裝的角色和功能的相關資訊。 等到套用更新時，伺服器執行 Windows Server 2008 或 Windows Server 2008 R2 會有的管理性狀態**無法存取-請確認舊版執行 Windows Management Framework 3.0**。

-   效能更新相關聯[知識庫文章 2682011](https://go.microsoft.com/fwlink/p/?LinkID=245487)允許伺服器管理員 來收集效能資料，從 Windows Server 2008 和 Windows Server 2008 R2。

伺服器管理員會執行在基本伺服器圖形化介面也就是說，當伺服器圖形化殼層功能解除安裝。 根據預設，在 Windows Server 2012 R2 和 Windows Server 2012 上安裝伺服器圖形化殼層功能。 如果您解除安裝伺服器圖形化殼層，伺服器管理員主控台仍舊可以執行，但無法使用某些應用程式或主控台提供的工具。 網際網路瀏覽器無法執行而不需要伺服器圖形化介面，因此網頁和應用程式，例如 HTML 說明 （F1 的 mmc 說明，例如） 無法開啟。 您無法開啟未安裝伺服器圖形化殼層; 時，設定 Windows 自動更新和意見反應的對話方塊在 [伺服器管理員] 主控台中開啟這些對話方塊的命令會重新導向至執行**sconfig.cmd**。

#### <a name="manage-remote-computers-from-a-client-computer"></a>從用戶端電腦管理遠端電腦
[伺服器管理員] 主控台隨附[遠端伺服器管理工具](https://go.microsoft.com/fwlink/?LinkID=304145)針對 Windows 8.1 並[遠端伺服器管理工具](https://go.microsoft.com/fwlink/p/?LinkID=238560)適用於 Windows 8。 請注意，用戶端電腦上安裝遠端伺服器管理工具時，您無法管理本機電腦使用伺服器管理員;伺服器管理員無法用來管理電腦或執行 Windows 用戶端作業系統的裝置。 您可以使用伺服器管理員來管理只以 Windows 為基礎的伺服器。

|伺服器管理員來源作業系統|目標為 Windows Server 2012 R2 |目標為 Windows Server 2012 |目標為 Windows Server 2008 R2 或 Windows Server 2008 |目標為 Windows Server 2003|
|-------------------------------|---------------------------------------|------------------------------------|-----------------------------------------------------------------------|------------------|
|Windows 8 或 Windows Server 2012 |不支援|完整支援|滿足[軟體和設定需求](#BKMK_softconfig)之後，就可以執行大部分的管理工作，但不能安裝或解除安裝角色或功能|有限支援；僅線上或離線狀態|
|Windows 8.1 或 Windows Server 2012 R2 |完整支援|完整支援|滿足[軟體和設定需求](#BKMK_softconfig)之後，就可以執行大部分的管理工作，但不能安裝或解除安裝角色或功能|有限支援；僅線上或離線狀態|

###### <a name="to-start-server-manager-on-a-client-computer"></a>在用戶端電腦上啟動伺服器管理員

1.  遵循指示[部署 「 遠端伺服器管理工具 」](https://go.microsoft.com/fwlink/?LinkID=238562)安裝遠端伺服器管理工具的 Windows 8.1 或遠端伺服器管理的 Windows 8 的工具。

2.  在 **開始**畫面上，按一下**伺服器管理員**。 安裝遠端伺服器管理工具後，就可以使用 [伺服器管理員]  磚。

3.  如果既未**系統管理工具**也**伺服器管理員**磚會顯示在**啟動**螢幕之後安裝遠端伺服器管理工具，和搜尋伺服器管理員**開始**畫面未顯示任何結果，確認**顯示系統管理工具**設定已開啟。 若要檢視這項設定，滑鼠游標停留的右上角**開始**畫面，，然後按**設定**。 如果 [顯示系統管理工具]  為關閉狀態，請將設定開啟，以顯示隨遠端伺服器管理工具一起安裝的工具。

如需執行系統管理工具的 Windows 8 遠端伺服器來管理遠端伺服器的詳細資訊，請參閱 <<c0> [ 遠端伺服器管理工具](https://go.microsoft.com/fwlink/?LinkID=221055)TechNet Wiki 上。

#### <a name="configure-remote-management-on-servers-that-you-want-to-manage"></a>在您要管理的伺服器上設定遠端管理

> [!IMPORTANT]
> 根據預設，會啟用 Windows Server 2012 R2 和 Windows Server 2012 中的伺服器管理員與 Windows PowerShell 遠端管理。

使用伺服器管理員，在遠端伺服器上執行管理工作，您必須設定您想要管理的遠端伺服器，以允許遠端管理使用伺服器管理員與 Windows PowerShell。 如果已停用遠端管理 Windows Server 2012 R2 或 Windows Server 2012 上，而且您想要再次啟用它，請執行下列步驟。

##### <a name="BKMK_windows"></a>若要設定伺服器管理員遠端管理 Windows Server 2012 R2 或 Windows Server 2012 上，使用 Windows 介面

1.  > [!NOTE]
    > 所控制的設定**設定遠端管理**對話方塊不會影響使用 DCOM 進行遠端通訊的組件的 伺服器管理員。

    執行下列命令以開啟 伺服器管理員，如果它不是已開啟。

    -   在 Windows 工作列中，按一下 [伺服器管理員] 按鈕。

    -   在 **開始**畫面上，按一下**伺服器管理員**。

2.  在 **屬性**區域**本機伺服器**頁面上，按一下超連結值**遠端管理**屬性。

3.  執行下列其中一項動作，然後按一下 [確定] 。

    -   若要讓這部電腦無法從遠端管理使用伺服器管理員 （或如果已安裝的 Windows PowerShell），請清除**啟用遠端管理這部伺服器從其他電腦**核取方塊。

    -   若要讓使用伺服器管理員] 或 [Windows PowerShell 從遠端管理這部電腦，請選取**啟用遠端管理這部伺服器從其他電腦**。

##### <a name="BKMK_ps"></a>若要啟用伺服器管理員遠端管理 Windows Server 2012 R2 或 Windows Server 2012 上的使用 Windows PowerShell

1.  執行下列其中一項。

    -   若要系統管理員身分從執行 Windows PowerShell**開始**畫面上，以滑鼠右鍵按一下**Windows PowerShell**圖格，然後按一下**系統管理員身分執行**。

    -   若要從桌面以系統管理員身分執行 Windows PowerShell，請以滑鼠右鍵按一下**Windows PowerShell**在工作列上，然後按一下快顯**系統管理員身分執行**。

2.  輸入下列命令，並按**Enter**來啟用所有必要的防火牆規則例外狀況。

    **Configure-SMremoting.exe -Enable**

    > [!NOTE]
    > 這個命令在已經使用提高的使用者權限 (即 [以系統管理員身分執行]) 開啟的命令提示字元中也可以運作。

    如果啟用遠端管理失敗，請參閱[about_remote_Troubleshooting](https://go.microsoft.com/fwlink/p/?LinkID=135188)取得疑難排解秘訣和最佳作法的 Microsoft TechNet 上。

###### <a name="to-enable-server-manager-and-windows-powershell-remote-management-on-older-operating-systems"></a>在舊版作業系統上啟用伺服器管理員與 Windows PowerShell 遠端管理

-   執行下列其中一項。

    -   若要啟用遠端管理執行 Windows Server 2008 R2 的伺服器上，請參閱[使用伺服器管理員遠端管理](https://go.microsoft.com/fwlink/?LinkID=137378)Windows Server 2008 R2 說明中。

    -   若要啟用遠端管理執行 Windows Server 2008 的伺服器上，請參閱[啟用，並在 Windows PowerShell 中使用遠端命令](https://go.microsoft.com/fwlink/p/?LinkId=242565)。

    -   若要在執行 Windows Server 2003 的伺服器上啟用遠端管理，請在 Windows 防火牆中啟用 WMI DCOM 例外。 如需如何在執行 Windows Server 2003 的伺服器上進行這項操作的詳細資訊，請參閱 MSDN 上的＜ [透過 Windows 防火牆連線](https://msdn.microsoft.com/library/aa389286.aspx) ＞。

## <a name="BKMK_tasks"></a>您可以執行伺服器管理員 中的工作
伺服器管理員可讓伺服器系統管理更有效率地讓系統管理員利用單一工具執行下表中的工作。 在 Windows Server 2012 R2 和 Windows Server 2012 中，伺服器的標準使用者和 Administrators 群組的成員可以執行管理工作在 [伺服器管理員] 中，但根據預設，標準使用者無法執行一些工作，如中所示下表。

系統管理員可以在伺服器管理員 cmdlet 模組中，使用兩個 Windows PowerShell cmdlet [Enable-servermanagerstandarduserremoting](https://technet.microsoft.com/library/jj205470.aspx)並[Disable-servermanagerstandarduserremoting](https://technet.microsoft.com/library/jj205468.aspx)至進一步控制標準使用者存取某些額外的資料。 **Enable-servermanagerstandarduserremoting** cmdlet 可以提供事件、 服務、 效能計數器和角色與功能的清查資料的一或多個標準、 非系統管理員的使用者存取。

> [!IMPORTANT]
> 伺服器管理員無法用來管理較新版本的 Windows Server 作業系統。 在 Windows Server 2012 或 Windows 8 上執行的伺服器管理員無法用來管理執行 Windows Server 2012 R2 的伺服器。

|工作描述|系統管理員 (內建的 Administrator 帳戶)|標準伺服器使用者|
|----------|----------------------------------|-------------|
|將遠端伺服器新增到伺服器集區的 伺服器管理員可以用來管理。|是|否|
|建立和編輯自訂的伺服器，例如位於特定地理位置，或有特定用途的伺服器群組。|是|是|
|安裝或解除安裝角色、 角色服務與功能在本機或遠端伺服器執行 Windows Server 2012 R2 或 Windows Server 2012 上。 如需定義角色、 角色服務與功能，請參閱[角色、 角色服務與功能](https://go.microsoft.com/fwlink/p/?LinkId=239558)。|是|否|
|檢視和變更安裝在本機或遠端伺服器上的伺服器角色與功能。 **注意：** 在 [伺服器管理員] 中，角色和功能資料會顯示在系統中，也稱為系統預設 GUI 語言或在作業系統安裝期間選取的語言的基礎語言。|是|標準使用者可以檢視及管理角色和功能，並執行檢視角色事件這類工作，但無法新增或移除角色服務。|
|啟動 Windows PowerShell 或 mmc 嵌入式管理單元等管理工具。您可以啟動 Windows PowerShell 工作階段中以滑鼠右鍵按一下伺服器的遠端伺服器為目標**伺服器**圖格，然後按一下**Windows PowerShell**。 您可以開始從的 mmc 嵌入式管理單元**工具**功能表，伺服器管理員 主控台中，然後再點遠端的電腦 嵌入式管理單元之後將 mmc 已經開啟。|是|是|
|以滑鼠右鍵按一下 [伺服器]  磚中的伺服器，然後按一下 [管理身分] ，就可以用不同的認證管理遠端伺服器。 您可以使用 [管理身分] 進行一般伺服器及檔案和存放服務的管理工作。|是|否|
|執行的伺服器，例如啟動或停止服務; 的操作週期相關聯的管理工作然後啟動其他工具，可讓您設定伺服器的網路設定、 使用者和群組，以及遠端桌面連線。|是|標準使用者無法啟動或停止服務。 他們可以變更本機伺服器的名稱、 群組或網域成員資格以及遠端桌面設定，但系統會提示使用者帳戶控制提供系統管理員認證才能完成這些工作。 他們無法變更遠端管理設定。|
|執行與伺服器上安裝之角色的操作週期相關的管理工作，包括掃描角色是否符合最佳做法。|是|標準使用者無法執行最佳做法分析程式掃描。|
|判定伺服器狀態、識別重大事件，以及分析和疑難排解設定問題或失敗。|是|是|
|自訂事件、 效能資料、 服務和您想要收到在 伺服器管理員儀表板上的最佳做法分析程式結果。|是|是|
|重新啟動伺服器。|是|否|
|重新整理顯示的受管理伺服器 [伺服器管理員] 主控台中的資料。|是|否|

> [!NOTE]
> 伺服器管理員只能接收執行 Windows Server 2003 之伺服器的線上或離線狀態。 伺服器管理員無法用來執行 Windows Server 2008 R2、 Windows Server 2008 或 Windows Server 2003 的伺服器新增角色及功能。

## <a name="BKMK_start"></a>啟動 伺服器管理員
伺服器管理員會執行系統管理員群組登入伺服器的成員時，Windows Server 2012 的伺服器上的預設自動啟動。 如果您關閉伺服器管理員，重新啟動它以下列方式之一。 本章節也包含用來變更預設行為，及防止自動啟動伺服器管理員的步驟。

#### <a name="to-start-server-manager-from-the-start-screen"></a>若要從 [開始] 畫面啟動伺服器管理員

-   在 Windows 上**開始**畫面上，按一下**伺服器管理員**圖格。

#### <a name="to-start-server-manager-from-the-windows-desktop"></a>從 Windows 桌面啟動伺服器管理員

-   在 Windows 工作列上按一下 [伺服器管理員]。

#### <a name="to-prevent-server-manager-from-starting-automatically"></a>防止伺服器管理員自動啟動

1.  在 [伺服器管理員] 主控台中，在**管理**功能表上，按一下**伺服器管理員屬性**。

2.  在 [伺服器管理員屬性]  對話方塊中，核取 [登入時不要自動啟動伺服器管理員] 核取方塊。 按一下 [確定] 。

3.  或者，您可以防止伺服器管理員啟用群組原則設定，自動啟動**在登入時不要自動啟動伺服器管理員**。 這個原則設定，在 [本機群組原則編輯器] 主控台中，路徑為電腦設定 \ 系統管理伺服器管理員。

## <a name="BKMK_restart"></a>重新啟動遠端伺服器
您可以重新啟動從遠端伺服器**伺服器**磚，在 [伺服器管理員] 中的角色或群組頁面。

> [!IMPORTANT]
> 重新啟動遠端伺服器會強制伺服器重新啟動，即使使用者仍登入遠端伺服器，或含有未儲存資料的程式仍開啟。 這個行為與關閉或重新啟動本機電腦不同，關閉或重新啟動會提示您儲存未儲存的程式資料，並確認您要強制登出已登入的使用者。 請確定您能夠將其他使用者強制登出遠端伺服器，而且您可以捨棄在遠端伺服器執行之程式所未儲存的資料。
> 
> 如果自動重新整理在受管理的伺服器會關閉並重新啟動，可能會發生錯誤的受管理的伺服器，因為直到完成為止，將無法連接到遠端伺服器的伺服器管理員] 的重新整理和管理性狀態時，就會發生在 [伺服器管理員正在重新啟動。

#### <a name="to-restart-remote-servers-in-server-manager"></a>在伺服器管理員中重新啟動遠端伺服器

1.  在 [伺服器管理員] 中開啟角色或伺服器群組頁面。

2.  選取一或多個您已新增到伺服器管理員的遠端伺服器。 當您選取多個伺服器時按住 **Ctrl**，就可以一次選取多個伺服器。 如需如何將伺服器新增至伺服器管理員的伺服器集區的詳細資訊，請參閱[將伺服器新增到伺服器管理員](add-servers-to-server-manager.md)。

3.  在選取的伺服器上按一下滑鼠右鍵，然後按一下 [重新啟動伺服器] 。

## <a name="BKMK_export"></a>將伺服器管理員設定匯出到其他電腦
在 伺服器管理員 中，您的受管理的伺服器清單變更為 伺服器管理員主控台設定，，而且您已建立的自訂群組會儲存在下列兩個檔案。 您可以重複使用這些設定在其他執行相同版本的伺服器管理員電腦 （不執行 Server Core 安裝選項的電腦） 或 Windows 8 的電腦上。 若要將伺服器管理員設定匯出到這些電腦的 Windows 用戶端型電腦上，就必須執行遠端伺服器管理工具。

-   %*appdata*%\Microsoft\Windows\ServerManager\Serverlist.xml

-   %*appdata*%\Local\Microsoft_Corporation\ServerManager.exe_StrongName_*GUID*\6.2.0.0\user.config

> [!NOTE]
> -   伺服器集區中伺服器的 [管理身分] (或備用) 認證不會儲存在漫遊設定檔中。 伺服器管理員使用者必須將這些認證新增到他們要管理的每一部電腦。
> -   網路共用漫遊設定檔要在使用者初次登入網路再登出後，才會建立。 這時會建立 **Serverlist.xml** 檔案。

您可以將伺服器管理員設定匯出，讓伺服器管理員設定的可攜性，或其他電腦上使用下列兩種方式之一。

-   若要將設定匯出至另一部已加入網域的電腦，設定 伺服器管理員使用者具有漫遊設定檔在 active directory 使用者和電腦。 您必須是網域系統管理員變更使用者內容，在 active directory 使用者和電腦。

-   若要將設定匯出到另一部電腦位於工作群組中，複製上述的兩個檔案到您要使用伺服器管理員管理的電腦上的相同位置。

#### <a name="to-export-server-manager-settings-to-other-domain-joined-computers"></a>將伺服器管理員設定匯出至其他加入網域的電腦

1.  在 active directory 使用者和電腦，開啟**屬性**伺服器管理員使用者 對話方塊。

2.  在 **設定檔**索引標籤上，新增網路共用來儲存使用者設定檔的路徑。

3.  執行下列其中一項。

    -   在英文 (en-我們) 組建，變成**Serverlist.xml**檔案會自動儲存到設定檔。 繼續下一個步驟。

    -   在其他組建中，下列兩個檔案複製到屬於使用者漫遊設定檔之網路共用執行伺服器管理員的電腦上。

        -   %*appdata*%\Microsoft\Windows\ServerManager\Serverlist.xml

        -   %*localappdata*%\Microsoft_Corporation\ServerManager.exe_StrongName_*GUID*\6.2.0.0\user.config

4.  按一下 [確定]  儲存變更，並關閉 [屬性]  對話方塊。

#### <a name="to-export-server-manager-settings-to-computers-in-workgroups"></a>將伺服器管理員設定匯出至工作群組中的電腦

-   您要管理的電腦上遠端伺服器，並以相同的檔案，從另一部電腦執行伺服器管理員 中，且具有您想要的設定覆寫下列兩個檔案。

    -   %*appdata*%\Microsoft\Windows\ServerManager\Serverlist.xml

    -   %*localappdata*%\Microsoft_Corporation\ServerManager.exe_StrongName_*GUID*\6.2.0.0\user.config


