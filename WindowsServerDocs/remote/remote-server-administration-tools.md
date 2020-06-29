---
title: 遠端伺服器管理工具
description: 遠端伺服器管理工具的最上層主題
ms.prod: windows-server
ms.technology: manage-rsat
ms.topic: get-started-article
ms.assetid: d54a1f5e-af68-497e-99be-97775769a7a7
author: coreyp-at-msft
ms.author: coreyp
manager: dansimp
ms.openlocfilehash: 69b31c8ef0ce093604ee9fd8fe382d75f7f88595
ms.sourcegitcommit: aeefdf7814a4672b2dcd7537204205bb7ee5f9a0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/08/2020
ms.locfileid: "84514908"
---
# <a name="remote-server-administration-tools"></a>遠端伺服器管理工具

>適用於：Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題支援適用於 Windows 10 的遠端伺服器管理工具。

> [!IMPORTANT]
> 從 Windows 10 2018 年 10 月更新開始，RSAT 會以一組**功能隨選安裝**的形式納入 Windows 10。 如需安裝指示，請參閱**使用各個 RSAT 版本的時機**。

RSAT 可讓 IT 系統管理員從 Windows 10 電腦管理 Windows Server 的角色和功能。

遠端伺服器管理工具包括伺服器管理員、Microsoft Management Console (mmc) 嵌入式管理單元、主控台、Windows PowerShell Cmdlet 及提供者，以及用於管理 Windows Server 上執行之角色及功能的一些命令列工具。

遠端伺服器管理工具包括 Windows PowerShell Cmdlet 模組，可用來管理在遠端伺服器上執行的角色和功能。 雖然 Windows Server 2016 預設會啟用 Windows PowerShell 遠端管理，但在 Windows 10 上預設不會啟用此功能。 若要對遠端伺服器執行遠端伺服器管理工具中所包含的 Cmdlet，請在安裝遠端伺服器管理工具之後，在您的 Windows 用戶端電腦上，在使用提高的使用者權限 (也就是以系統管理員身分執行) 開啟的 Windows PowerShell 工作階段中執行 `Enable-PSremoting`。

## <a name="remote-server-administration-tools-for-windows-10"></a><a name="BKMK_Thresh"></a>適用於 Windows 10 的遠端伺服器管理工具
使用 Windows 10 的遠端伺服器管理工具，在執行 Windows Server 2019、Windows Server 2016、Windows Server 2012 R2 的電腦上，以及 Windows Server 2012 或 Windows Server 2008 R2 (在少數情況下) 中管理特定技術。

適用於 Windows 10 的遠端伺服器管理工具包含對電腦遠端管理的支援，這些電腦執行 Windows Server 2016、Windows Server 2012 R2 的 Server Core 安裝選項或基本伺服器介面組態 (在某些案例中則是執行 Windows Server 2012 的 Server Core 安裝選項)。 不過，適用於 Windows 10 的遠端伺服器管理工具無法安裝在任何版本的 Windows Server 作業系統上。

