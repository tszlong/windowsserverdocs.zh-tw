---
title: 設定 DNS 區域的存取範圍
description: 本主題是 Windows Server 2016 中的 IP 位址管理 (IPAM) 管理指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a211dde-80eb-4888-b5bb-4e28fe8dc7df
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 73d96a9be7c13afbe4c96d46fffefc0096046c8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813979"
---
# <a name="set-access-scope-for-a-dns-zone"></a>設定 DNS 區域的存取範圍

>適用於：Windows Server （半年通道），Windows Server 2016

若要使用 IPAM 用戶端主控台中設定 DNS 區域的存取範圍，您可以使用本主題。  
  
若要執行此程序，至少需要 **Administrators** 的成員資格或同等權限。  
  
### <a name="to-set-the-access-scope-for-a-dns-zone"></a>若要設定 DNS 區域的存取範圍  
  
1.  在 [伺服器管理員] 中，按一下**IPAM**。 IPAM 用戶端主控台隨即出現。  
  
2.  在 [導覽] 窗格中，按一下**DNS 區域**。 在 顯示 窗格中，以滑鼠右鍵按一下您要變更的存取範圍。，，然後按一下 DNS 區域**設定存取領域**。  
  
    ![設定存取領域](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_02.jpg)  
  
3.  **設定存取領域**對話方塊隨即開啟。 如果您的部署所需，按一下以取消選取**從父系繼承存取領域**。 在 **選取 存取領域**，選取一個項目，然後按一下 **確定**。  
  
    ![選取 存取領域](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_03.jpg)  
  
4.  在 [IPAM 用戶端主控台中顯示] 窗格中，確認區域的存取範圍已變更。  
  
    ![確認區域的存取範圍已變更](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_04.jpg)  
  
## <a name="see-also"></a>另請參閱  
[角色型存取控制](Role-based-Access-Control.md)  
[管理 IPAM](Manage-IPAM.md)  
  


