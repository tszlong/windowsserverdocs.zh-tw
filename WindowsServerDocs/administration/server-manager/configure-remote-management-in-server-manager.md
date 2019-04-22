---
title: 在 [伺服器管理員] 中設定遠端管理
description: 伺服器管理員
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 509182ed-c37d-4b81-84bc-aee43d006873
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4a66fe7a274756de9bed9f6b14f5b9e491e5b623
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819579"
---
# <a name="configure-remote-management-in-server-manager"></a>在 [伺服器管理員] 中設定遠端管理

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

在 Windows Server 中，您可以使用伺服器管理員在遠端伺服器上執行管理工作。 在執行 Windows Server 2016 的伺服器上預設會啟用遠端管理。 若要使用伺服器管理員遠端管理伺服器，新增伺服器至伺服器管理員的伺服器集區。

您可以使用伺服器管理員來管理遠端伺服器執行 Windows Server 的版本還舊，但下列更新才能完整管理這些舊版作業系統。

若要管理執行 Windows Server 版本早於 Windows Server 2016 的伺服器，請安裝下列軟體和更新，讓 Windows Server 2016 中使用伺服器管理員的可管理的 Windows Server 的版本還舊。

|作業系統|所需的軟體|管理性|
|----------|-----------|---------|
| Windows Server 2012 R2 或 Windows Server 2012 |-   [.NET Framework 4.6](https://www.microsoft.com/download/details.aspx?id=45497)<br />-   [Windows Management Framework 5.0](https://go.microsoft.com/fwlink/?LinkID=395058)。 Windows Management Framework 5.0 下載套件會更新 Windows Server 2012 R2、 Windows Server 2012 和 Windows Server 2008 R2 上的 Windows Management Instrumentation (WMI) 提供者。 更新的 WMI 提供者可讓伺服器管理員收集受管理的伺服器上已安裝的角色和功能的相關資訊。 等到套用更新之後，執行 Windows Server 2012 R2、 Windows Server 2012 或 Windows Server 2008 R2 的伺服器會有的管理性狀態**無法存取**。<br />-效能更新相關聯[知識庫文章 2682011](https://go.microsoft.com/fwlink/p/?LinkID=245487)不再需要在執行 Windows Server 2012 R2 的伺服器或 Windows Server 2012 上。||
| Windows Server 2008 R2 |-   [.NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)<br />-   [Windows Management Framework 4.0](https://go.microsoft.com/fwlink/?LinkId=293881)。 Windows Management Framework 4.0 下載套件會更新 Windows Server 2008 R2 上的 Windows Management Instrumentation (WMI) 提供者。 更新的 WMI 提供者可讓伺服器管理員收集受管理的伺服器上已安裝的角色和功能的相關資訊。 等到套用更新之後，執行 Windows Server 2008 R2 的伺服器會有的管理性狀態**無法存取**。<br />-效能更新相關聯[知識庫文章 2682011](https://go.microsoft.com/fwlink/p/?LinkID=245487)讓收集效能資料，從 Windows Server 2008 R2 的伺服器管理員。||
| Windows Server 2008 |-   [.NET framework 4](https://www.microsoft.com/download/en/details.aspx?id=17718)<br />-   [Windows Management Framework 3.0](https://go.microsoft.com/fwlink/p/?LinkID=229019) Windows Management Framework 3.0 下載套件會更新 Windows Server 2008 上的 Windows Management Instrumentation (WMI) 提供者。 更新的 WMI 提供者可讓伺服器管理員收集受管理的伺服器上已安裝的角色和功能的相關資訊。 等到套用更新之後，執行 Windows Server 2008 的伺服器會有的管理性狀態**無法存取-請確認舊版執行 Windows Management Framework 3.0**。<br />-效能更新相關聯[知識庫文章 2682011](https://go.microsoft.com/fwlink/p/?LinkID=245487)讓收集效能資料，從 Windows Server 2008 的伺服器管理員。||

如需如何新增要管理，或從工作群組電腦是執行伺服器管理員管理遠端伺服器的伺服器的詳細資訊，請參閱[將伺服器新增到伺服器管理員](add-servers-to-server-manager.md)。

## <a name="BKMK_remote"></a>啟用或停用遠端管理
在 Windows Server 2016 中，預設會啟用遠端管理。 您可以連接到正在使用伺服器管理員從遠端執行 Windows Server 2016 的電腦之前，伺服器管理員遠端管理必須啟用在目的地電腦上，如果已停用。 本節中的程序說明如何停用遠端管理，以及如何重新啟用已停用的遠端管理。 在 [伺服器管理員] 主控台中，本機伺服器的遠端管理狀態會顯示在**屬性**區域**本機伺服器**頁面。

即使已經啟用遠端管理，除了內建的 Administrator 帳戶以外的本機系統管理員帳戶可能沒有遠端管理伺服器的權限。 遠端使用者帳戶控制 (UAC) **LocalAccountTokenFilterPolicy**登錄設定必須設為允許的內建的 administrator 帳戶以外的系統管理員群組，從遠端管理本機帳戶伺服器。

在 Windows Server 2016 中，伺服器管理員會依賴 Windows 遠端管理 (WinRM) 和分散式元件物件模型 (DCOM) 才能進行遠端通訊。 所控制的設定**設定遠端管理**對話方塊只會影響使用 WinRM 進行遠端通訊的伺服器管理員與 Windows PowerShell 組件。 它們不會影響使用 DCOM 進行遠端通訊的組件的 伺服器管理員。 例如，伺服器管理員會使用 WinRM 來與執行 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 的遠端伺服器通訊，但會使用 DCOM 來與執行 Windows Server 2008 的伺服器和 Windows Server 2008 R2，通訊但並沒有[Windows Management Framework 4.0](https://go.microsoft.com/fwlink/?LinkId=293881)或是[Windows Management Framework 3.0](https://go.microsoft.com/fwlink/p/?LinkID=229019)套用更新。 Microsoft Management Console (mmc) 和其他舊版管理工具會使用 DCOM。 如需如何變更這些設定的詳細資訊，請參閱[若要設定 dcom 的 mmc 或其他工具的遠端管理](#BKMK_dcom)本主題中。

> [!NOTE]
> 本節中的程序僅能在執行 Windows Server 的電腦上完成。 您無法啟用或停用遠端管理執行 Windows 10 使用這些程序，因為用戶端作業系統無法使用伺服器管理員管理的電腦上。

-   若要啟用 WinRM 遠端管理，請選取下列其中一項程序。

    -   [若要使用 Windows 介面啟用伺服器管理員遠端管理](#BKMK_windows)

    -   [若要使用 Windows PowerShell 啟用伺服器管理員遠端管理](#BKMK_ps)

    -   [若要使用命令列啟用伺服器管理員遠端管理](#BKMK_cmdline)

    -   [若要啟用伺服器管理員與 Windows PowerShell 遠端管理有關較早版本的 Windows Server](#BKMK_old)

-   若要停用 WinRM 和伺服器管理員遠端管理，請選取下列程序的其中一個。

    -   [使用群組原則停用遠端管理](#BKMK_disableGP)

    -   [若要使用回應檔案，自動安裝期間停用遠端管理](#BKMK_unattend)

-   若要設定 DCOM 遠端管理，請參閱[設定 DCOM 遠端管理](#BKMK_dcom)。

### <a name="BKMK_windows"></a>若要使用 Windows 介面啟用伺服器管理員遠端管理

1.  > [!NOTE]
    > 所控制的設定**設定遠端管理**對話方塊不會影響使用 DCOM 進行遠端通訊的組件的 伺服器管理員。

    在您想要遠端管理的電腦，開啟 伺服器管理員，如果它不是已開啟。 在 Windows 工作列上按一下 [伺服器管理員]。 在 **開始**畫面上，按一下**伺服器管理員**圖格。

2.  在 **屬性**區域**本機伺服器**頁面上，按一下超連結值**遠端管理**屬性。

3.  執行下列其中一項動作，然後按一下 [確定] 。

    -   若要讓這部電腦無法從遠端管理使用伺服器管理員 （或如果已安裝的 Windows PowerShell），請清除**啟用遠端管理這部伺服器從其他電腦**核取方塊。

    -   若要讓使用伺服器管理員] 或 [Windows PowerShell 從遠端管理這部電腦，請選取**啟用遠端管理這部伺服器從其他電腦**。

### <a name="BKMK_ps"></a>若要使用 Windows PowerShell 啟用伺服器管理員遠端管理

1.  在您想要遠端管理的電腦，執行下列命令來使用提高的使用者權限開啟 Windows PowerShell 工作階段。

    -   在 Windows 桌面上，用滑鼠右鍵按一下工作列上的 [Windows PowerShell]，然後按一下 [以系統管理員身分執行]。

    -   在 Windows 上**開始**畫面上，以滑鼠右鍵按一下**Windows PowerShell**，然後在應用程式列中，按一下 **系統管理員身分執行**。

2.  輸入下列命令，並按**Enter**來啟用所有必要的防火牆規則例外狀況。

    **Configure-SMremoting.exe -enable**

### <a name="BKMK_cmdline"></a>若要使用命令列啟用伺服器管理員遠端管理

1.  在想要遠端管理的電腦上，以提升的使用者權限開啟命令提示字元工作階段。 若要這樣做，請在**開始**畫面上，輸入**cmd**，以滑鼠右鍵按一下**命令提示字元**圖格時，它會顯示在**應用程式**結果，並然後在應用程式列中，按一下**系統管理員身分執行**。

2.  執行下列可執行檔。

    **%windir%\system32\Configure-SMremoting.exe**

3.  執行下列其中一項：

    -   若要停用遠端管理，請輸入**SMremoting.exe-停用**，然後按**Enter**。

    -   若要啟用遠端管理，請輸入**SMremoting.exe-啟用**，然後按**Enter**。

    -   若要檢視目前的遠端管理設定，請輸入**SMremoting.exe-取得**，然後按 ENTER 鍵。

### <a name="BKMK_old"></a>若要啟用伺服器管理員與 Windows PowerShell 遠端管理有關較早版本的 Windows Server

-   執行下列其中一項：

    -   若要啟用遠端管理執行 Windows Server 2012 的伺服器上，請參閱[若要使用 Windows 介面啟用伺服器管理員遠端管理](#BKMK_windows)本主題中。

    -   若要啟用遠端管理執行 Windows Server 2008 R2 的伺服器上，請參閱[使用伺服器管理員遠端管理](https://go.microsoft.com/fwlink/?LinkID=137378)Windows Server 2008 R2 說明中。

    -   若要啟用遠端管理執行 Windows Server 2008 的伺服器上，請參閱[啟用，並在 Windows PowerShell 中使用遠端命令](https://go.microsoft.com/fwlink/p/?LinkId=242565)。

### <a name="BKMK_dcom"></a>若要設定 dcom 的 mmc 或其他工具的遠端管理

1.  執行下列其中一項以開啟 [具有進階安全性的 Windows 防火牆] 嵌入式管理單元。

    -   在 **屬性**區域**本機伺服器**頁面上 伺服器管理員 中，按一下超文字值**Windows 防火牆**屬性，然後再按一下**進階設定**。

    -   在 **開始**畫面上，輸入**WF.msc**，然後按一下 嵌入式管理單元 圖格，當它出現在**應用程式**結果。

2.  在樹狀目錄窗格中選取 [輸入規則] 。

3.  確認下列防火牆規則的例外狀況已啟用，而且不由群組原則設定已停用。 如果有任一個規則未啟用，請繼續下一個步驟。

    -   COM+ 網路存取 (DCOM-In)

    -   遠端事件記錄檔管理 (Np-in)

    -   遠端事件記錄檔管理 (RPC)

    -   遠端事件記錄檔管理 (RPC-EPMAP)

4.  用滑鼠右鍵按一下未啟用的規則，然後按一下內容功能表上的 [啟用規則]  。

5.  關閉 [具有進階安全性的 Windows 防火牆] 嵌入式管理單元。

### <a name="BKMK_disableGP"></a>使用群組原則停用遠端管理

1.  執行下列命令以開啟 本機群組原則編輯器。

    -   在執行 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012，在伺服器上**開始**畫面上，輸入**gpedit.msc**，然後按一下**gpedit**圖格當它會顯示。

    -   在執行 Windows Server 2008 R2 或 Windows Server 2008 的伺服器上**執行** 對話方塊中，輸入**gpedit.msc**，然後按**Enter**。

2.  開啟**電腦設定 \ 系統管理範本 \windows 元件 \windows 遠端管理 (WinRM) \WinRM 服務**。

3.  在內容窗格中，按兩下 [允許透過 WinRM 進行遠端伺服器管理]。

4.  在 [允許透過 WinRM 進行遠端伺服器管理]  原則設定的對話方塊中，選取 [停用]  以停用遠端管理。 按一下 [確定]  以儲存變更，然後關閉原則設定對話方塊。

### <a name="BKMK_unattend"></a>若要使用回應檔案，自動安裝期間停用遠端管理

1.  使用 Windows 系統映像管理員 (Windows SIM) 建立 Windows Server 2016 安裝的自動的安裝回應檔案。 如需如何建立回應檔案，並使用 Windows SIM 的詳細資訊，請參閱[什麼是 Windows 系統映像管理員？](https://technet.microsoft.com/library/cc766347.aspx)和[逐步：適用於 IT 專業人員的基本 Windows 部署](https://technet.microsoft.com/library/dd349348.aspx)。

2.  在回應檔案中，找出設定**Microsoft-Windows-Web-Services-for-Management-Core\EnableServerremoteManagement**。

3.  若要停用您要套用回應檔案的所有伺服器上的預設伺服器管理員遠端管理，請設定**Microsoft-Windows-Web-Services-for-Management-Core \EnableServerremoteManagement**到**False**.

    > [!NOTE]
    > 這項設定會在作業系統安裝程序期間停用遠端管理。 進行這項設定不會防止系統管理員啟用伺服器管理員遠端管理伺服器上，在作業系統安裝完成後。 系統管理員可以啟用伺服器管理員 中的步驟一次所使用的遠端管理[藉由使用 Windows 介面設定伺服器管理員遠端管理](#BKMK_windows)或[使用啟用伺服器管理員遠端管理Windows PowerShell](#BKMK_ps)本主題中。
    > 
    > 如果您依預設停用遠端管理，做為一部分的自動安裝，而且未在安裝之後再次啟用伺服器上的遠端管理時，要套用這個回應檔案的伺服器不能完全管理使用伺服器管理員。 伺服器執行 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 （和具有預設停用遠端管理） 會產生 [伺服器管理員] 主控台中管理性狀態錯誤之後新增至伺服器管理員伺服器集區。

## <a name="windows-remote-management-winrm-listener-settings"></a>Windows 遠端管理 (WinRM) 接聽程式設定
伺服器管理員會依賴您想要管理的遠端伺服器上的預設 WinRM 接聽程式設定。 如果預設值已變更的預設驗證機制或遠端伺服器上的 WinRM 接聽程式連接埠號碼，伺服器管理員無法與遠端伺服器通訊。

下列清單顯示使用伺服器管理員管理的預設 WinRM 接聽程式設定。

-   WinRM 服務正在執行。

-   已經建立透過連接埠號碼 5985 接受 HTTP 要求的 WinRM 接聽程式。

-   已經在 [Windows 防火牆] 設定中啟用連接埠號碼 5985 以允許通過 WinRM 的要求。

-   已經同時啟用 **Kerberos** 與**交涉**驗證類型。

WinRM 與遠端電腦通訊的預設連接埠號碼為 5985。

如需有關如何設定 WinRM 接聽程式設定，在命令提示字元中，輸入**winrm 說明組態**，然後按 ENTER 鍵。

## <a name="see-also"></a>另請參閱
[將伺服器新增到伺服器管理員](add-servers-to-server-manager.md)
[Windows PowerShell: Windows Server TechCenter 上的 about_remote_Troubleshooting](https://technet.microsoft.com/library/dd347642.aspx)
[使用者帳戶控制的描述](https://support.microsoft.com/kb/951016)



