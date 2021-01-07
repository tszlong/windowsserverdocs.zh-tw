---
title: 安裝或解除安裝角色、角色服務或功能
description: 瞭解如何將角色及功能安裝到本機或遠端伺服器，或 (Vhd) 的離線虛擬硬碟。
ms.topic: article
ms.assetid: 04f16d84-45c2-4771-84c1-1cc973d0ee02
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 0b82bf3941b64d0a8456c086ce9022aa531ab281
ms.sourcegitcommit: 605a9b46b74b2c7a9116e631e902467ea02a6e70
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/07/2021
ms.locfileid: "97965123"
---
# <a name="install-or-uninstall-roles-role-services-or-features"></a>安裝或解除安裝角色、角色服務或功能

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在 Windows Server 中，適用于伺服器管理員的伺服器管理員主控台和 Windows PowerShell Cmdlet 允許將角色及功能安裝到本機或遠端伺服器，或是離線虛擬硬碟 (Vhd) 。 您可以在單一遠端伺服器上安裝多個角色和功能，或在單一的 [新增角色及功能] Wizard 或 Windows PowerShell 會話中安裝離線 VHD。

> [!IMPORTANT]
> 伺服器管理員無法用來管理較新版本的 Windows Server 作業系統。 在 Windows Server 2012 R2 或 Windows 8.1 上執行的伺服器管理員不能用來在執行 Windows Server 2016 的伺服器上安裝角色、角色服務和功能。

您必須以系統管理員身分登入伺服器，才能安裝或卸載角色、角色服務和功能。 使用目標伺服器上不具有系統管理員權限的帳戶登入本機電腦時，請以滑鼠右鍵按一下 [伺服器] 磚中的目標伺服器，然後按一下 [管理為] 以提供具系統管理員權限的帳戶。 您要掛接離線 VHD 的伺服器必須新增到 [伺服器管理員]，而且您必須具備該伺服器的系統管理員權限。

