---
title: 在伺服器管理員中設定遠端系統管理
description: 瞭解如何將伺服器新增到伺服器管理員伺服器集區，以在遠端伺服器上執行管理工作。
ms.topic: article
ms.assetid: 509182ed-c37d-4b81-84bc-aee43d006873
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 71df577f4d1e487204ff25d66c9d3155bc1b9e67
ms.sourcegitcommit: 605a9b46b74b2c7a9116e631e902467ea02a6e70
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/07/2021
ms.locfileid: "97965730"
---
# <a name="configure-remote-management-in-server-manager"></a>在伺服器管理員中設定遠端系統管理

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在 Windows Server 中，您可以使用伺服器管理員在遠端伺服器上執行管理工作。 預設會在執行 Windows Server 2016 的伺服器上啟用遠端系統管理。 若要使用伺服器管理員從遠端管理伺服器，請將伺服器新增至伺服器管理員伺服器集區。

您可以使用伺服器管理員來管理執行較舊版本 Windows Server 的遠端伺服器，但需要下列更新才能完整管理這些舊版的作業系統。

若要管理執行早于 Windows Server 2016 之 Windows Server 版本的伺服器，請安裝下列軟體和更新，以使用 Windows Server 2016 中的伺服器管理員來管理舊版 Windows Server。

