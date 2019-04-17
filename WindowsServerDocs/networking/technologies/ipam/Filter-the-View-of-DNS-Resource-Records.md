---
title: 篩選 DNS 資源記錄檢視
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
ms.assetid: 5b80294a-7325-476b-84eb-69f0d051e8b2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 35c0e822daa9f2c8c49ae7e6f2f40ec0411cb6fa
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="filter-the-view-of-dns-resource-records"></a>篩選 DNS 資源記錄檢視

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用此主題篩選 IPAM client 主機的資源 DNS 記錄的檢視。  
  
資格在**系統管理員**，或相當於，才能執行此程序最小值。  
  
### <a name="to-filter-the-view-of-dns-resource-records"></a>篩選 DNS 資源記錄檢視  
  
1.  在伺服器管理員中，按一下**IPAM**。 顯示 IPAM client 主機。  
  
2.  在瀏覽窗格中，在**監視和管理**，按一下 [ **DNS 區域**。  瀏覽窗格中分為左上角的瀏覽窗格中，較低的瀏覽窗格。  
  
3.  在較低的瀏覽窗格中，按一下 [**向前查詢**。 顯示窗格搜尋結果中，會顯示 IPAM 管理 DNS 向前搜尋的所有區域。  
  
4.  按一下您想要檢視和篩選其的記錄的區域。  
  
5.  在 [顯示] 窗格中，按一下**目前的檢視**，然後按一下 [**資源記錄**。 [顯示] 窗格中顯示的資源記錄區域。  
  
6.  在 [顯示] 窗格中，按一下**[新增條件**。  
  
    ![[新增條件](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_01.jpg)  
  
7.  從下拉式清單中選取的條件。 如果您想要檢視的特定記錄類型，例如，按**記錄類型**。  
  
    ![選取 [條件](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_02.jpg)  
  
8.  按一下**新增**。  
  
    ![[新增條件](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_03.jpg)  
  
9. **錄製輸入**以搜尋參數中新增了。 輸入您要尋找的記錄類型的文字。 例如，如果您想要檢視只 SRV 記錄，輸入**SRV**。  
  
    ![指定您要尋找的記錄類型](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_04.jpg)  
  
10. 按下 ENTER。 DNS 資源記錄篩選依據條件，並搜尋您所指定的句子。  
  
    ![執行篩選](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_05.jpg)  
  
## <a name="see-also"></a>也了  
[DNS 資源記錄管理](DNS-Resource-Record-Management.md)  
[管理 IPAM](Manage-IPAM.md)  
  


