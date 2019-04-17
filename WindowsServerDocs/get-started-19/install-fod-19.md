---
title: Server Core 應用程式相容性功能隨選安裝 (FOD)
description: 如何安裝 Windows Server 功能隨選安裝
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
ms.sourcegitcommit: dbb4738fdac3b7911952ff11f1eaed9649d6567a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/07/2019
ms.locfileid: "9149898"
---
# Server Core 應用程式相容性功能隨選安裝 (FOD)

> 適用於 Windows Server 2019 和 Windows Server 版本 1809

**Server Core 應用程式相容性功能隨選安裝**是 Windows Server 2019 Server Core 安裝，或 Windows Server 版本 1809，隨時都可以新增的選用功能套件。

如需在隨選安裝 (FOD) 功能的詳細資訊，請參閱[功能隨](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities)。


## 為什麼要安裝應用程式相容性功能 FOD？ 

包括沒有加入的二進位檔和套件從 [含有桌面體驗的 Windows Server 的子集，藉以應用程式相容性功能隨選安裝適用於 Server Core 大幅改進的應用程式相容性的 Windows Server Core 安裝選項Windows Server 桌面體驗圖形環境。 此選用套件適用於不同的 ISO，或從 Windows Update，但只可以新增到 Windows Server Core 安裝和映像。

應用程式相容性功能 FOD 提供兩個主要的值為：

1.  增加 Server core 伺服器應用程式相容性，且已經在市場或有已經由組織所開發和部署。

2.  可協助提供 OS 元件和增加應用程式相容性的軟體嚴重的疑難排解和偵錯案例中所使用的工具。

因為的 Server Core 應用程式相容性功能 FOD 一部分包含可用的作業系統元件：

-   Microsoft Management Console (mmc.exe)

-   事件檢視器 (Eventvwr.msc)

-   效能監視器 (PerfMon.exe)

-   資源監視器 (Resmon.exe)

-   裝置管理員 \] (Devmgmt.msc)

-   檔案總管 \] (Explorer.exe)

-   Windows PowerShell (Powershell_ISE.exe)

-   磁碟管理] (Diskmgmt.msc)

-   容錯移轉叢集管理員 (CluAdmin.msc)

    -   第一次需要額外的容錯移轉叢集的 Windows Server 功能。

        -   若要新增，使用 Powershell Cmdlet `Install-WindowsFeature -NameFailover-Clustering -IncludeManagementTools`

        -   若要執行容錯移轉叢集管理員中，輸入**cluadmin**在命令提示字元。

## 安裝應用程式相容性 FOD

 >[!IMPORTANT] 
   >應用程式相容性功能 FOD 只能在 Server Core 安裝。 請勿嘗試新增至 Windows Server 安裝含有桌面體驗的 Windows Server 的 Server Core 應用程式相容性功能 FOD。

