---
title: 篩選 DNS 資源記錄的檢視
description: 瞭解如何在 IPAM 用戶端主控台中篩選 DNS 資源記錄的顯示。
manager: brianlic
ms.topic: article
ms.assetid: 5b80294a-7325-476b-84eb-69f0d051e8b2
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: b17c071d78079933a04663e4fd1cd3528a9b86cb
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98039558"
---
# <a name="filter-the-view-of-dns-resource-records"></a>篩選 DNS 資源記錄的檢視

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來篩選 IPAM 用戶端主控台中的 DNS 資源記錄的顯示。

若要執行此程序，至少需要 **Administrators** 的成員資格或同等權限。

### <a name="to-filter-the-view-of-dns-resource-records"></a>篩選 DNS 資源記錄的顯示

1.  在伺服器管理員中，按一下 [  **IPAM**]。 IPAM 用戶端主控台隨即出現。

2.  在流覽窗格的 [ **監視和管理**] 中，按一下 [ **DNS 區域**]。  流覽窗格會分成上方流覽窗格和下方流覽窗格。

3.  在下方流覽窗格中，按一下 [ **向前查閱**]。 所有 IPAM 管理的 DNS 正向對應區域都會顯示在顯示窗格搜尋結果中。

4.  按一下您想要查看並篩選其記錄的區域。

5.  在 [顯示] 窗格中，按一下 [ **目前的視圖**]，然後按一下 [ **資源記錄**]。 區域的資源記錄會顯示在 [顯示] 窗格中。

6.  在 [顯示] 窗格中，按一下 [ **新增準則**]。

    ![新增準則](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_01.jpg)

7.  從下拉式清單中選取準則。 例如，如果您想要查看特定的記錄類型，請按一下 [ **記錄類型**]。

    ![選取準則](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_02.jpg)

8.  按一下 [新增] 。

    ![新增準則](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_03.jpg)

9. **記錄類型** 會新增為搜尋參數。 輸入您想要尋找之記錄類型的文字。 例如，如果您只想要查看 SRV 記錄，請輸入 **srv**。

    ![指定您想要尋找的記錄類型](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_04.jpg)

10. 按 ENTER 鍵。 DNS 資源記錄會根據您指定的準則和搜尋片語進行篩選。

    ![執行篩選](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_05.jpg)

## <a name="see-also"></a>另請參閱
[DNS 資源記錄管理](DNS-Resource-Record-Management.md) 
[管理 IPAM](Manage-IPAM.md)



