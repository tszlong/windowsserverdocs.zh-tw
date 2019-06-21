---
title: 篩選 DNS 資源記錄的檢視
description: 本主題是 Windows Server 2016 中的 IP 位址管理 (IPAM) 管理指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5b80294a-7325-476b-84eb-69f0d051e8b2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cc3f2b8ec6e7c5149ef6351639fbbf8f0def8be8
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283939"
---
# <a name="filter-the-view-of-dns-resource-records"></a>篩選 DNS 資源記錄的檢視

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題篩選 IPAM 用戶端主控台中的 DNS 資源記錄的檢視。  
  
若要執行此程序，至少需要 **Administrators** 的成員資格或同等權限。  
  
### <a name="to-filter-the-view-of-dns-resource-records"></a>若要篩選 DNS 資源記錄的檢視  
  
1.  在 [伺服器管理員] 中，按一下**IPAM**。 IPAM 用戶端主控台隨即出現。  
  
2.  在 [導覽] 窗格中，在**監視與管理**，按一下**DNS 區域**。  瀏覽窗格會分割成上方瀏覽窗格和下方的瀏覽窗格中。  
  
3.  在下方瀏覽窗格中，按一下**正向對應**。 在顯示窗格搜尋結果中，會顯示所有受 IPAM 管理 DNS 正向對應區域。  
  
4.  區域中按一下您想要檢視和篩選的記錄。  
  
5.  在 [顯示] 窗格中，按一下**目前檢視**，然後按一下**資源記錄**。 區域的資源記錄會顯示在 [顯示] 窗格中。  
  
6.  在 [顯示] 窗格中，按一下**新增準則**。  
  
    ![新增準則](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_01.jpg)  
  
7.  從下拉式清單中選取的準則。 例如，如果您想要檢視特定記錄類型，按一下**記錄類型**。  
  
    ![選取準則](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_02.jpg)  
  
8.  按一下 **\[新增\]** 。  
  
    ![新增準則](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_03.jpg)  
  
9. **記錄類型**加入做為搜尋參數。 輸入您想要尋找的記錄類型的文字。 例如，如果您想要檢視僅有的 SRV 記錄時，輸入**SRV**。  
  
    ![指定您想要尋找的記錄類型](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_04.jpg)  
  
10. 按 ENTER 鍵。 DNS 資源記錄會根據準則篩選，並搜尋您指定的片語。  
  
    ![執行篩選器](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_05.jpg)  
  
## <a name="see-also"></a>另請參閱  
[DNS 資源記錄管理](DNS-Resource-Record-Management.md)  
[管理 IPAM](Manage-IPAM.md)  
  


