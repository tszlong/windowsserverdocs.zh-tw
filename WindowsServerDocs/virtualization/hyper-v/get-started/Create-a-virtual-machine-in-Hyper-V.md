---
title: 在 Hyper-V 中建立虛擬機器
description: 提供使用 Hyper-v 管理員或 Windows PowerShell 建立虛擬機器的指示
manager: dongill
ms.topic: get-started-article
ms.assetid: 59297022-a898-456c-b299-d79cd5860238
author: kbdazure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 22dc2e83f1c7370bfd6f97d821a83041a9c528fe
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87996608"
---
# <a name="create-a-virtual-machine-in-hyper-v"></a>在 Hyper-V 中建立虛擬機器

>適用于： Windows 10、Windows Server 2016、Microsoft Hyper-v Server 2016、Windows Server 2019、Microsoft Hyper-v Server 2019

瞭解如何使用 Hyper-v 管理員和 Windows PowerShell 建立虛擬機器，以及當您在 Hyper-v 管理員中建立虛擬機器時有哪些選項。

## <a name="create-a-virtual-machine-by-using-hyper-v-manager"></a>使用 Hyper-v 管理員建立虛擬機器

1.  開啟 [ **Hyper-v 管理員**]。

2.  在 [**動作**] 窗格中，按一下 [**新增**]，然後按一下 [**虛擬機器**]。

3.  從 [**新增虛擬機器] 嚮導**，按 **[下一步]**。

