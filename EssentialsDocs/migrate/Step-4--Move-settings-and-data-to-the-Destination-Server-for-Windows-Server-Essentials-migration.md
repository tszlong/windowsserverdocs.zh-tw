---
title: "步驟 4： 移至目的地 Windows Server Essentials 伺服器移轉的設定和資料"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e143df43-e227-4629-a4ab-9f70d9bf6e84
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: db1169a5415039498718a7988c711d068945b4b7
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="step-4-move-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>步驟 4： 移至目的地 Windows Server Essentials 伺服器移轉的設定和資料

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

本章節從來源伺服器提供移轉資料和設定的相關資訊。 前往設定和資料目的伺服器，如下所示：  
  
-   [複製到目的伺服器的資料](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_CopyData)  
  
-   [設定網路](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_Network)  
  
-   [允許電腦帳號地圖](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_MapPermittedComputers)  
  
##  <a name="BKMK_CopyData"></a>複製到目的伺服器的資料  
 資料來源伺服器複製目的伺服器之前，請執行下列工作：  
  
-   檢視原始檔伺服器，包括針對每個資料夾的權限上的共用資料夾清單。 建立自訂以符合您的移轉的來源伺服器資料夾結構目的伺服器上的資料夾。  
  
-   檢視的每個資料夾的大小，並確保目的伺服器已儲存空間。  
  
-   請來源伺服器上的共用的資料夾唯讀所有使用者的因此不書寫可能需要在硬碟上的位置，當您將檔案複製到目的伺服器。  
  
-   **Client 電腦備份**資料夾無法目的地伺服器移轉。 之前伺服器移轉，請確定 client 的所有電腦都都良好。 伺服器移轉之後，建議您的設定，並開始 client 電腦備份，以確保您的資料已備份的所有重要 client 電腦。  
  
-   **檔案歷史備份**資料夾無法直接移轉到 Windows Server Essentials 資料夾結構和備份中繼資料變更因為目的伺服器。 不過，可能是移轉到**檔案歷史備份**針對特定電腦上的特定使用者的資料夾。 若要這樣做，您應該找出**資料**資料夾**檔案歷史備份**資料夾，使用者並電腦，然後複製的**資料**資料夾到**檔案歷史備份**目的伺服器上的資料夾。  
  
#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>將資料來源伺服器的複製到目的伺服器  
  
1.  目的地伺服器網域，系統管理員身分登入，然後打開命令提示字元視窗或 Windows PowerShell 命令提示字元。  
  
2.  如果您使用的命令提示字元視窗中，輸入下列命令，，然後按 ENTER 鍵：  
  
    `robocopy \\<SourceServerName>\<SharedSourceFolderName> "<PathOfTheDestination>\<SharedDestinationFolderName>" /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`
  
     位置：  
  
    -   \ < SourceServerName\ > 是來源伺服器的名稱  
  
    -   \ < SharedSourceFolderName\ > 是來源伺服器上的共用資料夾的名稱  
  
    -   \ < PathOfTheDestination\ > 是絕對路徑您想要移動的資料夾位置  
  
    -   \ < SharedDestinationFolderName\ > 是資料將被複製目的伺服器上的資料夾  
  
     例如， `robocopy \\sourceserver\MyData "d:\ServerFolders\MyData" /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`。  
  
3.  如果您使用 Windows PowerShell，輸入下列命令，，然後按 ENTER 鍵。  
  
     `Add-Wssfolder  Path \ -Name  -KeepPermission`  
  
4.  針對每個您要從來源伺服器移轉的共用資料夾重複此程序。  
  
##  <a name="BKMK_Network"></a>設定網路  
  
#### <a name="to-configure-the-network"></a>若要設定網路  
  
1.  在目的伺服器，開放儀表板。  
  
2.  儀表板上**Home**頁面上，按一下 [**設定**，按一下 [**設定隨處存取**，，然後選擇 [**按一下任何地方存取設定**選項。  
  
3.  隨處存取精靈設定會出現。 完成精靈中的指示來設定您的路由器和網域名稱。  
  
 如果您的路由器不支援 UPnP 架構，或是 UPnP 架構已停用，黃色警告圖示可能會出現路由器名稱旁邊。 請確定下列連接埠開放以及它們導向之目的伺服器的 IP 位址：  
  
-   連接埠 80: HTTP 網路流量  
  
-   連接埠 443: HTTPS 網路流量  
  
> [!NOTE]
>  如果您想要設定的公用網域名稱目的伺服器上，您必須從來源避免競賽，動態更新版 DNS 伺服器發行網域名稱。  
  
##  <a name="BKMK_MapPermittedComputers"></a>允許電腦帳號地圖  
 移轉舊版 Windows 小型企業 Server 或 Windows Server Essentials 的每個使用者帳號必須對應至一部以上的電腦。  
  
#### <a name="to-map-user-accounts-to-computers"></a>若要對應帳號到電腦  
  
1.  打開 Windows Server Essentials 儀表板。  
  
2.  在瀏覽列中，按一下 [**使用者**。  
  
3.  在清單中的使用者帳號，帳號，以滑鼠右鍵按一下，然後按一下**檢視 account 屬性**。  
  
4.  按一下**隨處存取**索引標籤，然後按一下 [**可讓遠端 Web Access 與網路服務] 應用程式存取**。  
  
5.  按一下**共用資料夾**，按一下**電腦**，按一下 [**首頁連結**，然後按一下 [**套用**。  
  
6.  按一下**電腦存取**索引標籤，然後再按一下您要允許存取電腦的名稱。  
  
7.  每個使用者 account 重複步驟 3、4、5、6。  
  
> [!NOTE]
>  您不需要變更 client 電腦的設定。 它會自動設定。  
  
> [!NOTE]
>  完成移轉，如果您遇到問題，當您建立目的伺服器上的第一個新的使用者帳號之後，移除帳號，您所加入，然後再重新建立。  
  
## <a name="next-steps"></a>後續步驟  
 您已經目的伺服器來移動您的設定和資料。 立即移至[執行 「 步驟 5： 讓資料夾重新導向目的地 Windows Server Essentials 伺服器移轉在](Step-5--Enable-folder-redirection-on-the-Destination-Server-for-Windows-Server-Essentials-migration.md)。  
  

若要檢視所有的步驟，請查看[Windows Server essentials 移轉](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。

