---
title: "將 Windows SBS 2011 標準設定和資料移到目的地 Windows Server Essentials 伺服器移轉"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 16b24026-2fe3-4bd0-b82f-900e1564be99
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: b0ee150be2452fdf4c31c6a2a372958640fa5e4a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="move-windows-sbs-2011-standard-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>將 Windows SBS 2011 標準設定和資料移到目的地 Windows Server Essentials 伺服器移轉

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

前往設定和資料目的伺服器，如下所示：  
  

1.  [複製到目的伺服器的資料](Move-Windows-SBS-2011-Standard-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_CopyData)  
  
2.  [Active Directory 帳號匯入 Windows Server Essentials 儀表板（選擇性）](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_ImportADaccounts)  
  
3.  [從來源伺服器前往路由器 DHCP 伺服器角色](Move-Windows-SBS-2011-Standard-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_MoveDHCP)  
  
4.  [設定網路](Move-Windows-SBS-2011-Standard-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_Network)  
  
5.  [移除舊版 Active Directory 群組原則物件（選擇性）](Move-Windows-SBS-2011-Standard-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_RemoveLegacyADGPO)  
  
6.  [允許電腦帳號地圖](Move-Windows-SBS-2011-Standard-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_MapPermittedComputers)  
  
##  <a name="BKMK_CopyData"></a>複製到目的伺服器的資料  
 資料來源伺服器複製目的伺服器之前，請執行下列工作：  
  
-   檢視原始檔伺服器，包括針對每個資料夾的權限上的共用資料夾清單。 建立自訂以符合您的移轉的來源伺服器資料夾結構目的伺服器上的資料夾。  
  
-   檢視的每個資料夾的大小，並確保目的伺服器已儲存空間。  
  
-   請來源伺服器上的共用的資料夾 Read-only 針對所有的使用者，不書寫可能需要將磁碟機上時，您將檔案複製到目的伺服器。  
  
#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>將資料來源伺服器的複製到目的伺服器  
  
1.  目的地伺服器網域，系統管理員身分登入，然後打開在命令視窗。  
  
2.  在命令提示字元中，輸入下列命令，，然後按 ENTER 鍵：  
  
    `robocopy \\<SourceServerName> \<SharedSourceFolderName> \\<DestinationServerName> \<SharedDestinationFolderName> /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`  
  
     位置：
     - \ < SourceServerName\ > 是來源伺服器的名稱
     - \ < SharedSourceFolderName\ > 是來源伺服器上的共用資料夾的名稱
     - \ < DestinationServerName\ > 是目的地伺服器的名稱
     - \ < SharedDestinationFolderName\ > 是資料將被複製目的伺服器上的共用的資料夾。  
  
3.  重複上一個步驟的每個您要從來源伺服器移轉的共用資料夾。  
  
##  <a name="BKMK_ImportADaccounts"></a>Active Directory 帳號匯入 Windows Server Essentials 儀表板（選擇性）  
 根據預設，所有來源伺服器上建立的使用者帳號自動在 Windows Server Essentials 的儀表板都移轉。 不過，如果某些屬性不符合移轉需求，將會失敗的 Active Directory 使用者 account 自動移轉。 您可以使用下列的 Windows PowerShell cmdlet 匯入 Active Directory 使用者。  
  
#### <a name="to-import-an-active-directory-user-account-to-the-windows-server-essentials-dashboard"></a>Active Directory 使用者 account 匯入 Windows Server Essentials 儀表板  
  
1.  網域系統管理員身分登入目的伺服器。  
  
2.  以系統管理員身分開放 Windows PowerShell。  
  
3.  執行下列 cmdlet，其中`[AD username]`的 Active Directory 帳號，您要匯入的名稱：  
  
     `Import-WssUser  SamAccountName [AD username]`  
  
##  <a name="BKMK_MoveDHCP"></a>從來源伺服器前往路由器 DHCP 伺服器角色  
 如果您的來源伺服器 DHCP 角色執行，執行下列步驟移動 DHCP 角色路由器。  
  
#### <a name="to-move-the-dhcp-role-from-the-source-server-to-the-router"></a>前往 DHCP 角色從來源伺服器路由器  
  
1.  要關閉 DHCP 伺服器上的服務來源，方式如下：  
  
    1.  來源在伺服器上，按一下 [ **[開始]**，按一下**系統管理工具]**，然後按一下 [**服務**。  
  
    2.  在清單中目前執行的服務，以滑鼠右鍵按一下**DHCP 伺服器**，然後按**屬性**。  
  
    3.  適用於**開始輸入]**、**停用**。  
  
    4.  停止服務。  
  
2.  將您的路由器上 DHCP 角色。  
  
    1.  請依照您的路由器的文件中的指示，將在路由器上 DHCP 角色。  
  
    2.  若要確保發出來源伺服器的 IP 位址維持不變，依照您的路由器的文件中的指示來設定 DHCP 範圍來源伺服器上的 DHCP 範圍相同路由器上。  
  
        > [!IMPORTANT]
        >  如果您有未設定路由器上靜態 IP 或 DHCP 晚餐目的伺服器，並 DHCP 範圍不來源伺服器相同，則可能路由器將發行新的 IP 位址，目的地伺服器。 發生這種情形，如果重設轉送規則轉寄給新的目的伺服器的 IP 位址路由器的連接埠。  
  
##  <a name="BKMK_Network"></a>設定網路  
 您的路由器移動 DHCP 角色之後，網路設定目的伺服器上。  
  
#### <a name="to-configure-the-network"></a>若要設定網路  
  
