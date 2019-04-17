---
title: Delete DNS 資源記錄
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
ms.assetid: 366e6fd5-d563-4de3-9551-5614cbb8f2cb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f9ebfeca1da9e36cd00272113f2e86c33174074b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="delete-dns-resource-records"></a>Delete DNS 資源記錄

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題以 delete 一或多個 DNS 資源記錄使用 IPAM client 主機。  
  
資格在**系統管理員**，或相當於，才能執行此程序最小值。  
  
### <a name="to-delete-dns-resource-records"></a>若要移除 DNS 資源記錄  
  
1.  在伺服器管理員中，按一下**IPAM**。 顯示 IPAM client 主機。  
  
2.  在瀏覽窗格中，在**監視和管理**，按一下 [ **DNS 區域**。  瀏覽窗格中分為左上角的瀏覽窗格中，較低的瀏覽窗格。  
  
3.  按一下以展開**向前查詢**，以及您想要 delete 區域和資源記錄位於何處網域。 按一下的區域，然後在 [顯示] 窗格中，按一下**目前檢視]**。 按一下**資源記錄**。  
  
4.  在 [顯示] 窗格中，尋找並選取您想要 delete 資源記錄。  
  
    ![選取要 delete 資源記錄](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_01.jpg)  
  
5.  選取的記錄，以滑鼠右鍵按一下，然後按一下**Delete DNS 資源記錄**。  
  
    ![Delete 記錄](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_02.jpg)  
  
6.  **Delete DNS 資源記錄**對話方塊。 確認已選取正確的 DNS 伺服器。 如果不是，按一下**的 DNS 伺服器**，然後選取您要移除的資源記錄的伺服器。 按一下**[確定]**。 IPAM 將的資源記錄移除 DNS 伺服器。  
  
    ![確認已選取正確的 DNS 伺服器，並移除記錄](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_03.jpg)  
  
## <a name="see-also"></a>也了  
[DNS 資源記錄管理](DNS-Resource-Record-Management.md)  
[管理 IPAM](Manage-IPAM.md)  
  


