---
title: 設定 DNS 資源記錄存取領域
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
ms.assetid: a96a8752-5678-49c5-b069-d2cce8042a51
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 06c33a633975497e80863cc8d42b14a0f9ac8193
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="set-access-scope-for-dns-resource-records"></a>設定 DNS 資源記錄存取領域

>適用於：Windows Server（以每年次管道）、Windows Server 2016

若要使用 IPAM client 主機設定 DNS 資源記錄存取範圍，您可以使用此主題。  
  
資格在**系統管理員**，或相當於，才能執行此程序最小值。  
  
### <a name="to-set-access-scope-for-dns-resource-records"></a>若要設定 DNS 資源記錄存取範圍  
  
1.  在伺服器管理員中，按一下**IPAM**。 顯示 IPAM client 主機。  
  
2.  在瀏覽窗格中，按一下**DNS 區域**。  在較低的瀏覽窗格中，展開**向前查詢**並瀏覽]，然後選取 [包含您想要變更其存取範圍資源記錄區域。  
  
3.  在 [顯示] 窗格中，尋找並選取您想要變更其存取範圍資源記錄。  
  
    ![選取資源記錄](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_02.jpg)  
  
4.  選取的 DNS 資源記錄上按一下滑鼠右鍵，然後按一下**設定存取範圍**。  
  
    ![將存取範圍設定](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_03.jpg)  
  
5.  **設定存取範圍**對話方塊。 如果您需要為您的部署，按一下 [取消選取**繼承存取範圍家長的**。 在**選擇存取範圍**，選取項目，然後按一下 [ **[確定]**。  
  
    ![選取存取領域](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_04.jpg)  
  
## <a name="see-also"></a>也了  
[以角色為基礎存取控制](Role-based-Access-Control.md)  
[管理 IPAM](Manage-IPAM.md)  
  


