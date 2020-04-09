---
title: 管理 Windows Server Essentials 中的 VPN
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: cc2b264a-b9a8-4114-9f7b-8604f77096e5
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 38c0b0e7bfc29f0b7717cd18a103ae068bbb259b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852691"
---
# <a name="manage-vpn-in-windows-server-essentials"></a>管理 Windows Server Essentials 中的 VPN

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials 
  
 虛擬私人網路 (VPN) 連線可讓在家中或差旅中工作的使用者，使用公用網路 (例如網際網路) 所提供的基礎結構來存取私人網路上的伺服器。 若要使用 VPN 存取伺服器資源，您必須完成以下各項：  
  
-   [在伺服器上啟用 VPN 以進行遠端存取](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [設定網路使用者的 VPN 許可權](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [將用戶端電腦連線到伺服器](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_Connect)  
  
-   [使用 VPN 連接到 Windows Server Essentials](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_3)  
  
##  <a name="enable-vpn-for-remote-access-on-the-server"></a><a name="BKMK_1"></a>在伺服器上啟用 VPN 以進行遠端存取  
 請完成以下程序，在 Windows Server Essentials 設定 VPN 以啟用遠端存取。  
  
#### <a name="to-enable-vpn-in-windows-server-essentials"></a>啟用 Windows Server Essentials 中的 VPN  
  
1.  開啟 [儀表板]。  
  
2.  按一下 [設定]，然後按一下 [隨處存取] 索引標籤。  
  
3.  按一下 **[設定]** 。 此時會出現 [設定隨處存取精靈]。  
  
4.  在 [選擇要啟用的隨處存取功能] 頁面，選取 [虛擬私人網路] 核取方塊。  
  
5.  遵循指示以完成精靈。  
  
##  <a name="set-vpn-permissions-for-network-users"></a><a name="BKMK_2"></a>設定網路使用者的 VPN 許可權  
 您可以使用 VPN 來連線到 Windows Server Essentials，並存取您所有儲存在伺服器上的資源。 當您有已設定網路帳戶的用戶端電腦且那些帳戶可透過 VPN 連線來連線到託管的 Windows Server Essentials 伺服器時，這會特別有用。 所有在託管的 Windows Server Essentials 伺服器上新建立的使用者帳戶，在第一次登入用戶端電腦時都必須使用 VPN。  
  
#### <a name="to-set-vpn-permissions-for-network-users"></a>變更網路使用者的 VPN 權限  
  
1.  開啟 [儀表板]。  
  
2.  在瀏覽列上，按一下 [使用者]。  
  
3.  在使用者帳戶清單中，選取您想要授與桌面遠端存取權限的使用者帳戶。  
  
4.  在 [ **< 的使用者帳戶\>** 工作] 窗格中，按一下 [**屬性**]。  
  
5.  在 [ **< 使用者帳戶\>** 內容] 中，按一下 [**隨處存取**] 索引標籤。  
  
6.  若要允許使用者使用 VPN 來連線到伺服器，請在 [隨處存取] 索引標籤上，選取 [允許虛擬私人網路 (VPN)]  核取方塊。  
  
7.  按一下 [套用]，然後按一下 [確定]。  
  
##  <a name="connect-client-computers-to-the-server"></a><a name="BKMK_Connect"></a>將用戶端電腦連線到伺服器  
 在執行 Windows Server Essentials 的伺服器上啟用 VPN 用於遠端存取後，您就可以使用 VPN 連線連線到伺服器，並使用伺服器上儲存的所有資源。 不過，您必須先將電腦連線到伺服器。 當您使用 [將我的電腦連線到伺服器精靈] 將電腦連線到伺服器時，會自動在用戶端電腦上產生 VPN 網路連線，您可以在家中或旅途中工作時使用這個連線存取伺服器資源。 如需將電腦連線到伺服器的逐步指示，請參閱 [Connect computers to the server](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)。  
  
##  <a name="use-vpn-to-connect-to-windows-server-essentials"></a><a name="BKMK_3"></a>使用 VPN 連接到 Windows Server Essentials  
 如果您有已設定網路帳戶的用戶端電腦，並且那些帳戶可透過 VPN 連線來連線到執行 Windows Server Essentials 的託管伺服器，則所有在該託管伺服器上新建立的使用者帳戶在第一次登入該用戶端電腦時都必須使用 VPN。 從連線到伺服器的用戶端電腦完成以下程序。  
  
#### <a name="to-use-vpn-to-remotely-access-server-resources"></a>使用 VPN 從遠端存取伺服器資源  
  
1.  在用戶端電腦上按 Ctrl + Alt + Delete。  
  
2.  在登入畫面按一下 [切換使用者]。  
  
3.  按一下畫面右下角的網路登入圖示。  
  
4.  使用您的網路使用者名稱和密碼來登入 Windows Server Essentials 網路。  
  
## <a name="see-also"></a>另請參閱  
  
-   [遠端工作](../use/Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [管理隨處存取](Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)
