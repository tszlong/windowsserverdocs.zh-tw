---
title: 管理 Windows Server Essentials 中的裝置
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: f5fe1088-ebe7-4799-a47d-075b0048dea1
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f05d156447cddf9afbbe20fddc4b590459a37ae7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852811"
---
# <a name="manage-devices-in-windows-server-essentials"></a>管理 Windows Server Essentials 中的裝置

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials
 
 下列各節討論伺服器的裝置管理功能，並說明如何在您的網路上設定及使用裝置：  
  
-   [使用儀表板管理裝置](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [指派使用者帳戶許可權以登入特定網路電腦](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [從伺服器移除電腦](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [設定資料夾重新導向和安全性的群組原則設定](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [使用遠端桌面會話連接到網路電腦](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [View computer properties](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_8)  
  
##  <a name="manage-devices-by-using-the-dashboard"></a><a name="BKMK_1"></a>使用儀表板管理裝置  
 Windows Server Essentials 可讓您使用 [Windows Server Essentials 儀表板] 來執行一般系統管理工作。 [儀表板] 的 [裝置] 頁面提供下列各項：  
  
-   一份網路電腦清單，當中顯示：  
  
    -   電腦的名稱  
  
    -   電腦的狀態 ([線上] 或 [離線])  
  
    -   電腦描述  
  
    -   電腦的備份狀態  
  
    -   電腦的更新狀態  
  
    -   電腦的安全性狀態  
  
    -   電腦的警示狀態  
  
    -   電腦的「群組原則」資訊  
  
-   一個含有所選電腦之其他相關資訊的詳細資料窗格  
  
-   一個包含一組裝置系統管理工作 (例如檢視電腦內容和警示、設定電腦備份，以及從備份還原檔案和資料夾) 的工作窗格  
  
#### <a name="to-view-the-status-of-network-computers"></a>檢視網路電腦的狀態  
  
1. 開啟 [Windows Server Essentials 儀表板]。  
  
2. 在瀏覽列上，按一下 [裝置]。  
  
3. 在清單窗格中檢視網路中所有電腦的狀態。  
  
   下表說明 [Windows Server Essentials 儀表板] 上可用的各種電腦和備份工作。 部分工作是電腦特定的工作，只有當您選取清單中的某部電腦時才能看見。  
  
### <a name="computer-tasks-in-the-dashboard"></a>儀表板中的電腦工作  
  
|工作名稱|描述|  
|---------------|-----------------|  
|檢視電腦內容|會顯示所選電腦的一般資訊，並可讓您檢視電腦備份的詳細資料。|  
|設定這部電腦的備份|會執行 [設定備份精靈]。|  
|自訂電腦的備份|會開啟備份內容，您可以從該處對所選電腦的備份設定進行變更。|  
|開始伺服器的備份|會開始所選電腦的備份。|  
|停止電腦的備份|會停止所選電腦的備份。|  
|還原電腦的檔案或資料夾|會執行 [還原檔案和資料夾精靈]，可讓您還原特定檔案、資料夾或磁碟機。|  
|檢視電腦的警示|會顯示重要警示及其他資訊警示，並可讓您儘可能採取修正動作。|  
|連線至電腦的遠端桌面|會開啟連至所選電腦的「遠端桌面連線」。|  
|移除電腦|會執行 [移除電腦精靈]，這會將電腦與 [Windows Server Essentials 儀表板] 中斷連結。|  
|自訂電腦備份和「檔案歷程記錄」設定|會開啟備份設定頁面，您可以從該處對用戶端電腦的備份排程和「檔案歷程記錄」設定進行變更。|  
|如何將電腦連線到伺服器？|會開啟說明主題，說明將電腦加入網路所需執行的步驟。|  
|實作群組原則|會將原則設定套用到加入網域的 Windows 8 和 Windows 7 電腦。|  
  
##  <a name="assign-user-accounts-permission-to-log-on-to-specific-network-computers"></a><a name="BKMK_2"></a>指派使用者帳戶許可權以登入特定網路電腦  
 您可以指派權限給使用者帳戶，讓使用者在從遠端位置存取 Windows Server Essentials 網路時，只能登入特定的網路電腦。  
  
#### <a name="to-change-the-computer-access-for-a-user-account"></a>變更使用者帳戶的電腦存取權  
  
1.  開啟 [Windows Server Essentials 儀表板]。  
  
2.  在瀏覽列上，按一下 [使用者]。  
  
3.  在使用者帳戶清單中，選取您想要變更的使用者帳戶。  
  
4.  在 **< 的使用者帳戶\>** 工作 窗格中，按一下**查看帳戶屬性**。 使用者帳戶的 [內容] 頁面隨即出現。  
  
5.  在 [電腦存取] 索引標籤上，選取這個使用者可以從遠端存取的電腦，然後按一下 [確定]。  
  
##  <a name="remove-a-computer-from-the-server"></a><a name="BKMK_3"></a>從伺服器移除電腦  
 當您使用 [儀表板] 將電腦從執行 Windows Server Essentials 的伺服器中移除時，它就不再受伺服器管理。 如此一來，在將它從網路中移除之後，伺服器就會停止建立電腦備份或監視它的健康情況。  
  
> [!NOTE]
>  將電腦從伺服器中移除並不會中斷電腦與網路的連線。 電腦仍然可以存取網路上的資源，就像在它連線到伺服器之前一樣。 若要防止電腦存取伺服器資源並中斷其與伺服器的連線，您必須將電腦從網域中移除。 此外，將電腦從伺服器中移除並不會自動將「連線程式」軟體或「啟動列」從被移除的電腦中解除安裝。 您必須手動從電腦移除「連線程式」軟體。 如需詳細資訊，請參閱在[取得連線](../use/Get-Connected-in-Windows-Server-Essentials.md)中卸載連接器軟體一節。  
  
#### <a name="to-remove-a-computer-from-the-network-by-using-the-dashboard"></a>使用儀表板將電腦從網路中移除  
  
1.  開啟 [Windows Server Essentials 儀表板]。  
  
2.  在瀏覽列中，按一下 [裝置] 索引標籤。  
  
3.  在電腦清單中，於您想要從網路中移除的電腦上按一下滑鼠右鍵，然後按一下 [移除電腦]。  
  
##  <a name="configure-group-policy-settings-for-folder-redirection-and-security"></a><a name="BKMK_5"></a>設定資料夾重新導向和安全性的群組原則設定  
 您可以使用 [Windows Server Essentials 儀表板] 來設定「群組原則」並將它部署到 Windows Server Essentials 網路中的電腦。 Windows Server Essentials 中的「群組原則」包括會影響 Windows Update、Windows Defender 及網路防火牆的資料夾重新導向和安全性設定。  
  
#### <a name="to-configure-group-policy-in-windows-server-essentials"></a>設定 Windows Server Essentials 中的群組原則  
  
1.  開啟 [Windows Server Essentials 儀表板]。  
  
2.  在瀏覽列上，按一下 [裝置]。  
  
3.  針對 Windows Server Essentials：在全域 [**使用者**工作] 窗格中，按一下 [**執行群組原則**]。  
  
     針對 Windows Server Essentials：在 [全域**裝置**工作] 窗格中，按一下 [**執行群組原則**]。  
  
4.  [實作群組原則精靈] 隨即開啟。  
  
5.  在精靈的 [啟用資料夾重新導向群組原則] 頁面上，您可以選擇您想要重新導向的使用者資料夾。  
  
6.  在精靈的 [啟用安全性原則設定] 頁面上，您可以選擇啟用 [Windows Update]、[Windows Defender] 及 [網路防火牆] 的「群組原則」設定。  
  
7.  按一下 [完成] 來實作「群組原則」設定。  
  
##  <a name="connect-to-a-network-computer-by-using-a-remote-desktop-session"></a><a name="BKMK_7"></a>使用遠端桌面會話連接到網路電腦  
 當您不在辦公室時，若要從遠端存取您的 Windows Server Essentials 網路電腦，請使用您的網頁瀏覽器登入組織的遠端 Web 存取網站，然後在 [**電腦**] 索引標籤上，按一下電腦的名稱。  
  
 [狀態] 欄會顯示您是否可以連線到網路上的某部電腦，並且可包含下列值：  
  
-   **有**  
  
     電腦已開啟並且可供遠端連線使用。 當有協力廠商防火牆封鎖連線時，即使您看見這個狀態，仍然有可能無法連線到這部電腦。  
  
-   **離線或睡眠中**  
  
     電腦已關閉，或是處於 [睡眠] 或 [休眠] 模式。 如果電腦已離線或正在睡眠，系統會即時更新狀態，讓您知道電腦何時可供使用。  
  
-   **不支援的作業系統**  
  
     電腦上的作業系統不支援遠端桌面。 如果這個狀態有變更，可能需要多達 6 小時的時間才會在伺服器上更新。  
  
-   **連接已停用**  
  
     電腦連線被防火牆封鎖，或遠端桌面在電腦上被停用或被「群組原則」停用。 如果這個狀態有變更，可能需要多達 6 小時的時間才會在伺服器上更新。  
  
##  <a name="view-computer-properties"></a><a name="BKMK_8"></a>View computer properties  
 [Windows Server Essentials 儀表板] 的 [裝置] 區段會顯示網路電腦的清單。 這份清單也會提供每部電腦的其他相關資訊。  
  
#### <a name="to-view-a-list-of-computers"></a>檢視電腦清單  
  
1.  開啟 [Windows Server Essentials 儀表板]。  
  
2.  在主瀏覽列上，按一下 [裝置]。  
  
3.  [儀表板] 會顯示目前的電腦清單。  
  
#### <a name="to-view-or-change-properties-for-a-computer"></a>檢視或變更電腦的內容  
  
1.  在電腦清單中，選取您想要檢視或變更內容的帳戶。  
  
2.  在 [ **< Computername\>** 工作] 窗格中，按一下 **[查看電腦**內容]。 電腦的 [內容] 頁面隨即出現。  
  
3.  按一下索引標籤以顯示該電腦的內容。  
  
4.  若要儲存您對電腦內容所做的任何變更，請按一下 [套用]。  
  
## <a name="see-also"></a>另請參閱  
  
-   [管理遠端 Web 存取](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [使用遠端 Web 存取](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [使用儀表板管理使用者帳戶](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
