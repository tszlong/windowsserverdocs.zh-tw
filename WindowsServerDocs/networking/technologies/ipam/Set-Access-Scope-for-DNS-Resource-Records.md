---
title: 設定 DNS 資源記錄的存取範圍
description: 本主題是 Windows Server 2016 中的 IP 位址管理 (IPAM) 管理指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a96a8752-5678-49c5-b069-d2cce8042a51
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c79e1f63b9bcb43520a57defca8228b76db68a31
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283865"
---
# <a name="set-access-scope-for-dns-resource-records"></a>設定 DNS 資源記錄的存取範圍

>適用於：Windows Server （半年通道），Windows Server 2016

若要使用 IPAM 用戶端主控台中設定 DNS 資源記錄的存取範圍，您可以使用本主題。  
  
若要執行此程序，至少需要 **Administrators** 的成員資格或同等權限。  
  
### <a name="to-set-access-scope-for-dns-resource-records"></a>若要設定 DNS 資源記錄的存取範圍  
  
1.  在 [伺服器管理員] 中，按一下**IPAM**。 IPAM 用戶端主控台隨即出現。  
  
2.  在 [導覽] 窗格中，按一下**DNS 區域**。  在下方瀏覽窗格中，依序展開**正向對應**和瀏覽至並選取包含您想要變更其存取範圍的資源記錄的區域。  
  
3.  在 [顯示] 窗格中，找出並選取您想要變更其存取範圍的資源記錄。  
  
    ![選取的資源記錄](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_02.jpg)  
  
4.  所選的 DNS 資源記錄，以滑鼠右鍵按一下，然後按一下**設定存取領域**。  
  
    ![設定存取領域](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_03.jpg)  
  
5.  **設定存取領域**對話方塊隨即開啟。 如果您的部署所需，按一下以取消選取**從父系繼承存取領域**。 在 **選取 存取領域**，選取一個項目，然後按一下 **確定**。  
  
    ![選取 存取領域](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_04.jpg)  
  
## <a name="see-also"></a>另請參閱  
[角色型存取控制](Role-based-Access-Control.md)  
[管理 IPAM](Manage-IPAM.md)  
  


