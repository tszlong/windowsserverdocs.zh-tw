---
title: 建立存取原則
description: 瞭解如何在 IPAM 用戶端主控台中建立存取原則。
manager: brianlic
ms.topic: article
ms.assetid: 854bd064-2f86-4678-a940-a04b3e48ae10
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 3fd8490321070dc73658bd6c6cad08f09bb9025c
ms.sourcegitcommit: 84b97d34d606b6bf4b6ec8760a93107f1b311428
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/02/2021
ms.locfileid: "99245485"
---
# <a name="create-an-access-policy"></a>建立存取原則

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題，在 IPAM 用戶端主控台中建立存取原則。

若要執行此程序，至少需要 **Administrators** 的成員資格或同等權限。

> [!NOTE]
> 您可以為特定使用者或 Active Directory 中的使用者群組建立存取原則。 當您建立存取原則時，您必須選取內建的 IPAM 角色或您所建立的自訂角色。 如需自訂角色的詳細資訊，請參閱 [建立存取控制的使用者角色](../../technologies/ipam/Create-a-User-Role-for-Access-Control.md)。

### <a name="to-create-an-access-policy"></a>建立存取原則

1.  在伺服器管理員中，按一下 [  **IPAM**]。 IPAM 用戶端主控台隨即出現。

2.  在流覽窗格中，按一下 [ **存取控制**]。 在下方流覽窗格中，以滑鼠右鍵按一下 [ **存取原則**]，然後按一下 [ **新增存取原則**]。

    ![顯示已醒目提示 [存取原則] optoin 的伺服器管理員螢幕擷取畫面，以及可供選取的 [新增存取原則] 選項。](../../media/Create-an-Access-Policy/ipam_CreateAP_01.jpg)

3.  [ **新增存取原則** ] 對話方塊隨即開啟。 在 [ **使用者設定**] 中，按一下 [ **新增**]。

    ![[新增存取原則] 對話方塊的螢幕擷取畫面。](../../media/Create-an-Access-Policy/ipam_CreateAP_02.jpg)

4.  [ **選取使用者或群組** ] 對話方塊隨即開啟。 按一下 [位置]。

    ![使用者或群組位置](../../media/Create-an-Access-Policy/ipam_CreateAP_03.jpg)

5.  [ **位置** ] 對話方塊隨即開啟。 流覽至包含使用者帳戶的位置，選取位置，然後按一下 **[確定]**。 [ **位置** ] 對話方塊隨即關閉。

    ![選取位置](../../media/Create-an-Access-Policy/ipam_CreateAP_04.jpg)

6.  在 [ **選取使用者或群組** ] 對話方塊的 [ **輸入物件名稱來選取**] 中，輸入您要建立存取原則的使用者帳戶名稱。 按一下 [確定]  。

7.  在 [ **新增存取原則**] 的 [ **使用者設定**] 中，[ **使用者別名** ] 現在包含套用原則的使用者帳戶。 在 [ **存取設定**] 中，按一下 [ **新增**]。

    ![新的存取設定](../../media/Create-an-Access-Policy/ipam_CreateAP_05.jpg)

8.  在 [新增 **存取原則**] 中， **存取設定** 會變更為 [ **新增] 設定**。

    ![對話方塊名稱變更為新設定](../../media/Create-an-Access-Policy/ipam_CreateAP_06.jpg)

9. 按一下 [ **選取角色** ] 展開角色清單。 選取其中一個內建角色，或者，如果您已建立新的角色，請選取您所建立的其中一個角色。 例如，如果您已建立要套用至使用者的 IPAMSrv 角色，請按一下 [ **IPAMSrv**]。

    ![選取職責](../../media/Create-an-Access-Policy/ipam_CreateAP_07.jpg)

10. 按一下 [新增設定]。

    ![新增設定](../../media/Create-an-Access-Policy/ipam_CreateAP_08.jpg)

11. 角色會新增至存取原則。 若要建立其他存取 **原則，請按一下 [** 套用]，然後針對您想要建立的每個原則重複這些步驟。 如果您不想要建立其他原則，請按一下 **[確定]**。

    ![按一下 [套用] 或 [確定]](../../media/Create-an-Access-Policy/ipam_CreateAP_09.jpg)

12. 在 IPAM 用戶端主控台的 [顯示] 窗格中，確認已建立新的存取原則。

    ![查看新的存取原則](../../media/Create-an-Access-Policy/ipam_CreateAP_09a.jpg)

## <a name="see-also"></a>另請參閱
以[角色為基礎的存取控制](Role-based-Access-Control.md) 
[管理 IPAM](Manage-IPAM.md)



