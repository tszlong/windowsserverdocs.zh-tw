---
title: Server Core 應用程式相容性功能隨選安裝 (FOD)
description: 如何依需求安裝 Windows Server 功能
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a4d55cca754
author: coreyp-at-msft
ms.author: coreyp
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: b8211ace56aa6565295a15adce26a8dfbc98e1e9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866109"
---
# <a name="server-core-app-compatibility-feature-on-demand-fod"></a>Server Core 應用程式相容性功能隨選安裝 (FOD)

> 適用於 Windows Server 2019 和 Windows Server 版 1809

**Server Core 應用程式相容性功能隨選**是選用功能的套件，可以新增至 Windows Server 2019 Server Core 安裝或 Windows Server 版 1809，在任何時間。

如需有關在 on Demand (FOD) 功能的詳細資訊，請參閱[功能隨選](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities)。


## <a name="why-install-the-app-compatibility-fod"></a>為什麼安裝應用程式相容性 FOD？ 

包含的二進位檔和 Windows Server 含桌面體驗的套件子集，而不會增加應用程式相容性，適用於 Server Core 的隨選功能大幅改善的資訊，請參閱 Windows Server Core 安裝選項的應用程式相容性Windows Server 桌面體驗的圖形化環境。 此選擇性套件可用在個別的 ISO，或是從 Windows Update，但只會新增至 Windows Server Core 安裝和映像。

應用程式相容性 FOD 提供的兩個主要的值如下：

1.  增加 Server Core 的相容的性，已在市場中或已被組織所開發並部署伺服器應用程式。

2.  使用提供的作業系統元件和增加的應用程式相容性的嚴重的疑難排解和偵錯案例中使用的軟體工具的協助。

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

        -   使用 Powershell Cmdlet 來新增， `Install-WindowsFeature -NameFailover-Clustering -IncludeManagementTools`

        -   若要執行容錯移轉叢集管理員，請輸入**cluadmin**在命令提示字元。

## <a name="installing-the-app-compatibility-fod"></a>安裝應用程式相容性 FOD

 >[!IMPORTANT] 
   >應用程式相容性 FOD 只能安裝在 Server Core 上。 請勿嘗試將 Server Core 應用程式相容性 FOD 新增至 Windows Server 安裝的 Windows 伺服器含桌面體驗。

