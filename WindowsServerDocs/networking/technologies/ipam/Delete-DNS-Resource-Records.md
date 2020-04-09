---
title: 刪除 DNS 資源記錄
description: 本主題是 Windows Server 2016 中 IP 位址管理（IPAM）管理指南的一部分。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ipam
ms.topic: article
ms.assetid: 366e6fd5-d563-4de3-9551-5614cbb8f2cb
ms.author: lizross
author: eross-msft
ms.openlocfilehash: a5658e53aebf9f6fa20a5de9f3f7cb5a0287b408
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860671"
---
# <a name="delete-dns-resource-records"></a>刪除 DNS 資源記錄

>適用於：Windows Server (半年通道)、Windows Server 2016

您可以使用本主題，使用 IPAM 用戶端主控台刪除一或多個 DNS 資源記錄。  
  
若要執行此程序，至少需要 **Administrators** 的成員資格或同等權限。  
  
### <a name="to-delete-dns-resource-records"></a>刪除 DNS 資源記錄  
  
1.  在伺服器管理員中，按一下 [ **IPAM**]。 IPAM 用戶端主控台隨即出現。  
  
2.  在流覽窗格的 [**監視及管理**] 中，按一下 [ **DNS 區域**]。  流覽窗格會分割成上方流覽窗格和較低的流覽窗格。  
  
3.  按一下以展開 [**正**向對應]，以及您要刪除的區域和資源記錄所在的網域。 按一下該區域，然後在 [顯示] 窗格中，按一下 [**目前的視圖**]。 按一下 [**資源記錄**]。  
  
4.  在 [顯示] 窗格中，找出並選取您想要刪除的資源記錄。  
  
    ![選取要刪除的資源記錄](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_01.jpg)  
  
5.  以滑鼠右鍵按一下選取的記錄，然後按一下 [**刪除 DNS 資源記錄**]。  
  
    ![刪除記錄](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_02.jpg)  
  
6.  [**刪除 DNS 資源記錄**] 對話方塊隨即開啟。 確認已選取正確的 DNS 伺服器。 如果不是，請按一下 [ **DNS 伺服器**]，然後選取您要從中刪除資源記錄的伺服器。 按一下 [確定]。 IPAM 會從 DNS 伺服器刪除資源記錄。  
  
    ![確認已選取正確的 DNS 伺服器，並刪除記錄](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_03.jpg)  
  
## <a name="see-also"></a>另請參閱  
[DNS 資源記錄管理](DNS-Resource-Record-Management.md)  
[管理 IPAM](Manage-IPAM.md)  
  


