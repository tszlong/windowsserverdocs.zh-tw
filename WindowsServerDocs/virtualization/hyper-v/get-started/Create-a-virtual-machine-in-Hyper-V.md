---
title: 在 HYPER-V 中建立虛擬機器
description: 說明如何建立虛擬機器使用 HYPER-V 管理員或 Windows PowerShell
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 59297022-a898-456c-b299-d79cd5860238
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 3b850d19663115b72d809af1ae92a444f5491496
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810432"
---
# <a name="create-a-virtual-machine-in-hyper-v"></a>在 HYPER-V 中建立虛擬機器

>適用於：Windows 10，Windows Server 2016、 Microsoft HYPER-V Server 2016、windows Server 2019，Microsoft HYPER-V Server 2019

了解如何使用 HYPER-V 管理員建立虛擬機器，以及 Windows PowerShell，以及哪些選項時，有您在 HYPER-V Manager 中建立虛擬機器。  

## <a name="create-a-virtual-machine-by-using-hyper-v-manager"></a>使用 HYPER-V 管理員建立虛擬機器  

1.  開啟**HYPER-V 管理員**。  

2.  從**動作**窗格中，按一下**新增**，然後按一下**虛擬機器**。  

3.  從**新的虛擬機器精靈**，按一下**下一步**。  