### <a name="to-add-the-server-core-app-compatibility-feature-on-demand-fod-to-a-running-instance-of-server-core"></a>若要加入 Server Core 的執行個體上的 demand (FOD) 上的 Server Core 應用程式相容性功能

 >[!NOTE] 
   > 此程序會使用部署映像服務與管理 (DISM.exe)，命令列工具。 如需 DISM 命令的詳細資訊，請參閱[DISM 功能封裝服務命令列選項](https://docs.microsoft.com/windows-hardware/manufacture/desktop/dism-capabilities-package-servicing-command-line-options)。

>[!NOTE] 
   > Windows Server 2019 Server Core 安裝或 Windows Server 版本 1809年、 安裝可用的相同 FOD 選擇性套件 ISO。

>[!NOTE] 
   > 如果您的電腦或執行 Server Core 的虛擬機器能夠連線到 Windows Update 時，就可以略過步驟 1-7 如下。 但請務必省略 /Source 和 /LimitAccess 步驟 8 中的 DISM 命令。

1. 下載伺服器 FOD 選擇性套件 ISO，並將 ISO 複製到您的區域網路上的共用資料夾：

 - 如果您有大量授權，您可以從相同的入口網站取得 OS ISO 映像檔的位置下載 Server FOD ISO 映像檔：[大量授權服務中心](https://www.microsoft.com/Licensing/servicecenter/default.aspx)。
 - Server FOD ISO 映像檔上也會提供[Microsoft 評估中心](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019)或在[Visual Studio 入口網站](https://visualstudio.microsoft.com)訂閱者。


2. 登入系統管理員身分，連接到您的區域網路和您想要新增至 FOD 在 Server Core 電腦上。

3. 使用  **net 使用**，或透過其他方法，來連接到 FOD ISO 的位置。

4. 將 FOD ISO 複製到您選擇的本機資料夾。

5. 啟動 PowerShell，輸入**powershell.exe**在命令提示字元。

6. 使用下列命令，以掛接 FoD ISO:

        Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso

7. 型別**結束**結束 PowerShell。

8.  執行下列命令：

        DISM /Online /Add-Capability /CapabilityName:"ServerCore.AppCompatibility~~~~0.0.1.0" /Source:drive_letter_of_mounted_ISO: /LimitAccess

9.  進度列完成後，重新啟動作業系統。

### <a name="to-optionally-add-internet-explorer-11-to-server-core-after-adding-the-server-core-app-compatibility-fod"></a>（之後加入 Server Core 應用程式相容性 FOD） （選擇性） 新增到 Server Core 的 Internet Explorer 11

 >[!NOTE]  
   > Server Core 應用程式相容性 FOD 需要 Internet Explorer 11，新增，但是 Internet Explorer 11 時不需要加入 Server Core 應用程式相容性 FOD。

1.  管理員身分登入已加入應用程式相容性 FOD 的 Server Core 電腦及伺服器 FOD 選用封裝，使用 ISO 在本機複製。

2.  啟動 PowerShell，輸入**powershell.exe**在命令提示字元。

3.  使用下列命令，以掛接 FoD ISO:

         Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso

4.  型別**結束**結束 PowerShell。


5.  執行下列命令：

        Dism /online /add-package:drive_letter_of_mounted_iso:"Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~.cab"

6.  進度列完成後，重新啟動作業系統。

 
#### <a name="release-notes-and-suggestions-for-the-server-core-app-compatibility-fod-and-internet-explorer-11-optional-package"></a>版本資訊與建議的 Server Core 應用程式相容性 FOD 和 Internet Explorer 11 的選擇性套件

- **重要事項：** 請閱讀 Windows Server 2019 版本資訊的任何問題、 考量或再繼續進行安裝和使用 Server Core 應用程式相容性 FOD 和 Internet Explorer 11 的選擇性套件的指引。
 
 >[!NOTE] 
   > 它可能會遇到新增應用程式相容性 FOD 之後使用 Windows Update 以安裝累計更新時，Server Core 的主控台體驗的閃爍。  2018 年 12 月，以解決此問題的更新。  如需詳細的資訊和解決步驟，請參閱[4481610 眭妎踱恅：在 Windows Server 2019 Server Core 安裝 Server Core 應用程式相容性 FOD 之後，畫面閃爍](https://support.microsoft.com/help/4481610/screen-flickers-after-fod-installation-windows2019-server-core)。

- 安裝後的應用程式相容性 FOD 和重新啟動伺服器，命令主控台視窗框架色彩會變更為不同為藍色陰影。

- 如果您選擇同時安裝 Internet Explorer 11 選擇性的套件，請注意，不支援以開啟 本機儲存的.htm 檔案按兩下。 不過，您可以**上按一下滑鼠右鍵**，然後選擇**開啟 IE**，或者您可以直接從 Internet Explorer 中開啟它**檔案** -> **開啟**. 

- 若要進一步加強應用程式相容性 FOD 與 Server Core 的應用程式相容性，IIS 管理主控台已新增到 Server Core 為選用的元件。  不過，它是先將新增使用 IIS 管理主控台應用程式相容性 FOD 絕對必要。 IIS 管理主控台依賴 Microsoft Management Console (mmc.exe)，這只是 Server Core 上加上應用程式相容性 FOD 可用。  使用 Powershell [ **Install-windowsfeature** ](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature?view=win10-ps)加入 IIS 管理主控台。

- 因為有時候必須使用無訊息安裝選項和指示，指導方針，在伺服器上的安裝應用程式核心 （不論這些選擇性的套件） 它一般點。 
    
 - 做為範例，SQL Server 2016 和 SQL Server 2017 的 SQL Server Management Studio 可以在 Server Core 上安裝，並有應用程式相容性 FOD 時可完整運作。  查看，請[從命令提示字元安裝 SQL Server](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-from-the-command-prompt?view=sql-server-2017)。
 - 如果不需要 SQL Server Management Studio，則不必安裝 Server Core 應用程式相容性 FOD。  查看，請[Server Core 上安裝 SQL Server](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-on-server-core?view=sql-server-2017)。

