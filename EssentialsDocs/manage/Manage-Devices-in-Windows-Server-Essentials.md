---
title: "管理 Windows Server Essentials 的裝置"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f5fe1088-ebe7-4799-a47d-075b0048dea1
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 288d62a3fe4d9073ba2c0e3fdff385d8317f20d4
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="manage-devices-in-windows-server-essentials"></a>管理 Windows Server Essentials 的裝置

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集
 
 下列章節討論管理的裝置功能伺服器，並如何設定及使用您的網路上的裝置：  
  
-   [管理的裝置使用儀表板](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [登入至特定網路電腦權限指派帳號](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [移除伺服器的電腦](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [群組原則設定資料夾重新導向和安全性](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [使用遠端桌面工作階段連上網路的電腦](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [檢視電腦屬性](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_8)  
  
##  <a name="BKMK_1"></a>管理的裝置使用儀表板  
 Windows Server Essentials 可讓您可以使用 Windows Server Essentials 儀表板執行一般管理工作。 **裝置**頁面上的儀表板可提供動作：  
  
-   網路的電腦清單會顯示：  
  
    -   電腦的名稱  
  
    -   狀態的電腦，其中一個**Online**或**離線**  
  
    -   電腦描述  
  
    -   備份電腦的狀態  
  
    -   更新電腦的狀態  
  
    -   電腦的安全狀態  
  
    -   警示狀態的電腦  
  
    -   適用於電腦的群組原則資訊  
  
-   選取電腦的其他資訊的詳細資料窗格  
  
-   包含裝置管理工作，例如檢視電腦屬性和通知，設定備份的電腦，以及從備份還原的檔案和資料夾的一組工作] 窗格  
  
#### <a name="to-view-the-status-of-network-computers"></a>若要檢視的狀態的電腦  
  
1.  打開 Windows Server Essentials 儀表板。  
  
2.  在瀏覽列中，按一下 [**裝置**。  
  
3.  在網路清單窗格中檢視所有的電腦狀態。  
  
 下表描述各種不同的電腦和備份可在 Windows Server Essentials 儀表板中的工作。 一些的工作電腦特定且都是只顯示當您在清單中選取 [電腦。  
  
### <a name="computer-tasks-in-the-dashboard"></a>電腦儀表板中的工作  
  
|工作的名稱|描述|  
|---------------|-----------------|  
|檢視電腦屬性|顯示一般資訊用於所選取的電腦，並讓您檢視的電腦備份的詳細資料。|  
|設定此電腦的備份|設定備份精靈會執行。|  
|自訂備份的電腦|開啟備份] 功能表，您可以選取電腦的備份設定以進行變更。|  
|備份電腦的 [開始]|從開始選取電腦的備份。|  
|停止備份的電腦|停止選取電腦的備份。|  
|還原的檔案或資料夾的電腦|執行還原的檔案和資料夾精靈，可讓您要還原特定的檔案、資料夾或磁碟機。|  
|電腦檢視警示|顯示重大和其他資訊警示，並讓您有可能可以修正動作。|  
|遠端桌面到電腦|開啟遠端桌面連接至所選取的電腦。|  
|移除電腦|電腦精靈，分離將電腦從 Windows Server Essentials 儀表板執行移除。|  
|自訂備份的電腦和檔案歷史設定|開啟備份設定頁面，，您可以變更 client 檔案歷史設定與備份排程電腦。|  
|如何連接伺服器電腦？|開啟協助主題描述的步驟執行，才能將電腦加入該網路。|  
|實作群組原則|適用於 Windows 8 和 Windows 7 的電腦所加入網域的原則設定。|  
  
##  <a name="BKMK_2"></a>登入至特定網路電腦權限指派帳號  
 讓使用者可以登入只有特定網路電腦從遠端位置存取 Windows Server Essentials 網路時，您可以權限指派給使用者帳號。  
  
#### <a name="to-change-the-computer-access-for-a-user-account"></a>若要變更使用者 account 電腦存取  
  
1.  打開 Windows Server Essentials 儀表板。  
  
2.  在瀏覽列中，按一下 [**使用者**。  
  
3.  在清單中的使用者帳號，選取您想要變更使用者 account。  
  
4.  在**< 使用者 Account\ > 工作**窗格中，按**檢視 account 屬性**。 **屬性**頁面上的使用者 account 顯示。  
  
5.  在**電腦存取**索引標籤上，選取 [，這位使用者可以存取遠端電腦上，然後按一下 [電腦] **[確定]**。  
  
##  <a name="BKMK_3"></a>移除伺服器的電腦  
 當您的電腦移除使用儀表板執行 Windows Server Essentials 的伺服器時，不再是由伺服器管理。 如此一來，伺服器會停止建立備份的電腦或其健康監視之後從網路移除它。  
  
> [!NOTE]
>  伺服器從電腦移除的網路不會中斷電腦。 電腦仍可在相同的方式它之前連接伺服器可能會存取網路上的資源。 若要防止電腦存取伺服器資源，並將它拔除從伺服器，您必須移除電腦的網域。 此外，伺服器從電腦移除不會自動解除安裝的連接器軟體或 Launchpad 的電腦上已停用。 您必須手動從電腦移除的連接器軟體。 如需詳細資訊，查看 [] 區段中解除安裝的連接器軟體[連接](../use/Get-Connected-in-Windows-Server-Essentials.md)。  
  
#### <a name="to-remove-a-computer-from-the-network-by-using-the-dashboard"></a>若要使用儀表板中移除網路的電腦  
  
1.  打開 Windows Server Essentials 儀表板。  
  
2.  在瀏覽列中，按一下 [**裝置**索引標籤。  
  
3.  在清單中的電腦，以滑鼠右鍵按一下您想要移除的網路，然後按一下 [電腦**電腦移除**。  
  
##  <a name="BKMK_5"></a>群組原則設定資料夾重新導向和安全性  
 您可以設定群組原則，並將它在 Windows Server Essentials 網路中電腦部署使用 Windows Server Essentials 儀表板。 在 Windows Server Essentials 的群組原則包含資料夾重新導向和安全性設定，Windows Update 的影響，Windows Defender 及網路防火牆。  
  
#### <a name="to-configure-group-policy-in-windows-server-essentials"></a>若要在 Windows Server Essentials 設定群組原則  
  
1.  打開 Windows Server Essentials 儀表板。  
  
2.  在瀏覽列中，按一下 [**裝置**。  
  
3.  針對 Windows Server Essentials：在全球**使用者工作**窗格中，按**實作群組原則**。  
  
     針對 Windows Server Essentials：在全球**裝置工作**窗格中，按**實作群組原則**。  
  
4.  開啟實作群組原則精靈。  
  
5.  在**讓資料夾重新導向群組原則**頁面上的精靈中，您可以選擇您想要重新導向使用者資料夾。  
  
6.  在**支援的安全性原則設定**頁面上的精靈中，您可以選擇群組原則設定可讓**Windows Update**、**Windows Defender**，和**網路防火牆**。  
  
7.  按一下**完成**來實作的群組原則設定。  
  
##  <a name="BKMK_7"></a>使用遠端桌面工作階段連上網路的電腦  
 遠端存取您的 Windows Server Essentials 網路電腦當您離開您的 office 時，使用您的網頁瀏覽器來登入您的組織 s 遠端網站存取的網站，並在**電腦**索引標籤上，按一下 [電腦名稱。  
  
 **狀態**欄顯示您，您可以連接到電腦上您的網路，並可以包含下列值：  
  
-   **可使用**  
  
     電腦已，並可連接遠端。 即使您會看到這個狀態，您仍然無法如果步驟 8 封鎖連接到這部電腦連接。  
  
-   **離線或休眠**  
  
     電腦已關閉或處於睡眠或休眠模式。 如果電腦是離線或休眠狀態即時更新，讓您知道電腦時使用。  
  
-   **不支援的作業系統**  
  
     遠端桌面不支援的作業系統，在電腦上。 可能需要 6 小時的時間來更新伺服器上，如果有一項變更此狀態。  
  
-   **連接已停用**  
  
     電腦連接可能被阻擋防火牆，或在電腦上或群組原則來停用遠端桌面。 可能需要 6 小時的時間來更新伺服器上，如果有一項變更此狀態。  
  
##  <a name="BKMK_8"></a>檢視電腦屬性  
 **裝置**Windows Server Essentials 儀表板的區段會顯示電腦的清單。 清單也提供每一台電腦的其他資訊。  
  
#### <a name="to-view-a-list-of-computers"></a>若要檢視的電腦清單  
  
1.  打開 Windows Server Essentials 儀表板。  
  
2.  在主要瀏覽列中，按一下 [**裝置**。  
  
3.  儀表板顯示目前的電腦清單。  
  
#### <a name="to-view-or-change-properties-for-a-computer"></a>若要檢視，或變更電腦的屬性  
  
1.  在的電腦清單中，選取您要檢視，或變更屬性的 account。  
  
2.  在**< Computername\ > 工作**窗格中，按**檢視電腦屬性**。 **屬性**頁面上的電腦會出現。  
  
3.  按一下 [索引標籤，以顯示該電腦的屬性。  
  
4.  若要儲存對電腦屬性的任何變更，請按一下**套用]**。  
  
## <a name="see-also"></a>也了  
  
-   [管理網路遠端存取](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [使用遠端存取](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [管理使用者帳號使用儀表板](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