### 將 Server Core 應用程式相容性功能隨選安裝 (FOD) 新增至伺服器核心執行個體

 >[!NOTE] 
   > 此程序會使用部署映像服務與管理 (DISM.exe)，命令列工具。 如需 DISM 命令的詳細資訊，請參閱[DISM 功能封裝維護命令列選項](https://docs.microsoft.com/windows-hardware/manufacture/desktop/dism-capabilities-package-servicing-command-line-options)。

>[!NOTE] 
   > 相同 FOD 選用套件 ISO 可以用於 Windows Server 2019 Server Core 安裝，或是 Windows Server 版本 1809年，安裝。

>[!NOTE] 
   > 如果您的電腦或虛擬機器正在執行 Server Core 能夠連線至 Windows Update 時，就可以略過步驟 1-7 下方。 但請務必關閉 /Source 和 /LimitAccess 離開從 DISM 命令中的步驟 8。

1. 下載 ISO，伺服器 FOD 選用套件，並將 ISO 複製到您的區域網路上的共用資料夾：

 - 如果您有大量授權您可以從何處取得 OS ISO 映像檔相同入口網站下載伺服器 FOD ISO 映像檔：[大量授權服務中心](https://www.microsoft.com/Licensing/servicecenter/default.aspx)。
 - 伺服器 FOD ISO 映像檔也會提供[Microsoft 評估中心](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019)或訂閱者在[Visual Studio 入口網站](https://visualstudio.microsoft.com)上的。


2. 登入系統管理員身分在 Server Core 電腦，連接到您的區域網路，以及您想要新增至 FOD 上。

3. 使用**淨使用**或其他方法，來連線到 FOD ISO 的位置。

4. 將 FOD ISO 複製到您選擇的本機資料夾。

5. 輸入**powershell.exe**命令提示字元中啟動 PowerShell。

6. 使用下列命令來掛接 FoD ISO:

        Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso

7. 輸入以結束 PowerShell 的**結束**。

8.  執行下列命令：

        DISM /Online /Add-Capability /CapabilityName:"ServerCore.AppCompatibility~~~~0.0.1.0" /Source:drive_letter_of_mounted_ISO: /LimitAccess

9.  進度列完成後，重新啟動作業系統。

### 選擇性地將 Internet Explorer 11 新增至伺服器核心 （之後新增 [Server Core 應用程式相容性功能 FOD）

 >[!NOTE]  
   > Server Core 應用程式相容性功能 FOD 需要加入的 Internet Explorer 11，但不需要新增 [Server Core 應用程式相容性功能 FOD Internet Explorer 11。

1.  登入系統管理員身分 Server Core 電腦具有已新增應用程式相容性功能 FOD 和伺服器 FOD 選用套件，使用 ISO 複製在本機上。

2.  輸入**powershell.exe**命令提示字元中啟動 PowerShell。

3.  使用下列命令來掛接 FoD ISO:

         Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso

4.  輸入以結束 PowerShell 的**結束**。


5.  執行下列命令：

        Dism /online /add-package:drive_letter_of_mounted_iso:"Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~.cab"

6.  進度列完成後，重新啟動作業系統。

 
#### 版本資訊及建議的 Server Core 應用程式相容性功能 FOD 和 Internet Explorer 11 的選用套件

- **重要：** 請閱讀任何問題、 考量或再繼續進行安裝和使用 Server Core 應用程式相容性功能 FOD 和 Internet Explorer 11 的選用套件的指導方針的 Windows Server 2019 版本資訊。
 
 >[!NOTE] 
   > 很可能會遇到閃爍與 Server Core 主控台體驗，當您使用 Windows Update 安裝累積更新之後新增應用程式相容性功能 FOD。  2018 年 12 月日與已解決此問題的更新。  如需詳細資訊和解析度的步驟，請參閱[知識庫文章 4481610： 畫面閃爍之後您在 Windows Server 2019 Server Core 安裝 Server Core 應用程式相容性功能 FOD](https://support.microsoft.com/help/4481610/screen-flickers-after-fod-installation-windows2019-server-core)。

- 安裝後的應用程式相容性功能 FOD 和伺服器的重新開機，命令主控台視窗框架色彩會變更為不同淺藍色。

- 如果您選擇也安裝 Internet Explorer 11 的選用套件，請注意，不支援以開啟的檔案儲存在本機.htm 按兩次。 不過，您可以**按一下滑鼠右鍵**並選擇 [**開啟 ie**，或您可以將它開啟直接從 Internet Explorer**檔案** -> **開啟**。 

- 若要進一步增強應用程式相容性功能 FOD 與 Server core 應用程式相容性，IIS 管理主控台已新增至伺服器核心為選用元件。  不過，它是絕對必要，先新增應用程式相容性功能 FOD，若要使用 IIS 管理主控台。 IIS 管理主控台依賴 Microsoft Management Console (mmc.exe)，只是在 Server Core 上，新增應用程式相容性功能 FOD 可用。  使用 Powershell[**安裝 Add-windowsfeature**](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature?view=win10-ps)新增 IIS 管理主控台。

- 一般指導方針，在伺服器上的安裝 app 的核心 （使用或不需要這些選用套件） 它的點是有時需要使用無訊息安裝選項與指示。 
    
 - 做為範例，可以在 Server Core 安裝 SQL Server 2016 和 SQL Server 2017 SQL Server Management Studio，並能完全正常運作的應用程式相容性功能 FOD 存在時。  請參閱[安裝 SQL Server 從命令提示字元](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-from-the-command-prompt?view=sql-server-2017)。
 - 如果不需要 SQL Server Management Studio，則不需要安裝 Server Core 應用程式相容性功能 FOD。  請參閱，[安裝在 Server Core 的 SQL Server](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-on-server-core?view=sql-server-2017)。

