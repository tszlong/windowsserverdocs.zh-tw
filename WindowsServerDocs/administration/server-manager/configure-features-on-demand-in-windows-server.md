---
title: Configure Features on Demand in Windows Server
description: 伺服器管理員
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e663bbea-d025-41fa-b16c-c2bff00a88e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c77bd088a02d405b17a4f7decd6093785296d5e8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818929"
---
# <a name="configure-features-on-demand-in-windows-server"></a>Configure Features on Demand in Windows Server

>適用於：Windows Server 2016

本主題說明如何使用 Uninstall-WindowsFeature Cmdlet 移除 [功能隨選安裝] 設定中的功能檔案。

依需求的功能是引進 Windows 8 和 Windows Server 2012，可讓您移除角色及功能檔案中的功能 (有時稱為功能*承載*) 以節省磁碟空間，並安裝角色的作業系統並從遠端位置或安裝媒體，而不是從本機電腦的功能。 您可以從執行中的實體或虛擬電腦移除功能檔案。 您也可以在 Windows 映像 (WIM) 檔或離線虛擬硬碟 (VHD) 中新增或移除功能檔案，以建立可重製的「功能隨選安裝」設定複本。

在 「 功能隨選安裝設定，當功能檔案不是可用的電腦上，如果安裝需要這些功能檔案，Windows Server 2012 R2 或 Windows Server 2012 可以指示從並排顯示功能存放 （的共用資料夾取得檔案包含功能檔案，並可供網路上的電腦），從 Windows Update 或是從安裝媒體。 根據預設，當目標伺服器未提供功能檔案時，[功能隨選安裝] 會依顯示的順序執行下列工作來搜尋遺漏的功能檔案。

1.  已新增角色及功能精靈 或 DISM 安裝命令的使用者所指定的位置搜尋

2.  評估的群組原則設定，設定**選用之元件安裝和元件修復的電腦設定 \ 系統管理指定設定**

3.  搜尋 Windows Update

您可以執行下列任一項操作來覆寫預設的功能隨選安裝行為。

-   新增 `Install-WindowsFeature` 參數，以便在 `Source` Cmdlet 中指定替代來源路徑。

-   在 [指定替代來源路徑**確認安裝選項**頁面上，您會使用新增角色及功能精靈] 來安裝功能時

-   設定 [指定選用之元件安裝和元件修復的相關設定] 

本主題涵蓋下列各節。

