---
title: 使用伺服器管理員安裝網路控制卡伺服器角色
description: 本主題提供有關如何使用 Windows Server 2016 中的伺服器管理員安裝網路控制卡伺服器角色的指示。
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 3a6e4352-ff62-4290-b8a4-5c83740070fc
ms.author: anpaul
author: AnirbanPaul
ms.openlocfilehash: a93d737b80e41fbc4401105a9f72426c3a73fa7d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859671"
---
# <a name="install-the-network-controller-server-role-using-server-manager"></a>使用伺服器管理員安裝網路控制卡伺服器角色

>適用於：Windows Server (半年通道)、Windows Server 2016

本主題提供有關如何使用伺服器管理員安裝網路控制卡伺服器角色的指示。

>[!IMPORTANT]
>請勿在實體主機上部署網路控制卡伺服器角色。 若要部署網路控制站，您必須在 hyper-v 虛擬機器上安裝網路控制站伺服器角色，\(安裝在 Hyper-v 主機上的 VM\)。 在三部不同的\-Hyper-v 主機上的 Vm 上安裝網路控制站之後，您必須使用 Windows PowerShell 命令**NetworkControllerServer**，將主機新增至網路控制站，以啟用軟體定義網路的超\-主機 \(SDN\)。 如此一來，您就可以讓 SDN 軟體 Load Balancer 運作。 如需詳細資訊，請參閱[NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver)。
  
安裝網路控制站之後，您必須使用 Windows PowerShell 命令來進行其他網路控制站設定。 如需詳細資訊，請參閱[使用 Windows PowerShell 部署網路控制](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)站。  
  
### <a name="to-install-network-controller"></a>安裝網路控制卡  
  
1.  在 [伺服器管理員] 中，按一下 [**管理**]，然後按一下 [**新增角色及功能**]。 [新增角色及功能] wizard 隨即開啟。 按 [下一步]。  
  
2.  在 [**選取安裝類型**] 中，保留預設設定，然後按 **[下一步]** 。  
  
3.  在 [**選取目的地伺服器**] 中，選擇您要安裝網路控制卡的伺服器，然後按 **[下一步]** 。  
  
4.  在 [**選取伺服器角色**] 的 [**角色**] 中，按一下 [**網路控制**站]。  
  
    ![網路控制卡伺服器角色](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)  
  
5.  [**新增網路控制器所需的功能**] 對話方塊隨即開啟。 按一下 **[新增功能]** 。  
  
    ![新增網路控制卡的功能](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_06.jpg)  
  
6.  在 [**選取伺服器角色**] 中，按 **[下一步]** 。  
  
    ![按 [下一步]](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)  
  
7.  在 [**選取功能**] 中，按 **[下一步]** 。  
  
8.  在**網路控制**卡中按 **[下一步]** 。  
  
9. 在 [**確認安裝選項**] 中，檢查您的選擇。 安裝網路控制卡時，需要您在執行嚮導之後重新開機電腦。 因此，按一下 **[必要時自動重新開機目的地伺服器**]。 [**新增角色及功能嚮導**] 對話方塊隨即開啟。 按一下 [是]。  
  
    ![新增角色與功能精靈](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_11.jpg)  
  
10. 在 [確認安裝選項] 中，按一下 [安裝]。  
  
11. 網路控制卡伺服器角色會安裝在目的地伺服器上，然後伺服器會重新開機。  
  
12. 電腦重新開機之後，請透過查看伺服器管理員來登入電腦並確認網路控制站安裝。  
  
    ![伺服器管理員](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/nc_013.jpg)  
  
## <a name="see-also"></a>另請參閱  
[網路控制卡](Network-Controller.md)  
  


