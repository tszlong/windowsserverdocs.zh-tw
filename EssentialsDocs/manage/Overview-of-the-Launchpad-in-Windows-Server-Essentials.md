---
title: Windows Server Essentials 中的啟動列概觀
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 198d16cb-3d07-4706-be89-ad14a5f7dc47
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: c20fd35a1a90e6d635891cd2257913c669273735
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89623018"
---
# <a name="overview-of-the-launchpad-in-windows-server-essentials"></a>Windows Server Essentials 中的啟動列概觀

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

[Windows Server Essentials 啟動列] 是電腦第一次電腦連線到伺服器時安裝在電腦上的小型應用程式。 [啟動列] 可讓已驗證的使用者存取 Windows Server Essentials 的重要功能，包括電腦備份、共用檔案和媒體，以及「遠端 Web 存取」網站。 使用者可以從已加入網域的電腦或未加入網域的電腦存取這些功能。 [啟動列] 也提供有關電腦健康情況的即時資訊和通知。 即使電腦未連線到網路，系統管理員仍可使用 [啟動列] 來存取伺服器的 [儀表板]。

 開發適用於 Windows Server Essentials 之增益集的 OEM 和獨立軟體廠商 (ISV) 可以使用 [啟動列] 將增益集功能延伸到網路上的電腦。

 下列的 Windows 作業系統支援使用 [Windows Server Essentials 啟動列]：

- **Windows 8**：所有版本。

- **Windows 7**：所有版本。
- **Windows 10**：所有版本。

  下列的 Windows 作業系統不支援使用 [Windows Server Essentials 啟動列]：

- **其他伺服器**：您無法在執行 Windows Server 作業系統的任何其他電腦上執行 [Windows Server Essentials 啟動列]。

  本主題內容：

- [使用啟動列](Overview-of-the-Launchpad-in-Windows-Server-Essentials.md#BKMK_Launchpad)

- [在 Mac 電腦使用啟動列](Overview-of-the-Launchpad-in-Windows-Server-Essentials.md#BKMK_Mac)

##  <a name="use-the-launchpad"></a><a name="BKMK_Launchpad"></a> 使用啟動控制板
 [Windows Server Essentials 啟動列] 上提供下列連結和資訊。

### <a name="backup"></a>Backup
 按一下 [備份]**** 以開啟電腦的 [備份內容]****。 在 [備份內容]**** 頁面上，您可以：

- 開始或停止備份。

- 檢視最新備份的狀態和詳細資料。

- 指定當執行備份時如何管理電腦電源。

  如需如何使用啟動控制板來備份電腦的相關資訊，請參閱 [管理用戶端備份](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md)。

### <a name="remote-web-access"></a>遠端 Web 存取
 按一下 [遠端 Web 存取]**** 以將網頁瀏覽器開啟至「遠端 Web 存取」網站。 「遠端 Web 存取」網站可讓您從辦公室內或從具有已啟用網際網路之電腦的遠端位置，連線到其他電腦並存取部分網路資源。 如需遠端 Web 存取的詳細資訊，請參閱 [管理遠端 Web 存取](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)。

### <a name="shared-folders"></a>共用資料夾
 按一下 [共用資料夾]**** 以將 [Windows 檔案總管] 開啟至伺服器上共用資料夾的位置。 如需有關共用檔案和資料夾的詳細資訊，請參閱 [管理伺服器資料夾](Manage-Server-Folders-in-Windows-Server-Essentials.md)主題。

### <a name="dashboard"></a>儀表板
 按一下 [儀表板]  **** 以開啟 [登入] **** 頁面來存取 Windows Server Essentials 儀表板。 在您登入之後，就會開啟連到伺服器 [儀表板] 的「遠端桌面」連線。 如需儀表板的詳細資訊，請參閱 [儀表板總覽](Overview-of-the-Dashboard-in-Windows-Server-Essentials.md)。

> [!NOTE]
>  若要使用這個功能，您必須具有適當的存取權或權限來登入伺服器。

### <a name="microsoft-365"></a>Microsoft 365
 只有當使用者有 Microsoft 365 帳戶時，才會在啟動列上顯示 **Microsoft 365** 連結。 按一下 [  **Microsoft 365** ] 以存取 Microsoft 365 資源的其他連結。 如需詳細資訊，請參閱 [快速入門手冊使用 Microsoft 365](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md)。

### <a name="computer-health-alerts"></a>電腦健康狀態警示
 顯示在 [啟動列] 上的警示可提供有關電腦立即健康情況的快速狀態。 若要檢視有關健康狀態警示的資訊，請按一下警示指示來開啟警示檢視器。 健康狀態警示會根據嚴重性層級顯示在檢視器中。 最嚴重的警示會顯示在清單中的最前面；較不嚴重的警示會顯示在清單中較後面的地方。 如需電腦健康情況警示的詳細資訊，請參閱 [管理系統健康](Manage-System-Health-in-Windows-Server-Essentials.md)情況。

##  <a name="use-the-launchpad-with-a-mac-computer"></a><a name="BKMK_Mac"></a> 使用啟動控制板與 Mac 電腦
 您可以將執行 &reg; Mac OS X &reg; 10.5 或更新版本的 Mac 電腦連線到 Windows server Essentials、Windows server Essentials 或 windows Server 2012 R2，或下載並安裝連接器軟體。 當您安裝完連線程式軟體時，您可以選擇在啟動時自動啟動 [啟動列]。

 [啟動列] 是一個小型應用程式，可讓已驗證的使用者存取伺服器的重要功能，包括共用檔案和媒體、增益集及「遠端 Web 存取」。 [啟動列] 也提供有關電腦健康情況的即時資訊和通知。

> [!NOTE]
>  伺服器系統管理員無法使用 Mac 電腦上的 [啟動列] 或 [遠端 Web 存取] 來開啟伺服器 [儀表板] 並管理伺服器。

### <a name="backup"></a>Backup
 按一下 [備份]**** 以設定 Time Machine 來備份您的電腦，以及變更 Time Machine 設定。 如需有關 Time Machine 的詳細資訊，請參閱您電腦製造商的文件。

### <a name="remote-web-access"></a>遠端 Web 存取
 按一下 [ **遠端 Web 存取** ]，將網頁瀏覽器開啟至遠端 Web 存取網站。 遠端 Web 存取可讓您從具有已啟用網際網路之電腦的任何遠端位置存取伺服器上的共用檔案和資料夾。 您可以上傳檔案、在 Web 型的「媒體播放」播放音樂和視訊，以及檢視圖片和播放投影片。 如需詳細資訊，請參閱 [使用遠端 Web 存取](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)。

### <a name="shared-folders"></a>共用資料夾
 按一下 [共用資料夾]**** 以將 Finder 開啟至伺服器上共用資料夾的位置。 如需共用檔案與資料夾的相關資訊，請參閱 [使用共用資料夾](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md)。

### <a name="computer-health-alerts"></a>電腦健康狀態警示
 顯示在 [啟動列] 上的警示可提供有關電腦立即健康情況的快速狀態。 若要檢視有關健康狀態警示的資訊，請按一下警示指示來開啟警示檢視器。 健康狀態警示會根據嚴重性層級顯示在檢視器中。 最嚴重的警示會顯示在清單中的最前面。 較不嚴重的警示會顯示在清單中較後面的地方。

## <a name="additional-references"></a>其他參考資料

-   [取得連線](../use/Get-Connected-in-Windows-Server-Essentials.md)

-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)

-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)