### <a name="tools-available-in-this-release"></a>這個版本可用的工具
如需適用於 Windows 10 的遠端伺服器管理工具中可用工具的清單，請參閱[適用於 Windows 作業系統的遠端伺服器管理工具 (RSAT)](https://support.microsoft.com/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems) 中的表格。

### <a name="system-requirements"></a>系統需求
適用於 Windows 10 的遠端伺服器管理工具只能安裝在執行 Windows 10 的電腦上。 遠端伺服器管理工具無法安裝在執行 Windows RT 8.1 的電腦或其他單晶片系統裝置上。

適用於 Windows 10 的遠端伺服器管理工具可在 x86 及 x64 型的 Windows 10 版本上執行。

> [!IMPORTANT]
> 您不應該在執行 Windows 8.1、Windows 8、Windows Server 2008 R2、Windows Server 2008、Windows Server 2003 或 Windows 2000 Server 的系統管理工具包之電腦上安裝適用於 Windows 10 的遠端伺服器管理工具。 在安裝適用於 Windows 10 的遠端伺服器管理工具之前，請從電腦上移除所有舊版系統管理工具包或遠端伺服器管理工具，包括較舊的發行前版本，以及不同語言或地區設定的工具版本。

若要使用此版本的伺服器管理員存取和管理執行 Windows Server 2012 R2、Windows Server 2012 或 Windows Server 2008 R2 的遠端伺服器，您必須安裝數個更新，才能使用伺服器管理員來管理這些舊版的 Windows Server 作業系統。 如需如何準備 Windows Server 2012 R2、Windows Server 2012 和 Windows Server 2008 R2，以便在適用於 WIndows 10 的遠端伺服器管理工具中使用伺服器管理員加以管理的相關資訊，請參閱[使用伺服器管理員管理多部遠端伺服器](https://technet.microsoft.com/library/hh831456.aspx)。
        
您必須在遠端伺服器上啟用 Windows PowerShell 和伺服器管理員遠端管理，才能使用屬於適用於 Windows 10 的遠端伺服器管理工具的工具來管理它們。 在執行 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 的伺服器上，預設會啟用遠端管理。 如需遠端管理停用時如何啟用的詳細資訊，請參閱＜ [使用伺服器管理員管理多部遠端伺服器](https://go.microsoft.com/fwlink/p/?LinkId=241358)＞。
        
## <a name="install-uninstall-and-turn-offon-rsat-tools"></a>安裝、解除安裝和關閉/開啟 RSAT 工具        

### <a name="use-features-on-demand-fod-to-install-specific-rsat-tools-on-windows-10-october-2018-update-or-later"></a>使用功能隨選安裝 (FoD) 在 Windows 10 2018 年 10 月更新或更高版本上安裝特定的 RSAT 工具。

從 Windows 10 2018 年 10 月更新開始，RSAT 會以一組**功能隨選安裝**的形式納入 Windows 10。 現在，您無需下載 RSAT 套件，只需移至 [設定]  中的 [管理選用功能]  ，然後按一下 [新增功能]  ，就能查看可用的 RSAT 工具清單。 選取並安裝所需的特定 RSAT 工具。 若要查看安裝進度，請按一下 [返回]  按鈕，以在 [管理選用功能]  頁面上查看狀態。
        
[請透過**功能隨選安裝**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-non-language-fod#remote-server-administration-tools-rsat)，參閱可用的 RSAT 工具。 除了透過圖形化**設定**應用程式安裝，您也可以使用 [**DISM /Add-Capability**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#using-dism-add-capability-to-add-or-remove-fods)，透過命令列或自動化來安裝特定的 RSAT 工具。

功能隨選安裝的其中一項優點是，已安裝的功能會保存在各個 Windows 10 版本升級中。        
        
#### <a name="to-uninstall-specific-rsat-tools-on-windows-10-october-2018-update-or-later-after-installing-with-fod"></a>在 Windows 10 2018 年 10 月更新或更高版本上解除安裝特定的 RSAT 工具 (使用 FoD 安裝之後)        

在 Windows 10 上，開啟 [設定]  應用程式，移至 [管理選用功能]  ，選取並解除安裝要移除的特定 RSAT 工具。 請注意，在某些情況下，您需要手動將相依性解除安裝。 具體而言，如果 RSAT 工具 B 需要 RSAT 工具 A，那麼如果已安裝 RSAT 工具 B，則選擇將 RSAT 工具 A 解除安裝的作業會失敗。 在這種情況下，請先將 RSAT 工具 B 解除安裝，然後再將 RSAT 工具 A 解除安裝。另外也請注意，在某些情況下，即使工具仍然處於安裝狀態，將 RSAT 工具解除安裝的作業可能看起來還是會成功。 在這種情況下，請將電腦重新開機以移除工具。

請參閱[包含相依性的 RSAT 工具清單](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-non-language-fod#remote-server-administration-tools-rsat)。 除了透過圖形化設定應用程式解除安裝，您也可以使用 [**DISM /Add-Capability**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#using-dism-add-capability-to-add-or-remove-fods)，透過命令列或自動化將特定的 RSAT 工具解除安裝。

### <a name="when-to-use-which-rsat-version"></a>各個 RSAT 版本的使用時機

如果您在 2018 年 10 月更新 (1809) 之前已具備 Windows 10 版本，就無法使用**功能隨選安裝**。 您需要下載並安裝 RSAT 套件。

- **如上所述，直接從 Windows 10 安裝 RSAT FOD**：在 Windows 10 2018 年 10 月更新 (1809) 或更高版本上安裝時，可用於管理 Windows Server 2019 或舊版。

- **如下所述，下載並安裝 WS_1803 RSAT 套件**：在 Windows 10 2018 年 4 月更新 (1803) 或更yrbr 版本上安裝時，可用於管理版本 1803 或版本 1709 的 Windows Server。

- **如下所述，下載並安裝 WS2016 RSAT 套件**：在 Windows 10 2018 年 4 月更新 (1803) 或更高版本上安裝時，可用於管理 Windows Server 2016 或舊版。

#### <a name="download-the-rsat-package-to-install-remote-server-administration-tools-for-windows-10"></a><a name="BKMK_installthresh"></a>下載 RSAT 套件以安裝適用於 Windows 10 的遠端伺服器管理工具

1.  從 [Microsoft 下載中心](https://go.microsoft.com/fwlink/?LinkID=404281)下載適用於 Windows 10 的遠端伺服器管理工具套件。 您可以從下載中心網站執行安裝程式，或者將下載的套件儲存到本機電腦或共用。

    > [!IMPORTANT]
    > 您只能在執行 Windows 10 的電腦上安裝適用於 Windows 10 的遠端伺服器管理工具。 遠端伺服器管理工具無法安裝在執行 Windows RT 8.1 的電腦或其他單晶片系統裝置上。

2.  如果您將下載的套件儲存到本機電腦或共用，根據您要安裝這些工具的電腦架構而定，按兩下安裝程式 **WindowsTH-KB2693643-x64.msu** 或 **WindowsTH-KB2693643-x86.msu**。

3.  當 [Windows Update Standalone Installer]  對話方塊提示您安裝更新時，按一下 [是]  。

4.  閱讀並接受授權條款。 按一下 [我接受]  。

5.  安裝需要數分鐘的時間才會完成。    
        
##### <a name="to-uninstall-remote-server-administration-tools-for-windows-10-after-rsat-package-install"></a>將適用於 Windows 10 的遠端伺服器管理工具解除安裝 (安裝 RSAT 套件之後)
        
1. 在桌面上依序按一下 [開始]  、[所有應用程式]  、[Windows 系統]  ，然後按 [控制台]  。

2. 在 [程式集]  之下，按一下 [解除安裝程式]  。

3. 按一下 [檢視安裝的更新]  。

4. 以滑鼠右鍵按一下 [Microsoft Windows 更新 (KB2693643)]  ，然後按一下 [解除安裝]  。

5. 當詢問您是否確定要解除安裝更新時，按一下 [是]  。
   S
   ##### <a name="to-turn-off-specific-tools-after-rsat-package-install"></a>關閉特定工具 (安裝 RSAT 套件之後)
        
6. 在桌面上依序按一下 [開始]  、[所有應用程式]  、[Windows 系統]  ，然後按 [控制台]  。

7. 按一下 [程式集]  ，然後按一下 [程式和功能]  中的 [開啟或關閉 Windows 功能]  。

8. 在 [Windows 功能]  對話方塊中，展開 [遠端伺服器管理工具]  ，然後展開 [角色管理工具]  或 [功能管理工具]  。

9. 清除您想要關閉的任何工具的核取方塊。

   > [!NOTE]
   > 如果您關閉伺服器管理員，必須重新啟動電腦，而且原本從伺服器管理員的 [工具]  功能表存取的工具，必須從 [系統管理工具]  資料夾開啟。
        
10. 當您完成關閉不要使用的工具時，按一下 [確定]  。

### <a name="run-remote-server-administration-tools"></a>執行遠端伺服器管理工具

> [!NOTE]
> 安裝適用於 Windows 10 的遠端伺服器管理工具後，[開始]  功能表上會顯示 [管理工具]  資料夾。 您可以從下列位置存取工具。
>
> -   伺服器管理員主控台中的 [工具]  功能表。
> -   [控制台\系統及安全性\管理工具]  。
> -   儲存 [管理工具]  資料夾的捷徑至桌面 (請以滑鼠右鍵按一下 [控制台\系統及安全性\管理工具]  連結，然後再按一下 [建立捷徑]  。

安裝為屬於適用於 Windows 10 的遠端伺服器管理工具的工具無法用來管理本機用戶端電腦。 無論執行何種工具，都必須指定要在其中執行工具的一或多部遠端伺服器。 因為大部分工具都與伺服器管理員整合，所以在使用 [工具]  功能表中的工具管理伺服器之前，要先將您要管理的遠端伺服器加入伺服器管理員伺服器集區中。 如需如何將伺服器新增到伺服器集區以及建立自訂伺服器群組的詳細資訊，請參閱＜ [將伺服器新增到伺服器管理員](https://go.microsoft.com/fwlink/p/?LinkId=241353) ＞和＜ [建立和管理伺服器群組](https://go.microsoft.com/fwlink/?LinkId=247328)＞。

在適用於 Windows 10 的遠端伺服器管理工具中，所有 GUI 型伺服器管理工具 (例如 MMC 嵌入式管理單元和對話方塊) 都可由伺服器管理員主控台的 [工具]  功能表存取。 雖然執行「適用於 Windows 10 的遠端伺服器管理工具」的電腦執行的是用戶端型作業系統，但在安裝這些工具之後，預設會在用戶端電腦自動開啟「適用於 Windows 10 的遠端伺服器管理工具」隨附的伺服器管理員。 請注意，在用戶端電腦上執行的伺服器管理員主控台中沒有 [本機伺服器]  頁面。

##### <a name="to-start-server-manager-on-a-client-computer"></a>在用戶端電腦上啟動伺服器管理員

1.  在 [開始]  功能表上，按一下 [所有應用程式]  ，然後按 [管理工具]  。

2.  在 [管理工具]  資料夾中，按一下 [伺服器管理員]  。

雖然它們並未列在伺服器管理員主控台 [工具]  功能表中，但 Windows PowerShell Cmdlet 與命令提示字元管理工具也會針對角色及功能，安裝為「遠端伺服器管理工具」的一部分。 例如，如果您使用提高的使用者權限 (以系統管理員身分執行) 開啟 Windows PowerShell 工作階段，並執行 Cmdlet `Get-Command -Module RDManagement`，只要 Cmdlet 的目標為執行全部或部分「遠端桌面服務」角色的遠端伺服器，結果會包含安裝遠端伺服器管理工具之後，現在可在本機電腦執行的遠端桌面服務 Cmdlet 清單。

##### <a name="to-start-windows-powershell-with-elevated-user-rights-run-as-administrator"></a>使用提升的使用者權限啟動 Windows PowerShell (以系統管理員身分執行)

1.  在 [開始]  功能表上，依序按一下 [所有應用程式]  、[Windows 系統]  和 [Windows PowerShell]  。

2.  若要從桌面以系統管理員身分執行 Windows PowerShell，請在 **Windows PowerShell** 捷徑上按一下滑鼠右鍵，然後按一下 [以系統管理員身分執行]  。

> [!NOTE]
> 您也可以用滑鼠右鍵按一下伺服器管理員的角色或群組頁面中受管理的伺服器，啟動以特定伺服器為目標的 Windows PowerShell 工作階段，然後按一下 [Windows PowerShell]  。
        

## <a name="known-issues"></a>已知問題

### <a name="issue-rsat-fod-installation-fails-with-error-code-0x800f0954"></a>**問題**：RSAT FOD 安裝失敗，錯誤碼為 0x800f0954

> **影響**：WSUS/Configuration Manager 環境中 Windows 10 1809 (2018 年 10 月更新) 上的 RSAT FOD
> 
> **解決方法**：若要在已加入網域的電腦上安裝 FOD，以透過 WSUS 或 Configuration Manager 接收更新，您需要變更群組原則設定，以便直接從 Windows Update 或本機共用下載 FOD。 如需有關如何變更該設定的詳細資訊和指示，請參閱[如何在使用 WSUS/SCCM 時讓功能隨選安裝和語言套件可供使用](https://docs.microsoft.com/windows/deployment/update/fod-and-lang-packs)。

---

### <a name="issue-rsat-fod-installation-via-settings-app-does-not-show-statusprogress"></a>**問題**：透過設定應用程式安裝 RSAT FOD 不會顯示狀態/進度

> **影響**：Windows 10 1809 (2018 年 10 月更新) 上的 RSAT FOD
> 
> **解決方法**：若要查看安裝進度，請按一下 [返回]  按鈕，以在 [管理選用功能]  頁面上查看狀態。

---

### <a name="issue-rsat-fod-uninstallation-via-settings-app-may-fail"></a>**問題**：透過設定應用程式將 RSAT FOD 解除安裝可能會失敗

> **影響**：Windows 10 1809 (2018 年 10 月更新) 上的 RSAT FOD
> 
> **解決方法**：在某些情況下，解除安裝失敗的原因是需要手動將相依性解除安裝。 具體而言，如果 RSAT 工具 B 需要 RSAT 工具 A，那麼如果已安裝 RSAT 工具 B，則選擇將 RSAT 工具 A 解除安裝的作業會失敗。 在這種情況下，請先將 RSAT 工具 B 解除安裝，然後再將 RSAT 工具 A 解除安裝。查看包含相依性的 RSAT FOD 清單。

---

### <a name="issue-rsat-fod-uninstallation-appears-to-succeed-but-the-tool-is-still-installed"></a>**問題**：RSAT FOD 的解除安裝作業似乎已成功，但此工具仍顯示已安裝狀態

> **影響**：Windows 10 1809 (2018 年 10 月更新) 上的 RSAT FOD
> 
> **解決方法**：請將電腦重新開機以移除工具。

---

### <a name="issue-rsat-missing-after-windows-10-upgrade"></a>**問題**：Windows 10 升級後遺失 RSAT

> **影響**：系統不會自動重新安裝任何 RSAT .MSU 套件安裝 (在 RSAT FOD 之前)
> 
> **解決方法**：由於 RSAT .MSU 以 Windows Update 套件的形式傳遞，因此無法在作業系統升級期間保存 RSAT 安裝。 請在升級 Windows 10 之後，再次安裝 RSAT。 請注意，這項限制是我們從 Windows 10 1809 開始移至 FOD 的其中一個原因。 未來的 Windows 10 版本升級中會保存已安裝的 RSAT FOD。

## <a name="see-also"></a>另請參閱
>- [適用於 Windows 10 的遠端伺服器管理工具](https://go.microsoft.com/fwlink/?LinkID=404281)
>- [適用於 Windows Vista、Windows 7、Windows 8、Windows Server 2008、Windows Server 2008 R2、Windows Server 2012 及 Windows Server 2012 R2 的遠端伺服器管理工具 (RSAT)](https://go.microsoft.com/fwlink/p/?LinkID=221055)                                                                                                                                                                                                                                                                                                                                                                                    