1.  在目的伺服器，開放儀表板。  
  
2.  在**家用**頁面上，按一下 [**設定**，按一下 [**設定隨處存取**，，然後選擇 [**按一下任何地方存取設定**選項。  
  
3.  完成精靈中的指示來設定您的路由器和網域名稱。  
  
 如果您的路由器不支援 UPnP 架構，或是 UPnP 架構已停用，黃色警告圖示可能會出現路由器名稱旁邊。 請確定下列連接埠開放以及它們導向之目的伺服器的 IP 位址：  
  
-   連接埠 80: HTTP 網路流量  
  
-   連接埠 443: HTTPS 網路流量  
  
> [!NOTE]
>  如果您已將在第二個的伺服器上場所 Exchange server 設定，您必須確定已連接埠 25（SMTP) 也開放和，它會重新導向至先 Exchange server 的 IP 位址。  
  
##  <a name="BKMK_RemoveLegacyADGPO"></a>移除舊版 Active Directory 群組原則物件（選擇性）  
 群組原則物件 (Gpo) 的 Windows Server Essentials 的更新。 有更多的 Windows 小型企業 Server 2011 Gpo。 針對 Windows Server Essentials，Windows 小型企業 Server 2011 Gpo 及 Windows 管理檢測 (WMI) 篩選一些必須手動刪除以防止衝突 Windows Server Essentials Gpo 與 WMI 篩選。  
  
> [!NOTE]
>  如果修改原始 Windows 小型企業 Server 2011 群組原則物件，您應該將複本儲存在不同的位置，並再加以移除 Windows 小型企業 Server 2011。  
  
#### <a name="to-remove-old-group-policy-objects-from-windows-small-business-server-2011"></a>若要從 Windows 小型企業 Server 2011 移除舊的群組原則物件  
  
1.  登入的來源伺服器管理員。  
  
2.  按一下**[開始]**，然後按一下 [**伺服器管理]**。  
  
3.  在瀏覽窗格中，按一下 [**進階管理**，按一下 [**群組原則管理**，然後按一下 [**樹系：***< YourDomainName\ >*。  
  
4.  按一下**網域**，按一下 [ *< YourDomainName\ >*，然後按一下 [**群組原則物件**。  
  
5.  以滑鼠右鍵按一下**小型企業伺服器稽核原則**，按一下 [ **Delete**，然後按一下 [ **[確定]**。  
  
6.  重複執行「步驟 5 delete 下列 Gpo 套用到您的網路：  
  
    -   Windows SBS Client Windows 7 和 Windows Vista 原則  
  
    -   Windows SBS Client Windows XP 原則  
  
    -   Windows SBS CSE 原則  
  
    -   Windows SBS 使用者原則  
  
    -   更新服務 Client 電腦原則  
  
    -   更新服務一般設定原則  
  
    -   更新服務伺服器電腦原則  
  
7.  請確認所有 Gpo 刪除。  
  
#### <a name="to-remove-wmi-filters-from-the-source-server"></a>若要移除來源伺服器 WMI 篩選  
  
1.  登入的來源伺服器管理員。  
  
2.  按一下**[開始]**，然後按一下 [**伺服器管理]**。  
  
3.  在瀏覽窗格中，按一下 [**功能**，按一下 [**群組原則管理**，然後按一下 [**樹系：***< YourNetworkDomainName\ >*  
  
4.  按一下**網域**，按一下 [ *< YourNetworkDomainName\ >*，然後按一下 [ **WMI 篩選**。  
  
5.  以滑鼠右鍵按一下**Windows SBS Client**，按一下 [ **Delete**，然後按一下 [ **[是]**。  
  
6.  以滑鼠右鍵按一下**Windows SBS Client Windows 7 和 Windows Vista**，按一下 [ **Delete**，然後按一下 [ **[是]**。  
  
7.  以滑鼠右鍵按一下**Windows SBS Client Windows XP**，按一下 [ **Delete**，然後按一下 [ **[是]**。  
  
8.  請確認刪除的下列三種 WMI 篩選。  
  
##  <a name="BKMK_MapPermittedComputers"></a>允許電腦帳號地圖  
 在 Windows 小型企業 Server 2011，如果使用者連接到遠端的網路存取，會顯示網路中的所有的電腦。 這可能包括使用者並不需要存取權限的電腦。 在 Windows Server Essentials，使用者必須明確指派到電腦，它會顯示在網路上的存取。 移轉 Windows 小型企業 Server 2011 從每個使用者帳號必須對應至一部以上的電腦。  
  
#### <a name="to-map-user-accounts-to-computers"></a>若要對應帳號到電腦  
  
1.  打開 Windows Server Essentials 儀表板。  
  
2.  在瀏覽列中，按一下 [**使用者**。  
  
3.  在清單中的使用者帳號，帳號，以滑鼠右鍵按一下，然後按一下**檢視 account 屬性**。  
  
4.  按一下**隨處存取**索引標籤，然後按一下 [**可讓遠端 Web Access 與網路服務] 應用程式存取**。  
  
5.  選取**共用資料夾**，請選取**電腦**，選取**首頁連結**，然後按一下**套用**。  
  
6.  按一下**電腦存取**索引標籤，然後再按一下您要允許存取電腦的名稱。  
  
7.  每個使用者 account 重複步驟 3、4、5、6。  
  
> [!NOTE]
>  您不需要變更 client 電腦的設定。 它會自動設定。  
  
> [!NOTE]
>  完成移轉，如果您遇到問題，當您建立目的伺服器上的第一個新的使用者帳號之後，移除帳號，您所加入，然後再重新建立。
