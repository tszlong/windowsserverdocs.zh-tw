---
title: "管理 Windows Server Essentials 的 VPN"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cc2b264a-b9a8-4114-9f7b-8604f77096e5
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 08a08a13d696371420bdfdf89f54320c787636b0
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="manage-vpn-in-windows-server-essentials"></a>管理 Windows Server Essentials 的 VPN

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集 
  
 連接私人網路 virtual (VPN) 可讓使用者在家或外出工作使用的基礎結構提供公用網路，例如網際網路存取私人網路上的伺服器。 若要使用 VPN 存取伺服器資源，您必須完成以下：  
  
-   [遠端存取伺服器上的讓 VPN](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [網路使用者的設定 VPN 權限](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [連接至伺服器 client 電腦](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_Connect)  
  
-   [使用 Windows Server Essentials 連接的 VPN](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_3)  
  
##  <a name="BKMK_1"></a>遠端存取伺服器上的讓 VPN  
 完成設定，可讓遠端存取的 Windows Server Essentials VPN 下列程序。  
  
#### <a name="to-enable-vpn-in-windows-server-essentials"></a>若要讓 Windows Server Essentials 的 VPN  
  
1.  打開儀表板。  
  
2.  按一下**設定**，然後按一下 [**隨處存取**索引標籤。  
  
3.  按一下**設定**。 設定任何地方存取精靈會出現。  
  
4.  在**選擇隨處存取功能，可讓**頁面上，選取**Virtual 私人網路**核取方塊。  
  
5.  請依照下列指示完成精靈。  
  
##  <a name="BKMK_2"></a>網路使用者的設定 VPN 權限  
 您可以連接到 Windows Server Essentials 並存取所有資源是儲存在伺服器上使用 VPN。 這是您的電腦與網路帳號，可以用來連接的 VPN 連接到 Windows Server Essentials 伺服器裝載設定使用 client 的尤其是實用。 Windows Server Essentials 裝載伺服器上的所有新建立的使用者帳號必須使用 VPN 第一次登入 client 的電腦。  
  
#### <a name="to-set-vpn-permissions-for-network-users"></a>若要設定網路使用者的 VPN 權限  
  
1.  打開儀表板。  
  
2.  在瀏覽列中，按一下 [**使用者**。  
  
3.  在清單中的使用者帳號，選取您想要給予權限來遠端存取桌面帳號。  
  
4.  在**< 使用者 Account\ > 工作**窗格中，按**屬性**。  
  
5.  在**< 使用者 Account\ > 屬性**，按一下 [**隨處存取**索引標籤。  
  
6.  在**隨處存取**索引標籤，讓使用者使用 VPN、 連接到伺服器選取**允許 Virtual 私人網路 (VPN)**核取方塊。  
  
7.  按一下**套用**，然後按**[確定]**。  
  
##  <a name="BKMK_Connect"></a>連接至伺服器 client 電腦  
 VPN 會支援執行 Windows Server Essentials 遠端存取伺服器上之後，您可以使用 VPN 連接連接到並存取所有您儲存在伺服器的資源。 不過，您必須伺服器第一次連接的電腦。 當您使用 「 連接我的電腦伺服器精靈電腦連接到伺服器時，VPN 上網也會自動上，可用來存取伺服器資源在家或外出時。 如需有關您的電腦連接伺服器逐步指示，請查看[電腦連接到伺服器](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)。  
  
##  <a name="BKMK_3"></a>使用 Windows Server Essentials 連接的 VPN  
 如果您的電腦與網路帳號，可以用來連接到執行 Windows Server Essentials 透過 VPN 連接，所有的新建立的使用者帳號裝載的伺服器設定使用 client 裝載的伺服器必須第一次登入 client 電腦使用 VPN。 完成 client 電腦連接到伺服器從下列程序。  
  
#### <a name="to-use-vpn-to-remotely-access-server-resources"></a>若要使用 VPN 遠端存取伺服器資源  
  
1.  按下 Ctrl + Alt + Delete client 電腦上。  
  
2.  按一下**切換使用者**畫面上。  
  
3.  按一下 [網路登入畫面右下角的圖示。  
  
4.  使用您的網路使用者名稱和密碼登入 Windows Server Essentials 的網路。  
  
## <a name="see-also"></a>也了  
  
-   [從遠端使用](../use/Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [管理隨時隨地存取](Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)
