---
title: Server Core 應用程式相容性功能隨選安裝 (FOD)
description: 如何安裝 Windows Server 的功能隨選安裝
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a4d55cca754
author: jasongerend
ms.author: jgerend
manager: jasgroce
ms.localizationpriority: medium
ms.date: 06/07/2019
ms.openlocfilehash: 441cd9593371cb7e5018a61c3d7a6af991ce8fed
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280172"
---
# <a name="server-core-app-compatibility-feature-on-demand-fod"></a>Server Core 應用程式相容性功能隨選安裝 (FOD)

> 適用於：Windows Server 2019、Windows Server 半年通道

**Server Core 應用程式相容性功能隨選安裝**是選用的功能套件，可以隨時新增至 Windows Server 2019 Server Core 安裝或 Windows Server 半年通道。

如需有關功能隨選安裝 (FOD) 的詳細資訊，請參閱[功能隨選安裝](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities)。

## <a name="why-install-the-app-compatibility-fod"></a>為什麼要安裝應用程式相容性 FOD？

「應用程式相容性」屬於 Server Core 的功能隨選安裝，無須加入 Windows Server 桌面體驗圖形環境，即可納入包含桌面體驗的 Windows Server 二進位檔和套件子集，藉以大幅改進 Windows Server Core 安裝選項的應用程式相容性。 此選用套件可從不同 ISO 或從 Windows Update 取得，但僅能新增到 Windows Server Core 安裝和映像。

應用程式相容性 FOD 提供兩個主要價值：

- 針對已在市場中或已由組織開發並部署的伺服器應用程式，提高 Server Core 的相容性。
- 協助提供作業系統元件，和增加軟體工具用在嚴重疑難排解和偵錯案例時的應用程式相容性。

作為 Server Core 應用程式相容性 FOD 一部分提供的作業系統元件包括：

-   Microsoft Management Console (mmc.exe)

-   事件檢視器 (Eventvwr.msc)

-   效能監視器 (PerfMon.exe)

-   資源監視器 (Resmon.exe)

-   裝置管理員 (Devmgmt.msc)

-   檔案總管 (Explorer.exe)

-   Windows PowerShell (Powershell_ISE.exe)

-   磁碟管理 (Diskmgmt.msc)

-   容錯移轉叢集管理員 (CluAdmin.msc)

    -   必須先新增「Windows Server 容錯移轉叢集」功能。

        -   從提升權限的 PowerShell 工作階段： 

            ```PowerShell
            Install-WindowsFeature -NameFailover-Clustering -IncludeManagementTools
            ```

        -   若要執行容錯移轉叢集管理員，請在命令提示字元中輸入 **cluadmin**。

執行 Windows Server 版本 1903 和更新版本的伺服器支援下列元件 (使用相同版本的應用程式相容性 FOD 時)：

- Hyper-V 管理員 (virtmgmt.msc)
- 工作排程器 (taskschd.msc)

## <a name="installing-the-app-compatibility-fod"></a>安裝應用程式相容性 FOD

應用程式相容性 FOD 只能安裝在 Server Core 上。 請勿嘗試將 Server Core 應用程式相容性 FOD 新增至 Windows Server (含桌面體驗) 的 Windows Server 安裝。 相同的 FOD 選用套件 ISO 可用在 Windows Server 2019 核心安裝或 Windows Server 半年通道安裝。

1. 如果伺服器可以連線到 Windows Update，您所需要做的就是從提高權限的 PowerShell 工作階段中執行下列命令，然後在命令完成執行後，重新啟動 Windows Server：

    ```PowerShell
    Add-WindowsCapability -Online -Name ServerCore.AppCompatibility~~~~0.0.1.0
    ```