-   [建立功能檔案或並排顯示存放區](#BKMK_store)

-   [移除功能檔案的方法](#BKMK_methods)

-   [使用 Uninstall-windowsfeature 移除功能檔案](#BKMK_remove)

## <a name="BKMK_store"></a>建立功能檔案或並排顯示存放區
本節說明如何設定遠端功能檔案共用資料夾 （也稱為並排顯示存放區） 儲存在執行 Windows Server 2012 R2 的伺服器或 Windows Server 2012 上安裝角色、 角色服務與功能所需的檔案。 您已設定功能存放之後，您可以執行兩個作業系統的伺服器上安裝角色、 角色服務與功能，並指定功能存放做為安裝來源檔的位置。

#### <a name="to-create-a-feature-file-store"></a>建立功能檔案存放

1.  在網路的伺服器上建立共用資料夾。 例如，  *\\\network\share\sxs*。

2.  確認您已在功能存放指派正確的權限。 來源路徑或檔案共用必須授與**讀取**權限給**Everyone**群組 （不建議基於安全性理由），或電腦帳戶 (*網域*\\ *SERverNAME*$) 的伺服器想要使用這個功能存放安裝功能; 授與使用者帳戶存取權不足夠。

    您可以在 Windows 桌面上執行下列任一項操作，以存取檔案共用和權限設定。

    -   在共用資料夾上按一下滑鼠右鍵，按一下 [內容]，然後在 [安全性] 索引標籤上變更允許的使用者及其對資料夾的存取權限。

    -   在共用資料夾上按一下滑鼠右鍵，指向 [共用對象] ，然後按一下 [特定人員] 。

    > [!NOTE]
    > 即使工作群組伺服器的電腦帳戶擁有外部共用的 [讀取]  權限，工作群組中的伺服器仍無法存取外部檔案共用。 用於工作群組伺服器的替代來源位置包括安裝媒體、Windows Update，以及儲存在本機工作群組伺服器上的 VHD 或 WIM 檔。

3.  從 Windows Server 安裝媒體將 **Sources\SxS** 複製到您在步驟 1 中建立的共用資料夾。

## <a name="BKMK_methods"></a>移除功能檔案的方法
在「功能隨選安裝」設定中，有兩種方法可以從 Windows Server 移除功能檔案。

-   `remove`參數的`Uninstall-WindowsFeature`cmdlet 可讓您刪除伺服器或離線虛擬硬碟 (VHD) 執行 Windows Server 2012 R2 或 Windows Server 2012 中的功能檔案。 有效值`remove`參數是角色、 角色服務與功能的名稱。

-   部署映像服務與管理 (DISM) 命令可讓您建立自訂 WIM 檔，藉由省略不需要或是可以從其他遠端來源取得的功能檔案來節省磁碟空間。 如需有關使用 DISM 準備自訂映像的詳細資訊，請參閱 [如何啟用或停用 Windows 功能](https://technet.microsoft.com/library/hh824822.aspx)。

## <a name="BKMK_remove"></a>使用 Uninstall-windowsfeature 移除功能檔案
若要從伺服器和執行 Windows Server 2012 R2 或 Windows Server 2012 的離線 Vhd 解除安裝角色、 角色服務與功能以及刪除功能檔案，您可以使用 Uninstall-windowsfeature cmdlet。 您可以同時解除安裝，如有需要，刪除相同的角色、 角色服務與同一個命令的功能。

> [!IMPORTANT]
> 當您刪除的角色功能檔案時，角色服務或功能、 角色、 角色服務和功能，取決於您要移除的檔案也會一併刪除。 如果您要刪除角色服務或子功能的功能檔案，而且不保留父系角色或功能的任何其他角色服務或子功能，則整個父系角色或功能的檔案都會被刪除。 若要檢視 `Uninstall-WindowsFeature -remove` 命令會刪除的所有功能檔案，請將 `whatif` 參數新增至命令以執行，然後在不實際刪除功能檔案的情況下檢視結果。

#### <a name="to-remove-role-and-feature-files-by-using-uninstall-windowsfeature"></a>使用 Uninstall-WindowsFeature 移除角色和功能檔案

1.  執行下列其中一個動作，使用提高的使用者權限開啟 Windows PowerShell 工作階段。

    > [!NOTE]
    > 如果您從遠端伺服器解除安裝角色和功能，您不需要以提高的使用者權限執行 Windows PowerShell。

    -   在 Windows 桌面上，用滑鼠右鍵按一下工作列上的 [Windows PowerShell]，然後按一下 [以系統管理員身分執行]。

    -   在 Windows 上**開始**畫面，[Windows PowerShell] 磚中，以滑鼠右鍵按一下，然後在應用程式列中，按一下**系統管理員身分執行**。

    -   在伺服器上執行的 Server Core 安裝選項，輸入**PowerShell**至命令提示字元中，然後按**Enter**。

2.  輸入下列程式碼，然後按 **Enter**。

    ```
    Uninstall-WindowsFeature -Name <feature_name> -computerName <computer_name> -remove
    ```

    **範例:**「遠端桌面授權」是遠端桌面服務最後剩下要安裝的角色服務。 命令會解除安裝遠端桌面授權，然後從指定的伺服器 *contoso_1* 刪除整個遠端桌面服務角色的功能檔案。

    ```
    Uninstall-WindowsFeature -Name rdS-Licensing -computerName contoso_1 -remove
    ```

    **範例:** 在下列範例中，命令會從離線 VHD 移除 active directory 網域服務和群組原則管理。 角色和功能會最先解除安裝，然後其功能檔案會從離線 VHD *Contoso.vhd* 整個移除。

    > [!NOTE]
    > 您必須新增`computerName`參數，如果您電腦上執行此指令程式執行 Windows 8.1 或 Windows 8。
    > 
    > 如果您所輸入的 VHD 檔案的名稱從網路共用，該共用必須授與其**讀取**並**撰寫**您選取要掛接 VHD 之伺服器的電腦帳戶的權限。 只有使用者的帳戶存取權是不夠的。 該共用可以將 [讀取]  和 [寫入]  權限授與 **Everyone** 群組以允許存取 VHD，但基於安全性因素，不建議您這樣做。

    ```
    Uninstall-WindowsFeature -Name AD-Domain-Services,GPMC -VHD C:\WS2012VHDs\Contoso.vhd -computerName ContosoDC1
    ```

## <a name="see-also"></a>另請參閱
[安裝或解除安裝角色、 角色服務或功能](install-or-uninstall-roles-role-services-or-features.md)
[Windows Server Installation Options](https://technet.microsoft.com/library/hh831786.aspx)
[如何啟用或停用 Windows 功能](https://technet.microsoft.com/library/hh824822.aspx)
[部署映像服務與管理 (DISM) 概觀](https://technet.microsoft.com/library/hh825236.aspx)