4.  每個頁面上，請您的虛擬機器的適當選擇。 如需詳細資訊，請參閱 <<c0> [ 新的虛擬機器選項和 HYPER-V 管理員中的預設值](#options-in-hyper-v-manager-new-virtual-machine-wizard)本主題稍後的。  

5.  確認您的選擇，在後**摘要**頁面上，按一下**完成**。  

6.  在 HYPER-V 管理員 中，以滑鼠右鍵按一下 虛擬機器，然後選取**連線**。  

7.  在 [虛擬機器連線] 視窗中，選取**動作** > **啟動**。  

## <a name="create-a-virtual-machine-by-using-windows-powershell"></a>使用 Windows PowerShell 建立虛擬機器  

1. 在 Windows 桌面上，按一下 \[開始\] 按鈕，然後輸入 **Windows PowerShell** 名稱的任何一部分。  

2. 以滑鼠右鍵按一下**Windows PowerShell** ，然後選取**系統管理員身分執行**。  

3. 取得您想要使用的虛擬機器的虛擬交換器名稱[Get-vmswitch](https://technet.microsoft.com/library/hh848499.aspx)。  例如，  

   ```  
   Get-VMSwitch  * | Format-Table Name  
   ```  

4. 使用[NEW-VM](https://technet.microsoft.com/library/hh848537.aspx) cmdlet 來建立虛擬機器。  請參閱以下範例。  

   > [!NOTE]  
   > 您可以將此虛擬機器移至執行 Windows Server 2012 R2 的 HYPER-V 主機，如果使用-版本參數搭配[NEW-VM](https://technet.microsoft.com/library/hh848537.aspx)將虛擬機器設定版本設定為 5。 Windows Server 2016 的預設虛擬機器設定版本不支援 Windows Server 2012 R2 或更早版本。 建立虛擬機器之後，您無法變更虛擬機器設定版本。 如需詳細資訊，請參閱 <<c0> [ 支援的虛擬機器設定版本](../deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md#supported-virtual-machine-configuration-versions)。  

   - **現有的虛擬硬碟**-建立虛擬機器使用現有虛擬硬碟，您可以使用下列命令，  
     - **-Name**是您提供您要建立虛擬機器的名稱。  
     - **-MemoryStartupBytes**是在啟動虛擬機器可使用的記憶體數量。  
     - **-BootDevice**是裝置，例如網路介面卡 （網路介面卡） 或虛擬硬碟 (VHD) 啟動時開機至虛擬機器。  
     - **-VHDPath**是您想要使用的虛擬機器磁碟的路徑。  
     - **-Path**是儲存虛擬機器設定檔的路徑。  
     - **世代**是虛擬機器世代。 使用第 1 代 VHD 和 vhdx 第 2 代。 請參閱[應該在 HYPER-V 中建立 1 或 2 代虛擬機器嗎？](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)  
     - **-切換**是您想要用來連線到其他虛擬機器或網路的虛擬機器的虛擬交換器名稱。 請參閱[建立 HYPER-V 虛擬機器的虛擬交換器](Create-a-virtual-switch-for-Hyper-V-virtual-machines.md)。  

       ```  
       New-VM -Name <Name> -MemoryStartupBytes <Memory> -BootDevice <BootDevice> -VHDPath <VHDPath> -Path <Path> -Generation <Generation> -Switch <SwitchName>  
       ```  

       例如:  

       ```  
       New-VM -Name Win10VM -MemoryStartupBytes 4GB -BootDevice VHD -VHDPath .\VMs\Win10.vhdx -Path .\VMData -Generation 2 -Switch ExternalSwitch  
       ```  

       這會建立名為 4 GB 的記憶體與 Win10VM 第 2 代虛擬機器。 它會從目前的目錄中的資料夾 VMs\Win10.vhdx 開機，並使用名為 ExternalSwitch 虛擬交換器。 虛擬機器設定檔會儲存在資料夾 VMData。  

   - **新的虛擬硬碟**-若要使用新的虛擬硬碟建立虛擬機器，取代 **-VHDPath**與上述範例中的參數 **-NewVHDPath** ，並新增 **-NewVHDSizeBytes**參數。 例如，  

     ```  
     New-VM -Name Win10VM -MemoryStartupBytes 4GB -BootDevice VHD -NewVHDPath .\VMs\Win10.vhdx -Path .\VMData -NewVHDSizeBytes 20GB -Generation 2 -Switch ExternalSwitch  
     ```  

   - **新虛擬硬碟開機至作業系統映像**-若要建立虛擬機器使用新的虛擬磁碟開機至作業系統映像，請參閱中的 PowerShell 範例[上建立適用於 HYPER-V 的虛擬機器的逐步解說Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_create_vm)。  

5. 使用啟動虛擬機器[START-VM](https://technet.microsoft.com/library/hh848589.aspx) cmdlet。 執行下列 cmdlet，其中名稱是您所建立的虛擬機器的名稱。  

   ```  
   Start-VM -Name <Name>  
   ```  

   例如:  

   ```  
   Start-VM -Name Win10VM  
   ```  

6. 使用虛擬機器連線 (VMConnect)，以連接至虛擬機器。  

   ```  
   VMConnect.exe  
   ```  

## <a name="options-in-hyper-v-manager-new-virtual-machine-wizard"></a>在 HYPER-V 管理員新增虛擬機器精靈 的選項  
下表列出您可以選擇當您建立虛擬機器在 HYPER-V 管理員和預設值為每個選項。  

|Page|適用於 Windows Server 2016 和 Windows 10 的預設值|其他選項|  
|--------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|  
|**指定名稱和位置**|名稱：新的虛擬機器。<br /><br />位置:**C:\ProgramData\Microsoft\Windows\Hyper-V\\** .|您也可以輸入自己的名稱，然後選擇為虛擬機器的另一個位置。<br /><br />這是將儲存虛擬機器設定檔。|  
|**指定世代**|第 1 代|您也可以選擇建立第 2 代虛擬機器。 如需詳細資訊，請參閱[應該在 HYPER-V 中建立 1 或 2 代虛擬機器嗎？](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)|  
|**指派記憶體**|啟動記憶體：1024 MB<br /><br />動態記憶體：**未選取**|您可以設定從 32 MB 的啟動記憶體 5902 mb。<br /><br />您也可以選擇使用動態記憶體。 如需詳細資訊，請參閱 < [HYPER-V 動態記憶體概觀](https://technet.microsoft.com/library/hh831766.aspx)。|  
|**設定網路功能**|未連接|您可以選取要使用現有的虛擬交換器的清單中的虛擬機器的網路連線。 請參閱[建立 HYPER-V 虛擬機器的虛擬交換器](Create-a-virtual-switch-for-Hyper-V-virtual-machines.md)。|  
|**連接虛擬硬碟**|建立虛擬硬碟<br /><br />名稱： <*vmname*>.vhdx<br /><br />**位置**：**C:\Users\Public\Documents\Hyper-V\Virtual Hard Disks\\**<br /><br />**大小**:127GB|您也可以選擇使用現有的虛擬硬碟或等候，並稍後連接虛擬硬碟。|  
|**安裝選項**|稍後安裝作業系統|這些選項會變更虛擬機器的開機順序，以便您可以從.iso 檔案、 可開機的磁碟或網路安裝服務，例如 Windows 部署服務 (WDS) 安裝。|  
|**摘要**|會顯示您選擇的選項，讓您可以確認他們正確。<br /><br />-名稱<br />世代<br />記憶體內部<br />-網路<br />-硬碟<br />作業系統|**變更複寫用快取資料夾路徑**您可以從頁面複製摘要，並將它貼到電子郵件或其他可協助您追蹤您的虛擬機器的位置。|  

## <a name="see-also"></a>另請參閱  

- [New-VM](https://technet.microsoft.com/library/hh848537.aspx)  

- [支援的虛擬機器設定版本](../deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md#supported-virtual-machine-configuration-versions)  

-   [應該在 HYPER-V 中建立 1 或 2 代虛擬機器嗎？](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)  

-   [為 Hyper-V 虛擬機器建立虛擬交換器](Create-a-virtual-switch-for-Hyper-V-virtual-machines.md)  
