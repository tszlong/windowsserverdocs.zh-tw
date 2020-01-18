---
title: 遠端伺服器管理工具
description: 遠端伺服器管理工具的最上層主題
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-rsat
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: d54a1f5e-af68-497e-99be-97775769a7a7
author: coreyp-at-msft
ms.author: coreyp
manager: dansimp
ms.openlocfilehash: e8e8206531e0939a1b6d6dfd17f5c5dd59947c81
ms.sourcegitcommit: 51e0b575ef43cd16b2dab2db31c1d416e66eebe8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/17/2020
ms.locfileid: "76259133"
---
# <a name="remote-server-administration-tools"></a>遠端伺服器管理工具

>適用于： Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題支援適用于 Windows 10 的遠端伺服器管理工具。

> [!IMPORTANT]
> 從 Windows 10 2018 年10月更新開始，RSAT 在 Windows 10 中是以一組隨**選功能**的形式包含。 請參閱以下的**使用 RSAT 版本**以取得安裝指示。

RSAT 可讓 IT 系統管理員從 Windows 10 電腦管理 Windows Server 角色和功能。

遠端伺服器管理工具組括伺服器管理員、Microsoft Management Console （mmc）嵌入式管理單元、主控台、Windows PowerShell Cmdlet 和提供者，以及用於管理在 Windows Server 上執行之角色和功能的一些命令列工具。

遠端伺服器管理工具組含 Windows PowerShell Cmdlet 模組，可用來管理在遠端伺服器上執行的角色和功能。 雖然 windows Server 2016 預設會啟用 Windows PowerShell 遠端系統管理，但在 Windows 10 上預設不會啟用此功能。 若要對遠端伺服器執行遠端伺服器管理工具中的 Cmdlet，請在安裝遠端伺服器管理工具之後，在 Windows 用戶端電腦上以提高的使用者權限（也就是以系統管理員身分執行）開啟的 Windows PowerShell 會話中執行 `Enable-PSremoting`。

## <a name="BKMK_Thresh"></a>適用于 Windows 10 的遠端伺服器管理工具
使用適用于 Windows 10 的遠端伺服器管理工具，在執行 Windows Server 2019、Windows Server 2016、Windows Server 2012 R2 的電腦上，以及在有限的情況下、Windows Server 2012 或 Windows Server 2008 R2 中管理特定技術。

適用于 Windows 10 的遠端伺服器管理工具組含對執行 Server Core 安裝選項的電腦遠端系統管理，或 Windows Server 2016、Windows Server 2012 R2 和有限的基本伺服器介面設定的支援案例，Windows Server 2012 的 Server Core 安裝選項。 不過，Windows 10 的遠端伺服器管理工具無法安裝在任何版本的 Windows Server 作業系統上。

