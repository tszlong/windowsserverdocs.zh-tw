---
title: 建立存取原則
description: 本主題是在 Windows Server 2016 的 IP 位址管理 (IPAM) 管理組節目表的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 854bd064-2f86-4678-a940-a04b3e48ae10
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ac8229952c7e038f9af8fc4f9287b1821db887ac
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="create-an-access-policy"></a>建立存取原則

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用此主題中 IPAM client 主機建立存取原則。  
  
資格在**系統管理員**，或相當於，才能執行此程序最小值。  
  
> [!NOTE]  
> 您可以在 Active Directory 中建立特定的使用者或群組使用者存取原則。 當您建立存取原則時，您必須選取建 IPAM 角色或建立自訂角色。 適用於自訂角色詳細資訊，請查看[進行存取控制建立使用者角色](../../technologies/ipam/Create-a-User-Role-for-Access-Control.md)。  
  
### <a name="to-create-an-access-policy"></a>若要建立存取原則  
  
1.  在伺服器管理員中，按一下**IPAM**。 顯示 IPAM client 主機。  
  
2.  在瀏覽窗格中，按一下**存取控制**。 較低的瀏覽窗格中，以滑鼠右鍵按一下**存取原則**，然後按**新增存取原則**。  
  
    ![新增存取原則](../../media/Create-an-Access-Policy/ipam_CreateAP_01.jpg)  
  
3.  **新增存取原則**對話方塊。 在**的使用者設定**，按一下 [**新增]**。  
  
    ![新增存取原則](../../media/Create-an-Access-Policy/ipam_CreateAP_02.jpg)  
  
4.  **選取使用者或群組**對話方塊。 按一下**位置**。  
  
    ![使用者或群組的位置](../../media/Create-an-Access-Policy/ipam_CreateAP_03.jpg)  
  
5.  **位置**對話方塊。 瀏覽至含有帳號位置，選取位置，然後按一下**[確定]**。 **位置**關閉對話方塊。  
  
    ![選取的位置](../../media/Create-an-Access-Policy/ipam_CreateAP_04.jpg)  
  
6.  在**選取使用者或群組**對話方塊中，在**輸入物件名稱來選取 [**，輸入您要建立存取原則的使用者 account 名稱。 按一下**[確定]**。  
  
7.  在**新增存取原則**，請在**使用者設定**、**使用者別名**原則套用到帳號現在也包含。 在**存取設定**，按一下 [**新增]**。  
  
    ![新增存取設定](../../media/Create-an-Access-Policy/ipam_CreateAP_05.jpg)  
  
8.  在**新增存取原則**，**存取設定**變更為**新的設定**。  
  
    ![對話方塊中名稱變更為新的設定](../../media/Create-an-Access-Policy/ipam_CreateAP_06.jpg)  
  
9. 按一下**選擇角色**，展開清單中的角色。 選取一個建的角色，或如果您已建立新的角色，選取其中一個您所建立的角色。 例如，如果您建立套用到使用者 IPAMSrv 的角色，按一下**IPAMSrv**。  
  
    ![選取 [角色](../../media/Create-an-Access-Policy/ipam_CreateAP_07.jpg)  
  
10. 按一下**[新增設定**。  
  
    ![新增新的設定](../../media/Create-an-Access-Policy/ipam_CreateAP_08.jpg)  
  
11. 的角色被新增到存取原則。 建立其他存取原則，請按**套用]**，然後針對您想要建立的每個原則重複這些步驟。 如果您不想要建立額外的原則，請按一下**[確定]**。  
  
    ![按一下 [套用] 或 [確定]](../../media/Create-an-Access-Policy/ipam_CreateAP_09.jpg)  
  
12. 確認 IPAM client 主機顯示窗格中，會建立新的存取原則。  
  
    ![檢視新存取原則](../../media/Create-an-Access-Policy/ipam_CreateAP_09a.jpg)  
  
## <a name="see-also"></a>也了  
[以角色為基礎存取控制](Role-based-Access-Control.md)  
[管理 IPAM](Manage-IPAM.md)  
  


