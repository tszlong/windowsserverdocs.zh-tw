---
title: 安裝或解除安裝角色、角色服務或功能
description: 伺服器管理員
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 04f16d84-45c2-4771-84c1-1cc973d0ee02
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 32d356b3ae70b7b15f23a40247e73b4b8f61c3db
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822369"
---
# <a name="install-or-uninstall-roles-role-services-or-features"></a>安裝或解除安裝角色、角色服務或功能

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

Windows Server 中的伺服器管理員 主控台和 Windows PowerShell cmdlet 的伺服器管理員允許安裝角色和功能移轉到本機或遠端伺服器或離線虛擬硬碟 (Vhd)。 您可以在單一的遠端伺服器上安裝多個角色和功能，或在單一的離線 VHD 新增角色及功能精靈 或 Windows PowerShell 工作階段。  
  
> [!IMPORTANT]  
> 伺服器管理員無法用來管理較新版本的 Windows Server 作業系統。 在 Windows Server 2012 R2 或 Windows 8.1 上執行的伺服器管理員不能在執行 Windows Server 2016 的伺服器上安裝角色、 角色服務與功能。  
  
您必須登入到伺服器以系統管理員身分安裝或解除安裝角色、 角色服務與功能。 使用目標伺服器上不具有系統管理員權限的帳戶登入本機電腦時，請以滑鼠右鍵按一下 [伺服器] 磚中的目標伺服器，然後按一下 [管理為] 以提供具系統管理員權限的帳戶。 您要掛接離線 VHD 的伺服器必須新增到 [伺服器管理員]，而且您必須具備該伺服器的系統管理員權限。  
  