如需角色、角色服務和功能的詳細資訊，請參閱 [角色、角色](https://go.microsoft.com/fwlink/p/?LinkId=239558)服務和功能。

本主題包含下列各節。

-   [使用 [新增角色及功能] 嚮導來安裝角色、角色服務和功能](#install-roles-role-services-and-features-by-using-the-add-roles-and-features-wizard)

-   [使用 Windows PowerShell Cmdlet 來安裝角色、角色服務與功能](#install-roles-role-services-and-features-by-using-windows-powershell-cmdlets)

-   [使用 [移除角色及功能] Wizard 移除角色、角色服務和功能](#remove-roles-role-services-and-features-by-using-the-remove-roles-and-features-wizard)

-   [使用 Windows PowerShell Cmdlet 來移除角色、角色服務與功能](#remove-roles-role-services-and-features-by-using-windows-powershell-cmdlets)

-   [執行 Windows PowerShell 指令碼，在多個伺服器上安裝角色和功能](#install-roles-and-features-on-multiple-servers-by-running-a-windows-powershell-script)

-   [安裝 .NET Framework 3.5 和其他指定功能](#install-net-framework-35-and-other-features-on-demand)

## <a name="install-roles-role-services-and-features-by-using-the-add-roles-and-features-wizard"></a>使用 [新增角色及功能] 嚮導來安裝角色、角色服務和功能
在 [新增角色及功能] 嚮導的單一會話中，您可以在本機伺服器上安裝角色、角色服務和功能、已新增至伺服器管理員的遠端伺服器，或是離線 VHD。 如需有關如何將伺服器新增至伺服器管理員進行管理的詳細資訊，請參閱 [將伺服器新增至伺服器管理員](add-servers-to-server-manager.md)。

> [!NOTE]
> 如果您執行的是 Windows Server 2016 或 Windows 10 上的伺服器管理員，您可以使用 [新增角色及功能] Wizard，只在執行 Windows Server 2016 的伺服器和離線 Vhd 上安裝角色和功能。

#### <a name="to-install-roles-and-features-by-using-the-add-roles-and-features-wizard"></a>使用新增角色及功能 Wizard 來安裝角色和功能

1.  如果已經開啟伺服器管理員，請移至下一個步驟。 如果尚未開啟伺服器管理員，請執行下列其中一項動作來將它開啟。

    -   在 Windows 桌面上，按一下 Windows 工作列中的 [伺服器管理員]  來啟動 [伺服器管理員]。

    -   在 Windows [ **開始** ] 畫面上，按一下 [ **伺服器管理員** ] 磚。

2.  在 [ **管理** ] 功能表上，按一下 [ **新增角色及功能**]。

3.  在 [在您開始前] 頁面上，確認已準備好目的地伺服器和網路環境，以便安裝您要的角色和功能。 按一下 [下一步] 。

4.  在 [選取安裝類型] 頁面上，選取 [角色型或功能型安裝] 以在單一伺服器上安裝角色或功能的所有部分，或 [遠端桌面服務安裝] 來為遠端桌面服務安裝虛擬機器型桌面基礎結構或工作階段型桌面基礎結構。 [遠端桌面服務安裝]  選項可讓系統管理員視需要將遠端桌面服務角色的邏輯部分分散到不同的伺服器。 按一下 [下一步] 。

5.  在 [選取目的地伺服器]  頁面上，從伺服器集區選取一部伺服器，或者選取一個離線 VHD。 若要選取離線 VHD 做為目的地伺服器，請先選取要掛接 VHD 的伺服器，然後選取 VHD 檔案。 如需有關如何將伺服器新增到伺服器集區的詳細資訊，請參閱 [將伺服器新增至伺服器管理員](add-servers-to-server-manager.md)。 選取目的地伺服器之後，按 [下一步]。

    > [!NOTE]
    > 若要在離線 VHD 上安裝角色及功能，目標 VHD 必須符合下列需求。
    >
    > -   Vhd 必須執行符合您正在執行之伺服器管理員版本的 Windows Server 版本。 請參閱 [安裝角色、角色服務和功能的開頭的附注，方法是使用 [新增角色及功能]](#install-roles-role-services-and-features-by-using-the-add-roles-and-features-wizard)。
    > -   VHD 不能有多個系統磁碟區或磁碟分割。
    > -   儲存在 VHD 檔案中的網路共用資料夾必須授與下列存取權限給您選取要掛接 VHD 之伺服器的電腦 (或本機系統) 帳戶。 只有使用者的帳戶存取權是不夠的。 該共用可以將 [讀取] 和 [寫入] 權限授與 **Everyone** 群組以允許存取 VHD，但基於安全性因素，不建議您這樣做。
    >
    >     -   [檔案共用] 對話方塊上的 [讀取/寫入] 存取權。
    >     -   [**安全性**] 索引標籤、[檔案 **或資料夾內容**] 對話方塊上的 [**完全控制**] 存取權。

6.  選取角色，選取角色的角色服務 (如果適用)，然後按 [下一步] 以選取功能。

    當您繼續進行時，[新增角色及功能] 嚮導會自動通知您是否已在目的地伺服器上找到可以防止選取的角色或功能進行安裝或正常操作的衝突。 系統也會提示您新增任何所選取的角色或功能所需的角色、角色服務或功能。

    此外，如果您計畫從遠端管理角色 (可能是從另一部伺服器，或從執行遠端伺服器管理工具的 Windows 用戶端電腦)，則可以選擇不要在目的地伺服器上安裝適用於角色的管理工具和嵌入式管理單元。 依預設，在 [新增角色及功能] 嚮導中，已選取要安裝的管理工具。

7.  在 [確認安裝選項] 頁面中，檢閱您選取的角色、功能及伺服器。 如果您已經準備好進行安裝，請按一下 [安裝]。

    您也可以將您的選項匯出到 XML 設定檔，讓您可以使用 Windows PowerShell 自動安裝。 若要匯出您在 [新增角色及功能] 嚮導會話中指定的設定，請按一下 [ **匯出設定**]，然後將 XML 檔案儲存到方便存取的位置。

    [確認安裝選項] 頁面上的 [指定替代來源路徑] 命令，可以讓您針對所選伺服器上安裝角色與功能所需的檔案指定替代來源路徑。 在 Windows server 2012 和更新版本的 Windows Server 中，依需求提供的 [功能](https://go.microsoft.com/fwlink/p/?LinkID=241573) 可讓您從遠端獨立管理的伺服器中移除角色及功能檔案，以減少作業系統使用的磁碟空間量。 如果您已經使用 `Uninstall-WindowsFeature -remove` Cmdlet 移除伺服器中的角色與功能檔案，您日後可以透過指定替代來源路徑或指定儲存必要角色與功能檔案的共用，在伺服器上安裝角色與功能。 來源路徑或檔案共用必須將 [**讀取**] 許可權授與 **Everyone** 群組 (基於安全性理由，不建議這麼做) 或目的地伺服器的電腦帳戶 (*DOMAIN* \\ *SERverNAME*$) ; 授與使用者帳戶存取權並不足夠。 如需功能隨選安裝的相關資訊，請參閱 [Windows Server 安裝選項](https://go.microsoft.com/fwlink/p/?LinkId=241573)。

    當您在執行中的實體伺服器上安裝角色、角色服務和功能時，您可以將 WIM 檔案指定為替代功能檔案來源。 WIM 檔案的來源路徑應該使用下列格式，加上 **WIM** 做為首碼，並用功能檔案所在的索引做為尾碼：**WIM:e:\sources\install.wim:4**。 不過，您無法直接使用 WIM 檔案作為將角色、角色服務和功能安裝至離線 VHD 的來源;您必須掛接離線 VHD 並指向其原始程式檔的掛接路徑，或者您必須指向包含 WIM 檔案內容複本的資料夾。

8.  按一下 [ **安裝**] 之後，[ **安裝進度** ] 頁面會顯示安裝進度、結果及訊息，例如警告、失敗或您安裝的角色或功能所需的後續安裝設定步驟。 在 Windows Server 2012 和更新版本的 Windows Server 中，您可以在安裝仍在進行時關閉 [新增角色及功能]，並在伺服器管理員主控台頂端的 [ **通知** ] 區域中查看安裝結果或其他訊息。 按一下 [ **通知** ] 旗標圖示，即可查看您在伺服器管理員中執行的安裝或其他工作的詳細資料。

## <a name="install-roles-role-services-and-features-by-using-windows-powershell-cmdlets"></a>使用 Windows PowerShell Cmdlet 來安裝角色、角色服務與功能
適用于 Windows PowerShell 的伺服器管理員部署 Cmdlet 與以 GUI 為基礎的 [新增角色及功能] Wizard 和 [移除角色及功能] Wizard 有很大的差異。 在 Windows PowerShell 中，依預設不會包含角色的管理工具和嵌入式管理單元，與 [新增角色及功能] 嚮導不同。 若要在角色安裝中包含管理工具，請新增 `IncludeManagementTools` 參數到 Cmdlet。 如果您要在執行 Windows Server 2012 或更新版本之 Server Core 安裝選項的伺服器上安裝角色和功能，您可以將角色的管理工具新增到安裝中，但是無法在執行 Windows Server 之 Server Core 安裝選項的伺服器上安裝以 GUI 為基礎的管理工具和嵌入式管理單元。 只有命令列和 Windows PowerShell 管理工具可以安裝在 Server Core 安裝選項上。

#### <a name="to-install-roles-and-features-by-using-the-install-windowsfeature-cmdlet"></a>使用 Install-WindowsFeature Cmdlet 安裝角色及功能

1. 執行下列其中一個動作，使用提高的使用者權限開啟 Windows PowerShell 工作階段。

   > [!NOTE]
   > 如果您要在遠端伺服器上安裝角色和功能，則不需要以提高的使用者權限來執行 Windows PowerShell。

   -   在 Windows 桌面上，用滑鼠右鍵按一下工作列上的 [Windows PowerShell]，然後按一下 [以系統管理員身分執行]。

   -   在 Windows [ **開始** ] 畫面上，以滑鼠右鍵按一下 Windows PowerShell 的磚，然後在應用程式行上，按一下 [以 **系統管理員身分執行**]。

2. 輸入 **Get-WindowsFeature**，然後按 **Enter**，以檢視本機伺服器上可用且已安裝的角色及功能清單。 如果本機電腦不是伺服器，或是您想要遠端伺服器的相關資訊，請執行 **<** <em>computer_name</em> **>** ，其中 *computer_name* 代表執行 Windows server 2016 的遠端電腦名稱稱。 Cmdlet 的結果包含您在步驟4中新增至 Cmdlet 的角色和功能的命令名稱。

   > [!NOTE]
   > 在 Windows PowerShell 3.0 和更新版本的 Windows PowerShell 中，在執行屬於模組的 Cmdlet 之前，不需要將伺服器管理員 Cmdlet 模組匯入 Windows PowerShell 會話中。 當您首次執行屬於模組的 Cmdlet 時，會自動匯入模組。 此外，Windows PowerShell Cmdlet 和 Cmdlet 所使用的功能名稱都不區分大小寫。

3. 輸入 **Get-help Install-** 的，然後按下 **enter** 鍵，以查看 Cmdlet 的語法和接受的參數 `Install-WindowsFeature` 。

4. 輸入下列命令，然後按 **enter**，其中 *feature_name* 代表您要安裝的角色或功能的命令名稱 (在步驟 2) 中取得，而 *computer_name* 代表您要安裝角色和功能的遠端電腦。 使用逗號來分隔多個 *feature_name* 值。 如果角色或功能安裝需要重新啟動目的地伺服器，則 `Restart` 參數會自動重新啟動目的地伺服器。

   ```
   Install-WindowsFeature -Name <feature_name> -computerName <computer_name> -Restart
   ```

   若要在離線 VHD 上安裝角色及功能，請新增 `computerName` 參數和 `VHD` 參數。 如果您未新增 `computerName` 參數，這個 Cmdlet 會假設本機電腦已掛接以存取 VHD。 `computerName` 參數包含要掛接 VHD 的伺服器名稱， `VHD` 參數則包含指定伺服器上的 VHD 檔案路徑。

   > [!NOTE]
   > `computerName`如果您是從執行 Windows 用戶端作業系統的電腦執行 Cmdlet，則必須新增參數。
   >
   > 若要在離線 VHD 上安裝角色及功能，目標 VHD 必須符合下列需求。
   >
   > -   Vhd 必須執行符合您正在執行之伺服器管理員版本的 Windows Server 版本。 請參閱 [安裝角色、角色服務和功能的開頭的附注，方法是使用 [新增角色及功能]](#install-roles-role-services-and-features-by-using-the-add-roles-and-features-wizard)。
   > -   VHD 不能有多個系統磁碟區或磁碟分割。
   > -   儲存在 VHD 檔案中的網路共用資料夾必須授與下列存取權限給您選取要掛接 VHD 之伺服器的電腦 (或本機系統) 帳戶。 只有使用者的帳戶存取權是不夠的。 該共用可以將 [讀取] 和 [寫入] 權限授與 **Everyone** 群組以允許存取 VHD，但基於安全性因素，不建議您這樣做。
   >
   >     -   [檔案共用] 對話方塊上的 [讀取/寫入] 存取權。
   >     -   [**安全性**] 索引標籤、[檔案 **或資料夾內容**] 對話方塊上的 [**完全控制**] 存取權。

   ```
   Install-WindowsFeature -Name <feature_name> -VHD <path> -computerName <computer_name> -Restart
   ```

   **範例：** 下列 Cmdlet 會在遠端伺服器 ContosoDC1 上安裝 active directory 網域服務角色和群組原則管理功能。 管理工具和嵌入式管理單元都已使用 `IncludeManagementTools` 參數新增，如果安裝需要重新啟動伺服器，則會自動啟動目的地伺服器。

   ```
   Install-WindowsFeature -Name AD-Domain-Services,GPMC -computerName ContosoDC1 -IncludeManagementTools -Restart
   ```

5. 當安裝完成時，請在伺服器管理員中開啟 [ **所有伺服器** ] 頁面，選取已安裝角色和功能的伺服器，然後在所選伺服器的頁面上，查看 [ **角色和功能** ] 磚，以確認安裝。 您也可以執行以 `Get-WindowsFeature` 選定伺服器為目標的 Cmdlet (的 <*computer_name*>) ，以查看伺服器上安裝的角色和功能清單。

## <a name="remove-roles-role-services-and-features-by-using-the-remove-roles-and-features-wizard"></a>使用 [移除角色及功能] Wizard 移除角色、角色服務和功能
您必須以系統管理員身分登入伺服器，才能卸載角色、角色服務和功能。 使用解除安裝目標伺服器上不具有系統管理員權限的帳戶登入本機電腦時，請以滑鼠右鍵按一下 [伺服器] 磚中的目標伺服器，然後按一下 [管理為] 以提供具系統管理員權限的帳戶。 您要掛接離線 VHD 的伺服器必須新增到 [伺服器管理員]，而且您必須具備該伺服器的系統管理員權限。

#### <a name="to-remove-roles-and-features-by-using-the-remove-roles-and-features-wizard"></a>使用移除角色及功能 Wizard 移除角色和功能

1.  如果已經開啟伺服器管理員，請移至下一個步驟。 如果尚未開啟伺服器管理員，請執行下列其中一項動作來將它開啟。

    -   在 Windows 桌面上，按一下 Windows 工作列中的 [伺服器管理員]  來啟動 [伺服器管理員]。

    -   在 Windows [開始] 畫面上，按一下 [伺服器管理員] 磚。

2.  在 [管理] 功能表上，按一下 [移除角色及功能]。

3.  在 [在您開始前] 頁面中，確認您已準備好從伺服器移除角色或功能。 按一下 [下一步] 。

4.  在 [ **選取目的地伺服器** ] 頁面上，從伺服器集區選取伺服器，或選取離線 VHD。 若要選取離線 VHD，請先選取要掛接 VHD 的伺服器，然後選取 VHD 檔案。

    > [!NOTE]
    > 儲存在 VHD 檔案中的網路共用資料夾必須授與下列存取權限給您選取要掛接 VHD 之伺服器的電腦 (或本機系統) 帳戶。 只有使用者的帳戶存取權是不夠的。 該共用可以將 [讀取] 和 [寫入] 權限授與 **Everyone** 群組以允許存取 VHD，但基於安全性因素，不建議您這樣做。
    >
    > -   [檔案共用] 對話方塊上的 [讀取/寫入] 存取權。
    > -   檔案或資料夾 [內容] 對話方塊的 [安全性] 索引標籤上的 [完全控制] 存取權。

    如需有關如何將伺服器新增到伺服器集區的詳細資訊，請參閱 [將伺服器新增至伺服器管理員](add-servers-to-server-manager.md)。 選取目的地伺服器之後，按 [下一步]。

    > [!NOTE]
    > 您可以使用 [移除角色及功能] Wizard，從執行相同 Windows Server 版本（支援您所使用的伺服器管理員版本）的伺服器移除角色和功能。 如果您在 Windows Server 2012 R2、Windows Server 2012 或 Windows 8 上執行伺服器管理員，則無法從執行 Windows Server 2016 的伺服器移除角色、角色服務或功能。 您無法使用 [移除角色及功能] Wizard，從執行 Windows Server 2008 或 Windows Server 2008 R2 的伺服器移除角色和功能。

5.  選取角色，選取角色的角色服務 (如果適用)，然後按 [下一步] 以選取功能。

    當您繼續時，[移除角色及功能] 嚮導會自動提示您移除無法執行的任何角色、角色服務或功能，而不需要您要移除的角色或功能。

    此外，您可以選擇移除目的地伺服器上角色的管理工具和嵌入式管理單元。 依預設，在 [移除角色及功能] 嚮導中，已選取要移除的管理工具。 如果您計畫使用所選取伺服器來管理其他遠端伺服器上的角色，可以保留管理工具和嵌入式管理單元。

6.  在 [確認移除選項] 頁面上，檢閱您的角色、功能和伺服器選取項目。 如果您已準備好移除角色或功能，請按一下 [ **移除**]。

7.  按一下 [ **移除**] 之後，[移除 **進度** ] 頁面會顯示移除進度、結果及訊息，例如警告、失敗或必要的移除後設定步驟，例如重新開機目的地伺服器。 在 Windows Server 2012 和更新版本的 Windows Server 中，您可以在移除仍在進行時關閉 [移除角色及功能] 嚮導，並在伺服器管理員主控台頂端的 [ **通知** ] 區域中，查看移除結果或其他訊息。 按一下 [ **通知** ] 旗標，以查看您在伺服器管理員中執行的移除或其他工作的詳細資料。

## <a name="remove-roles-role-services-and-features-by-using-windows-powershell-cmdlets"></a>使用 Windows PowerShell Cmdlet 來移除角色、角色服務與功能
適用于 Windows PowerShell 的伺服器管理員部署 Cmdlet 與以 GUI 為基礎的 [移除角色及功能] Wizard 有很大的差異。 在 Windows PowerShell 中，與 [移除角色及功能] Wizard 不同的是，預設不會移除角色的管理工具和嵌入式管理單元。 若要移除管理工作以做為角色移除的一部分，請將 `IncludeManagementTools` 參數新增到 Cmdlet。 如果您要從執行 Windows Server 2012 或更新版本之 Windows server 的 server Core 安裝選項的伺服器中卸載角色和功能，此參數會移除指定角色和功能的命令列和 Windows PowerShell 管理工具。

#### <a name="to-remove-roles-and-features-by-using-the-uninstall-windowsfeature-cmdlet"></a>使用 Uninstall-WindowsFeature Cmdlet 移除角色及功能

1. 執行下列其中一個動作，使用提高的使用者權限開啟 Windows PowerShell 工作階段。

   > [!NOTE]
   > 如果您要從遠端伺服器卸載角色與功能，則不需要以提高的使用者權限來執行 Windows PowerShell。

   -   在 Windows 桌面上，用滑鼠右鍵按一下工作列上的 [Windows PowerShell]，然後按一下 [以系統管理員身分執行]。

   -   在 Windows [ **開始** ] 畫面上，以滑鼠右鍵按一下 [Windows PowerShell] 磚，然後在應用程式行上，按一下 [以 **系統管理員身分執行**]。

2. 輸入 **Get-WindowsFeature**，然後按 **Enter**，以檢視本機伺服器上可用且已安裝的角色及功能清單。 如果本機電腦不是伺服器，或是您想要遠端伺服器的相關資訊，請執行 **<** <em>computer_name</em> **>** ，其中 *computer_name* 代表執行 Windows server 2016 的遠端電腦名稱稱。 Cmdlet 的結果包含您在步驟4中新增至 Cmdlet 的角色和功能的命令名稱。

   > [!NOTE]
   > 在 Windows PowerShell 3.0 和更新版本的 Windows PowerShell 中，在執行屬於模組的 Cmdlet 之前，不需要將伺服器管理員 Cmdlet 模組匯入 Windows PowerShell 會話中。 當您首次執行屬於模組的 Cmdlet 時，會自動匯入模組。 此外，Windows PowerShell Cmdlet 和 Cmdlet 所使用的功能名稱都不區分大小寫。

3. 輸入 **Get-help Uninstall**，然後按下 **enter** 鍵，以查看 Cmdlet 的語法和接受的參數 `Uninstall-WindowsFeature` 。

4. 輸入下列內容，然後按 **Enter**，其中 *feature_name* 代表您要移除的角色或功能的命令名稱 (可在步驟 2 中取得)，而 *computer_name* 代表您要移除角色及功能的遠端電腦。 使用逗號來分隔多個 *feature_name* 值。 如果角色或功能移除需要重新啟動目的地伺服器，則 `Restart` 參數會自動重新啟動目的地伺服器。

   ```
   Uninstall-WindowsFeature -Name <feature_name> -computerName <computer_name> -Restart
   ```

   若要從離線 VHD 上移除角色及功能，請新增 `computerName` 參數和 `VHD` 參數。 如果您未新增 `computerName` 參數，這個 Cmdlet 會假設本機電腦已掛接以存取 VHD。 `computerName` 參數包含要掛接 VHD 的伺服器名稱， `VHD` 參數則包含指定伺服器上的 VHD 檔案路徑。

   > [!NOTE]
   > `computerName`如果您是從執行 Windows 用戶端作業系統的電腦執行 Cmdlet，則必須新增參數。
   >
   > 儲存在 VHD 檔案中的網路共用資料夾必須授與下列存取權限給您選取要掛接 VHD 之伺服器的電腦 (或本機系統) 帳戶。 只有使用者的帳戶存取權是不夠的。 該共用可以將 [讀取] 和 [寫入] 權限授與 **Everyone** 群組以允許存取 VHD，但基於安全性因素，不建議您這樣做。
   >
   > -   [檔案共用] 對話方塊上的 [讀取/寫入] 存取權。
   > -   [**安全性**] 索引標籤、[檔案 **或資料夾內容**] 對話方塊上的 [**完全控制**] 存取權。

   ```
   Uninstall-WindowsFeature -Name <feature_name> -VHD <path> -computerName <computer_name> -Restart
   ```

   **範例：** 下列 Cmdlet 會從遠端伺服器 ContosoDC1 中移除 active directory 網域服務角色和群組原則管理功能。 也會將管理工具和嵌入式管理單元移除，如果移除需要重新啟動伺服器，則會自動啟動目的地伺服器。

   ```
   Uninstall-WindowsFeature -Name AD-Domain-Services,GPMC -computerName ContosoDC1 -IncludeManagementTools -Restart
   ```

5. 移除完成時，請在伺服器管理員中開啟 [ **所有伺服器** ] 頁面，選取移除角色和功能的伺服器，並在頁面上查看所選伺服器的 [ **角色和功能** ] 磚，以確認已移除角色和功能。 您也可以執行以 `Get-WindowsFeature` 選定伺服器為目標的 Cmdlet (的 <*computer_name*>) ，以查看伺服器上安裝的角色和功能清單。

## <a name="install-roles-and-features-on-multiple-servers-by-running-a-windows-powershell-script"></a>執行 Windows PowerShell 指令碼，在多個伺服器上安裝角色和功能
雖然您無法在單一 Wizard 會話中使用 [新增角色及功能] 嚮導來安裝多部目標伺服器上的角色、角色服務和功能，但是您可以使用 Windows PowerShell 腳本，在您使用伺服器管理員管理的多個目標伺服器上安裝角色、角色服務和功能。 您用來執行批次部署的腳本（呼叫此程式）會指向 XML 設定檔，您可以使用 [新增角色及功能] 嚮導輕鬆地建立 XML 設定檔，然後在 [新增角色及功能] Wizard 的 [**確認安裝選項**] 頁面上按 [匯出設定] 頁面，然後按一下 [**匯出設定**]。

> [!IMPORTANT]
> 腳本中指定的所有目標伺服器都必須執行符合您在本機電腦上執行之伺服器管理員版本的 Windows Server 版本。 例如，如果您是在 Windows 10 上執行伺服器管理員，就可以在執行 Windows Server 2016 的伺服器上安裝角色、角色服務和功能。 如果將 GUI 管理工具新增至安裝，安裝程式會自動將執行 Windows Server 之 Server Core 安裝選項的目標伺服器，轉換成完整安裝選項 (server 的完整 GUI，也稱為執行伺服器圖形化 Shell) 。
>
> 本節提供的腳本是如何使用 `Install-WindowsFeature` Cmdlet 和 Windows PowerShell 腳本來執行 batch 部署的範例。 在多個伺服器上執行批次部署還有其他可能的指令碼和方法。 若要搜尋或提供部署角色和功能的其他指令碼，請搜尋 [Script Center Repository (指令碼中心存放庫)](https://gallery.technet.microsoft.com/ScriptCenter)。

#### <a name="to-install-roles-and-features-on-multiple-servers"></a>在多個伺服器上安裝角色和功能

1.  如果您尚未這麼做，請建立 XML 設定檔，其中包含您想要在多部伺服器上安裝的角色、角色服務和功能。 您可以執行 [新增角色及功能] 嚮導、選取您想要的角色、角色服務和功能，然後按一下 [**確認安裝選項**] 頁面上的 [匯出設定]，然後按一下 [**匯出設定**] 頁面，以建立此設定檔。 將設定檔儲存到方便的位置。 如果您只是要建立設定檔才執行，就不需要按一下 [安裝] 或完成精靈。

2.  執行下列其中一個動作，使用提高的使用者權限開啟 Windows PowerShell 工作階段。

    -   在 Windows 桌面上，用滑鼠右鍵按一下工作列上的 [Windows PowerShell]，然後按一下 [以系統管理員身分執行]。

    -   在 Windows [ **開始** ] 畫面上，以滑鼠右鍵按一下 [Windows PowerShell] 磚，然後在應用程式行上，按一下 [以 **系統管理員身分執行**]。

3.  將下列腳本複製並貼到您的 Windows PowerShell 會話中。

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

    1.  建立變數以儲存目標電腦的名稱，以逗號分隔。 在下列範例中，變數 `$ServerNames` 儲存了目標伺服器名稱 *Contoso_01* 和 *Contoso_02*。 按 **Enter**。

        ```
        # Sample Invocation
        $ServerNames = 'Contoso_01','Contoso_02'
        Invoke-WindowsFeatureBatchDeployment -computerNames $ServerNames -ConfigurationFilepath C:\Users\sampleuser\Desktop\DeploymentConfigTemplate.xml
        ```

    2.  若要執行函式，輸入下列內容，然後按 **Enter**，其中 `$ServerNames` 是您在先前步驟中建立的變數範例，而 *C:\Users\Sampleuser\Desktop\DeploymentConfigTemplate.xml* 是您在步驟 1 中建立的設定檔路徑的範例。

        **Invoke-windowsfeaturebatchdeployment-computerNames $ServerNames-ConfigurationFilepath C:\Users\Sampleuser\Desktop\DeploymentConfigTemplate.xml**

5.  當安裝完成時，請在伺服器管理員中開啟 [ **所有伺服器** ] 頁面，選取已安裝角色和功能的伺服器，然後在所選伺服器的頁面上，查看 [ **角色和功能** ] 磚，以確認安裝。 您也可以執行以 `Get-WindowsFeature` 特定伺服器為目標的 Cmdlet (`Get-WindowsFeature -computerName`  < *computer_name*>) ，以查看伺服器上安裝的角色和功能清單。

## <a name="install-net-framework-35-and-other-features-on-demand"></a>安裝 .NET Framework 3.5 和其他指定功能
從 Windows Server 2012 和 Windows 8 開始，預設不會在本機電腦上使用 .NET Framework 3.5 (的功能檔案，包括 .NET Framework 2.0 和 .NET Framework 3.0) 。 這些檔案已移除。 在「功能隨選安裝」設定中已移除的功能檔案，以及 .NET Framework 3.5 的功能檔案，皆可透過 Windows Update 提供。 依預設，如果執行 Windows Server 2012 或更新版本的目的地伺服器上沒有可用的功能檔案，安裝程式會藉由連接到 Windows Update 來搜尋遺失的檔案。 您可以藉由設定群組原則設定，或在安裝期間指定替代來源路徑來覆寫預設行為，無論您是使用 [新增角色及功能]、[新增角色及功能]、[新增角色及功能]，或命令列來安裝

您可以執行下列其中一個動作來安裝 .NET Framework 3.5。

-   藉由[執行 Install-WindowsFeature Cmdlet 來安裝 .NET Framework 3.5](#to-install-net-framework-35-by-running-the-install-windowsfeature-cmdlet)，以新增 `Source` 參數並指定要從中取得 .NET Framework 3.5 功能檔案的來源。 如果您沒有新增 `Source` 參數，安裝程序會先判斷群組原則設定是否已指定功能檔案的路徑，如果找不到這個路徑，則使用 Windows Update 來搜尋遺失的功能檔案。

-   您可以使用 [新增角色及功能]，在 [新增角色及功能] Wizard 的 [**確認安裝選項**] 頁面上，使用 [[新增角色及功能] wizard](#to-install-net-framework-35-by-using-the-add-roles-and-features-wizard)指定替代來源檔案位置，以安裝 .NET Framework 3.5。

-   藉由[使用 DISM 來安裝 .NET Framework 3.5](#to-install-net-framework-35-by-using-dism)，以根據預設取得 Windows Update 的檔案，或是指定安裝媒體的來源路徑。

如果在本機電腦中找不到功能檔案，請針對 .NET Framework 3.5 或其他功能[設定群組原則中功能檔案的替代來源](#configure-alternate-sources-for-feature-files-in-group-policy)。

> [!IMPORTANT]
> 當您從遠端來源安裝功能檔案，來源路徑或檔案共用必須將 [讀取] 權限授與 **Everyone** 群組 (基於安全性因素不建議這麼做) 或目的地伺服器的電腦 (本機系統) 帳戶；授與使用者帳戶存取權是不夠的。
>
> 即使工作群組伺服器的電腦帳戶擁有外部共用的 [讀取] 權限，工作群組中的伺服器仍無法存取外部檔案共用。 用於工作群組伺服器的替代來源位置包括安裝媒體、Windows Update，以及儲存在本機工作群組伺服器上的 VHD 或 WIM 檔。
>
> 當您在執行中的實體伺服器上安裝角色、角色服務和功能時，您可以將 WIM 檔案指定為替代功能檔案來源。 WIM 檔案的來源路徑應該使用下列格式，加上 **WIM** 做為首碼，並用功能檔案所在的索引做為尾碼：**WIM:e:\sources\install.wim:4**。 不過，您無法直接使用 WIM 檔案作為將角色、角色服務和功能安裝至離線 VHD 的來源;您必須掛接離線 VHD 並指向其原始程式檔的掛接路徑，或者您必須指向包含 WIM 檔案內容複本的資料夾。

### <a name="to-install-net-framework-35-by-running-the-install-windowsfeature-cmdlet"></a>執行 Install-WindowsFeature Cmdlet 來安裝 .NET Framework 3.5

1.  執行下列其中一個動作，使用提高的使用者權限開啟 Windows PowerShell 工作階段。

    > [!NOTE]
    > 如果您是從遠端伺服器安裝角色和功能，則不需要以提高的使用者權限來執行 Windows PowerShell。

    -   在 Windows 桌面上，用滑鼠右鍵按一下工作列上的 [Windows PowerShell]，然後按一下 [以系統管理員身分執行]。

    -   在 Windows [ **開始** ] 畫面上，以滑鼠右鍵按一下 [Windows PowerShell] 磚，然後在應用程式行上，按一下 [以 **系統管理員身分執行**]。

    -   在執行 Windows Server 2012 R2 或 Windows Server 2012 之 Server Core 安裝選項的伺服器上，在命令提示字元中輸入 **PowerShell** ，然後按 **enter**。

2.  輸入下列命令，然後按 **enter**。 在下列範例中，來源檔案位於 D 磁碟機上安裝媒體中的並列存放 (簡稱 **SxS**)。

    ```
    Install-WindowsFeature NET-Framework-Core -Source D:\Sources\SxS
    ```

    如果您要命令將 Windows Update 當做遺失功能檔案的來源，或已使用群組原則來設定預設來源，那麼除非您要指定不同的來源，否則就不需要新增 `Source` 參數。

### <a name="to-install-net-framework-35-by-using-the-add-roles-and-features-wizard"></a>使用新增角色及功能 Wizard 安裝 .NET Framework 3。5

1. 在伺服器管理員的 [ **管理** ] 功能表上，按一下 [ **新增角色及功能**]。

2. 選取正在執行 Windows Server 2016 的目的地伺服器。

3. 在 [新增角色及功能] Wizard 的 [ **選取功能** ] 頁面上，選取 **.NET Framework 3.5**。

4. 如果群組原則設定允許本機電腦這麼做，安裝程序會試圖使用 Windows Update 來取得遺失的功能檔案。 按一下 [安裝]；您不需要繼續下一步驟。

   如果群組原則設定不允許這種情況，或您想要使用 .NET Framework 3.5 功能檔案的另一個來源，請在嚮導的 [ **確認安裝選項** ] 頁面上，按一下 [ **指定替代來源路徑**]。

5. 在安裝媒體中提供並列存放 (稱為 **SxS**) 或 WIM 檔案的路徑。 在下列範例中，安裝媒體位於 D 磁碟機上。

   **D：\Sources\SxS\\**

   若要指定 WIM 檔案，請新增 **WIM:** 首碼，然後將影像索引新增到 WIM 檔案中當做尾碼，如以下範例所示。

   **WIM： \\ \\**<em>server_name</em>**\share\install.wim： 3**

6. 按一下 [確定]，然後按一下 [安裝]。

### <a name="to-install-net-framework-35-by-using-dism"></a>使用 DISM 來安裝 .NET Framework 3.5

1.  執行下列其中一個動作，使用提高的使用者權限開啟 Windows PowerShell 工作階段。

    > [!NOTE]
    > 如果您是從遠端伺服器安裝角色和功能，則不需要以提高的使用者權限來執行 Windows PowerShell。

    -   在 Windows 桌面上，用滑鼠右鍵按一下工作列上的 [Windows PowerShell]，然後按一下 [以系統管理員身分執行]。

    -   在 Windows [ **開始** ] 畫面上，以滑鼠右鍵按一下 [Windows PowerShell] 磚，然後在應用程式行上，按一下 [以 **系統管理員身分執行**]。

    -   在執行 Server Core 安裝選項的伺服器上，在命令提示字元中輸入 **PowerShell** ，然後按 **enter** 鍵。

2.  執行下列其中一個 DISM 命令。

    -   如果電腦可以存取 Windows Update，或群組原則中已設定預設的來源檔案位置，請執行下列命令。

        ```
        DISM /online /Enable-Feature /Featurename:NetFx3 /All
        ```

    -   如果電腦具有安裝媒體的存取權，請執行類似下列的命令。 在下列範例中，作業系統安裝媒體位於 D 磁碟機。 `LimitAccess` 參數可防止命令試圖聯繫 Windows Update 或是執行 WSUS 的伺服器。

        ```
        DISM /online /Enable-Feature /Featurename:NetFx3 /All /LimitAccess /Source:d:\sources\sxs
        ```

    > [!NOTE]
    > DISM 命令會區分大小寫。

### <a name="configure-alternate-sources-for-feature-files-in-group-policy"></a>設定群組原則中功能檔案的替代來源
本節描述的群組原則設定會指定 .NET Framework 3.5 檔案的授權來源位置，以及其他被視為「功能隨選安裝」設定一部分而遭移除的功能檔案。 原則設定 [ **指定選用元件安裝的設定] 和 [元件修復** ] 位於群組原則管理主控台或本機群組原則編輯器的 [ **電腦設定 \ 系統管理元件 \ 系統** ] 資料夾中。

> [!NOTE]
> 您必須是 Administrators 群組成員，才能在本機電腦上變更群組原則設定。 如果您要管理的電腦是在網域層級控制群組原則設定，那麼您必須是 Domain Administrators 群組的成員，才能變更群組原則設定。

##### <a name="to-configure-a-default-alternate-source-path-in-group-policy"></a>設定群組原則中的預設替代來源路徑

1. 在 [本機群組原則編輯器] 或群組原則管理主控台中，開啟下列原則設定。

   **選用元件安裝和元件修復的電腦設定 \ 管理 \ （系統指定相關）設定**

2. 若尚未啟用原則設定，請選取 [已啟用] 啟用。

3. 在 [選項] 區域中的 [替代來源檔案路徑] 文字方塊中，指定共用資料夾或 WIM 檔案的完整路徑。 若要將 WIM 檔案指定為替代來源檔案位置，請將首碼 **WIM:** 新增至路徑，然後新增影像索引，在 WIM 檔案中當做尾碼。 下列是您可以指定的值範例。

   - 共用資料夾的路徑： **\\\\** <em>server_name</em>**\share \\** <em>folder_name</em>

   - wim 檔案的路徑，其中 **3** 代表功能檔案所在之映射的索引： **WIM： \\ \\** <em>server_name</em>**\share\install.wim： 3**

4. 如果您不希望由此原則設定控制的電腦在 Windows Update 中搜尋遺失的功能檔案，請選取 [ **永遠不要嘗試從 Windows Update 下載** 承載]。

5. 如果此原則設定控制的電腦通常是透過 WSUS 接收更新，但您偏好透過 Windows Update 而非 WSUS 來尋找遺失的功能檔案，請選取 [直接連絡 Windows Update 下載修復內容，而不是從 Windows Server Update Services (WSUS)]。

6. 您完成變更此原則設定之後，請按一下 [確定]，然後關閉群組原則編輯器。

## <a name="see-also"></a>另請參閱
[Windows Server 安裝選項](https://go.microsoft.com/fwlink/p/?LinkId=241573) 
[Microsoft .NET Framework 3.5 部署考慮](https://go.microsoft.com/fwlink/p/?LinkId=248869) 
[如何啟用或停用 Windows 功能](https://go.microsoft.com/fwlink/p/?LinkId=246552)