4.  針對每個頁面上的虛擬機器進行適當的選擇。 如需詳細資訊，請參閱本主題稍後的 [ [Hyper-v 管理員中的新虛擬機器選項和預設值](#options-in-hyper-v-manager-new-virtual-machine-wizard)]。

5.  確認您在 [**摘要**] 頁面中的選擇之後，按一下 **[完成]**。

6.  在 [Hyper-v 管理員] 中，以滑鼠右鍵按一下虛擬機器，然後選取 **[連線]**。

7.  在 [虛擬機器連線] 視窗中，選取 [**動作**] [  >  **啟動**]。

## <a name="create-a-virtual-machine-by-using-windows-powershell"></a>使用 Windows PowerShell 建立虛擬機器

1. 在 Windows 桌面上，按一下 [開始] 按鈕，然後輸入 **Windows PowerShell** 名稱的任何一部分。

2. 以滑鼠右鍵按一下 [Windows PowerShell]，然後選取 [以系統管理員身分執行]。

3. 取得您想[要讓虛擬](/powershell/module/hyper-v/get-vmswitch?view=win10-ps)機使用的虛擬交換器名稱。  例如

   ```
   Get-VMSwitch  * | Format-Table Name
   ```

4. 使用[新的-VM](/powershell/module/hyper-v/new-vm?view=win10-ps) Cmdlet 來建立虛擬機器。  請參閱以下範例。

   > [!NOTE]
   > 如果您可以將此虛擬機器移至執行 Windows Server 2012 R2 的 Hyper-v 主機，請使用-Version 參數搭配[新的-VM](/powershell/module/hyper-v/new-vm?view=win10-ps) ，將虛擬機器設定版本設為5。 Windows Server 2012 R2 或更舊版本不支援 Windows Server 2016 的預設虛擬機器設定版本。 建立虛擬機器之後，您就無法變更虛擬機器設定版本。 如需詳細資訊，請參閱[支援的虛擬機器設定版本](../deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md#supported-virtual-machine-configuration-versions)。

   - **現有虛擬硬碟**-若要建立具有現有虛擬硬碟的虛擬機器，您可以使用下列命令，其中
     - **-Name** 是您為要建立的虛擬機器提供的名稱。
     - **-MemoryStartupBytes** 是虛擬機器在啟動時可用的記憶體數量。
     - **-BootDevice**是虛擬機器啟動時啟動的裝置（例如網路介面卡） (NetworkAdapter) 或虛擬硬碟 (VHD) 。
     - **-VHDPath** 是您要使用的虛擬機器磁碟所在的路徑。
     - **-Path** 是用來儲存虛擬機器組態檔的路徑。
     - **-Generation** 是虛擬機器的世代。 VHD 應使用第 1 代，VHDX 則使用第 2 代。 請參閱[我應該在 hyper-v 中建立第1代或第2代虛擬機器嗎？。](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)
     - **-Switch** 是您要讓虛擬機器用來連線至其他虛擬機器或網路的虛擬交換器名稱。 請參閱[為 hyper-v 虛擬機器建立虛擬交換器](Create-a-virtual-switch-for-Hyper-V-virtual-machines.md)。

       ```
       New-VM -Name <Name> -MemoryStartupBytes <Memory> -BootDevice <BootDevice> -VHDPath <VHDPath> -Path <Path> -Generation <Generation> -Switch <SwitchName>
       ```

       例如：

       ```
       New-VM -Name Win10VM -MemoryStartupBytes 4GB -BootDevice VHD -VHDPath .\VMs\Win10.vhdx -Path .\VMData -Generation 2 -Switch ExternalSwitch
       ```

       這會建立名為 Win10VM 的第2代虛擬機器，其記憶體為4GB。 該機器會從現行目錄中的 VMs\Win10.vhdx 資料夾開機，並使用名為 ExternalSwitch 的虛擬交換器。 虛擬機器組態檔會儲存在 VMData 資料夾中。

   - **新增虛擬硬碟**-若要使用新的虛擬硬碟來建立虛擬機器，請使用 **-NewVHDPath**取代上述範例中的 **-VHDPath**參數，並新增 **-NewVHDSizeBytes**參數。 例如

     ```
     New-VM -Name Win10VM -MemoryStartupBytes 4GB -BootDevice VHD -NewVHDPath .\VMs\Win10.vhdx -Path .\VMData -NewVHDSizeBytes 20GB -Generation 2 -Switch ExternalSwitch
     ```

   - **開機進入作業系統映射的新虛擬硬碟**-若要建立虛擬機器，並啟動新的虛擬磁片以開機進入作業系統映射，請參閱在[Windows 10 上建立 hyper-v 的虛擬機器逐步](/virtualization/hyper-v-on-windows/quick-start/create-virtual-machine)解說中的 PowerShell 範例。

5. 使用[啟動 VM](/powershell/module/hyper-v/start-vm?view=win10-ps) Cmdlet 來啟動虛擬機器。 執行下列 Cmdlet，其中 Name 是您所建立之虛擬機器的名稱。

   ```
   Start-VM -Name <Name>
   ```

   例如：

   ```
   Start-VM -Name Win10VM
   ```

6. 使用虛擬機器連線 (VMConnect) 連接到虛擬機器。

   ```
   VMConnect.exe
   ```

## <a name="options-in-hyper-v-manager-new-virtual-machine-wizard"></a>Hyper-v 管理員中的選項新增虛擬機器嚮導
下表列出當您在 [Hyper-v 管理員] 中建立虛擬機器時，可以選擇的選項，以及每個虛擬機器的預設值。

|頁面|Windows Server 2016 和 Windows 10 的預設值|其他選項|
|--------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|
|**指定名稱和位置**|名稱：新的虛擬機器。<p>位置： **C:\ProgramData\Microsoft\Windows\Hyper-V \\ **。|您也可以輸入自己的名稱，並為虛擬機器選擇另一個位置。<p>這是將儲存虛擬機器組態檔案的位置。|
|**指定世代**|第 1 代|您也可以選擇建立第2代虛擬機器。 如需詳細資訊，請參閱[我應該在 hyper-v 中建立第1代或第2代虛擬機器嗎？。](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)|
|**指派記憶體**|啟動記憶體： 1024 MB<p>動態記憶體：**未選取**|您可以將啟動記憶體從32MB 設定為5902MB。<p>您也可以選擇使用動態記憶體。 如需詳細資訊，請參閱[hyper-v 動態記憶體總覽](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831766(v=ws.11))。|
|**設定網路功能**|Not connected|您可以從現有的虛擬交換器清單中選取要用於虛擬機器的網路連線。 請參閱[為 hyper-v 虛擬機器建立虛擬交換器](Create-a-virtual-switch-for-Hyper-V-virtual-machines.md)。|
|**連接虛擬硬碟**|建立虛擬硬碟<p>名稱： <*vmname*> .vhdx<p>**位置**： **C:\Users\Public\Documents\Hyper-V\Virtual \\ 硬碟**<p>**大小**：127GB|您也可以選擇使用現有的虛擬硬碟，或稍後等候並連接虛擬硬碟。|
|**安裝選項**|稍後安裝作業系統|這些選項會變更虛擬機器的開機順序，讓您可以從 .iso 檔案、可開機的磁片或網路安裝服務（如 Windows 部署服務 (WDS) ）進行安裝。|
|**總結**|顯示您所選擇的選項，讓您可以確認它們是否正確。<p>-   Name<br />-產生<br />-   記憶體<br />-網路<br />-硬碟<br />-作業系統|**秘訣：** 您可以從頁面複製摘要，並將其貼到電子郵件或其他地方，以協助您追蹤虛擬機器。|

## <a name="additional-references"></a>其他參考資料

- [New-VM](/powershell/module/hyper-v/new-vm?view=win10-ps)

- [支援的虛擬機器設定版本](../deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md#supported-virtual-machine-configuration-versions)

-   [我應在 Hyper-V 建立第 1 或 2 代的虛擬機器嗎？](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)

-   [為 Hyper-V 虛擬機器建立虛擬交換器](Create-a-virtual-switch-for-Hyper-V-virtual-machines.md)