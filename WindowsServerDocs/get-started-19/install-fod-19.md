---
title: Server Core 應用程式相容性功能隨選安裝 (FOD)
description: 如何依需求安裝 Windows Server 功能
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a4d55cca754
author: jasongerend
ms.author: jgerend
manager: jasgroce
ms.localizationpriority: medium
ms.date: 05/29/2019
ms.openlocfilehash: e76b7862549814d5453717c40cec45e341141d7a
ms.sourcegitcommit: 8eea7aadbe94f5d4635c4ffedc6a831558733cc0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66308597"
---
# <a name="server-core-app-compatibility-feature-on-demand-fod"></a>Server Core 應用程式相容性功能隨選安裝 (FOD)

> 適用於：Windows Server 2019，Windows Server 半年通道

**Server Core 應用程式相容性功能隨選**是選用功能的套件，可以隨時新增至 Windows Server 2019 Server Core 安裝或 Windows Server 半年通道。

如需有關在 on Demand (FOD) 功能的詳細資訊，請參閱[功能隨選](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities)。

## <a name="why-install-the-app-compatibility-fod"></a>為什麼安裝應用程式相容性 FOD？

包含的二進位檔和 Windows Server 含桌面體驗的套件子集，而不會增加應用程式相容性，適用於 Server Core 的隨選功能大幅改善的資訊，請參閱 Windows Server Core 安裝選項的應用程式相容性Windows Server 桌面體驗的圖形化環境。 此選擇性套件可用在個別的 ISO，或是從 Windows Update，但只會新增至 Windows Server Core 安裝和映像。

應用程式相容性 FOD 提供的兩個主要的值如下：

- 增加 Server Core 的相容的性，已在市場中或已被組織所開發並部署伺服器應用程式。
- 使用提供的作業系統元件和增加的應用程式相容性的嚴重的疑難排解和偵錯案例中使用的軟體工具的協助。

因為的 Server Core 應用程式相容性 FOD 一部分包含可用的作業系統元件：

-   Microsoft Management Console (mmc.exe)

-   事件檢視器 (Eventvwr.msc)

-   效能監視器 (PerfMon.exe)

-   資源監視器 (Resmon.exe)

-   裝置管理員 (Devmgmt.msc)

-   File Explorer (Explorer.exe)

-   Windows PowerShell (Powershell_ISE.exe)

-   磁碟管理 (Diskmgmt.msc)

-   容錯移轉叢集管理員 (CluAdmin.msc)

    -   這必須先新增 「 容錯移轉叢集 Windows 伺服器 」 功能。

        -   從提升權限的 PowerShell 工作階段： 

            ```PowerShell
            Install-WindowsFeature -NameFailover-Clustering -IncludeManagementTools
            ```

        -   若要執行容錯移轉叢集管理員，請輸入**cluadmin**在命令提示字元。

執行 Windows Server 的伺服器，1903年和更新版本也支援下列元件：

- HYPER-V 管理員 (virtmgmt.msc)
- 工作排程器 (taskschd.msc)

## <a name="installing-the-app-compatibility-fod"></a>安裝應用程式相容性 FOD

應用程式相容性 FOD 只能安裝在 Server Core 上。 請勿嘗試將 Server Core 應用程式相容性 FOD 新增至 Windows Server 安裝的 Windows 伺服器含桌面體驗。 Windows Server 2019 Server Core 安裝或 Windows Server 半年通道安裝可用的相同 FOD 選擇性套件 ISO。

1. 如果伺服器可以連線到 Windows Update 中，您只需要就是從提高權限的 PowerShell 工作階段中執行下列命令，然後在命令完成執行之後重新啟動 Windows Server:

    ```PowerShell
    Add-WindowsCapability -Online -Name ServerCore.AppCompatibility~~~~0.0.1.0
    ```

