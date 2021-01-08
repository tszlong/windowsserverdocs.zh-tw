---
title: 刪除 DNS 資源記錄
description: 瞭解如何使用 IPAM 用戶端主控台刪除一或多個 DNS 資源記錄。
manager: brianlic
ms.topic: article
ms.assetid: 366e6fd5-d563-4de3-9551-5614cbb8f2cb
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: a305457d50985c2cefa56dbfe0d243bc9cd8b504
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98039568"
---
# <a name="delete-dns-resource-records"></a>刪除 DNS 資源記錄

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用此主題來刪除一或多個 DNS 資源記錄，方法是使用 IPAM 用戶端主控台。

若要執行此程序，至少需要 **Administrators** 的成員資格或同等權限。

### <a name="to-delete-dns-resource-records"></a>刪除 DNS 資源記錄

1.  在伺服器管理員中，按一下 [  **IPAM**]。 IPAM 用戶端主控台隨即出現。

2.  在流覽窗格的 [ **監視和管理**] 中，按一下 [ **DNS 區域**]。  流覽窗格會分成上方流覽窗格和下方流覽窗格。

3.  按一下以展開 [ **正** 向對應]，以及您想要刪除之區域和資源記錄所在的網域。 按一下該區域，然後按一下 [顯示] 窗格中的 [ **目前的視圖**]。 按一下 [ **資源記錄**]。

4.  在 [顯示] 窗格中，找出並選取您想要刪除的資源記錄。

    ![選取要刪除的資源記錄](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_01.jpg)

5.  以滑鼠右鍵按一下選取的記錄，然後按一下 [ **刪除 DNS 資源記錄**]。

    ![刪除記錄](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_02.jpg)

6.  [ **刪除 DNS 資源記錄** ] 對話方塊隨即開啟。 確認已選取正確的 DNS 伺服器。 如果不是，請按一下 [ **DNS 伺服器** ]，然後選取您想要從中刪除資源記錄的伺服器。 按一下 [確定]。 IPAM 會刪除 DNS 伺服器的資源記錄。

    ![確認已選取正確的 DNS 伺服器並刪除記錄](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_03.jpg)

## <a name="see-also"></a>另請參閱
[DNS 資源記錄管理](DNS-Resource-Record-Management.md) 
[管理 IPAM](Manage-IPAM.md)



