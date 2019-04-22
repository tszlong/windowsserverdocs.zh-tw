---
title: 將 Windows SBS 2003 設定和資料移至進行 Windows Server Essentials 移轉的目的地伺服器
description: 描述如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 67087ccb-d820-4642-8ca2-7d2d38714014
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 74375d65845a7a5e9c2d6752bdee49993b8cc791
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822699"
---
# <a name="move-windows-sbs-2003-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>將 Windows SBS 2003 設定和資料移至進行 Windows Server Essentials 移轉的目的地伺服器

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

如下所示將設定和資料移動到目的地伺服器：

1.  [將資料複製到目的地伺服器](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_CopyData)  
  
2.  [Active Directory 使用者帳戶匯入到 Windows Server Essentials 儀表板 （選擇性）](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_ImportADaccounts)  
  
3.  [移除舊登入指令碼 （選擇性）](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_RemoveScripts)  
  
4.  [移除舊版 Active Directory 群組原則物件 （選擇性）](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_RemoveLegacyADGPO)  
  
5.  [設定網路](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_Configure)  
  
6.  [允許的電腦對應到使用者帳戶](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_MapPermittedComputers)  
 
##  <a name="BKMK_CopyData"></a> 將資料複製到目的地伺服器  
 在您將資料從來源伺服器複製到目的地伺服器之前，請執行下列工作：  
  
-   檢閱來源伺服器上共用資料夾的清單，包括每個資料夾的權限。 建立或自訂目的地伺服器上的資料夾，以符合您從來源伺服器移轉的資料夾結構。  
  
-   檢閱每個資料夾的大小，並確定目的地伺服器具有足夠的儲存空間。  
  
-   針對所有使用者將來源伺服器上的共用資料夾變成唯讀，因此在您將檔案複製到目的伺服器時，磁碟機上不會發生寫入動作。  
  
#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>從來源伺服器將資料複製到目的地伺服器  
  
1.  以網域系統管理員身分登入目的地伺服器。  
  
2.  按一下 [開始] ，在搜尋方塊中輸入 **cmd** ，然後按 ENTER。  
  
3.  在命令提示字元中輸入下列命令，然後按 ENTER：  
  
    `robocopy \\<SourceServerName> \<SharedSourceFolderName> \\<DestinationServerName> \<SharedDestinationFolderName> /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`  
  
     其中：
     - \<SourceServerName\>是來源伺服器的名稱
     - \<Sharedsourcefoldername&gt\>是來源伺服器上的共用資料夾的名稱
     - \<D\>是目的地伺服器的名稱
     - \<Shareddestinationfoldername&gt\>是要將資料複製到其中的目的地伺服器上的共用的資料夾。  
 
4.  在每一個從來源伺服器移轉的共用資料夾重複上述步驟。  
  
##  <a name="BKMK_ImportADaccounts"></a> Active Directory 使用者帳戶匯入到 Windows Server Essentials 儀表板 （選擇性）  
 根據預設，來源伺服器上建立的所有使用者帳戶會自動都移轉到 Windows Server Essentials 儀表板。 不過，如果某些屬性不符合移轉需求，Active Directory 使用者帳戶的自動移轉會失敗。 您可以使用以下 Windows PowerShell Cmdlet 匯入 Active Directory 使用者。  
  
#### <a name="to-import-an-active-directory-user-account-to-the-windows-server-essentials-dashboard"></a>若要匯入 Windows Server Essentials 儀表板的 Active Directory 使用者帳戶  
  
1.  以網域系統管理員身分登入目的地伺服器。  
  
2.  以系統管理員身分開啟 Windows PowerShell。  
  
3.  執行下列的 Cmdlet，其中 `[AD username]` 為您想要匯入的 Active Directory 使用者帳戶名稱：  
  
     `Import-WssUser SamAccountName [AD username]`  
  