2. 如果伺服器無法連線到 Windows Update，改為下載伺服器 FOD 選擇性套件 ISO，並將 ISO 複製到您的區域網路上的共用資料夾：

   - 如果您有大量授權，您可以從相同的入口網站取得 OS ISO 映像檔的位置下載 Server FOD ISO 映像檔：[大量授權服務中心](https://www.microsoft.com/Licensing/servicecenter/default.aspx)。
   - Server FOD ISO 映像檔上也會提供[Microsoft 評估中心](https://www.microsoft.com/evalcenter/evaluate-windows-server)或在[Visual Studio 入口網站](https://visualstudio.microsoft.com)訂閱者。

3. 使用 Server Core 電腦，其會連線到您的區域網路和您想要新增至 FOD 上系統管理員帳戶登入。

4. 使用  **net 使用**，或透過其他方法，來連接到 FOD ISO 的位置。

5. 將 FOD ISO 複製到您選擇的本機資料夾。

6. 掛接 FOD ISO 提升權限的 PowerShell 工作階段中使用下列命令：

    ```PowerShell
    Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso
    ```

7. 執行下列命令：

    ```PowerShell
    Add-WindowsCapability -Online -Name ServerCore.AppCompatibility~~~~0.0.1.0 -Source <Mounted_Server_FOD_Drive> -LimitAccess
     ```

8. 進度列完成後，重新啟動作業系統。

 如需 DISM 命令的詳細資訊，請參閱[在 Windows PowerShell 中使用 DISM](https://docs.microsoft.com/windows-hardware/manufacture/desktop/use-dism-in-windows-powershell-s14)

## <a name="to-optionally-add-internet-explorer-11-to-server-core-after-adding-the-server-core-app-compatibility-fod"></a>（之後加入 Server Core 應用程式相容性 FOD） （選擇性） 新增到 Server Core 的 Internet Explorer 11

 >[!NOTE]  
   > Server Core 應用程式相容性 FOD 需要 Internet Explorer 11，新增，但是 Internet Explorer 11 時不需要加入 Server Core 應用程式相容性 FOD。

1. 管理員身分登入已加入應用程式相容性 FOD 的 Server Core 電腦及伺服器 FOD 選用封裝，使用 ISO 在本機複製。

2. 啟動 PowerShell，輸入**powershell.exe**在命令提示字元。

3. 使用下列命令，以掛接 FOD ISO:

    ```PowerShell
    Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso
    ```

4. 執行下列命令，並使用`$package_path`輸入 Internet Explorer 封包檔的路徑的變數：

    ```PowerShell
    $package_path = "D:\Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~.cab"

    Add-WindowsPackage -Online -PackagePath $package_path
    ```

5. 進度列完成後，重新啟動作業系統。

## <a name="release-notes-and-suggestions-for-the-server-core-app-compatibility-fod-and-internet-explorer-11-optional-package"></a>版本資訊與建議的 Server Core 應用程式相容性 FOD 和 Internet Explorer 11 的選擇性套件

> [!IMPORTANT]
> FODs 安裝在 Windows Server 上，版本 1809年不會留在原處之後進行就地升級至 Windows Server 版 1903，因此您必須將其安裝在升級之後再次。 或者，您可以將 FODs 加入新的 Windows Server 安裝來源，在升級之前。 這可確保任何 FODs 新版出現在升級完成之後。 如需詳細資訊，請參閱 <<c0> [ 離線 WIM 的 Server Core 映像新增功能和選擇性套件](install-fod-19.md#add-capabilities)。

- **重要事項：** 閱讀 Windows Server 2019 版本資訊的任何問題、 考量或再繼續進行安裝和使用 Server Core 應用程式相容性 FOD 和 Internet Explorer 11 的選擇性套件的指引。

- 它可能會遇到新增應用程式相容性 FOD 之後使用 Windows Update 以安裝累計更新時，Server Core 的主控台體驗的閃爍。  2018 年 12 月，以解決此問題的更新。  如需詳細的資訊和解決步驟，請參閱[4481610 眭妎踱恅：在 Windows Server 2019 Server Core 安裝 Server Core 應用程式相容性 FOD 之後，畫面閃爍](https://support.microsoft.com/help/4481610/screen-flickers-after-fod-installation-windows2019-server-core)。

- 安裝後的應用程式相容性 FOD 和重新啟動伺服器，命令主控台視窗框架色彩會變更為不同為藍色陰影。

- 如果您選擇同時安裝 Internet Explorer 11 選擇性的套件，請注意，不支援以開啟 本機儲存的.htm 檔案按兩下。 不過，您可以**上按一下滑鼠右鍵**，然後選擇**開啟 IE**，或者您可以直接從 Internet Explorer 中開啟它**檔案** -> **開啟**.

- 若要進一步加強應用程式相容性 FOD 與 Server Core 的應用程式相容性，IIS 管理主控台已新增到 Server Core 為選用的元件。  不過，它是先將新增使用 IIS 管理主控台應用程式相容性 FOD 絕對必要。 IIS 管理主控台依賴 Microsoft Management Console (mmc.exe)，這只是 Server Core 上加上應用程式相容性 FOD 可用。  使用 Powershell [ **Install-windowsfeature** ](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature?view=win10-ps)加入 IIS 管理主控台。

- 因為有時候必須使用無訊息安裝選項和指示，指導方針，在伺服器上的安裝應用程式核心 （不論這些選擇性的套件） 它一般點。 
    
 - 做為範例，SQL Server 2016 和 SQL Server 2017 的 SQL Server Management Studio 可以在 Server Core 上安裝，並有應用程式相容性 FOD 時可完整運作。  查看，請[從命令提示字元安裝 SQL Server](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-from-the-command-prompt?view=sql-server-2017)。
 - 如果不需要 SQL Server Management Studio，則不必安裝 Server Core 應用程式相容性 FOD。  查看，請[Server Core 上安裝 SQL Server](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-on-server-core?view=sql-server-2017)。

## <a name="a-idadd-capabilities-adding-capabilities-and-optional-packages-to-an-offline-wim-server-core-image"></a><a id="add-capabilities"> 將功能和選擇性的套件新增至離線 WIM 的 Server Core 映像

1. Windows Server 和 Server FOD ISO 映像檔案下載至 Windows 電腦上的本機資料夾。

   - 如果您有大量授權，您就可以下載的 Windows Server 和 Server FOD ISO 映像檔[Volume Licensing Service Center](https://www.microsoft.com/Licensing/servicecenter/default.aspx)。
   - Server FOD ISO 映像檔也會提供有關版本上的長期維護通道[Microsoft 評估中心](https://www.microsoft.com/evalcenter/evaluate-windows-server)或在[Visual Studio 入口網站](https://visualstudio.microsoft.com)訂閱者。

2. 開啟身為系統管理員的 PowerShell 工作階段，然後使用下列命令來掛接為磁碟機的映像檔：

   ```PowerShell
   Mount-DiskImage -ImagePath Path_To_ServerFOD_ISO
   Mount-DiskImage -ImagePath Path_To_Windows_Server_ISO
   ```

3. 複製到本機資料夾的 Windows Server ISO 檔案的內容 (例如*C:\SetupFiles\WindowsServer*)。

4. 取得您想要使用下列命令來修改 Install.wim 檔案中的映像名稱。<br>
使用`$install_wim_path`變數，以輸入 Install.wim 檔案，位於 \Sources 資料夾的 ISO 檔案的路徑。

   ```PowerShell
   $install_wim_path = "C:\SetupFiles\WindowsServer\sources\install.wim"

   Get-WindowsImage -ImagePath $install_wim_path
   ```

5. 掛接新的資料夾中的 Install.wim 檔案，使用下列命令範例變數的值替換為 您自己的圖片，並重複使用`$install_wim_path`變數前一個命令。<br>

   - `$image_name` -輸入您想要掛接映像的名稱。
   - `$mount_folder variable` -指定要存取的 Install.wim 檔案的內容時使用的資料夾。

   ```PowerShell
   $image_name = "Windows Server Datacenter"
   $mount_folder = "c:\test\offline"

   Mount-WindowsImage -ImagePath $install_wim_path -Name $image_name -path $mount_folder
   ```

6. 使用下列命令，並以您自己的取代範例變數值至掛接的 Install.wim 映像新增功能和您想要的套件。<br>

   - `$capability_name` -指定安裝 （在此情況下，AppCompatibility 功能） 功能的名稱。
   - `$package_path` -指定安裝 （在此情況下，Internet Explorer） 封裝的路徑。
   - `$fod_drive` -指定掛接 Server FOD 映像的磁碟機代號。

   ```PowerShell
   $capability_name = "ServerCore.AppCompatibility~~~~0.0.1.0"
   $package_path = "D:\Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~.cab"
   $fod_drive = "d:\"

   Add-WindowsCapability -Path $mount_folder -Name $capability_name -Source $fod_drive -LimitAccess
   Add-WindowsPackage -Path $mount_folder -PackagePath $package_path
   ```

7. 卸載並認可對 Install.wim 檔案的變更，使用下列命令，它會使用`$mount_folder`變數從先前的命令：

   ```PowerShell
   Dismount-WindowsImage -Path $mount_folder -Save
   ```

您現在可以從您建立 Windows Server 安裝檔案的資料夾中執行 setup.exe 來升級您的伺服器 (在此範例中：*C:\SetupFiles\WindowsServer*)。 這個資料夾現在包含額外的功能與選擇性的套件包含 Windows Server 安裝檔案。