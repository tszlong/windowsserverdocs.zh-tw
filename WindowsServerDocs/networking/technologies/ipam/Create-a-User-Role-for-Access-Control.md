---
title: 建立存取控制的使用者角色
description: 本主題是 Windows Server 2016 中的 IP 位址管理 (IPAM) 管理指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ae6a42db-a104-401b-a8e6-b85c47d30b46
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3798b074a0ca7e20602da7986fe6b54e81da5495
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284102"
---
# <a name="create-a-user-role-for-access-control"></a>建立存取控制的使用者角色

>適用於：Windows Server （半年通道），Windows Server 2016

在 IPAM 用戶端主控台中建立新的存取控制使用者角色，您可以使用本主題。  
  
若要執行此程序，至少需要 **Administrators** 的成員資格或同等權限。  
  
> [!NOTE]  
> 建立角色之後，您可以建立存取原則來將角色指派給特定使用者或 Active Directory 群組。 如需詳細資訊，請參閱 <<c0> [ 建立存取原則](../../technologies/ipam/Create-an-Access-Policy.md)。  
  
### <a name="to-create-a-role"></a>若要建立角色  
  
1.  在 [伺服器管理員] 中，按一下**IPAM**。 IPAM 用戶端主控台隨即出現。  
  
2.  在 導覽 窗格中，按一下**存取控制**，然後在下方瀏覽窗格中，按一下 **角色**。  
  
    ![存取控制角色](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_01.jpg)  
  
3.  以滑鼠右鍵按一下**角色**，然後按一下**新增使用者角色**。  
  
    ![新增使用者角色](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_02.jpg)  
  
4.  **新增或編輯角色**對話方塊隨即開啟。 在 **名稱**，輸入角色，可清除角色函式的名稱。 比方說，如果您想要建立角色，可讓系統管理員管理的 DNS SRV 資源記錄，您可能會指定名稱的角色**IPAMSrv**。 如有需要向下捲動**作業**找出您想要定義角色的作業類型。 針對此範例中，向下捲動至**DNS 資源記錄的管理作業**。  
  
    ![DNS 資源記錄的管理作業](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_03.jpg)  
  
5.  依序展開**DNS 資源記錄的管理作業**，然後找出**SRV 記錄的作業**。  
  
    ![SRV 記錄作業](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_04.jpg)  
  
6.  展開，然後選取**SRV 記錄的作業**，然後按一下**確定**。  
  
    ![選取 SRV 記錄的作業](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_05.jpg)  
  
7.  在 IPAM 用戶端主控台中，按一下您剛才建立的角色。 在 [**詳細資料] 檢視中，** 角色允許的作業會顯示。  
  
    ![新的角色詳細資料](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_06.jpg)  
  
## <a name="see-also"></a>另請參閱  
[角色型存取控制](Role-based-Access-Control.md)  
[管理 IPAM](Manage-IPAM.md)  
  


