---
title: 刪除 DNS 資源記錄
description: 本主題是 Windows Server 2016 中的 IP 位址管理 (IPAM) 管理指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 366e6fd5-d563-4de3-9551-5614cbb8f2cb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ecbe5dcc452aa39a9afca7e1c8d5fe70d8d4528d
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283972"
---
# <a name="delete-dns-resource-records"></a>刪除 DNS 資源記錄

>適用於：Windows Server （半年通道），Windows Server 2016

若要使用 IPAM 用戶端主控台刪除一或多個 DNS 資源記錄，您可以使用本主題。  
  
若要執行此程序，至少需要 **Administrators** 的成員資格或同等權限。  
  
### <a name="to-delete-dns-resource-records"></a>若要刪除 DNS 資源記錄  
  
1.  在 [伺服器管理員] 中，按一下**IPAM**。 IPAM 用戶端主控台隨即出現。  
  
2.  在 [導覽] 窗格中，在**監視與管理**，按一下**DNS 區域**。  瀏覽窗格會分割成上方瀏覽窗格和下方的瀏覽窗格中。  
  
3.  按一下以展開**正向對應**和您想要刪除的區域和資源記錄的所在位置的網域。 按一下在區域中，然後在 [顯示] 窗格中，按一下**目前檢視**。 按一下 **資源記錄**。  
  
4.  在 [顯示] 窗格中，找出並選取您想要刪除的資源記錄。  
  
    ![選取要刪除的資源記錄](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_01.jpg)  
  
5.  將選取的記錄，以滑鼠右鍵按一下，然後按一下**刪除 DNS 資源記錄**。  
  
    ![刪除記錄](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_02.jpg)  
  
6.  **刪除 DNS 資源記錄**對話方塊隨即開啟。 確認已選取正確的 DNS 伺服器。 如果沒有，請按一下**DNS 伺服器**，然後選取您要刪除的資源記錄的伺服器。 按一下 [確定]  。 IPAM 會刪除的資源記錄的 DNS 伺服器。  
  
    ![確認已選取正確的 DNS 伺服器和刪除記錄](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_03.jpg)  
  
## <a name="see-also"></a>另請參閱  
[DNS 資源記錄管理](DNS-Resource-Record-Management.md)  
[管理 IPAM](Manage-IPAM.md)  
  