##  <a name="BKMK_RemoveScripts"></a> 移除舊登入指令碼 （選擇性）  
 Windows SBS 2003 使用登入指令碼處理像是安裝軟體和自訂桌面等工作。  Windows Server Essentials 登入指令碼和群組原則物件的組合取代 Windows SBS 2003 登入指令碼。  
  
> [!NOTE]
>  如果您修改過 Windows SBS 2003 登入指令碼，您應重新命名指令碼以保存您的自訂項目。  
  
> [!NOTE]
>  Windows SBS 2003 登入指令碼只適用於使用 [新增使用者精靈] 新增的使用者帳戶。  
  
#### <a name="to-remove-the-windows-sbs-2003-logon-scripts"></a>移除 Windows SBS 2003 登入指令碼  
  
1.  按一下 [開始] ，指向 [系統管理工具] ，然後按一下 [Active Directory 使用者和電腦] 。  
  
2.  在 [Active Directory 使用者和電腦] 中，展開您的網路，然後按一下 [使用者] 。  
  
3.  以滑鼠右鍵按一下使用者名稱，按一下 [內容] ，然後按一下 [設定檔]  索引標籤。  
  
4.  刪除 [登入指令碼]  文字方塊的內容，然後按一下 [確定] 。  
  
5.  針對每個使用者重複步驟 3 和 4。  
  
##  <a name="BKMK_RemoveLegacyADGPO"></a> 移除舊版 Active Directory 群組原則物件 （選擇性）  
 Windows Server Essentials 的更新群組原則物件 (Gpo)。 它們是 Windows SBS 2003 GPO 的超集。 適用於 Windows Server Essentials，一些 Windows SBS 2003 Gpo 和 Windows Management Instrumentation (WMI) 篩選器必須以手動方式刪除的 Windows Server Essentials Gpo 和 WMI 篩選器以避免發生衝突。  
  
> [!NOTE]
>  如果您已修改原始的 Windows SBS 2003 群組原則物件，您應將它們的複本儲存在不同的位置，然後再從 Windows SBS 2003 刪除它們。  
  
#### <a name="to-remove-old-group-policy-objects-from-windows-sbs-2003"></a>從 Windows SBS 2003 移除舊的群組原則物件  
  
1.  以系統管理員帳戶登入來源伺服器。  
  
2.  按一下 [開始]，然後按一下 [伺服器管理]。  
  
3.  在 導覽 窗格中，按一下**進階管理**，按一下**群組原則管理**，然後按一下 **樹系: * * * < 您的網域名稱\>*。  
  
4.  按一下 **網域**，按一下 *< 您的網域名稱\>*，然後按一下**群組原則物件**。  
  
5.  以滑鼠右鍵按一下 [Small Business Server 稽核原則]，按一下 [刪除]，然後按一下 [確定]。  
  
6.  重複步驟 5，以刪除下列套用到您的網路的 GPO：  
  
    -   Small Business Server 用戶端電腦  
  
    -   Small Business Server 網域密碼原則  
  
         我們建議您以強制執行強式密碼的 Windows Server Essentials 中設定密碼原則。 如果要設定密碼原則，請使用儀表板，將設定寫入預設網域原則。 密碼原則設定不會寫入 Small Business Server 網域密碼原則物件，這與在 Windows SBS 2003 中的行為一樣。  
  
    -   Small Business Server 網際網路連線防火牆  
  
    -   Small Business Server 鎖定原則  
  
    -   Small Business Server 遠端協助原則  
  
    -   Small Business Server Windows 防火牆  
  
    -   Small Business Server 更新服務用戶端電腦原則  
  
         如果您是從 Windows SBS 2003 R2 移轉，將會顯示此 GPO。  
  
    -   Small Business Server 更新服務一般設定原則  
  
         如果您是從 Windows SBS 2003 R2 移轉，將會顯示此 GPO。  
  
    -   Small Business Server 更新服務伺服器電腦原則  
  
         如果您是從 Windows SBS 2003 R2 移轉，將會顯示此 GPO。  
  