|作業系統|必要軟體|管理能力|
|----------|-----------|---------|
| Windows Server 2012 R2 或 Windows Server 2012 |-   [.NET Framework 4。6](https://www.microsoft.com/download/details.aspx?id=45497)<br />-   [Windows Management Framework 5.0](https://go.microsoft.com/fwlink/?LinkID=395058)。 Windows Management Framework 5.0 下載套件更新 Windows Management Instrumentation Windows Server 2012 R2、Windows Server 2012 和 Windows Server 2008 R2 上 (WMI) 提供者。 更新的 WMI 提供者可讓伺服器管理員收集受管理伺服器上安裝的角色和功能的相關資訊。 在套用更新之前，執行 Windows Server 2012 R2、Windows Server 2012 或 Windows Server 2008 R2 的伺服器的管理性狀態為 [ **無法存取**]。<br />-執行 Windows Server 2012 R2 或 Windows Server 2012 的伺服器不再需要與 [知識庫文章 2682011](https://go.microsoft.com/fwlink/p/?LinkID=245487) 相關聯的效能更新。||
| Windows Server 2008 R2 |-   [.NET Framework 4。5](https://www.microsoft.com/download/details.aspx?id=30653)<br />-   [Windows Management Framework 4.0](https://go.microsoft.com/fwlink/?LinkId=293881)。 Windows Management Framework 4.0 下載套件更新 Windows Management Instrumentation Windows Server 2008 R2 (WMI) 提供者。 更新的 WMI 提供者可讓伺服器管理員收集受管理伺服器上安裝的角色和功能的相關資訊。 在套用更新之前，執行 Windows Server 2008 R2 的伺服器具有 **無法存取** 的管理性狀態。<br />-與 [知識庫文章 2682011](https://go.microsoft.com/fwlink/p/?LinkID=245487) 相關聯的效能更新可讓伺服器管理員從 Windows Server 2008 R2 收集效能資料。||
| Windows Server 2008 |-   [.NET Framework 4](https://www.microsoft.com/download/en/details.aspx?id=17718)<br />-   [Windows Management Framework 3.0](https://go.microsoft.com/fwlink/p/?LinkID=229019) Windows Management Framework 3.0 下載套件更新 Windows Management Instrumentation Windows Server 2008 上 (WMI) 提供者。 更新的 WMI 提供者可讓伺服器管理員收集受管理伺服器上安裝的角色和功能的相關資訊。 在套用更新之前，執行 Windows Server 2008 的伺服器的管理性狀態為 [ **無法存取-請確認舊版執行 Windows Management Framework 3.0**]。<br />-與 [知識庫文章 2682011](https://go.microsoft.com/fwlink/p/?LinkID=245487) 相關聯的效能更新可讓伺服器管理員收集 Windows Server 2008 的效能資料。||

如需有關如何新增工作組中的伺服器來管理，或從執行伺服器管理員的工作組電腦管理遠端伺服器的詳細資訊，請參閱 [將伺服器新增至伺服器管理員](add-servers-to-server-manager.md)。

## <a name="enabling-or-disabling-remote-management"></a>啟用或停用遠端管理
在 Windows Server 2016 中，預設會啟用遠端系統管理。 使用伺服器管理員從遠端連線到執行 Windows Server 2016 的電腦之前，必須先在目的地電腦上啟用 [遠端系統管理] 伺服器管理員（如果已停用）。 本節中的程序說明如何停用遠端管理，以及如何重新啟用已停用的遠端管理。 在伺服器管理員主控台中，本機伺服器的遠端系統管理狀態會顯示在 [**本機伺服器**] 頁面的 [**屬性**] 區域中。

即使已經啟用遠端管理，除了內建的 Administrator 帳戶以外的本機系統管理員帳戶可能沒有遠端管理伺服器的權限。 遠端使用者帳戶控制 (UAC) **LocalAccountTokenFilterPolicy** 登錄設定必須設定為允許內建系統管理員帳戶以外的本機帳戶從遠端管理伺服器。

在 Windows Server 2016 中，伺服器管理員依賴 Windows 遠端系統管理 (WinRM) 以及分散式元件物件模型 (DCOM) 用於遠端通訊。 [ **設定遠端系統管理** ] 對話方塊所控制的設定，只會影響使用 WinRM 進行遠端通訊的伺服器管理員和 Windows PowerShell 部分。 它們不會影響使用 DCOM 進行遠端通訊的伺服器管理員部分。 例如，伺服器管理員會使用 WinRM 來與執行 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 的遠端伺服器通訊，但會使用 DCOM 來與執行 Windows Server 2008 和 Windows Server 2008 R2 的伺服器進行通訊，但不會套用 [Windows Management Framework 4.0](https://go.microsoft.com/fwlink/?LinkId=293881) 或 Windows Management Framework 的 [3.0](https://go.microsoft.com/fwlink/p/?LinkID=229019) 更新。 Microsoft Management Console (mmc) 和其他舊版管理工具使用 DCOM。 如需有關如何變更這些設定的詳細資訊，請參閱本主題中的透過 [DCOM 設定 mmc 或其他工具遠端系統管理](#to-configure-mmc-or-other-tool-remote-management-over-dcom) 。

> [!NOTE]
> 本節中的程序僅能在執行 Windows Server 的電腦上完成。 您無法使用這些程式在執行 Windows 10 的電腦上啟用或停用遠端系統管理，因為無法使用伺服器管理員來管理用戶端作業系統。

-   若要啟用 WinRM 遠端管理，請選取下列其中一項程序。

    -   [使用 Windows 介面啟用伺服器管理員遠端管理](#to-enable-server-manager-remote-management-by-using-the-windows-interface)

    -   [使用 Windows PowerShell 啟用伺服器管理員遠端管理](#to-enable-server-manager-remote-management-by-using-windows-powershell)

    -   [使用命令列啟用伺服器管理員遠端管理](#to-enable-server-manager-remote-management-by-using-the-command-line)

    -   [在舊版的 Windows Server 上啟用伺服器管理員與 Windows PowerShell 遠端管理](#to-enable-server-manager-and-windows-powershell-remote-management-on-earlier-releases-of-windows-server)

-   若要停用 WinRM 並伺服器管理員遠端系統管理，請選取下列其中一個程式。

    -   [使用群組原則停用遠端管理](#to-disable-remote-management-by-using-group-policy)

    -   [在自動安裝期間使用回應檔案停用遠端管理](#to-disable-remote-management-by-using-an-answer-file-during-unattended-installation)

-   若要設定 DCOM 遠端管理，請參閱[設定 DCOM 遠端管理](#to-configure-mmc-or-other-tool-remote-management-over-dcom)。

### <a name="to-enable-server-manager-remote-management-by-using-the-windows-interface"></a>使用 Windows 介面啟用伺服器管理員遠端管理

1.  > [!NOTE]
    > [ **設定遠端系統管理** ] 對話方塊所控制的設定，不會影響使用 DCOM 進行遠端通訊的伺服器管理員部分。

    在您想要遠端系統管理的電腦上，開啟伺服器管理員（如果尚未開啟）。 在 Windows 工作列上按一下 [伺服器管理員]。 在 [ **開始** ] 畫面上，按一下 [ **伺服器管理員** ] 磚。

2.  在 [**本機伺服器**] 頁面的 [**屬性**] 區域中，按一下 [**遠端系統管理**] 屬性的超連結值。

3.  執行下列其中一項動作，然後按一下 [確定] 。

    -   若要防止這部電腦使用伺服器管理員來從遠端系統管理 (或 Windows PowerShell 安裝) ，請清除 [ **啟用從其他電腦遠端系統管理這部伺服器** ] 核取方塊。

    -   若要讓這部電腦使用伺服器管理員或 Windows PowerShell 從遠端系統管理，請選取 [ **啟用從其他電腦遠端系統管理這部伺服器**]。

### <a name="to-enable-server-manager-remote-management-by-using-windows-powershell"></a>使用 Windows PowerShell 啟用伺服器管理員遠端管理

1.  在您想要遠端系統管理的電腦上，執行下列其中一項動作，以提升的使用者權限開啟 Windows PowerShell 會話。

    -   在 Windows 桌面上，用滑鼠右鍵按一下工作列上的 [Windows PowerShell]，然後按一下 [以系統管理員身分執行]。

    -   在 Windows [ **開始** ] 畫面上，以滑鼠右鍵按一下 [ **Windows PowerShell**]，然後在應用程式行上，按一下 [以 **系統管理員身分執行**]。

2.  輸入下列各項，然後按 **enter** 鍵，以啟用所有必要的防火牆規則例外狀況。

    **Configure-SMremoting.exe-啟用**

### <a name="to-enable-server-manager-remote-management-by-using-the-command-line"></a>使用命令列啟用伺服器管理員遠端管理

1.  在想要遠端管理的電腦上，以提升的使用者權限開啟命令提示字元工作階段。 若要這樣做，請在 [ **開始** ] 畫面上輸入 **cmd**，在 [ **命令提示** 字元] 磚顯示于 [ **應用程式** ] 結果中時，以滑鼠右鍵按一下它，然後在應用程式行上按一下 [以 **系統管理員身分執行**]。

2.  執行下列可執行檔。

    **% windir% \system32\Configure-SMremoting.exe**

3.  執行下列其中一個動作：

    -   若要停用遠端系統管理，請輸入 **Configure-SMremoting.exe-disable**，然後按 **enter**。

    -   若要啟用遠端系統管理，請輸入 **Configure-SMremoting.exe-enable**，然後按 **enter**。

    -   若要查看目前的遠端系統管理設定，請輸入 **Configure-SMremoting.exe get**，然後按 enter。

### <a name="to-enable-server-manager-and-windows-powershell-remote-management-on-earlier-releases-of-windows-server"></a>在舊版的 Windows Server 上啟用伺服器管理員與 Windows PowerShell 遠端管理

-   執行下列其中一個動作：

    -   若要在執行 Windows Server 2012 的伺服器上啟用遠端系統管理，請參閱本主題中 [的使用 Windows 介面啟用伺服器管理員遠端系統管理](#to-enable-server-manager-remote-management-by-using-the-windows-interface) 。

    -   若要在執行 Windows Server 2008 R2 的伺服器上啟用遠端系統管理，請參閱 Windows Server 2008 R2 說明中的 [伺服器管理員的遠端系統管理](https://go.microsoft.com/fwlink/?LinkID=137378) 。

    -   若要在執行 Windows Server 2008 的伺服器上啟用遠端系統管理，請參閱 [Windows PowerShell 中的啟用和使用遠端命令](https://go.microsoft.com/fwlink/p/?LinkId=242565)。

### <a name="to-configure-mmc-or-other-tool-remote-management-over-dcom"></a>透過 DCOM 設定 mmc 或其他工具遠端系統管理

1.  執行下列其中一項以開啟 [具有進階安全性的 Windows 防火牆] 嵌入式管理單元。

    -   在伺服器管理員的 [**本機伺服器**]**頁面的 [內容] 區域中**，按一下 [ **Windows 防火牆**] 屬性的超文字值，然後按一下 [ **Advanced settings**]。

    -   在 [ **開始** ] 畫面上，輸入 **services.msc**，然後按一下 [ **應用程式** ] 結果中顯示的嵌入式管理單元磚。

2.  在樹狀目錄窗格中選取 [輸入規則] 。

3.  確認已啟用下列防火牆規則的例外狀況，但群組原則設定尚未停用這些例外狀況。 如果有任一個規則未啟用，請繼續下一個步驟。

    -   COM+ 網路存取 (DCOM-In)

    -   遠端事件記錄檔管理 (NP) 

    -   遠端事件記錄檔管理 (RPC) 

    -   遠端事件記錄檔管理 (的 rpc-epmap) 

4.  用滑鼠右鍵按一下未啟用的規則，然後按一下內容功能表上的 [啟用規則]。

5.  關閉 [具有進階安全性的 Windows 防火牆] 嵌入式管理單元。

### <a name="to-disable-remote-management-by-using-group-policy"></a>使用群組原則停用遠端管理

1.  執行下列其中一項動作，以開啟本機群組原則編輯器。

    -   在執行 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 的伺服器上，于 [ **開始** ] 畫面上輸入 **gpedit.msc**，然後按一下顯示的 [ **gpedit.msc** ] 磚。

    -   在執行 Windows Server 2008 R2 或 Windows Server 2008 的伺服器上，于 [ **執行** ] 對話方塊中輸入 **gpedit.msc**，然後按 **enter**。

2.  開啟 **電腦管理元件 \Windows 元件 \windows 遠端系統管理 (WinRM) \Winrm Service**。

3.  在內容窗格中，按兩下 [允許透過 WinRM 進行遠端伺服器管理]。

4.  在 [允許透過 WinRM 進行遠端伺服器管理] 原則設定的對話方塊中，選取 [停用] 以停用遠端管理。 按一下 [確定] 以儲存變更，然後關閉原則設定對話方塊。

### <a name="to-disable-remote-management-by-using-an-answer-file-during-unattended-installation"></a>在自動安裝期間使用回應檔案停用遠端管理

1.  使用 Windows 系統映射管理員 (Windows SIM) ，建立 Windows Server 2016 安裝的自動安裝回應檔案。 如需如何建立回應檔案與使用 Windows SIM 的詳細資訊，請參閱[何謂 Windows 系統映像管理員？](/previous-versions/windows/it-pro/windows-vista/cc766347(v=ws.10))及[逐步說明：IT 專業人員的基本 Windows 部署](/previous-versions/windows/it-pro/windows-7/dd349348(v=ws.10))。

2.  在您的回應檔案中，找出設定 **Microsoft-Windows-Web-Services-for-Management-Core\EnableServerremoteManagement**。

3.  **若要** 在您要套用回應檔案的所有伺服器上預設停用伺服器管理員遠端系統管理，請將 **\EnableServerremoteManagement 的 Microsoft-Web 服務-** -----

    > [!NOTE]
    > 這項設定會在作業系統安裝程序期間停用遠端管理。 設定此設定並不會防止系統管理員在作業系統安裝完成後，在伺服器上啟用伺服器管理員遠端系統管理。 系統管理員可以使用中的步驟，透過使用 [Windows 介面設定伺服器管理員遠端](#to-enable-server-manager-remote-management-by-using-the-windows-interface) 管理，或使用本主題中 [的 Windows PowerShell 啟用伺服器管理員遠端](#to-enable-server-manager-remote-management-by-using-windows-powershell) 管理，來再次啟用伺服器管理員遠端系統管理。
    >
    > 如果您在自動安裝過程中預設停用 [遠端系統管理]，但在安裝之後，未在伺服器上啟用遠端系統管理，則會使用伺服器管理員來完全管理套用此回應檔案的伺服器。 執行 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 (且已預設停用遠端系統管理的伺服器，在新增至伺服器管理員伺服器集區後，伺服器管理員主控台中) 產生管理性狀態錯誤。

## <a name="windows-remote-management-winrm-listener-settings"></a>Windows 遠端系統管理 (WinRM) 接聽程式設定
伺服器管理員依賴您要管理的遠端伺服器上的預設 WinRM 接聽程式設定。 如果遠端伺服器上的預設驗證機制或 WinRM 接聽程式埠號碼已從預設設定變更，伺服器管理員無法與遠端伺服器通訊。

下列清單顯示使用伺服器管理員管理的預設 WinRM 接聽程式設定。

-   WinRM 服務正在執行。

-   已經建立透過連接埠號碼 5985 接受 HTTP 要求的 WinRM 接聽程式。

-   已經在 [Windows 防火牆] 設定中啟用連接埠號碼 5985 以允許通過 WinRM 的要求。

-   已經同時啟用 **Kerberos** 與 **交涉** 驗證類型。

WinRM 與遠端電腦通訊的預設連接埠號碼為 5985。

如需有關如何設定 WinRM 接聽程式設定的詳細資訊，請在命令提示字元中輸入 **WinRM help config**，然後按 enter。

## <a name="see-also"></a>另請參閱
[將伺服器新增至伺服器管理員](add-servers-to-server-manager.md) 
[Windows PowerShell： Windows Server 技術中心](/previous-versions/dd347642(v=technet.10)) 
 上的 about_remote_Troubleshooting[使用者帳戶控制的描述](https://support.microsoft.com/kb/951016)