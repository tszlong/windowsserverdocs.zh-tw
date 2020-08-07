---
title: 篩選 DNS 資源記錄的檢視
description: 本主題是 Windows Server 2016 中的 IP 位址管理 (IPAM) 管理指南的一部分。
manager: brianlic
ms.topic: article
ms.assetid: 5b80294a-7325-476b-84eb-69f0d051e8b2
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 5c629c4f05e0eb1198a6dc1a2ac074967765ff7b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971785"
---
# <a name="filter-the-view-of-dns-resource-records"></a>篩選 DNS 資源記錄的檢視

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題，在 IPAM 用戶端主控台中篩選 DNS 資源記錄的視圖。

若要執行此程序，至少需要 **Administrators** 的成員資格或同等權限。

### <a name="to-filter-the-view-of-dns-resource-records"></a>篩選 DNS 資源記錄的視圖

1.  在伺服器管理員中，按一下 [ **IPAM**]。 IPAM 用戶端主控台隨即出現。

2.  在流覽窗格的 [**監視及管理**] 中，按一下 [ **DNS 區域**]。  流覽窗格會分割成上方流覽窗格和較低的流覽窗格。

3.  在下方的流覽窗格中，按一下 [**正向查閱**]。 所有 IPAM 管理的 DNS 正向對應區域都會顯示在顯示窗格的搜尋結果中。

4.  按一下您要查看和篩選其記錄的區域。

5.  在 [顯示] 窗格中，按一下 [**目前視圖**]，然後按一下 [**資源記錄**]。 區域的資源記錄會顯示在 [顯示] 窗格中。

6.  在 [顯示] 窗格中，按一下 [**新增準則**]。

    ![新增準則](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_01.jpg)

7.  從下拉式清單中選取條件。 例如，如果您想要查看特定的記錄類型，請按一下 [**記錄類型**]。

    ![選取準則](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_02.jpg)

8.  按一下 [新增] 。

    ![新增準則](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_03.jpg)

9. **記錄類型**會加入做為搜尋參數。 輸入您想要尋找之記錄類型的文字。 例如，如果您只想要查看 SRV 記錄，請輸入**srv**。

    ![指定您想要尋找的記錄類型](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_04.jpg)

10. 按 ENTER 鍵。 DNS 資源記錄會根據您指定的準則和搜尋片語進行篩選。

    ![執行篩選準則](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_05.jpg)

## <a name="see-also"></a>另請參閱
[DNS 資源記錄管理](DNS-Resource-Record-Management.md) 
[管理 IPAM](Manage-IPAM.md)



