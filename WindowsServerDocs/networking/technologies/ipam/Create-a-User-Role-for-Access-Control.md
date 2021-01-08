---
title: 建立存取控制的使用者角色
description: 瞭解如何在 IPAM 用戶端主控台中建立新的存取控制使用者角色。
manager: brianlic
ms.topic: article
ms.assetid: ae6a42db-a104-401b-a8e6-b85c47d30b46
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 88c0f82c6e6168d97681e0d6f795dd4101afc6a7
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98039718"
---
# <a name="create-a-user-role-for-access-control"></a>建立存取控制的使用者角色

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題，在 IPAM 用戶端主控台中建立新的存取控制使用者角色。

若要執行此程序，至少需要 **Administrators** 的成員資格或同等權限。

> [!NOTE]
> 建立角色之後，您可以建立存取原則，將角色指派給特定使用者或 Active Directory 群組。 如需詳細資訊，請參閱 [建立存取原則](../../technologies/ipam/Create-an-Access-Policy.md)。

### <a name="to-create-a-role"></a>建立角色

1.  在伺服器管理員中，按一下 [  **IPAM**]。 IPAM 用戶端主控台隨即出現。

2.  在流覽窗格中，按一下 [ **存取控制**]，然後在下方流覽窗格中按一下 [ **角色**]。

    ![存取控制角色](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_01.jpg)

3.  以滑鼠右鍵按一下 [ **角色**]，然後按一下 [ **新增使用者角色**]。

    ![新增使用者角色](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_02.jpg)

4.  [ **新增或編輯角色** ] 對話方塊隨即開啟。 在 [ **名稱**] 中，輸入讓角色功能清楚的角色名稱。 例如，如果您想要建立可讓系統管理員管理 DNS SRV 資源記錄的角色，則您可以將角色命名為 **IPAMSrv**。 如有需要，請在 **作業** 中向下移動，以找出您要為角色定義的作業類型。 針對此範例，請向下滾動至 **DNS 資源記錄管理作業**。

    ![DNS 資源記錄管理作業](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_03.jpg)

5.  展開 [ **DNS 資源記錄管理作業**]，然後找出 **SRV 記錄作業**。

    ![SRV 記錄作業](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_04.jpg)

6.  展開並選取 [ **SRV 記錄操作**]，然後按一下 **[確定]**。

    ![選取 SRV 記錄作業](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_05.jpg)

7.  在 IPAM 用戶端主控台中，按一下您剛才建立的角色。 在 **詳細資料檢視中，** 會顯示角色允許的作業。

    ![新的角色詳細資料](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_06.jpg)

## <a name="see-also"></a>另請參閱
以[角色為基礎的存取控制](Role-based-Access-Control.md) 
[管理 IPAM](Manage-IPAM.md)



