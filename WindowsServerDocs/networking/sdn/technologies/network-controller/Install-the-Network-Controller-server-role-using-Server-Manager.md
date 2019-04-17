---
title: 安裝網路控制站伺服器角色使用伺服器管理員
description: 本主題提供有關如何使用 Windows Server 2016 伺服器管理員安裝網路控制站伺服器角色的指示操作。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 3a6e4352-ff62-4290-b8a4-5c83740070fc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 15cb1ef3bad7038cc97784504807b44b4920def6
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="install-the-network-controller-server-role-using-server-manager"></a>安裝網路控制站伺服器角色使用伺服器管理員

>適用於：Windows Server（以每年次管道）、Windows Server 2016

本主題提供有關如何使用伺服器管理員安裝網路控制站伺服器角色的指示操作。

>[!IMPORTANT]
>不要部署實體主機上的 Network Controller 伺服器角色。 若要部署 Network Controller，您必須安裝網路控制站伺服器角色 HYPER-V 一樣上 \(VM\) HYPER-V 主機上安裝。 有三種不同的 Hyper\ HYPER-V 主機上 Vm 上安裝 Network Controller 之後，您必須讓 Hyper\ HYPER-V 主機的網路軟體定義 \(SDN\) 加到使用 Windows PowerShell 命令 Network Controller 的主機**新-NetworkControllerServer**。 如此一來，您會讓 SDN 軟體負載平衡器函式。 如需詳細資訊，請查看[新-NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver)。
  
Network Controller 安裝之後，您必須使用 Windows PowerShell 命令的額外 Network Controller 的設定。 如需詳細資訊，請查看[使用 Windows PowerShell 部署 Network Controller](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)。  
  
### <a name="to-install-network-controller"></a>若要安裝網路控制器  
  
1.  在伺服器管理員中，按一下**管理**，然後按**新增角色與功能**。 開啟精靈新增角色與功能]。 按一下**下一步**。  
  
2.  在**選擇安裝類型**，讓 [設定預設值，按一下 [**下一步**。  
  
3.  在**選擇目的地伺服器**，選擇您要安裝網路控制器，請然後按一下 [的伺服器**下**。  
  
4.  在**選取伺服器角色**，請在**角色**，按一下 [ **Network Controller**。  
  
    ![網路控制站伺服器角色](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)  
  
5.  **新增所需的網路控制器功能**對話方塊。 按一下**[新增功能**。  
  
    ![Network Controller 新增功能](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_06.jpg)  
  
6.  在**選取伺服器角色**，按一下 [**下**。  
  
    ![按 [下一步]](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)  
  
7.  在**選擇功能**，按一下 [**下**。  
  
8.  在**Network Controller**按**下**。  
  
9. 在**確認安裝選項**，檢視您的選擇。 安裝 Network Controller 需要重新開機之後精靈會執行。 因此，按一下 [**必要時自動重新開機目的伺服器**。 **新增角色與功能精靈**對話方塊。 按一下**[是]**。  
  
    ![新增角色與精靈中的功能](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_11.jpg)  
  
10. 在**確認安裝選項**，按一下 [**安裝**。  
  
11. Network Controller 伺服器角色伺服器上安裝目的，並再重新開機伺服器。  
  
12. 電腦重新開機之後，登入電腦，來檢視伺服器管理員確認 Network Controller 安裝。  
  
    ![伺服器管理員](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/nc_013.jpg)  
  
## <a name="see-also"></a>也了  
[Network Controller](Network-Controller.md)  
  


