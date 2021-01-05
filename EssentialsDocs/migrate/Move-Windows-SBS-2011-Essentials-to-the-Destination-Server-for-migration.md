---
title: 將 Windows SBS 2011 Essentials 的設定和資料移至目的地伺服器以進行 Windows Server Essentials 移轉
description: 瞭解如何將 Windows SBS 2011 Essentials 設定和資料移至目的地伺服器，以進行 Windows Server Essentials 遷移。
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 47548994-9fa0-42e0-afa4-c2ccbd063acb
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 25a155db15f0ea76695c1bbbda100d18e1c56b9a
ms.sourcegitcommit: 9e19436bd8b20af60284071ab512405aebfbec83
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/29/2020
ms.locfileid: "97810706"
---
# <a name="move-windows-sbs-2011-essentials-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>將 Windows SBS 2011 Essentials 的設定和資料移至目的地伺服器以進行 Windows Server Essentials 移轉

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

如下所示將設定和資料移動到目的地伺服器：


1.  [將資料複製到目的地伺服器](Move-Windows-SBS-2011-Essentials-to-the-Destination-Server-for-migration.md#BKMK_CopyData)

2.  [將 Active Directory 使用者帳戶匯入 Windows Server Essentials 儀表板 (選擇性) ](Move-Windows-SBS-2011-Essentials-to-the-Destination-Server-for-migration.md#BKMK_ImportADaccounts)

3.  [設定網路](Move-Windows-SBS-2011-Essentials-to-the-Destination-Server-for-migration.md#BKMK_Network)

4.  [將允許的電腦對應到使用者帳戶](Move-Windows-SBS-2011-Essentials-to-the-Destination-Server-for-migration.md#BKMK_MapPermittedComputers)

##  <a name="copy-data-to-the-destination-server"></a><a name="BKMK_CopyData"></a> 將資料複製到目的地伺服器
 在您將資料從來源伺服器複製到目的地伺服器之前，請執行下列工作：

-   檢閱來源伺服器上共用資料夾的清單，包括每個資料夾的權限。 建立或自訂目的地伺服器上的資料夾，以符合您從來源伺服器移轉的資料夾結構。

-   檢閱每個資料夾的大小，並確定目的地伺服器具有足夠的儲存空間。

-   針對所有使用者將來源伺服器上的共用資料夾變成唯讀，因此在您將檔案複製到目的伺服器時，磁碟機上不會發生寫入動作。

#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>從來源伺服器將資料複製到目的地伺服器

1.  以網域系統管理員身分登入目的地伺服器，然後開啟命令視窗。

2.  在命令提示字元中輸入下列命令，然後按 ENTER：

    `robocopy \\<SourceServerName> \<SharedSourceFolderName> \\<DestinationServerName> \<SharedDestinationFolderName> /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`

     其中：
     - \<SourceServerName\> 是來源伺服器的名稱
     - \<SharedSourceFolderName\> 是來源伺服器上共用資料夾的名稱
     - \<DestinationServerName\> 這是目的地伺服器的名稱，
     - \<SharedDestinationFolderName\> 這是目的地伺服器上要將資料複製到其中的共用資料夾。

3.  在每一個從來源伺服器移轉的共用資料夾重複上述步驟。

##  <a name="import-active-directory-user-accounts-to-the-windows-server-essentials-dashboard-optional"></a><a name="BKMK_ImportADaccounts"></a> 將 Active Directory 使用者帳戶匯入 Windows Server Essentials 儀表板 (選擇性) 
 根據預設，在來源伺服器上建立的所有使用者帳戶都會自動遷移到 Windows Server Essentials 中的儀表板。 不過，如果所有內容都不符合移轉需求，Active Directory 使用者帳戶的自動移轉將會失敗。 您可以使用以下 Windows PowerShell Cmdlet 匯入 Active Directory 使用者。

#### <a name="to-import-an-active-directory-user-account-to-the-windows-server-essentials-dashboard"></a>將 Active Directory 使用者帳戶匯入 Windows Server Essentials 儀表板

1.  以網域系統管理員身分登入目的地伺服器。

2.  以系統管理員身分開啟 Windows PowerShell。

3.  執行下列的 Cmdlet，其中 `[AD username]` 為您想要匯入的 Active Directory 使用者帳戶名稱：

     `Import-WssUser  SamAccountName [AD username]`

##  <a name="configure-the-network"></a><a name="BKMK_Network"></a> 設定網路

#### <a name="to-configure-the-network"></a>若要設定網路

1. 在目的地伺服器上，開啟儀表板。

2. 在 [儀表板] 的 [首頁] 頁面上，按一下 [設定]，按一下 [設定隨處存取]，然後選擇 [按一下以設定隨處存取] 選項。

3. 完成精靈中的指示，來設定您的路由器及網域名稱。

   如果您的路由器不支援 UPnP 架構，或已停用 UPnP 架構，路由器名稱旁邊可能會出現一個黃色警告圖示。 請確定下列連接埠已開啟，且會導向到目的地伺服器的 IP 位址：

-   連接埠 80：HTTP 網路流量

-   連接埠 443：HTTP 網路流量

##  <a name="map-permitted-computers-to-user-accounts"></a><a name="BKMK_MapPermittedComputers"></a> 將允許的電腦對應到使用者帳戶
 從 Windows Small Business Server 2011 Essentials 移轉的每個使用者帳戶都必須對應至一或多部電腦。

#### <a name="to-map-user-accounts-to-computers"></a>將使用者帳戶對應至電腦

1.  開啟 [Windows Server Essentials 儀表板]。

2.  在瀏覽列中，按一下 [使用者]。

3.  在使用者帳戶清單中，以滑鼠右鍵按一下使用者帳戶，然後按一下 [檢視帳戶內容]。

4.  按一下 [隨處存取] 索引標籤，然後按一下 [允許遠端 Web 存取，以及存取 Web 服務應用程式]。

5.  選取 [共用資料夾]，選取 [電腦]、[首頁連結]，然後按一下 [套用]。

6.  按一下 [電腦存取] 索引標籤，然後再按一下您想要允許存取的電腦名稱。

7.  對每個使用者帳戶重複步驟 3、4、5 和 6。

> [!NOTE]
>  您不需要變更用戶端電腦的設定。 它會自動設定。

> [!NOTE]
>  完成移轉之後，如果您在目的伺服器上建立第一個新的使用者帳戶時遇到問題，請移除您新增的使用者帳戶，然後再重新建立一次。