2. 如果伺服器無法連線到 Windows Update，請改為下載伺服器 FOD 選用套件 ISO，並將 ISO 複製到您區域網路上的共用資料夾：

   - 如果您有大量授權，您可以從取得作業系統 ISO 映像檔的相同入口網站下載伺服器 FOD ISO 映像檔：[大量授權服務中心](https://www.microsoft.com/Licensing/servicecenter/default.aspx)。
   - [Microsoft 評估中心](https://www.microsoft.com/evalcenter/evaluate-windows-server)或 [Visual Studio 入口網站](https://visualstudio.microsoft.com)也會提供伺服器 FOD ISO 映像檔給訂閱者。

3. 在您要連線到區域網路和要新增 FOD 的 Server Core 電腦上，使用系統管理員帳戶登入。

4. 使用 **net use** 或其他方法來連線到 FOD ISO 的位置。

5. 將 FOD ISO 複製到您選擇的本機資料夾。

6. 在提高權限的 PowerShell 工作階段中，使用下列命令來掛接 FOD ISO：

    ```PowerShell
    Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso
    ```

7. 執行下列命令：

    ```PowerShell
    Add-WindowsCapability -Online -Name ServerCore.AppCompatibility~~~~0.0.1.0 -Source <Mounted_Server_FOD_Drive> -LimitAccess
     ```

8. 進度列完成後，重新啟動作業系統。

   如需 DISM 命令的詳細資訊，請參閱[在 Windows PowerShell 中使用 DISM](https://docs.microsoft.com/windows-hardware/manufacture/desktop/use-dism-in-windows-powershell-s14)

## <a name="to-optionally-add-internet-explorer-11-to-server-core-after-adding-the-server-core-app-compatibility-fod"></a>(選擇性) 將 Internet Explorer 11 加入 Server Core (在加入 Server Core 應用程式相容性 FOD 之後)

 >[!NOTE]  
   > 加入 Internet Explorer 11 時需要 Server Core 應用程式相容性 FOD，但是加入 Server Core 應用程式相容性 FOD 時不需要 Internet Explorer 11。

1. 在已加入應用程式相容性 FOD 和已將伺服器 FOD 選用套件 ISO 複製到本機的 Server Core 電腦上，使用系統管理員身分登入。

2. 在命令提示字元中輸入 **powershell.exe**，以啟動 PowerShell。

3. 使用下列命令掛接 FOD ISO：

    ```PowerShell
    Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso
    ```

4. 執行下列命令，並使用 `$package_path` 變數輸入 Internet Explorer 封包檔的路徑：

    ```PowerShell
    $package_path = "D:\Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~.cab"

    Add-WindowsPackage -Online -PackagePath $package_path
    ```

5. 進度列完成後，重新啟動作業系統。

## <a name="release-notes-and-suggestions-for-the-server-core-app-compatibility-fod-and-internet-explorer-11-optional-package"></a>Server Core 應用程式相容性 FOD 和 Internet Explorer 11 選用套件的版本資訊與建議

> [!IMPORTANT]
> 就地升級至 Windows Server 版本 1903 之後，安裝在 Windows Server 版本 1809 上的 FOD 並不會保留，因此您必須在升級後再次進行安裝。 或者，您可以在升級之前，先將 FOD 加入新的 Windows Server 安裝來源。 這可確保所有新版 FOD 都可在升級完成之後出現。 如需詳細資訊，請參閱[將功能和選用套件新增至離線的 WIM Server Core 映像](install-fod-19.md#add-capabilities)。

- **重要事項：** 在繼續安裝和使用 Server Core 應用程式相容性 FOD 及 Internet Explorer 11 選用套件之前，請先閱讀 Windows Server 2019 的版本資訊，以了解任何問題、考量或指引。

- 使用 Windows Update 安裝累計更新後，您新增應用程式相容性 FOD 時，可能會遇到 Server Core 主控台體驗有閃爍的情形。  此問題已在 2018 年 12 月的更新中解決。  如需詳細資訊和解決步驟，請參閱[知識庫文章 4481610：在 Windows Server 2019 Server Core 中安裝 Server Core 應用程式相容性 FOD 後發生畫面閃爍](https://support.microsoft.com/help/4481610/screen-flickers-after-fod-installation-windows2019-server-core)。

- 安裝應用程式相容性 FOD 和重新啟動伺服器後，命令主控台視窗的框架色彩會變成不同的藍色色調。

- 如果您選擇同時安裝 Internet Explorer 11 選用套件，請注意，系統不支援以按兩下的方式開啟本機儲存的 .htm 檔案。 不過，您可以**按一下滑鼠右鍵**並選擇 [以 IE 開啟]  ，或者直接從 Internet Explorer 的 [檔案]   -> [開啟]  來開啟檔案。

- 為了透過應用程式相容性 FOD 進一步加強 Server Core 的應用程式相容性，IIS 管理主控台已新增為 Server Core 的選用元件。  不過，絕對要先加入應用程式相容性 FOD，才能使用 IIS 管理主控台。 IIS 管理主控台相依於 Microsoft Management Console (mmc.exe)，只有加入應用程式相容性 FOD 的 Server Core 上有此項目。  使用 Powershell 的 [**Install-WindowsFeature**](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature?view=win10-ps) 來加入 IIS 管理主控台。

- 以一般指導方針來說，您在 Server Core 上安裝應用程式時 (無論是否包含這些選用套件)，有時候必須使用無訊息安裝選項和指示。 
    
  - 例如，您可以將適用於 SQL Server 2016 和 SQL Server 2017 的 SQL Server Management Studio 安裝在 Server Core 上，當應用程式相容性 FOD 出現時，此服務就能完整地運作。  請參閱[從命令提示字元安裝 SQL Server](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-from-the-command-prompt?view=sql-server-2017)。
  - 如果不需要 SQL Server Management Studio，則不必安裝 Server Core 應用程式相容性 FOD。  請參閱[在 Server Core 上安裝 SQL Server](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-on-server-core?view=sql-server-2017)。

## <a name="a-idadd-capabilities-adding-capabilities-and-optional-packages-to-an-offline-wim-server-core-image"></a><a id="add-capabilities">將功能和選用套件新增至離線的 WIM Server Core 映像

1. 將 Windows Server 和 Server FOD ISO 映像檔下載至 Windows 電腦上的本機資料夾。

   - 如果您有大量授權，您可以從[大量授權服務中心](https://www.microsoft.com/Licensing/servicecenter/default.aspx)下載 Windows Server 和 Server FOD ISO 映像檔。
   - [Microsoft 評估中心](https://www.microsoft.com/evalcenter/evaluate-windows-server)或 [Visual Studio 入口網站](https://visualstudio.microsoft.com)也會提供適用長期維護通道版本的伺服器 FOD ISO 映像檔給訂閱者。

2. 以系統管理員身分開啟 PowerShell 工作階段，然後使用下列命令將映像檔掛接為磁碟機：

   ```PowerShell
   Mount-DiskImage -ImagePath Path_To_ServerFOD_ISO
   Mount-DiskImage -ImagePath Path_To_Windows_Server_ISO
   ```

3. 將 Windows Server ISO 檔案的內容複製到本機資料夾 (例如 C:\SetupFiles\WindowsServer  )。

4. 使用下列命令，在 Install.wim 檔案中取得您要修改的映像名稱。<br>
使用 `$install_wim_path` 變數來輸入 Install.wim 檔案的路徑 (位於 ISO 檔案的 \Sources 資料夾中)。

   ```PowerShell
   $install_wim_path = "C:\SetupFiles\WindowsServer\sources\install.wim"

   Get-WindowsImage -ImagePath $install_wim_path
   ```

5. 使用下列命令將 Install.wim 檔案掛接到新的資料夾中，並以您自己的值取代範例變數的值，以及重複使用先前命令中的 `$install_wim_path` 變數。<br>

   - `$image_name` - 輸入要掛接的映像名稱。
   - `$mount_folder variable` - 指定存取 Install.wim 檔案內容時要使用的資料夾。

   ```PowerShell
   $image_name = "Windows Server Datacenter"
   $mount_folder = "c:\test\offline"

   Mount-WindowsImage -ImagePath $install_wim_path -Name $image_name -path $mount_folder
   ```

6. 使用下列命令將您需要的功能及套件新增至 Install.wim 映像，並以您自己的值取代範例變數。<br>

   - `$capability_name` - 指定要安裝的功能名稱 (在此案例中為 AppCompatibility 功能)。
   - `$package_path` -指定要安裝的套件路徑 (在此案例中為 Internet Explorer)。
   - `$fod_drive` -指定掛接伺服器 FOD 映像的磁碟機代號。

   ```PowerShell
   $capability_name = "ServerCore.AppCompatibility~~~~0.0.1.0"
   $package_path = "D:\Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~.cab"
   $fod_drive = "d:\"

   Add-WindowsCapability -Path $mount_folder -Name $capability_name -Source $fod_drive -LimitAccess
   Add-WindowsPackage -Path $mount_folder -PackagePath $package_path
   ```

7. 使用下列命令 (其使用先前命令中的 `$mount_folder` 變數) 卸載並認可 Install.wim 檔案的變更：

   ```PowerShell
   Dismount-WindowsImage -Path $mount_folder -Save
   ```

您現在可以從您為 Windows Server 安裝檔案建立的資料夾中，執行 setup.exe 來升級伺服器 (在此範例中為：C:\SetupFiles\WindowsServer  )。 此資料夾現在包含具有額外功能與選用套件的 Windows Server 安裝檔案。