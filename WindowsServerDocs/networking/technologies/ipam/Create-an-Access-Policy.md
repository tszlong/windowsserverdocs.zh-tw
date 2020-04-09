---
title: 建立存取原則
description: 本主題是 Windows Server 2016 中 IP 位址管理（IPAM）管理指南的一部分。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ipam
ms.topic: article
ms.assetid: 854bd064-2f86-4678-a940-a04b3e48ae10
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 807f02387a64a26ed8b1c58a387b165bece83168
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80814811"
---
# <a name="create-an-access-policy"></a>建立存取原則

>適用於：Windows Server (半年通道)、Windows Server 2016

您可以使用本主題，在 IPAM 用戶端主控台中建立存取原則。  
  
若要執行此程序，至少需要 **Administrators** 的成員資格或同等權限。  
  
> [!NOTE]  
> 您可以為特定使用者或 Active Directory 中的使用者群組建立存取原則。 當您建立存取原則時，必須選取內建 IPAM 角色或您已建立的自訂角色。 如需自訂角色的詳細資訊，請參閱[建立存取控制的使用者角色](../../technologies/ipam/Create-a-User-Role-for-Access-Control.md)。  
  
### <a name="to-create-an-access-policy"></a>建立存取原則  
  
1.  在伺服器管理員中，按一下 [ **IPAM**]。 IPAM 用戶端主控台隨即出現。  
  
2.  在流覽窗格中，按一下 [**存取控制**]。 在下方的流覽窗格中，以滑鼠右鍵按一下 [**存取原則**]，然後按一下 [**新增存取原則**]。  
  
    ![新增存取原則](../../media/Create-an-Access-Policy/ipam_CreateAP_01.jpg)  
  
3.  [**新增存取原則**] 對話方塊隨即開啟。 在 [**使用者設定**] 中，按一下 [**新增**]。  
  
    ![新增存取原則](../../media/Create-an-Access-Policy/ipam_CreateAP_02.jpg)  
  
4.  [**選取使用者或群組**] 對話方塊隨即開啟。 按一下 [**位置**]。  
  
    ![使用者或群組位置](../../media/Create-an-Access-Policy/ipam_CreateAP_03.jpg)  
  
5.  [**位置**] 對話方塊隨即開啟。 流覽至包含使用者帳戶的位置，選取 [位置]，然後按一下 **[確定]** 。 [**位置**] 對話方塊隨即關閉。  
  
    ![選取位置](../../media/Create-an-Access-Policy/ipam_CreateAP_04.jpg)  
  
6.  在 [**選取使用者或群組**] 對話方塊的 [**輸入要選取的物件名稱**] 中，輸入您要為其建立存取原則的使用者帳戶名稱。 按一下 [確定]。  
  
7.  在 [**新增存取原則**] 的 [**使用者設定**] 中，[**使用者別名**] 現在包含套用原則的使用者帳戶。 在 [**存取設定**] 中，按一下 [**新增**]。  
  
    ![新的存取設定](../../media/Create-an-Access-Policy/ipam_CreateAP_05.jpg)  
  
8.  在 [新增**存取原則**] 中，[**存取設定**] 變更為 [**新設定**]。  
  
    ![對話方塊名稱變更為新增設定](../../media/Create-an-Access-Policy/ipam_CreateAP_06.jpg)  
  
9. 按一下 [**選取角色**] 以展開角色清單。 選取其中一個內建角色，或如果您已建立新的角色，請選取您所建立的其中一個角色。 例如，如果您已建立要套用至使用者的 IPAMSrv 角色，請按一下 [ **IPAMSrv**]。  
  
    ![選取角色](../../media/Create-an-Access-Policy/ipam_CreateAP_07.jpg)  
  
10. 按一下 [新增設定]。  
  
    ![加入新的設定](../../media/Create-an-Access-Policy/ipam_CreateAP_08.jpg)  
  
11. 角色會新增至存取原則。 若要建立其他存取**原則，請按一下 [** 套用]，然後針對您想要建立的每個原則重複這些步驟。 如果您不想要建立其他原則，請按一下 **[確定]** 。  
  
    ![按一下 [套用] 或 [確定]](../../media/Create-an-Access-Policy/ipam_CreateAP_09.jpg)  
  
12. 在 IPAM 用戶端主控台的 [顯示] 窗格中，確認已建立新的存取原則。  
  
    ![查看新的存取原則](../../media/Create-an-Access-Policy/ipam_CreateAP_09a.jpg)  
  
## <a name="see-also"></a>另請參閱  
[以角色為基礎的存取控制](Role-based-Access-Control.md)  
[管理 IPAM](Manage-IPAM.md)  
  


