---
title: 遠端伺服器管理工具
description: 遠端伺服器管理工具的上方層級主題
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-rsat
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: d54a1f5e-af68-497e-99be-97775769a7a7
author: coreyp-at-msft
ms.author: coreyp
manager: dansimp
ms.openlocfilehash: 30ca0a1e8a2f17f54a8f05d7270bf9512be7a8dc
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66805180"
---
# <a name="remote-server-administration-tools"></a>遠端伺服器管理工具

>適用於：Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

本主題支援遠端伺服器管理工具適用於 Windows 10。

> [!IMPORTANT]
> 開始使用 Windows 10 年 10 月 2018 Update、 RSAT 是否包含做為一組**功能隨**本身的 Windows 10 中。 請參閱**時使用的 RSAT 版本**下方的安裝指示。

RSAT 可讓 IT 系統管理員管理的 Windows 10 電腦的 Windows 伺服器角色和功能。

遠端伺服器管理工具包含 [伺服器管理員] Microsoft Management Console (mmc) 嵌入式管理單元、 主控台、 Windows PowerShell cmdlet 和提供者，以及用來管理角色和功能的 Windows Server 上執行一些命令列工具。

遠端伺服器管理工具包含可用來管理角色和功能的遠端伺服器上執行的 Windows PowerShell cmdlet 模組。 雖然 Windows Server 2016 上的預設會啟用 Windows PowerShell 遠端管理，它不會啟用 Windows 10 上的預設。 若要執行是針對遠端伺服器的遠端伺服器管理工具一部分的 cmdlet，執行`Enable-PSremoting`使用提高的使用者權限 （也就，系統管理員身分執行） 之後您 Windows 用戶端電腦上開啟的 Windows PowerShell 工作階段安裝遠端伺服器管理工具。

## <a name="BKMK_Thresh"></a>適用於 Windows 10 的遠端伺服器管理工具
您可以使用遠端伺服器管理工具適用於 Windows 10 來管理執行 Windows Server 2016、windows Server 2012 R2 的電腦上，並在有限的情況下，Windows Server 2012 或 Windows Server 2008 R2 中的特定技術。

遠端伺服器管理工具適用於 Windows 10 支援遠端管理執行 Server Core 安裝選項或基本伺服器介面設定，Windows Server 2016 中，Windows Server 2012 R2，並在有限的電腦Windows Server 2012 的 Server Core 安裝選項的情況。 不過，無法在任何版本的 Windows Server 作業系統上安裝遠端伺服器管理工具適用於 Windows 10。

