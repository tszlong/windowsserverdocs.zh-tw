---
title: 安裝網路控制站伺服器角色使用伺服器管理員
description: 本主題提供有關如何使用 Windows Server 2016 中的 伺服器管理員安裝網路控制卡伺服器角色的指示。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 3a6e4352-ff62-4290-b8a4-5c83740070fc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 699e2ca5c4ec33099d0ad948523b6f587ad118e4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859059"
---
# <a name="install-the-network-controller-server-role-using-server-manager"></a>安裝網路控制站伺服器角色使用伺服器管理員

>適用於：Windows Server （半年通道），Windows Server 2016

本主題提供有關如何使用伺服器管理員安裝網路控制卡伺服器角色的指示。

>[!IMPORTANT]
>不會部署在實體主機上的網路控制卡伺服器角色。 若要部署網路控制站，您必須為 HYPER-V 虛擬機器上安裝網路控制卡伺服器角色\(VM\)安裝於 HYPER-V 主機。 在三個不同的 Hyper-v Vm 上安裝網路控制站之後\-Hyperv 主機，您必須啟用 Hyper-v\-的軟體定義網路的 HYPER-V 主機\(SDN\)加上要使用網路控制站的主機Windows PowerShell 命令**新增 NetworkControllerServer**。 如此一來，您將使 SDN 軟體負載平衡器，函式。 如需詳細資訊，請參閱 <<c0> [ 新增 NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver)。
  
安裝網路控制站之後，您必須使用 Windows PowerShell 命令進行其他的網路控制站設定。 如需詳細資訊，請參閱 <<c0> [ 部署網路控制站使用 Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)。  
  
### <a name="to-install-network-controller"></a>若要安裝網路控制站  
  
1.  在 [伺服器管理員] 中，按一下 [**管理**]，然後按一下 [**新增角色及功能**]。 [新增角色及功能精靈] 隨即開啟。 按一下 [下一步] 。  
  
2.  在 [**選取安裝類型**，保留預設值，然後按一下**下一步]**。  
  
3.  在 **選取目的地伺服器**，選擇您想要用來安裝網路控制站，然後按一下的伺服器**下一步**。  
  
4.  在 **選取伺服器角色**，請在**角色**，按一下 **網路控制卡**。  
  
    ![網路控制站伺服器角色](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)  
  
5.  **新增功能所需的網路控制卡**對話方塊隨即開啟。 按一下 **將功能加入**。  
  
    ![將功能加入網路控制站](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_06.jpg)  
  
6.  在 [**選取伺服器角色**，按一下**下一步]**。  
  
    ![按 [下一步]](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)  
  
7.  在 [**選取的功能**，按一下**下一步]**。  
  
8.  在 **網路控制卡**按一下 **下一步**。  
  
9. 在 **確認安裝選項**，檢閱您的選擇。 安裝網路控制站必須執行精靈之後，重新啟動電腦。 基於這個原因，請按一下**需要時自動重新啟動目的地伺服器**。 **新增角色及功能精靈**對話方塊隨即開啟。 按一下 [ **是**]。  
  
    ![新增角色及功能精靈](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_11.jpg)  
  
10. 在 [確認安裝選項] 中，按一下 [安裝]。  
  
11. 網路控制卡伺服器角色安裝在目的地伺服器上，並再重新啟動伺服器。  
  
12. 在電腦重新啟動之後，登入電腦，並驗證網路控制站安裝伺服器管理員中檢視。  
  
    ![伺服器管理員](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/nc_013.jpg)  
  
## <a name="see-also"></a>另請參閱  
[網路控制站](Network-Controller.md)  
  


