---
title: 建立存取控制的使用者角色
description: 本主題是 Windows Server 2016 中 IP 位址管理（IPAM）管理指南的一部分。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ipam
ms.topic: article
ms.assetid: ae6a42db-a104-401b-a8e6-b85c47d30b46
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 90ba50189b0f42f1f581032b7dc8b52b8c3fca4d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80814761"
---
# <a name="create-a-user-role-for-access-control"></a>建立存取控制的使用者角色

>適用於：Windows Server (半年通道)、Windows Server 2016

您可以使用本主題，在 IPAM 用戶端主控台中建立新的存取控制使用者角色。  
  
若要執行此程序，至少需要 **Administrators** 的成員資格或同等權限。  
  
> [!NOTE]  
> 建立角色之後，您可以建立存取原則，將角色指派給特定使用者或 Active Directory 群組。 如需詳細資訊，請參閱[建立存取原則](../../technologies/ipam/Create-an-Access-Policy.md)。  
  
### <a name="to-create-a-role"></a>建立角色  
  
1.  在伺服器管理員中，按一下 [ **IPAM**]。 IPAM 用戶端主控台隨即出現。  
  
2.  在流覽窗格中，按一下 [**存取控制**]，然後在下方的流覽窗格中，按一下 [**角色**]。  
  
    ![存取控制角色](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_01.jpg)  
  
3.  以滑鼠右鍵按一下 [**角色**]，然後按一下 [**新增使用者角色**]。  
  
    ![新增使用者角色](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_02.jpg)  
  
4.  [**新增或編輯角色**] 對話方塊隨即開啟。 在 [**名稱**] 中，輸入讓角色功能清楚的角色名稱。 例如，如果您想要建立可讓系統管理員管理 DNS SRV 資源記錄的角色，您可以將角色命名為**IPAMSrv**。 如有需要，請在 [**作業**] 中向下移動，以找出您想要為角色定義的作業類型。 在此範例中，請向下流覽至 [ **DNS 資源記錄管理作業**]。  
  
    ![DNS 資源記錄管理作業](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_03.jpg)  
  
5.  展開 [ **DNS 資源記錄管理作業**]，然後找出 [ **SRV 記錄作業**]。  
  
    ![SRV 記錄作業](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_04.jpg)  
  
6.  展開並選取 [ **SRV 記錄作業**]，然後按一下 **[確定]** 。  
  
    ![選取 SRV 記錄作業](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_05.jpg)  
  
7.  在 IPAM 用戶端主控台中，按一下您剛才建立的角色。 在 [**詳細資料] 視圖中，** 會顯示角色允許的作業。  
  
    ![新增角色詳細資料](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_06.jpg)  
  
## <a name="see-also"></a>另請參閱  
[以角色為基礎的存取控制](Role-based-Access-Control.md)  
[管理 IPAM](Manage-IPAM.md)  
  