### <a name="tools-available-in-this-release"></a>這個版本可用的工具
如需 Windows 10 遠端伺服器管理工具中可用工具的清單，請參閱[windows 作業系統遠端伺服器管理工具（RSAT）](https://support.microsoft.com/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems)中的表格。

### <a name="system-requirements"></a>系統需求
適用于 Windows 10 的遠端伺服器管理工具只能安裝在執行 Windows 10 的電腦上。 遠端伺服器管理工具無法安裝在執行 Windows RT 8.1 或其他系統晶片裝置的電腦上。

適用于 Windows 10 的遠端伺服器管理工具可在 x86 架構和 x64 架構版本的 Windows 10 上執行。

> [!IMPORTANT]
> 適用于 Windows 10 的遠端伺服器管理工具不應該安裝在執行 Windows 8.1、Windows 8、Windows Server 2008 R2、Windows Server 2008、Windows Server 2003 或 Windows 2000 Server 之系統管理工具組的電腦上。 在安裝遠端伺服器管理之前，請先移除所有舊版的系統管理工具組或遠端伺服器管理工具，包括舊版的發行前版本，以及不同語言或地區設定的工具版本。適用于 Windows 10 的工具。

若要使用這一版的伺服器管理員來存取和管理執行 Windows Server 2012 R2、Windows Server 2012 或 Windows Server 2008 R2 的遠端伺服器，您必須安裝數個更新，才能使用 Se 管理舊版 Windows Server 作業系統伺服器管理員。 如需如何使用 Windows 10 遠端伺服器管理工具中的伺服器管理員來準備 Windows Server 2012 R2、Windows Server 2012 和 Windows Server 2008 R2 以進行管理的詳細資訊，請參閱[使用伺服器管理員管理多部遠端伺服器](https://technet.microsoft.com/library/hh831456.aspx)。

您必須在遠端伺服器上啟用 windows PowerShell 和伺服器管理員遠端系統管理，才能使用屬於 Windows 10 遠端伺服器管理工具之一部分的工具來管理它們。 在執行 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 的伺服器上，預設會啟用遠端系統管理。 如需遠端管理停用時如何啟用的詳細資訊，請參閱＜ [使用伺服器管理員管理多部遠端伺服器](https://go.microsoft.com/fwlink/p/?LinkId=241358)＞。

## <a name="install-uninstall-and-turn-offon-rsat-tools"></a>安裝、卸載和關閉/on RSAT 工具

### <a name="use-features-on-demand-fod-to-install-specific-rsat-tools-on-windows-10-october-2018-update-or-later"></a>使用功能隨選（FoD）在 Windows 10 2018 年10月更新（或更新版本）上安裝特定的 RSAT 工具

從 Windows 10 2018 年10月更新開始，RSAT 已包含在 Windows 10 中的一組隨**選功能**。 現在，您可以直接移至 [管理**設定**] 中的**選用功能**，然後按一下 [**新增功能**] 查看可用的 rsat 工具清單，而不是下載 RSAT 套件。 選取並安裝您需要的特定 RSAT 工具。 若要查看安裝進度，請按一下 [**上一步**] 按鈕，以在 [**管理選擇性功能**] 頁面上查看狀態。

查看[可透過**功能點播**取得的 RSAT 工具清單](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-non-language-fod#remote-server-administration-tools-rsat)。 除了透過圖形化**設定**應用程式安裝之外，您也可以透過命令列或使用[**DISM/Add-Capability**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#using-dism-add-capability-to-add-or-remove-fods)的自動化來安裝特定的 RSAT 工具。

依需求功能的其中一項優點是，已安裝的功能會保存在 Windows 10 版本升級中。

#### <a name="to-uninstall-specific-rsat-tools-on-windows-10-october-2018-update-or-later-after-installing-with-fod"></a>若要在 Windows 10 2018 年10月更新或更新版本上卸載特定的 RSAT 工具（使用 FoD 安裝之後）

在 Windows 10 上，開啟 [**設定**] 應用程式，移至 [**管理選擇性功能**]，選取並卸載您想要移除的特定 RSAT 工具。 請注意，在某些情況下，您將需要手動卸載相依性。 具體而言，如果 RSAT 工具 B 需要 RSAT 工具 A，然後選擇卸載 RSAT 工具 A，如果仍然安裝 RSAT 工具 B，將會失敗。 在此情況下，請先卸載 RSAT 工具 B，然後再卸載 RSAT 工具 A。另請注意，在某些情況下，即使仍然安裝工具，卸載 RSAT 工具可能還是會成功。 在此情況下，重新開機電腦將會完成工具的移除。

查看包含相依性的[RSAT 工具清單](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-non-language-fod#remote-server-administration-tools-rsat)。 除了透過圖形設定應用程式卸載以外，您也可以透過命令列或使用[**DISM/Remove-Capability**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#using-dism-add-capability-to-add-or-remove-fods)的自動化來卸載特定的 RSAT 工具。

### <a name="when-to-use-which-rsat-version"></a>何時使用哪個 RSAT 版本

如果您在2018年10月更新（1809）之前有 Windows 10 的版本，您就無法**視需要使用功能**。 您將需要下載並安裝 RSAT 套件。

- **如上面所述，直接從 windows 10 安裝 RSAT fod**：在 windows 10 2018 年10月更新（1809）或更新版本（用於管理 windows Server 2019 或舊版）上安裝時。

- **下載並安裝 WS_1803 RSAT 封裝，如下所述**：在 windows 10 2018 年4月更新（1803）或更早版本上安裝時，用於管理 windows server，版本1803或 Windows Server，版本1709。

- **下載並安裝 WS2016 RSAT 封裝，如下所述**：在 windows 10 2018 年4月更新（1803）或更早版本上安裝時，用於管理 windows Server 2016 或舊版。

#### <a name="BKMK_installthresh"></a>下載 RSAT 套件，以安裝適用于 Windows 10 的遠端伺服器管理工具

1.  從[Microsoft 下載中心](https://go.microsoft.com/fwlink/?LinkID=404281)下載適用于 Windows 10 的遠端伺服器管理工具套件。 您可以從下載中心網站執行安裝程式，或者將下載的套件儲存到本機電腦或共用。

    > [!IMPORTANT]
    > 您只能在執行 Windows 10 的電腦上安裝適用于 Windows 10 的遠端伺服器管理工具。 遠端伺服器管理工具無法安裝在執行 Windows RT 8.1 或其他系統晶片裝置的電腦上。

2.  如果您將下載的套件儲存到本機電腦或共用，根據您要安裝這些工具的電腦架構而定，按兩下安裝程式 **WindowsTH-KB2693643-x64.msu** 或 **WindowsTH-KB2693643-x86.msu**。

3.  當 [Windows Update Standalone Installer] 對話方塊提示您安裝更新時，按一下 [是]。

4.  閱讀並接受授權條款。 按一下 [我接受]。

5.  安裝需要數分鐘的時間才會完成。

##### <a name="to-uninstall-remote-server-administration-tools-for-windows-10-after-rsat-package-install"></a>卸載 Windows 10 遠端伺服器管理工具（在安裝 RSAT 套件之後）

1. 在桌面上依序按一下 [開始]、[所有應用程式]、[Windows 系統]，然後按 [控制台]。

2. 在 [程式集]之下，按一下 [解除安裝程式]。

3. 按一下 [檢視安裝的更新]。

4. 以滑鼠右鍵按一下 [Microsoft Windows 更新 (KB2693643)]，然後按一下 [解除安裝]。

5. 當詢問您是否確定要解除安裝更新時，按一下 [是]。
   S
   ##### <a name="to-turn-off-specific-tools-after-rsat-package-install"></a>關閉特定工具（安裝 RSAT 套件之後）

6. 在桌面上依序按一下 [開始]、[所有應用程式]、[Windows 系統]，然後按 [控制台]。

7. 按一下 [程式集]，然後按一下 [程式和功能] 中的 [開啟或關閉 Windows 功能]。

8. 在 [Windows 功能] 對話方塊中，展開 [遠端伺服器管理工具]，然後展開 [角色管理工具] 或 [功能管理工具]。

9. 清除您想要關閉的任何工具的核取方塊。

   > [!NOTE]
   > 如果您關閉伺服器管理員，必須重新開機電腦，而且必須從 [系統**管理工具**] 資料夾開啟伺服器管理員的 [**工具**] 功能表存取的工具。

10. 當您完成關閉不要使用的工具時，按一下 [確定]。

### <a name="run-remote-server-administration-tools"></a>執行遠端伺服器管理工具

> [!NOTE]
> 安裝適用于 Windows 10 的遠端伺服器管理工具之後，[系統**管理工具**] 資料夾會顯示在 [**開始**] 功能表上。 您可以從下列位置存取工具。
>
> -   伺服器管理員主控台中的 [**工具**] 功能表。
> -   [控制台\系統及安全性\管理工具]。
> -   儲存 [管理工具] 資料夾的捷徑至桌面 (請以滑鼠右鍵按一下 [控制台\系統及安全性\管理工具] 連結，然後再按一下 [建立捷徑]。

安裝為 Windows 10 遠端伺服器管理工具一部分的工具無法用來管理本機用戶端電腦。 無論您執行哪一種工具，都必須指定要在其上執行此工具的遠端伺服器或多部遠端伺服器。 因為大部分工具都與伺服器管理員整合，所以在使用 [**工具**] 功能表中的工具管理伺服器之前，您可以將想要管理的遠端伺服器新增到伺服器管理員伺服器集區。 如需如何將伺服器新增到伺服器集區以及建立自訂伺服器群組的詳細資訊，請參閱＜ [將伺服器新增到伺服器管理員](https://go.microsoft.com/fwlink/p/?LinkId=241353) ＞和＜ [建立和管理伺服器群組](https://go.microsoft.com/fwlink/?LinkId=247328)＞。

在適用于 Windows 10 的遠端伺服器管理工具中，所有 GUI 型伺服器管理工具，例如 mmc 嵌入式管理單元和對話方塊，都可從伺服器管理員主控台的 [**工具**] 功能表存取。 雖然執行 Windows 10 遠端伺服器管理工具的電腦執行的是以用戶端為基礎的作業系統，但是在安裝工具之後，伺服器管理員（隨附于 Windows 10 的遠端伺服器管理工具）預設會自動開啟在用戶端電腦上。 請注意，在用戶端電腦上執行的伺服器管理員主控台中沒有 [**本機伺服器**] 頁面。

##### <a name="to-start-server-manager-on-a-client-computer"></a>在用戶端電腦上啟動伺服器管理員

1.  在 [開始] 功能表上，按一下 [所有應用程式]，然後按 [管理工具]。

2.  在 [管理工具] 資料夾中，按一下 [伺服器管理員]。

雖然它們並未列在伺服器管理員主控台的 [**工具**] 功能表中，但 Windows PowerShell Cmdlet 和命令提示字元管理工具也會針對角色和功能安裝，做為遠端伺服器管理工具的一部分。 例如，如果您使用提高的使用者權限（以系統管理員身分執行）開啟 Windows PowerShell 會話，並執行 Cmdlet `Get-Command -Module RDManagement`，則在安裝遠端伺服器管理工具之後，只要 Cmdlet 是以執行全部或部分遠端桌面服務角色的遠端伺服器為目標，就會包含現在可在本機電腦上執行的遠端桌面服務 Cmdlet 清單。

##### <a name="to-start-windows-powershell-with-elevated-user-rights-run-as-administrator"></a>使用提升的使用者權限啟動 Windows PowerShell (以系統管理員身分執行)

1.  在 [開始] 功能表上，依序按一下 [所有應用程式]、[Windows 系統]和 [Windows PowerShell]。

2.  若要從桌面以系統管理員身分執行 Windows PowerShell，請在**Windows powershell**快捷方式上按一下滑鼠右鍵，然後按一下 [以**系統管理員身分執行**]。

> [!NOTE]
> 您也可以在伺服器管理員中，以滑鼠右鍵按一下角色或群組頁面中受管理的伺服器，啟動以特定伺服器為目標的 Windows PowerShell 會話，然後按一下 [ **Windows powershell**]。


## <a name="known-issues"></a>已知問題

### <a name="issue-rsat-fod-installation-fails-with-error-code-0x800f0954"></a>**問題**： RSAT FOD 安裝失敗，錯誤碼為0x800f0954

> **影響**： WSUS/SCCM 環境中 Windows 10 1809 上的 RSAT fod （2018年10月更新）
> 
> **解決**方式：若要在已加入網域的電腦上安裝 fod （透過 WSUS 或 SCCM 接收更新），您必須變更群組原則設定，讓直接從 Windows Update 或本機共用下載 fod。 如需有關如何變更該設定的詳細資訊和指示，請參閱[當您使用 WSUS/SCCM 時，如何視需要提供功能和語言套件](https://docs.microsoft.com/windows/deployment/update/fod-and-lang-packs)。

---

### <a name="issue-rsat-fod-installation-via-settings-app-does-not-show-statusprogress"></a>**問題**：透過 [設定] 應用程式安裝 RSAT FOD 不會顯示狀態/進度

> **影響**： Windows 10 1809 上的 RSAT fod （2018年10月更新）
> 
> **解決**方式：若要查看安裝進度，請按一下 [**上一步**] 按鈕，以在 [**管理選擇性功能**] 頁面上查看狀態。

---

### <a name="issue-rsat-fod-uninstallation-via-settings-app-may-fail"></a>**問題**：透過設定應用程式卸載 RSAT FOD 可能會失敗

> **影響**： Windows 10 1809 上的 RSAT fod （2018年10月更新）
> 
> **解決**方式：在某些情況下，卸載失敗的原因是需要手動卸載相依性。 具體而言，如果 RSAT 工具 B 需要 RSAT 工具 A，然後選擇卸載 RSAT 工具 A，如果仍然安裝 RSAT 工具 B，將會失敗。 在此情況下，請先卸載 RSAT 工具 B，然後再卸載 RSAT 工具 A。查看包含相依性的 RSAT Fod 清單。

---

### <a name="issue-rsat-fod-uninstallation-appears-to-succeed-but-the-tool-is-still-installed"></a>**問題**： RSAT FOD 卸載似乎已成功，但此工具仍已安裝

> **影響**： Windows 10 1809 上的 RSAT fod （2018年10月更新）
> 
> **解決**方式：重新開機電腦將會完成工具的移除。

---

### <a name="issue-rsat-missing-after-windows-10-upgrade"></a>**問題**： Windows 10 升級之後遺失 RSAT

> **影響**：任何 RSAT。MSU 套件安裝（在 RSAT Fod 之前）不會自動重新安裝
> 
> **解決**方式： rsat 安裝無法在作業系統升級期間因 rsat 而保存。MSU 以 Windows Update 封裝的形式傳遞。 請在升級 Windows 10 之後，再次安裝 RSAT。 請注意，這項限制是我們從 Windows 10 1809 開始移至 Fod 的其中一個原因。 已安裝的 RSAT Fod 會保存在未來的 Windows 10 版本升級中。

## <a name="see-also"></a>請參閱
>- [適用于 Windows 10 的遠端伺服器管理工具](https://go.microsoft.com/fwlink/?LinkID=404281)
>- [適用于 Windows Vista、Windows 7、Windows 8、Windows Server 2008、Windows Server 2008 R2、Windows Server 2012 和 Windows Server 2012 R2 的遠端伺服器管理工具（RSAT）](https://go.microsoft.com/fwlink/p/?LinkID=221055)
