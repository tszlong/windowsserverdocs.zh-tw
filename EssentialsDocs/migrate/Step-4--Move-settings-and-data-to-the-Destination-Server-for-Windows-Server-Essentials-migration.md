---
title: 步驟 4：將設定和資料移至進行 Windows Server Essentials 移轉的目的地伺服器
description: 說明如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e143df43-e227-4629-a4ab-9f70d9bf6e84
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d9aea85513e2453c02f6c14fb3f4d708be211d3f
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318750"
---
# <a name="step-4-move-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>步驟 4：將設定和資料移至進行 Windows Server Essentials 移轉的目的地伺服器

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

本節提供從來源伺服器移轉資料和設定的相關資訊。 如下所示將設定和資料移動到目的地伺服器：  
  
-   [將資料複製到目的地伺服器](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_CopyData)  
  
-   [設定網路](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_Network)  
  
-   [將允許的電腦對應到使用者帳戶](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_MapPermittedComputers)  
  
##  <a name="copy-data-to-the-destination-server"></a><a name="BKMK_CopyData"></a>將資料複製到目的地伺服器  
 在您將資料從來源伺服器複製到目的地伺服器之前，請執行下列工作：  
  
-   檢閱來源伺服器上共用資料夾的清單，包括每個資料夾的權限。 建立或自訂目的地伺服器上的資料夾，以符合您從來源伺服器移轉的資料夾結構。  
  
-   檢閱每個資料夾的大小，並確定目的地伺服器具有足夠的儲存空間。  
  
-   將來源伺服器上的共用資料夾對所有使用者設為唯讀，避免您將檔案複製到目的地伺服器時在硬碟上發生寫入的情況。  
  
-   [用戶端電腦備份] 資料夾無法移轉到目的地伺服器。 伺服器移轉之前，請確定所有用戶端電腦都處於健康的狀態。 伺服器移轉之後，建議您設定和啟動用戶端電腦備份，確保已為所有重要的用戶端電腦備份資料。  
  
-   [檔案歷程**記錄備份**] 資料夾無法直接遷移到目的地伺服器，因為 Windows Server Essentials 中的資料夾結構和備份中繼資料有所變更。 不過，還是可以移轉特定使用者在特定電腦上的 [檔案歷程記錄備份] 資料夾。 若要這麼做，您必須找出該使用者和電腦之 [檔案歷程記錄備份] 資料夾中的 [資料] 資料夾，然後複製該 [資料] 資料夾到目的伺服器上的 [檔案歷程記錄備份] 資料夾。  
  
#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>從來源伺服器將資料複製到目的地伺服器  
  
1. 以網域管理員身分登入目的地伺服器，然後開啟 [命令提示字元] 視窗或 Windows PowerShell 命令提示字元。  
  
2. 如果您使用 [命令提示字元] 視窗，請輸入下列命令，然後按 ENTER 鍵：  
  
   `robocopy \\<SourceServerName>\<SharedSourceFolderName> "<PathOfTheDestination>\<SharedDestinationFolderName>" /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`
  
    其中：  
  
   - \<SourceServerName\> 是來源伺服器的名稱  
  
   - \<SharedSourceFolderName\> 是來源伺服器上共用資料夾的名稱  
  
   - \<PathOfTheDestination\> 是您要移動資料夾的絕對路徑  
  
   - \<SharedDestinationFolderName\> 是要將資料複製到其中的目的地伺服器上的資料夾  
  
     例如，  `robocopy \\sourceserver\MyData "d:\ServerFolders\MyData" /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`。  
  
3. 如果您使用 Windows PowerShell，請輸入下列命令，然後按 ENTER 鍵。  
  
    `Add-Wssfolder  Path \ -Name  -KeepPermission`  
  
4. 針對您要從來源伺服器移轉的每個共用資料夾，重複此程序。  
  
##  <a name="configure-the-network"></a><a name="BKMK_Network"></a>設定網路  
  
#### <a name="to-configure-the-network"></a>若要設定網路  
  
1. 在目的地伺服器上，開啟儀表板。  
  
2. 在 [儀表板] 的 [首頁] 頁面上，按一下 [設定]，按一下 [設定隨處存取]，然後選擇 [按一下以設定隨處存取] 選項。  
  
3. 此時會出現「設定隨處存取精靈」。 完成精靈中的指示，來設定您的路由器及網域名稱。  
  
   如果您的路由器不支援 UPnP 架構，或已停用 UPnP 架構，路由器名稱旁邊可能會出現一個黃色警告圖示。 請確定下列連接埠已開啟，且會導向到目的地伺服器的 IP 位址：  
  
-   連接埠 80：HTTP 網路流量  
  
-   連接埠 443：HTTP 網路流量  
  
> [!NOTE]
>  如果您想要在目的地伺服器上設定公用網域名稱，您必須從來源伺服器釋放網域名稱以避免競爭動態 DNS 更新。  
  
##  <a name="map-permitted-computers-to-user-accounts"></a><a name="BKMK_MapPermittedComputers"></a>將允許的電腦對應到使用者帳戶  
 從舊版的 Windows Small Business Server 或 Windows Server Essentials 移轉的每個使用者帳戶必須對應至一個或多部電腦。  
  
#### <a name="to-map-user-accounts-to-computers"></a>將使用者帳戶對應至電腦  
  
1.  開啟 [Windows Server Essentials 儀表板]。  
  
2.  在瀏覽列中，按一下 [使用者]。  
  
3.  在使用者帳戶清單中，以滑鼠右鍵按一下使用者帳戶，然後按一下 [檢視帳戶內容]。  
  
4.  按一下 [隨處存取] 索引標籤，然後按一下 [允許遠端 Web 存取，以及存取 Web 服務應用程式]。  
  
5.  依序按一下 [共用資料夾]、[電腦]、[首頁連結]，然後按一下 [套用]。  
  
6.  按一下 [電腦存取] 索引標籤，然後再按一下您想要允許存取的電腦名稱。  
  
7.  對每個使用者帳戶重複步驟 3、4、5 和 6。  
  
> [!NOTE]
>  您不需要變更用戶端電腦的設定。 它會自動設定。  
  
> [!NOTE]
>  完成移轉之後，如果您在目的伺服器上建立第一個新的使用者帳戶時遇到問題，請移除您新增的使用者帳戶，然後再重新建立一次。  
  
## <a name="next-steps"></a>後續步驟  
 您已經將設定和資料移動到目的地伺服器。 現在請移至[步驟5：在目的地伺服器上啟用 Windows Server Essentials 遷移的資料夾](Step-5--Enable-folder-redirection-on-the-Destination-Server-for-Windows-Server-Essentials-migration.md)重新導向。  
  

若要查看所有步驟，請參閱[遷移至 Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。

