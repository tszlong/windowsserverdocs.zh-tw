---
title: 檢視 DNS 區域的 DNS 資源記錄
description: 本主題是 Windows Server 2016 中的 IP 位址管理 (IPAM) 管理指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 375feefc-949e-47c3-9e61-35b79e021966
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 44db34199257367e98279ccbcbc2d5041ee9884c
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283809"
---
# <a name="view-dns-resource-records-for-a-dns-zone"></a>檢視 DNS 區域的 DNS 資源記錄

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題來檢視 IPAM 用戶端主控台中的 DNS 區域的 DNS 資源記錄。  
  
若要執行此程序，至少需要 **Administrators** 的成員資格或同等權限。  
  
### <a name="to-view-dns-resource-records-for-a-zone"></a>若要檢視區域的 DNS 資源記錄  
  
1.  在 [伺服器管理員] 中，按一下**IPAM**。 IPAM 用戶端主控台隨即出現。  
  
2.  在 [導覽] 窗格中，在**監視與管理**，按一下**DNS 區域**。  瀏覽窗格會分割成上方瀏覽窗格和下方的瀏覽窗格中。  
  
3.  在下方瀏覽窗格中，按一下**正向對應**，然後展開網域和區域的清單，找出並選取您想要檢視的區域。 例如，如果您有一個名叫都柏林的區域，按一下**都柏林**。  
  
    ![選取您想要檢視的區域](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01a.jpg)  

  
4.  在 [顯示] 窗格中，預設檢視為區域的 DNS 伺服器。 若要變更檢視，請按一下**目前檢視**，然後按一下**資源記錄**。  
  
    ![將檢視變更為資源記錄](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_Zone_RR_02.jpg)  
  
5.  會顯示區域的 DNS 資源記錄。 若要篩選記錄，請輸入您想要在中尋找的文字**篩選**。  
  
    ![輸入要篩選記錄的文字](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01c.jpg)  
  
6.  若要篩選的資源記錄所記錄的型別、 存取領域或其他準則，按一下**新增準則**，然後進行選擇，從 [條件] 清單，再按一下**新增**。  
  
    ![使用條件來篩選記錄](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01d.jpg)  
  
## <a name="see-also"></a>另請參閱  
[DNS 區域管理](DNS-Zone-Management.md)  
[管理 IPAM](Manage-IPAM.md)  
  


