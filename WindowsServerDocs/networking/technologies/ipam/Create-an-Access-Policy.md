---
title: 建立存取原則
description: 本主題是 Windows Server 2016 中的 IP 位址管理 (IPAM) 管理指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 854bd064-2f86-4678-a940-a04b3e48ae10
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3e72d47dc3c32db7465f7c47b16dcdc777636fd9
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282217"
---
# <a name="create-an-access-policy"></a>建立存取原則

>適用於：Windows Server （半年通道），Windows Server 2016

在 IPAM 用戶端主控台中建立存取原則，您可以使用本主題。  
  
若要執行此程序，至少需要 **Administrators** 的成員資格或同等權限。  
  
> [!NOTE]  
> 您可以在 Active Directory 中建立特定使用者或使用者群組的存取原則。 當您建立存取原則時，您必須選取內建的 IPAM 角色或您已建立的自訂角色。 如需有關自訂角色的詳細資訊，請參閱 <<c0> [ 建立使用者角色存取控制的](../../technologies/ipam/Create-a-User-Role-for-Access-Control.md)。  
  
### <a name="to-create-an-access-policy"></a>若要建立存取原則  
  
1.  在 [伺服器管理員] 中，按一下**IPAM**。 IPAM 用戶端主控台隨即出現。  
  
2.  在 [導覽] 窗格中，按一下**存取控制**。 在下方瀏覽窗格中，以滑鼠右鍵按一下**存取原則**，然後按一下**新增存取原則**。  
  
    ![新增存取原則](../../media/Create-an-Access-Policy/ipam_CreateAP_01.jpg)  
  
3.  **新增存取原則**對話方塊隨即開啟。 在 **使用者設定**，按一下**新增**。  
  
    ![新增存取原則](../../media/Create-an-Access-Policy/ipam_CreateAP_02.jpg)  
  
4.  **選取使用者或群組**對話方塊隨即開啟。 按一下 **位置**。  
  
    ![使用者或群組的位置](../../media/Create-an-Access-Policy/ipam_CreateAP_03.jpg)  
  
5.  **位置**對話方塊隨即開啟。 瀏覽至包含的使用者帳戶的位置，選取位置]，然後按一下 [**確定**。 **位置**對話框會關閉。  
  
    ![選取位置](../../media/Create-an-Access-Policy/ipam_CreateAP_04.jpg)  
  
6.  在 **選取使用者或群組**對話方塊中，於**輸入物件名稱來選取**，輸入您要建立存取原則的使用者帳戶名稱。 按一下 [確定]  。  
  
7.  在 **新增存取原則**，請在**使用者設定**，**使用者別名**現在包含套用原則的使用者帳戶。 在 **存取設定**，按一下**新增**。  
  
    ![新的存取設定](../../media/Create-an-Access-Policy/ipam_CreateAP_05.jpg)  
  
8.  在 **新增存取原則**，**存取設定**變更為**新設定**。  
  
    ![對話方塊方塊的名稱變更為新的設定](../../media/Create-an-Access-Policy/ipam_CreateAP_06.jpg)  
  
9. 按一下 **選取角色**展開角色清單。 選取其中一個內建的角色，或者，如果您已建立新的角色，選取其中一個您所建立的角色。 例如，如果您建立要套用至使用者的 IPAMSrv 角色時，按一下**IPAMSrv**。  
  
    ![選取角色](../../media/Create-an-Access-Policy/ipam_CreateAP_07.jpg)  
  
10. 按一下 **新增設定**。  
  
    ![新增設定](../../media/Create-an-Access-Policy/ipam_CreateAP_08.jpg)  
  
11. 角色新增至存取原則。 若要建立其他的存取原則，請按一下**套用**，然後針對您想要建立每個原則重複這些步驟。 如果您不要建立額外的原則，按一下**確定**。  
  
    ![按一下 [套用] 或 [確定]](../../media/Create-an-Access-Policy/ipam_CreateAP_09.jpg)  
  
12. 在 [IPAM 用戶端主控台中顯示] 窗格中，確認會建立新的存取原則。  
  
    ![檢視新的存取原則](../../media/Create-an-Access-Policy/ipam_CreateAP_09a.jpg)  
  
## <a name="see-also"></a>另請參閱  
[角色型存取控制](Role-based-Access-Control.md)  
[管理 IPAM](Manage-IPAM.md)  
  