### <a name="tools-available-in-this-release"></a>這個版本可用的工具
如需遠端伺服器管理工具適用於 Windows 10 中可用工具的清單，請參閱中的資料表[遠端伺服器管理工具 (RSAT) 的 Windows 作業系統](https://support.microsoft.com/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems)。

### <a name="system-requirements"></a>系統需求
可以只在執行 Windows 10 的電腦上安裝遠端伺服器管理工具適用於 Windows 10。 無法在執行 Windows RT 8.1 或其他單晶片系統裝置的電腦上安裝遠端伺服器管理工具。

在 x86 型和 x64 型版本的 Windows 10 上，執行遠端伺服器管理工具適用於 Windows 10。

> [!IMPORTANT]
> 不應該執行 Windows 8.1，Windows 8、 Windows Server 2008 R2、 Windows Server 2008、 Windows Server 2003 或 Windows 2000 Server administration tools pack 的電腦上安裝遠端伺服器管理工具適用於 Windows 10。 移除所有舊版系統管理工具包或遠端伺服器管理工具，包括先前的發行前版本，並從電腦不同的語言或地區設定的工具版本，才能安裝遠端伺服器管理適用於 Windows 10 的工具。

若要使用此版本中的 伺服器管理員來存取和管理執行 Windows Server 2012 R2、 Windows Server 2012 或 Windows Server 2008 R2 的遠端伺服器，您必須安裝數項更新以便較舊的 Windows Server 作業系統管理使用 Server 管理員。 如需如何準備 Windows Server 2012 R2、 Windows Server 2012 和 Windows Server 2008 R2 管理遠端伺服器管理工具適用於 Windows 10 中使用伺服器管理員的詳細資訊，請參閱[Manage Multiple，Remote Servers使用伺服器管理員](https://technet.microsoft.com/library/hh831456.aspx)。

使用屬於遠端伺服器管理工具適用於 Windows 10 的工具來管理它們的必須在遠端伺服器上啟用 Windows PowerShell 和伺服器管理員遠端管理。 在執行 Windows Server 2016、 Windows Server 2012 R2 及 Windows Server 2012 的伺服器上預設會啟用遠端管理。 如需遠端管理停用時如何啟用的詳細資訊，請參閱＜ [使用伺服器管理員管理多部遠端伺服器](https://go.microsoft.com/fwlink/p/?LinkId=241358)＞。

## <a name="install-uninstall-and-turn-offon-rsat-tools"></a>安裝、 解除安裝及開啟/關閉 RSAT 工具

### <a name="use-features-on-demand-fod-to-install-specific-rsat-tools-on-windows-10-october-2018-update-or-later"></a>使用 on Demand (FoD) 在 Windows 上的 10 年 10 月 2018年安裝特定的 RSAT 工具的功能更新，或更新版本

開始使用 Windows 10 年 10 月 2018 Update、 RSAT 是否包含做為一組**功能隨**直接從 Windows 10。 現在，而不是下載 RSAT 套件可以直接前往**管理選擇性功能**中**設定**然後按一下**新增功能**若要查看可用的 RSAT 工具的清單。 選取並安裝所需的特定 RSAT 工具。 若要查看安裝進度，請按一下**回復**按鈕，以在檢視狀態**管理選擇性功能**頁面。

請參閱[清單中的 RSAT 工具可透過**功能隨**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-non-language-fod#remote-server-administration-tools-rsat)。 除了透過圖形化安裝**設定**應用程式中，您也可以安裝特定的 RSAT 工具透過命令列或使用自動化[ **DISM /Add-Capability**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#using-dism-add-capability-to-add-or-remove-fods)。

隨選功能的其中一個優點是，已安裝的功能來保存跨 Windows 10 版本升級 ！

#### <a name="to-uninstall-specific-rsat-tools-on-windows-10-october-2018-update-or-later-after-installing-with-fod"></a>若要解除安裝特定的 RSAT 工具在 Windows 上的 10 年 10 月 2018 Update 或更新版本 （安裝之後使用 FoD）

在 Windows 10 中，開啟**設定**應用程式，移至**管理選擇性功能**、 選取，然後解除安裝您想要移除特定的 RSAT 工具。 請注意，在某些情況下，您必須手動解除安裝相依性。 具體來說，如果需要 RSAT 工具的 RSAT 工具 B，然後選擇 解除安裝 RSAT 工具的將會失敗仍安裝 RSAT 工具 B。 在此情況下，先解除安裝 RSAT 工具 B，然後再解除安裝 RSAT 工具 a。也請注意，在某些情況下，解除安裝 RSAT 工具可能會成功，即使此工具仍會安裝。 在此情況下，重新啟動電腦，將會完成移除此工具。

請參閱[RSAT 工具包括相依性清單](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-non-language-fod#remote-server-administration-tools-rsat)。 除了透過圖形的 [設定] 應用程式解除安裝，您也可以解除安裝特定的 RSAT 工具透過命令列或使用自動化[ **DISM /Remove-Capability**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#using-dism-add-capability-to-add-or-remove-fods)。

### <a name="when-to-use-which-rsat-version"></a>使用 RSAT 版本的時機

如果您有在 2018 年 10 月的 Windows 10 的版本更新 (1809) 時，您將無法使用**功能隨**。 您必須下載並安裝 RSAT 套件。

- **如上面所述，直接從 Windows 10 安裝 RSAT FODs**:在 Windows 上安裝時 10 年 10 月 2018年更新 (1809) 或更新版本，來管理 Windows Server 2019 或之前的版本。

- **下載並安裝 WS_1803 RSAT 套件，如下所述**:當安裝在 Windows 10 April 2018 Update (1803) 或更早版本，來管理 Windows Server、 1803年版或 Windows Server 1709 版。

- **下載並安裝 WS2016 RSAT 套件，如下所述**:當安裝在 Windows 10 April 2018 Update (1803) 或更早版本，來管理 Windows Server 2016 或之前的版本。

#### <a name="BKMK_installthresh"></a>下載 RSAT 套件安裝遠端伺服器管理工具適用於 Windows 10

1.  下載遠端伺服器管理工具適用於 Windows 10 的包裝[Microsoft 下載中心](https://go.microsoft.com/fwlink/?LinkID=404281)。 您可以從下載中心網站執行安裝程式，或者將下載的套件儲存到本機電腦或共用。

    > [!IMPORTANT]
    > 您只能在執行 Windows 10 的電腦上安裝遠端伺服器管理工具適用於 Windows 10。 無法在執行 Windows RT 8.1 或其他單晶片系統裝置的電腦上安裝遠端伺服器管理工具。

2.  如果您將下載的套件儲存到本機電腦或共用，根據您要安裝這些工具的電腦架構而定，按兩下安裝程式 **WindowsTH-KB2693643-x64.msu** 或 **WindowsTH-KB2693643-x86.msu**。

3.  當 [Windows Update Standalone Installer]  對話方塊提示您安裝更新時，按一下 [是]  。

4.  閱讀並接受授權條款。 按一下 [我接受]  。

5.  安裝需要數分鐘的時間才會完成。

##### <a name="to-uninstall-remote-server-administration-tools-for-windows-10-after-rsat-package-install"></a>（之後 RSAT 套件安裝） 解除安裝遠端伺服器管理工具適用於 Windows 10

1. 在桌面上依序按一下 [開始]  、[所有應用程式]  、[Windows 系統]  ，然後按 [控制台]  。

2. 在 [程式集]  之下，按一下 [解除安裝程式]  。

3. 按一下 [檢視安裝的更新]  。

4. 以滑鼠右鍵按一下 [Microsoft Windows 更新 (KB2693643)]  ，然後按一下 [解除安裝]  。

5. 當詢問您是否確定要解除安裝更新時，按一下 [是]  。
   S
   ##### <a name="to-turn-off-specific-tools-after-rsat-package-install"></a>若要關閉特定工具 （之後安裝 RSAT 套件）

6. 在桌面上依序按一下 [開始]  、[所有應用程式]  、[Windows 系統]  ，然後按 [控制台]  。

7. 按一下 [程式集]  ，然後按一下 [程式和功能]  中的 [開啟或關閉 Windows 功能]  。

8. 在 [Windows 功能]  對話方塊中，展開 [遠端伺服器管理工具]  ，然後展開 [角色管理工具]  或 [功能管理工具]  。

9. 清除您想要關閉的任何工具的核取方塊。

   > [!NOTE]
   > 如果您關閉伺服器管理員，必須重新啟動電腦，而且工具可以從存取**工具**必須從開啟的 [伺服器管理員] 功能表**系統管理工具**資料夾。

10. 當您完成關閉不要使用的工具時，按一下 [確定]  。

### <a name="run-remote-server-administration-tools"></a>執行遠端伺服器管理工具

> [!NOTE]
> 安裝遠端伺服器管理工具的 Windows 10 之後,**系統管理工具**資料夾會顯示在**開始**功能表。 您可以從下列位置存取工具。
>
> -   **工具**伺服器管理員 主控台中的功能表。
> -   [控制台\系統及安全性\系統管理工具]  。
> -   儲存 [管理工具]  資料夾的捷徑至桌面 (請以滑鼠右鍵按一下 [控制台\系統及安全性\管理工具]  連結，然後再按一下 [建立捷徑]  。

安裝為遠端伺服器管理工具適用於 Windows 10 的一部分的工具無法用來管理本機用戶端電腦。 不論您執行此工具，您必須指定遠端伺服器或多部遠端伺服器的詳細資訊，用來執行此工具。 因為大部分工具都已整合使用伺服器管理員，您想要使用中的工具管理伺服器之前，管理伺服器集區的 伺服器管理員的遠端伺服器加入**工具**功能表。 如需如何將伺服器新增到伺服器集區以及建立自訂伺服器群組的詳細資訊，請參閱＜ [將伺服器新增到伺服器管理員](https://go.microsoft.com/fwlink/p/?LinkId=241353) ＞和＜ [建立和管理伺服器群組](https://go.microsoft.com/fwlink/?LinkId=247328)＞。

從存取遠端伺服器管理工具，適用於 Windows 10 中，所有的 GUI 型伺服器管理工具，例如 mmc 嵌入式管理單元及對話方塊方塊中，**工具**功能表的 [伺服器管理員] 主控台。 雖然執行遠端伺服器管理工具適用於 Windows 10 的電腦執行用戶端基礎作業系統，安裝工具之後，伺服器管理員 中，包含遠端伺服器管理的 Windows 10 的工具，會自動開啟預設在用戶端電腦。 請注意，沒有任何**本機伺服器**用戶端電腦執行 [伺服器管理員] 主控台中的頁面。

##### <a name="to-start-server-manager-on-a-client-computer"></a>在用戶端電腦上啟動伺服器管理員

1.  在 [開始]  功能表上，按一下 [所有應用程式]  ，然後按 [管理工具]  。

2.  在 [管理工具]  資料夾中，按一下 [伺服器管理員]  。

雖然它們並未列在 伺服器管理員主控台**工具**功能表、 Windows PowerShell cmdlet 和命令提示字元管理工具也會安裝角色與功能的遠端伺服器管理工具的一部分。 例如，如果您使用提高的使用者權限 （以系統管理員身分執行），開啟 Windows PowerShell 工作階段，並執行 cmdlet `Get-Command -Module RDManagement`，結果會包含一份現在都可在之後的本機電腦上執行的遠端桌面服務 cmdlet安裝遠端伺服器管理工具，只要 cmdlet 的目標為執行全部或一部分的遠端桌面服務角色的遠端伺服器。

##### <a name="to-start-windows-powershell-with-elevated-user-rights-run-as-administrator"></a>使用提升的使用者權限啟動 Windows PowerShell (以系統管理員身分執行)

1.  在 [開始]  功能表上，依序按一下 [所有應用程式]  、[Windows 系統]  和 [Windows PowerShell]  。

2.  若要從桌面以系統管理員身分執行 Windows PowerShell，請以滑鼠右鍵按一下**Windows PowerShell**捷徑，然後再按一下**系統管理員身分執行**。

> [!NOTE]
> 您也可以啟動 Windows PowerShell 工作階段為目標的特定伺服器上，以滑鼠右鍵按一下 受管理的伺服器角色或群組頁面中 伺服器管理員 中，然後按一下**Windows PowerShell**。


## <a name="known-issues"></a>已知問題

### <a name="issue-rsat-fod-installation-fails-with-error-code-0x800f0954"></a>**問題**:RSAT FOD 安裝失敗，錯誤碼為 0x800f0954

> **影響**:Windows 10 1809年上的 RSAT FODs （2018 年 10 月更新） 在 WSUS/SCCM 環境
> 
> **解析**:若要安裝 FODs 接收透過 WSUS 或 SCCM 的更新已加入網域的電腦上，您必須變更群組原則設定，讓下載 FODs 直接從 Windows Update 或本機共用。 如需詳細資訊和指示，有關如何變更該設定，請參閱 <<c0> [ 如何讓功能隨選和語言組件上使用當您使用 WSUS/SCCM](https://docs.microsoft.com/windows/deployment/update/fod-and-lang-packs)。

---

### <a name="issue-rsat-fod-installation-via-settings-app-does-not-show-statusprogress"></a>**問題**:透過 [設定] 應用程式的 RSAT FOD 安裝不會顯示狀態/進度

> **影響**:Windows 10 1809 （2018 年 10 月更新） 上的 RSAT FODs
> 
> **解析**:若要查看安裝進度，請按一下**回復**按鈕，以在檢視狀態**管理選擇性功能**頁面。

---

### <a name="issue-rsat-fod-uninstallation-via-settings-app-may-fail"></a>**問題**:透過設定應用程式的 RSAT FOD 解除安裝可能會失敗

> **影響**:Windows 10 1809 （2018 年 10 月更新） 上的 RSAT FODs
> 
> **解析**:在某些情況下，解除安裝失敗是因為您不必手動解除安裝相依性。 具體來說，如果需要 RSAT 工具的 RSAT 工具 B，然後選擇 解除安裝 RSAT 工具的將會失敗仍安裝 RSAT 工具 B。 在此情況下，先解除安裝 RSAT 工具 B，然後再解除安裝 RSAT 工具 a。請參閱 RSAT FODs 包括相依性的清單。

---

### <a name="issue-rsat-fod-uninstallation-appears-to-succeed-but-the-tool-is-still-installed"></a>**問題**:RSAT FOD 解除安裝就會成功，但仍安裝的工具

> **影響**:Windows 10 1809 （2018 年 10 月更新） 上的 RSAT FODs
> 
> **解析**:重新啟動電腦，將會完成移除此工具。

---

### <a name="issue-rsat-missing-after-windows-10-upgrade"></a>**問題**:遺失的 Windows 10 升級後的 RSAT

> **影響**:任何的 RSAT。MSU 套件 （在之前的安裝 RSAT FODs) 不會自動重新安裝
> 
> **解析**:RSAT 安裝不會保存到因為 RSAT 的 OS 升級。MSU Windows 更新套件的形式傳遞。 請在升級 Windows 10 之後，再次安裝 RSAT。 請注意，這項限制是為什麼我們已移至開頭為 Windows 10 1809 FODs 的原因之一。 安裝 RSAT FODs 會保存到未來的 Windows 10 版本升級。

## <a name="see-also"></a>另請參閱
>- [適用於 Windows 10 的遠端伺服器管理工具](https://go.microsoft.com/fwlink/?LinkID=404281)
>- [Windows vista、 Windows 7、windows 8、windows、 Windows Server 2008、 Windows Server 2008 R2、 Windows Server 2012 和 Windows Server 2012 R2 的遠端伺服器管理工具 (RSAT)](https://go.microsoft.com/fwlink/p/?LinkID=221055)