7.  請確認已刪除所有的 GPO。  
  
#### <a name="to-remove-wmi-filters-from-windows-sbs-2003"></a>從 Windows SBS 2003 移除 WMI 篩選器  
  
1.  以系統管理員帳戶登入來源伺服器。  
  
2.  按一下 [開始]，然後按一下 [伺服器管理]。  
  
3.  在 導覽 窗格中，按一下**進階管理**，按一下**群組原則管理**，然後按一下 **樹系: * * * < YourNetworkDomainName\>*  
  
4.  按一下 **網域**，按一下 *< YourNetworkDomainName\>*，然後按一下**WMI 篩選器**。  
  
5.  以滑鼠右鍵按一下 [PostSP2]，按一下 [刪除]，然後按一下 [是]。  
  
6.  以滑鼠右鍵按一下 [PreSP2] ，按一下 [刪除] ，然後按一下 [是] 。  
  
7.  請確認已刪除這三個 WMI 篩選器。  
  
##  <a name="BKMK_Configure"></a> 設定網路  
  
#### <a name="to-configure-the-network"></a>若要設定網路  
  
1.  在目的地伺服器上，開啟儀表板。  
  
2.  在 [儀表板] 的 [首頁]  頁面上，按一下 [設定] ，按一下 [設定隨處存取] ，然後選擇 [按一下以設定隨處存取]  選項。  
  
3.  依照 [設定隨處存取] 精靈中的指示來設定您的路由器和網域名稱。  
  
 如果您的路由器不支援 UPnP 架構，或已停用 UPnP 架構，路由器名稱旁邊可能會出現一個黃色警告圖示。 請確定下列連接埠已開啟，且會導向到目的地伺服器的 IP 位址：  
  
-   連接埠 80：HTTP 網路流量  
  
-   連接埠 443:HTTPS 網路流量  
  
> [!NOTE]
>  如果您已經在第二個伺服器上設定內部部署 Exchange Server，您必須確定連接埠 25 (SMTP) 也已開啟，而且會重新導向到內部部署 Exchange Server 的 IP 位址。  
  
##  <a name="BKMK_MapPermittedComputers"></a> 允許的電腦對應到使用者帳戶  
 在 Windows SBS 2003 中，如果使用者連線到遠端 Web 存取，就會顯示網路中的所有電腦。 這可能包括使用者沒有存取權限的電腦。 在 Windows Server Essentials 中，使用者必須明確地指派給它才會顯示在 遠端 Web 存取的電腦。 從 Windows SBS 2003 移轉的每個使用者帳戶都必須對應至一或多部電腦。  
  
#### <a name="to-map-user-accounts-to-computers"></a>將使用者帳戶對應至電腦  
  
1.  在目的地伺服器上，開啟 Windows Server Essentials 儀表板。  
  
2.  在瀏覽列中，按一下 [使用者]。  
  
3.  在使用者帳戶清單中，以滑鼠右鍵按一下使用者帳戶，然後按一下 [檢視帳戶內容]。  
  
4.  按一下 [隨處存取]  索引標籤，然後按一下 [允許遠端 Web 存取，以及對 Web 服務應用程式的存取] 。  
  
5.  依序按一下 [共用資料夾]、[電腦]、[首頁連結]，然後按一下 [套用]。  
  
6.  按一下 [電腦存取] 索引標籤，然後按一下您想要允許存取的電腦名稱。  
  
7.  對每個使用者帳戶重複步驟 3、4、5 和 6。  
  
> [!NOTE]
>  您不需要變更用戶端電腦的設定。 它會自動設定。  
  
> [!NOTE]
>  完成移轉之後，如果您在目的伺服器上建立第一個新的使用者帳戶時遇到問題，請移除您新增的使用者帳戶，然後再重新建立一次。