如需角色、 角色服務與功能的詳細資訊，請參閱 <<c0> [ 角色、 角色服務與功能](https://go.microsoft.com/fwlink/p/?LinkId=239558)。  
  
本主題涵蓋下列各節。  
  
-   [使用新增角色及功能精靈安裝角色、 角色服務與功能](#BKMK_installarfw)  
  
-   [使用 Windows PowerShell Cmdlet 來安裝角色、角色服務與功能](#BKMK_installwps)  
  
-   [移除角色、 角色服務與功能，使用移除角色及功能精靈](#BKMK_removerrfw)  
  
-   [使用 Windows PowerShell Cmdlet 來移除角色、角色服務與功能](#BKMK_removewps)  
  
-   [執行 Windows PowerShell 指令碼，在多個伺服器上安裝角色和功能](#BKMK_batch)  
  
-   [安裝 .NET Framework 3.5 和其他指定功能](#BKMK_FoD)  
  
## <a name="BKMK_installarfw"></a>使用新增角色及功能精靈安裝角色、 角色服務與功能  
在 新增角色及功能精靈 中的單一工作階段，您可以在本機伺服器，也就是已新增至伺服器管理員 中或離線 VHD 的遠端伺服器上安裝角色、 角色服務與功能。 如需如何將伺服器新增到伺服器管理員管理的詳細資訊，請參閱[將伺服器新增到伺服器管理員](add-servers-to-server-manager.md)。  
  
> [!NOTE]  
> 如果您在 Windows Server 2016 或 Windows 10 上執行伺服器管理員，您可以使用 [新增角色及功能精靈] 在伺服器和執行 Windows Server 2016 的離線 Vhd 上安裝角色和功能。  
  
#### <a name="to-install-roles-and-features-by-using-the-add-roles-and-features-wizard"></a>若要使用新增角色及功能精靈來安裝角色和功能  
  
1.  如果已經開啟伺服器管理員，請移至下一個步驟。 如果尚未開啟伺服器管理員，請執行下列其中一項動作來將它開啟。  
  
    -   在 Windows 桌面上，按一下 Windows 工作列中的 [伺服器管理員] 來啟動 [伺服器管理員]。  
  
    -   在 Windows 上**開始**畫面上，按一下**伺服器管理員**圖格。  
  
2.  在 **管理**功能表上，按一下**新增角色及功能**。  
  
3.  在 [在您開始前] 頁面上，確認已準備好目的地伺服器和網路環境，以便安裝您要的角色和功能。 按一下 [下一步] 。  
  
4.  在 [選取安裝類型] 頁面上，選取 [角色型或功能型安裝] 以在單一伺服器上安裝角色或功能的所有部分，或 [遠端桌面服務安裝] 來為遠端桌面服務安裝虛擬機器型桌面基礎結構或工作階段型桌面基礎結構。 [遠端桌面服務安裝]  選項可讓系統管理員視需要將遠端桌面服務角色的邏輯部分分散到不同的伺服器。 按一下 [下一步] 。  
  
5.  在 [選取目的地伺服器] 頁面上，從伺服器集區選取一部伺服器，或者選取一個離線 VHD。 若要選取離線 VHD 做為目的地伺服器，請先選取要掛接 VHD 的伺服器，然後選取 VHD 檔案。 如需有關如何將伺服器新增到伺服器集區的資訊，請參閱 <<c0> [ 將伺服器新增到伺服器管理員](add-servers-to-server-manager.md)。 選取目的地伺服器之後，按 [下一步] 。  
  
    > [!NOTE]  
    > 若要在離線 VHD 上安裝角色及功能，目標 VHD 必須符合下列需求。  
    >   
    > -   Vhd 必須執行符合版本的伺服器管理員正在執行的 Windows Server 的版本。 請參閱開頭的附註[使用新增角色及功能精靈安裝角色、 角色服務與功能](#BKMK_installarfw)。  
    > -   VHD 不能有多個系統磁碟區或磁碟分割。  
    > -   儲存在 VHD 檔案中的網路共用資料夾必須授與下列存取權限給您選取要掛接 VHD 之伺服器的電腦 (或本機系統) 帳戶。 只有使用者的帳戶存取權是不夠的。 該共用可以將 [讀取]  和 [寫入]  權限授與 **Everyone** 群組以允許存取 VHD，但基於安全性因素，不建議您這樣做。  
    >   
    >     -   [檔案共用] 對話方塊上的 [讀取/寫入] 存取權。  
    >     -   **完全控制**上存取**安全性**索引標籤、 檔案或資料夾**屬性** 對話方塊。  
  
6.  選取角色，選取角色的角色服務 (如果適用)，然後按 [下一步]  以選取功能。  
  
    當您繼續時，新增角色及功能精靈會自動通知您如果選取的角色或功能無法安裝或正常操作，導致在目的地伺服器上找不到的衝突。 系統也會提示您新增任何所選取的角色或功能所需的角色、角色服務或功能。  
  
    此外，如果您計畫從遠端管理角色 (可能是從另一部伺服器，或從執行遠端伺服器管理工具的 Windows 用戶端電腦)，則可以選擇不要在目的地伺服器上安裝適用於角色的管理工具和嵌入式管理單元。 根據預設，在新增角色及功能精靈 中，已選取要安裝的管理工具。  
  
7.  在 [確認安裝選項]  頁面中，檢閱您選取的角色、功能及伺服器。 如果您已經準備好進行安裝，請按一下 [安裝]。  
  
    您也可以使用 Windows PowerShell 的自動安裝，您可以使用以 XML 為基礎的組態檔匯出您的選擇。 若要匯出您在此指定的設定新增角色及功能精靈 工作階段中，按一下**匯出組態設定**，然後將 XML 檔案儲存到方便存取的位置。  
  
    [確認安裝選項]  頁面上的 [指定替代來源路徑]  命令，可以讓您針對所選伺服器上安裝角色與功能所需的檔案指定替代來源路徑。 在 Windows Server 2012 和更新版本的 Windows Server[功能隨](https://go.microsoft.com/fwlink/p/?LinkID=241573)可讓您減少使用的磁碟空間的作業系統，從獨立管理的伺服器中移除角色及功能檔案從遠端。 如果您已經使用 `Uninstall-WindowsFeature -remove` Cmdlet 移除伺服器中的角色與功能檔案，您日後可以透過指定替代來源路徑或指定儲存必要角色與功能檔案的共用，在伺服器上安裝角色與功能。 來源路徑或檔案共用必須授與**讀取**權限給**Everyone**群組 （不建議基於安全性理由），或電腦帳戶 (*網域*\\ *SERverNAME*$) 目的地伺服器的使用者帳戶存取權授與不足夠。 如需功能隨選安裝的相關資訊，請參閱 [Windows Server 安裝選項](https://go.microsoft.com/fwlink/p/?LinkId=241573)。  
  
    當您要執行的實體伺服器上安裝角色、 角色服務與功能時，您可以指定 WIM 檔案做為替代功能檔案來源。 WIM 檔案的來源路徑應該是以下列格式，與**WIM**做為首碼，並用功能檔案所在的尾碼索引：**WIM:e:\sources\install.wim:4**。 不過，您不能使用 WIM 檔案直接做為來源; 離線 VHD 安裝角色、 角色服務與功能您必須掛接離線 VHD 並指向其原始程式檔的掛接路徑，或必須指向包含 WIM 檔案的內容複本的資料夾。  
  
8.  按一下後**安裝**，則**Průběh instalace**頁面會顯示安裝進度、 結果及訊息，例如警告、 失敗或後續安裝設定步驟所需的角色或您所安裝的功能。 在 Windows Server 2012 和更新版本的 Windows Server 中，您可以關閉新增角色及功能精靈安裝仍在進行，並檢視安裝結果或其他訊息中的時**通知**上方區域伺服器管理員 主控台中。 按一下 [**通知**旗標圖示，以查看更多詳細的安裝或其他您正在執行伺服器管理員] 中的工作。  
  
## <a name="BKMK_installwps"></a>使用 Windows PowerShell cmdlet 安裝角色、 角色服務與功能  
Windows PowerShell 函式，類似於以 GUI 為基礎的伺服器管理員部署 cmdlet 新增角色及功能精靈，並移除角色及功能精靈 中，但有重要的差異。 在 Windows PowerShell 中，不同於在 [新增角色及功能精靈]，管理工具和嵌入式管理單元的角色不包含預設。 若要在角色安裝中包含管理工具，請新增 `IncludeManagementTools` 參數到 Cmdlet。 如果您要在執行 Windows Server 2012 或更新版本的 Server Core 安裝選項的伺服器上安裝角色和功能，您可以將角色的管理工具加入安裝，但無法安裝以 GUI 為基礎的管理工具和嵌入式管理單元在伺服器上執行 Windows Server 的 Server Core 安裝選項。 僅命令列，可以在 Server Core 安裝選項上安裝 Windows PowerShell 管理工具。  
  
#### <a name="to-install-roles-and-features-by-using-the-install-windowsfeature-cmdlet"></a>使用 Install-WindowsFeature Cmdlet 安裝角色及功能  
  
1.  執行下列其中一個動作，使用提高的使用者權限開啟 Windows PowerShell 工作階段。  
  
    > [!NOTE]  
    > 如果您要在遠端伺服器上安裝角色和功能，您不需要以提高的使用者權限執行 Windows PowerShell。  
  
    -   在 Windows 桌面上，用滑鼠右鍵按一下工作列上的 [Windows PowerShell]，然後按一下 [以系統管理員身分執行]。  
  
    -   在 Windows 上**開始**畫面，以滑鼠右鍵按一下適用於 Windows PowerShell 的圖格，然後在應用程式列中，按一下**系統管理員身分執行**。  
  
2.  輸入 **Get-WindowsFeature** ，然後按 **Enter** ，以檢視本機伺服器上可用且已安裝的角色及功能清單。 如果本機電腦不是伺服器，或您想要在遠端伺服器的資訊，請執行**Get-windowsfeature-computerName <***computer_name***>**，在其中*computer_name*代表正在執行 Windows Server 2016 的遠端電腦的名稱。 此 cmdlet 結果包含角色與功能，您將新增至您在步驟 4 中的 cmdlet 的命令的名稱。  
  
    > [!NOTE]  
    > 在 Windows PowerShell 3.0 和更新版本的 Windows PowerShell 中，沒有伺服器管理員 cmdlet 模組匯入 Windows PowerShell 工作階段中執行屬於模組的 cmdlet 之前需要。 當您首次執行屬於模組的 Cmdlet 時，會自動匯入模組。 此外，Windows PowerShell cmdlet 和與 cmdlet 中使用的功能名稱都不會區分大小寫。  
  
3.  型別**get-help Install-windowsfeature**，然後按**Enter**若要檢視的語法和接受的參數`Install-WindowsFeature`cmdlet。  
  
4.  輸入下列命令，並按**Enter**，其中*feature_name*表示命令名稱的角色或功能，您想要安裝 （在步驟 2 中取得），和*computer_name*代表您要安裝角色和功能的遠端電腦。 使用逗號來分隔多個 *feature_name* 值。 如果角色或功能安裝需要重新啟動目的地伺服器，則 `Restart` 參數會自動重新啟動目的地伺服器。  
  
    ```  
    Install-WindowsFeature -Name <feature_name> -computerName <computer_name> -Restart  
    ```  
  
    若要在離線 VHD 上安裝角色及功能，請新增 `computerName` 參數和 `VHD` 參數。 如果您未新增 `computerName` 參數，這個 Cmdlet 會假設本機電腦已掛接以存取 VHD。 `computerName` 參數包含要掛接 VHD 的伺服器名稱，`VHD` 參數則包含指定伺服器上的 VHD 檔案路徑。  
  
    > [!NOTE]  
    > 您必須新增`computerName`參數，如果您電腦上執行此指令程式執行 Windows 用戶端作業系統。  
    >   
    > 若要在離線 VHD 上安裝角色及功能，目標 VHD 必須符合下列需求。  
    >   
    > -   Vhd 必須執行符合版本的伺服器管理員正在執行的 Windows Server 的版本。 請參閱開頭的附註[使用新增角色及功能精靈安裝角色、 角色服務與功能](#BKMK_installarfw)。  
    > -   VHD 不能有多個系統磁碟區或磁碟分割。  
    > -   儲存在 VHD 檔案中的網路共用資料夾必須授與下列存取權限給您選取要掛接 VHD 之伺服器的電腦 (或本機系統) 帳戶。 只有使用者的帳戶存取權是不夠的。 該共用可以將 [讀取]  和 [寫入]  權限授與 **Everyone** 群組以允許存取 VHD，但基於安全性因素，不建議您這樣做。  
    >   
    >     -   [檔案共用] 對話方塊上的 [讀取/寫入] 存取權。  
    >     -   **完全控制**上存取**安全性**索引標籤、 檔案或資料夾**屬性** 對話方塊。  
  
    ```  
    Install-WindowsFeature -Name <feature_name> -VHD <path> -computerName <computer_name> -Restart  
    ```  
  
    **範例:** 下列 cmdlet 會在遠端伺服器 ContosoDC1 上安裝 active directory 網域服務角色和群組原則管理功能。 管理工具和嵌入式管理單元都已使用 `IncludeManagementTools` 參數新增，如果安裝需要重新啟動伺服器，則會自動啟動目的地伺服器。  
  
    ```  
    Install-WindowsFeature -Name AD-Domain-Services,GPMC -computerName ContosoDC1 -IncludeManagementTools -Restart  
    ```  
  
5.  完成安裝後，確認安裝 %installationdirectory**所有伺服器**管理員伺服器 頁面中，選取的伺服器，在其安裝角色和功能，並檢視**角色及功能**針對所選伺服器頁面上的圖格。 您也可以執行`Get-WindowsFeature`目標所選伺服器的 cmdlet (Get-windowsfeature-computerName <*computer_name*>) 來檢視伺服器上所安裝的角色和功能的清單。  
  
## <a name="BKMK_removerrfw"></a>移除角色、 角色服務與功能，使用移除角色及功能精靈  
您必須登入到伺服器解除安裝角色、 角色服務與功能的系統管理員身分。 使用解除安裝目標伺服器上不具有系統管理員權限的帳戶登入本機電腦時，請以滑鼠右鍵按一下 [伺服器]  磚中的目標伺服器，然後按一下 [管理為]  以提供具系統管理員權限的帳戶。 您要掛接離線 VHD 的伺服器必須新增到 [伺服器管理員]，而且您必須具備該伺服器的系統管理員權限。  
  
#### <a name="to-remove-roles-and-features-by-using-the-remove-roles-and-features-wizard"></a>若要移除角色及功能，使用移除角色及功能精靈  
  
1.  如果已經開啟伺服器管理員，請移至下一個步驟。 如果尚未開啟伺服器管理員，請執行下列其中一項動作來將它開啟。  
  
    -   在 Windows 桌面上，按一下 Windows 工作列中的 [伺服器管理員] 來啟動 [伺服器管理員]。  
  
    -   在 Windows [開始]  畫面上，按一下 [伺服器管理員]  磚。  
  
2.  在 [管理]  功能表上，按一下 [移除角色及功能] 。  
  
3.  在 [在您開始前] 頁面中，確認您已準備好從伺服器移除角色或功能。 按一下 [下一步] 。  
  
4.  在 **選取目的地伺服器**頁面上，從伺服器集區中，選取伺服器或選取離線 VHD。 若要選取離線 VHD，請先選取要掛接 VHD 的伺服器，然後選取 VHD 檔案。  
  
    > [!NOTE]  
    > 儲存在 VHD 檔案中的網路共用資料夾必須授與下列存取權限給您選取要掛接 VHD 之伺服器的電腦 (或本機系統) 帳戶。 只有使用者的帳戶存取權是不夠的。 該共用可以將 [讀取]  和 [寫入]  權限授與 **Everyone** 群組以允許存取 VHD，但基於安全性因素，不建議您這樣做。  
    >   
    > -   [檔案共用] 對話方塊上的 [讀取/寫入] 存取權。  
    > -   檔案或資料夾 [內容] 對話方塊的 [安全性] 索引標籤上的 [完全控制] 存取權。  
  
    如需有關如何將伺服器新增到伺服器集區的資訊，請參閱 <<c0> [ 將伺服器新增到伺服器管理員](add-servers-to-server-manager.md)。 選取目的地伺服器之後，按 [下一步] 。  
  
    > [!NOTE]  
    > 您可以使用移除角色及功能精靈 來移除角色和功能從伺服器執行相同版本的 Windows Server 的支援伺服器管理員的您所使用的版本。 您無法移除角色、 角色服務或功能從伺服器執行 Windows Server 2016 中，如果您執行 Windows Server 2012 R2、 Windows Server 2012 或 Windows 8 上的 伺服器管理員。 您無法使用移除角色及功能精靈 來移除角色和功能從執行 Windows Server 2008 的伺服器或 Windows Server 2008 R2。  
  
5.  選取角色，選取角色的角色服務 (如果適用)，然後按 [下一步]  以選取功能。  
  
    您要繼續，移除角色及功能精靈 會自動提示您移除任何角色時，角色服務或功能，才能執行的角色或功能，您會移除。  
  
    此外，您可以選擇移除管理工具和嵌入式管理單元，在目的地伺服器上的角色。 依預設，在移除角色及功能精靈，已選取要移除的管理工具。 如果您計畫使用所選取伺服器來管理其他遠端伺服器上的角色，可以保留管理工具和嵌入式管理單元。  
  
6.  在 [確認移除選項]  頁面上，檢閱您的角色、功能和伺服器選取項目。 如果您已準備好移除角色或功能，請按一下**移除**。  
  
7.  按一下 之後**移除**，則**移除進度**頁面會顯示移除進度、 結果及訊息，例如警告、 失敗或所需的資源，例如的後續移除設定步驟正在重新啟動目的地伺服器。 在 Windows Server 2012 和更新版本的 Windows Server 中，您可以關閉移除角色及功能精靈 時移除仍在進行，並檢視移除結果或其他訊息中的**通知**的上方區域伺服器管理員 主控台中。 按一下 [**通知**旗標，以查看更多詳細的移除或其他您正在執行伺服器管理員] 中的工作。  
  
## <a name="BKMK_removewps"></a>使用 Windows PowerShell cmdlet 來移除角色、 角色服務與功能  
Windows PowerShell 函式，類似於以 GUI 為基礎的伺服器管理員部署 cmdlet 會移除角色及功能精靈 中，但有重要的差異。 在 Windows PowerShell 中，不同於在移除角色及功能精靈，管理工具和嵌入式管理單元的角色不會移除預設值。 若要移除管理工作以做為角色移除的一部分，請將 `IncludeManagementTools` 參數新增到 Cmdlet。 如果您解除安裝角色及功能的伺服器執行 Windows Server 2012 的 Server Core 安裝選項或以較新版的 Windows Server、 此參數會移除命令列和指定的 Windows PowerShell 管理工具角色和功能。  
  
#### <a name="to-remove-roles-and-features-by-using-the-uninstall-windowsfeature-cmdlet"></a>使用 Uninstall-WindowsFeature Cmdlet 移除角色及功能  
  
1.  執行下列其中一個動作，使用提高的使用者權限開啟 Windows PowerShell 工作階段。  
  
    > [!NOTE]  
    > 如果您從遠端伺服器解除安裝角色和功能，您不需要以提高的使用者權限執行 Windows PowerShell。  
  
    -   在 Windows 桌面上，用滑鼠右鍵按一下工作列上的 [Windows PowerShell]，然後按一下 [以系統管理員身分執行]。  
  
    -   在 Windows 上**開始**畫面，[Windows PowerShell] 磚中，以滑鼠右鍵按一下，然後在應用程式列中，按一下**系統管理員身分執行**。  
  
2.  輸入 **Get-WindowsFeature** ，然後按 **Enter** ，以檢視本機伺服器上可用且已安裝的角色及功能清單。 如果本機電腦不是伺服器，或您想要在遠端伺服器的資訊，請執行**Get-windowsfeature-computerName <***computer_name***>**，在其中*computer_name*代表正在執行 Windows Server 2016 的遠端電腦的名稱。 此 cmdlet 結果包含角色與功能，您將新增至您在步驟 4 中的 cmdlet 的命令的名稱。  
  
    > [!NOTE]  
    > 在 Windows PowerShell 3.0 和更新版本的 Windows PowerShell 中，沒有伺服器管理員 cmdlet 模組匯入 Windows PowerShell 工作階段中執行屬於模組的 cmdlet 之前需要。 當您首次執行屬於模組的 Cmdlet 時，會自動匯入模組。 此外，Windows PowerShell cmdlet 和與 cmdlet 中使用的功能名稱都不會區分大小寫。  
  
3.  型別**get-help Uninstall-windowsfeature**，然後按**Enter**若要檢視的語法和接受的參數`Uninstall-WindowsFeature`cmdlet。  
  
4.  輸入下列內容，然後按 **Enter**，其中 *feature_name* 代表您要移除的角色或功能的命令名稱 (可在步驟 2 中取得)，而 *computer_name* 代表您要移除角色及功能的遠端電腦。 使用逗號來分隔多個 *feature_name* 值。 如果角色或功能移除需要重新啟動目的地伺服器，則 `Restart` 參數會自動重新啟動目的地伺服器。  
  
    ```  
    Uninstall-WindowsFeature -Name <feature_name> -computerName <computer_name> -Restart  
    ```  
  
    若要從離線 VHD 上移除角色及功能，請新增 `computerName` 參數和 `VHD` 參數。 如果您未新增 `computerName` 參數，這個 Cmdlet 會假設本機電腦已掛接以存取 VHD。 `computerName` 參數包含要掛接 VHD 的伺服器名稱，`VHD` 參數則包含指定伺服器上的 VHD 檔案路徑。  
  
    > [!NOTE]  
    > 您必須新增`computerName`參數，如果您電腦上執行此指令程式執行 Windows 用戶端作業系統。  
    >   
    > 儲存在 VHD 檔案中的網路共用資料夾必須授與下列存取權限給您選取要掛接 VHD 之伺服器的電腦 (或本機系統) 帳戶。 只有使用者的帳戶存取權是不夠的。 該共用可以將 [讀取]  和 [寫入]  權限授與 **Everyone** 群組以允許存取 VHD，但基於安全性因素，不建議您這樣做。  
    >   
    > -   [檔案共用] 對話方塊上的 [讀取/寫入] 存取權。  
    > -   **完全控制**上存取**安全性**索引標籤、 檔案或資料夾**屬性** 對話方塊。  
  
    ```  
    Uninstall-WindowsFeature -Name <feature_name> -VHD <path> -computerName <computer_name> -Restart  
    ```  
  
    **範例:** 下列 cmdlet 會從遠端伺服器 contosodc1 上移除 active directory 網域服務角色和群組原則管理功能。 也會將管理工具和嵌入式管理單元移除，如果移除需要重新啟動伺服器，則會自動啟動目的地伺服器。  
  
    ```  
    Uninstall-WindowsFeature -Name AD-Domain-Services,GPMC -computerName ContosoDC1 -IncludeManagementTools -Restart  
    ```  
  
5.  完成移除後，確認 [移除角色及功能，開啟**所有伺服器**管理員伺服器] 頁面中，選取的伺服器從中移除角色及功能，並檢視**角色和功能**針對所選伺服器頁面上的圖格。 您也可以執行`Get-WindowsFeature`目標所選伺服器的 cmdlet (Get-windowsfeature-computerName <*computer_name*>) 來檢視伺服器上所安裝的角色和功能的清單。  
  
## <a name="BKMK_batch"></a>在多部伺服器上安裝角色和功能，藉由執行 Windows PowerShell 指令碼  
雖然您無法使用新增角色及功能精靈，多部目標伺服器上安裝角色、 角色服務與功能，在單一精靈工作階段中，您可以使用 Windows PowerShell 指令碼在多個目標上安裝角色、 角色服務與功能您使用伺服器管理員管理的伺服器。 您用來執行批次部署時，呼叫此程序時，指令碼指向 XML 組態檔，您可以輕鬆地建立使用新增角色及功能精靈，然後按一下**匯出組態設定**之後透過精靈逐步引導**確認安裝選項**新增角色及功能精靈 頁面。  
  
> [!IMPORTANT]  
> 在您的指令碼中指定的所有目標伺服器必須都執行符合版本的伺服器管理員都正在本機電腦的 Windows Server 的版本。 比方說，如果您在 Windows 10 上執行伺服器管理員，您可以安裝角色、 角色服務與功能都執行 Windows Server 2016 的伺服器上。 以 GUI 為基礎的管理工具加入安裝中，如果安裝程序會自動將目標伺服器正在 Windows Server 的 Server Core 安裝選項的完整安裝選項 (server 含完整 GUI，也就是為執行伺服器圖形化介面）。  
>   
> 在本節中提供的指令碼是如何執行批次部署，使用範例`Install-WindowsFeature`指令程式和 Windows PowerShell 指令碼。 在多個伺服器上執行批次部署還有其他可能的指令碼和方法。 若要搜尋或提供部署角色和功能的其他指令碼，請搜尋 [Script Center Repository (指令碼中心存放庫)](https://gallery.technet.microsoft.com/ScriptCenter)。  
  
#### <a name="to-install-roles-and-features-on-multiple-servers"></a>在多個伺服器上安裝角色和功能  
  
1.  如果您尚未這樣做，建立 XML 組態檔，其中包含角色、 角色服務和您想要在多部伺服器上安裝的功能。 您可以建立此組態檔執行新增角色及功能精靈 中，選取角色、 角色服務和您想要的功能，然後按一下**匯出組態設定**之後透過精靈逐步引導到**確認安裝選項**頁面。 將設定檔儲存到方便的位置。 如果您只是要建立設定檔才執行，就不需要按一下 [安裝]  或完成精靈。  
  
2.  執行下列其中一個動作，使用提高的使用者權限開啟 Windows PowerShell 工作階段。  
  
    -   在 Windows 桌面上，用滑鼠右鍵按一下工作列上的 [Windows PowerShell]，然後按一下 [以系統管理員身分執行]。  
  
    -   在 Windows 上**開始**畫面，[Windows PowerShell] 磚中，以滑鼠右鍵按一下，然後在應用程式列中，按一下**系統管理員身分執行**。  
  
3.  複製並貼上下列指令碼到您的 Windows PowerShell 工作階段。  
  
    ```  
    function Invoke-WindowsFeatureBatchDeployment {  
        param (  
            [parameter(mandatory)]  
            [string[]] $computerNames,  
            [parameter(mandatory)]  
            [string] $ConfigurationFilepath  
        )  
  
        # Deploy the features on multiple computers simultaneously.  
        $jobs = @()  
        foreach($computerName in $computerNames) {  
            $jobs += start-Job -Command {  
                Install-WindowsFeature -ConfigurationFilepath $using:ConfigurationFilepath -computerName $using:computerName -Restart  
            }   
        }  
  
        Receive-Job -Job $jobs -Wait | select-Object Success, RestartNeeded, exitCode, FeatureResult  
    }  
    ```  
  
    如果您所選取的角色和功能需要，目標伺服器會自動重新啟動。  
  
4.  執行下列動作來執行函式。  
  
    1.  建立變數以儲存目標電腦的名稱，以逗號分隔。 在下列範例中，變數 `$ServerNames` 儲存了目標伺服器名稱 *Contoso_01* 和 *Contoso_02*。 按 **Enter** 鍵。  
  
        ```  
        # Sample Invocation  
        $ServerNames = 'Contoso_01','Contoso_02'  
        Invoke-WindowsFeatureBatchDeployment -computerNames $ServerNames -ConfigurationFilepath C:\Users\sampleuser\Desktop\DeploymentConfigTemplate.xml  
        ```  
  
    2.  若要執行函式，輸入下列內容，然後按 **Enter**，其中 `$ServerNames` 是您在先前步驟中建立的變數範例，而 *C:\Users\Sampleuser\Desktop\DeploymentConfigTemplate.xml* 是您在步驟 1 中建立的設定檔路徑的範例。  
  
        **Invoke-WindowsFeatureBatchDeployment -computerNames $ServerNames -ConfigurationFilepath C:\Users\Sampleuser\Desktop\DeploymentConfigTemplate.xml**  
  
5.  完成安裝後，確認安裝 %installationdirectory**所有伺服器**管理員伺服器 頁面中，選取的伺服器，在其安裝角色和功能，並檢視**角色及功能**針對所選伺服器頁面上的圖格。 您也可以執行`Get-WindowsFeature`針對特定伺服器為目標的 cmdlet (`Get-WindowsFeature -computerName` <*computer_name*>) 來檢視伺服器上所安裝的角色和功能的清單。  
  
## <a name="BKMK_FoD"></a>安裝.NET Framework 3.5 和其他功能-隨選  
從 Windows Server 2012 和 Windows 8 開始，.NET Framework 3.5 （其中包含.NET Framework 2.0 和.NET Framework 3.0） 的功能檔案不在本機電腦依預設，您可以使用。 這些檔案已移除。 在「功能隨選安裝」設定中已移除的功能檔案，以及 .NET Framework 3.5 的功能檔案，皆可透過 Windows Update 提供。 根據預設，如果功能檔案不是執行 Windows Server 2012 或更新版本中，在目的地伺服器上安裝程序搜尋遺漏的檔案藉由連線至 Windows Update。 您可以覆寫預設行為設定群組原則設定，或指定替代來源路徑，在安裝期間，不論您要使用新增角色及功能精靈 GUI 或命令列安裝。  
  
您可以執行下列其中一個動作來安裝 .NET Framework 3.5。  
  
-   使用 [執行 Install-WindowsFeature Cmdlet 來安裝 .NET Framework 3.5](#BKMK_dotnetcmdlet) 以加入 `Source` 參數，並指定要從中取得.NET Framework 3.5 功能檔案的來源。 如果您沒有新增 `Source` 參數，安裝程序會先判斷群組原則設定是否已指定功能檔案的路徑，如果找不到這個路徑，則使用 Windows Update 來搜尋遺失的功能檔案。  
  
-   使用[使用新增角色及功能精靈 安裝.NET Framework 3.5](#BKMK_arfw)上指定替代來源檔案位置**確認安裝選項**新增角色及功能精靈 頁面。  
  
-   使用 [使用 DISM 來安裝 .NET Framework 3.5](#BKMK_dism) 以根據預設取得 Windows Update 的檔案，或是指定安裝媒體的來源路徑。  
  
若在本機電腦上找不到功能檔案，則使用[設定群組原則中功能檔案的替代來源](#BKMK_configgp) 取得 .NET Framework 3.5 或其他功能。  
  
> [!IMPORTANT]  
> 當您從遠端來源安裝功能檔案，來源路徑或檔案共用必須將 [讀取]  權限授與 **Everyone** 群組 (基於安全性因素不建議這麼做) 或目的地伺服器的電腦 (本機系統) 帳戶；授與使用者帳戶存取權是不夠的。  
>   
> 即使工作群組伺服器的電腦帳戶擁有外部共用的 [讀取]  權限，工作群組中的伺服器仍無法存取外部檔案共用。 用於工作群組伺服器的替代來源位置包括安裝媒體、Windows Update，以及儲存在本機工作群組伺服器上的 VHD 或 WIM 檔。  
>   
> 當您要執行的實體伺服器上安裝角色、 角色服務與功能時，您可以指定 WIM 檔案做為替代功能檔案來源。 WIM 檔案的來源路徑應該是以下列格式，與**WIM**做為首碼，並用功能檔案所在的尾碼索引：**WIM:e:\sources\install.wim:4**。 不過，您不能使用 WIM 檔案直接做為來源; 離線 VHD 安裝角色、 角色服務與功能您必須掛接離線 VHD 並指向其原始程式檔的掛接路徑，或必須指向包含 WIM 檔案的內容複本的資料夾。  
  
### <a name="BKMK_dotnetcmdlet"></a>若要執行 Install-windowsfeature cmdlet 來安裝.NET Framework 3.5  
  
1.  執行下列其中一個動作，使用提高的使用者權限開啟 Windows PowerShell 工作階段。  
  
    > [!NOTE]  
    > 如果您要安裝角色和功能從遠端伺服器，您不需要以提高的使用者權限執行 Windows PowerShell。  
  
    -   在 Windows 桌面上，用滑鼠右鍵按一下工作列上的 [Windows PowerShell]，然後按一下 [以系統管理員身分執行]。  
  
    -   在 Windows 上**開始**畫面，[Windows PowerShell] 磚中，以滑鼠右鍵按一下，然後在應用程式列中，按一下**系統管理員身分執行**。  
  
    -   在執行 Windows Server 2012 R2 的 Server Core 安裝選項的伺服器或 Windows Server 2012 中，輸入**PowerShell**至命令提示字元中，然後按**Enter**。  
  
2.  輸入下列命令，並按**Enter**。 在下列範例中，來源檔案位於 D 磁碟機上安裝媒體中的並列存放 (簡稱 **SxS**)。  
  
    ```  
    Install-WindowsFeature NET-Framework-Core -Source D:\Sources\SxS  
    ```  
  
    如果您要命令將 Windows Update 當做遺失功能檔案的來源，或已使用群組原則來設定預設來源，那麼除非您要指定不同的來源，否則就不需要新增 `Source` 參數。  
  
### <a name="BKMK_arfw"></a>若要使用新增角色及功能精靈 安裝.NET Framework 3.5  
  
1.  在 **管理**功能表中 伺服器管理員 中，按一下 **新增角色及功能**。  
  
2.  選取目的地伺服器執行 Windows Server 2016。  
  
3.  在 **選取的功能**頁面的新增角色及功能精靈中，選取 **.NET Framework 3.5**。  
  
4.  如果群組原則設定允許本機電腦這麼做，安裝程序會試圖使用 Windows Update 來取得遺失的功能檔案。 按一下 [安裝] ；您不需要繼續下一步驟。  
  
    如果群組原則設定不允許這麼做，或您想要使用.NET Framework 3.5 功能檔案，在另一個來源**確認安裝選項**頁面的精靈中，按一下**指定替代來源路徑**.  
  
5.  在安裝媒體中提供並列存放 (稱為 **SxS**) 或 WIM 檔案的路徑。 在下列範例中，安裝媒體位於 D 磁碟機上。  
  
    **D:\Sources\SxS\\**  
  
    若要指定 WIM 檔案，請新增 **WIM:** 首碼，然後將影像索引新增到 WIM 檔案中當做尾碼，如以下範例所示。  
  
    **WIM:\\\\***server_name***\share\install.wim:3**  
  
6.  按一下 [確定]，然後按一下 [安裝]。  
  
### <a name="BKMK_dism"></a>若要使用 DISM 來安裝.NET Framework 3.5  
  
1.  執行下列其中一個動作，使用提高的使用者權限開啟 Windows PowerShell 工作階段。  
  
    > [!NOTE]  
    > 如果您要安裝角色和功能從遠端伺服器，您不需要以提高的使用者權限執行 Windows PowerShell。  
  
    -   在 Windows 桌面上，用滑鼠右鍵按一下工作列上的 [Windows PowerShell]，然後按一下 [以系統管理員身分執行]。  
  
    -   在 Windows 上**開始**畫面，[Windows PowerShell] 磚中，以滑鼠右鍵按一下，然後在應用程式列中，按一下**系統管理員身分執行**。  
  
    -   在伺服器上執行的 Server Core 安裝選項，輸入**PowerShell**至命令提示字元中，然後按**Enter**。  
  
2.  執行下列其中一個 DISM 命令。  
  
    -   如果電腦可以存取 Windows Update，或已在群組原則中，執行下列命令設定預設來源檔案位置。  
  
        ```  
        DISM /online /Enable-Feature /Featurename:NetFx3 /All  
        ```  
  
    -   如果電腦沒有安裝媒體的存取權，請執行類似下列的命令。 在下列範例中，作業系統安裝媒體位於 D 磁碟機。`LimitAccess` 參數可防止命令試圖聯繫 Windows Update 或是執行 WSUS 的伺服器。  
  
        ```  
        DISM /online /Enable-Feature /Featurename:NetFx3 /All /LimitAccess /Source:d:\sources\sxs  
        ```  
  
    > [!NOTE]  
    > DISM 命令會區分大小寫。  
  
### <a name="BKMK_configgp"></a>在 群組原則中設定替代功能檔案來源  
本節描述的群組原則設定會指定 .NET Framework 3.5 檔案的授權來源位置，以及其他被視為「功能隨選安裝」設定一部分而遭移除的功能檔案。 原則設定**指定選用之元件安裝和元件修復的設定**位於**電腦設定 \ 系統管理範本 \ 系統**的群組原則中的資料夾管理主控台 」 或 「 本機群組原則編輯器。  
  
> [!NOTE]  
> 您必須是 Administrators 群組成員，才能在本機電腦上變更群組原則設定。 如果您要管理的電腦是在網域層級控制群組原則設定，那麼您必須是 Domain Administrators 群組的成員，才能變更群組原則設定。  
  
##### <a name="to-configure-a-default-alternate-source-path-in-group-policy"></a>設定群組原則中的預設替代來源路徑  
  
1.  在本機群組原則編輯器] 或 [群組原則管理主控台中，開啟下列原則設定。  
  
    **選用之元件安裝和元件修復的電腦設定 \ 系統管理指定設定**  
  
2. Sselect**已啟用**啟用原則設定，如果尚未啟用。  
  
3.  在 [選項] 區域中的 [替代來源檔案路徑] 文字方塊中，指定共用資料夾或 WIM 檔案的完整路徑。 若要將 WIM 檔案指定為替代來源檔案位置，請將首碼 **WIM:** 新增至路徑，然後新增影像索引，在 WIM 檔案中當做尾碼。 下列是您可以指定的值範例。  
  
    -   共用的資料夾路徑: **\\\\***server_name***\share\\* * * folder_name*  
  
    -   要在其中的 WIM 檔案的路徑**3**代表功能檔案位於影像的索引：**WIM:\\\\***server_name***\share\install.wim:3**  
  
4.  如果您不想要搜尋 Windows Update 中遺失的功能檔案選取此原則設定所控制的電腦**永遠不會嘗試從 Windows Update 下載裝載**。  
  
5.  如果此原則設定控制的電腦通常是透過 WSUS 接收更新，但您偏好透過 Windows Update 而非 WSUS 來尋找遺失的功能檔案，請選取 [直接連絡 Windows Update 下載修復內容，而不是從 Windows Server Update Services (WSUS)]。  
  
6.  您完成變更此原則設定之後，請按一下 [確定]，然後關閉群組原則編輯器。  
  
## <a name="see-also"></a>另請參閱  
[Windows Server 安裝選項](https://go.microsoft.com/fwlink/p/?LinkId=241573)  
[Microsoft.NET Framework 3.5 部署考量](https://go.microsoft.com/fwlink/p/?LinkId=248869)  
[如何啟用或停用 Windows 功能](https://go.microsoft.com/fwlink/p/?LinkId=246552)  
  


