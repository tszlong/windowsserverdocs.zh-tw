---
title: 建立角色使用者存取控制
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
ms.assetid: ae6a42db-a104-401b-a8e6-b85c47d30b46
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fa0ed71d399ad638a648946952fe170d93f69ceb
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="create-a-user-role-for-access-control"></a>建立角色使用者存取控制

>適用於：Windows Server（以每年次管道）、Windows Server 2016

若要建立新的使用者存取控制角色 IPAM client 主控台中，您可以使用此主題。  
  
資格在**系統管理員**，或相當於，才能執行此程序最小值。  
  
> [!NOTE]  
> 您來建立角色之後，您可以建立給角色特定的使用者或群組 Active Directory 存取原則。 如需詳細資訊，請查看[建立存取原則](../../technologies/ipam/Create-an-Access-Policy.md)。  
  
### <a name="to-create-a-role"></a>來建立角色  
  
1.  在伺服器管理員中，按一下**IPAM**。 顯示 IPAM client 主機。  
  
2.  在瀏覽窗格中，按一下**存取控制**，在較低的瀏覽窗格中，按一下 [**角色**。  
  
    ![存取控制角色](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_01.jpg)  
  
3.  以滑鼠右鍵按一下**角色**，然後按一下 [ **[新增使用者角色**。  
  
    ![[新增使用者角色](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_02.jpg)  
  
4.  **[新增或編輯角色**對話方塊。 在**名稱**，輸入名稱，清除 [角色函式的角色。 例如，如果您想要建立的角色，可讓系統管理員，管理 DNS-SRV 資源記錄，您可能會名稱角色**IPAMSrv**。 如果需要然後向下捲動**作業**以找出您想要定義角色作業的類型。 針對此範例中，向下捲動**DNS 資源使用碼表進行的管理作業**。  
  
    ![DNS 資源使用碼表進行的管理作業](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_03.jpg)  
  
5.  展開**DNS 資源使用碼表進行的管理作業**，然後尋找**SRV 使用碼表進行操作**。  
  
    ![SRV 記錄作業](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_04.jpg)  
  
6.  展開，選取 [ **SRV 使用碼表進行操作**，然後按一下 [ **[確定]**。  
  
    ![選取 [SRV 使用碼表進行作業](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_05.jpg)  
  
7.  在 IPAM client 主控台中，按一下您剛建立的角色。 在**的詳細資料] 檢視中，**允許角色作業會顯示。  
  
    ![新的角色詳細資料](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_06.jpg)  
  
## <a name="see-also"></a>也了  
[以角色為基礎存取控制](Role-based-Access-Control.md)  
[管理 IPAM](Manage-IPAM.md)  
  


