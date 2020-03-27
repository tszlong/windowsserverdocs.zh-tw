---
title: 檢視 DNS 區域的 DNS 資源記錄
description: 本主題是 Windows Server 2016 中 IP 位址管理（IPAM）管理指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 375feefc-949e-47c3-9e61-35b79e021966
ms.author: lizross
author: eross-msft
ms.openlocfilehash: ca2cf532fc9fa7e1f12e5294b40d29863fd42261
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312333"
---
# <a name="view-dns-resource-records-for-a-dns-zone"></a>檢視 DNS 區域的 DNS 資源記錄

>適用於：Windows Server (半年通道)、Windows Server 2016

您可以使用本主題，在 IPAM 用戶端主控台中，查看 DNS 區域的 DNS 資源記錄。  
  
若要執行此程序，至少需要 **Administrators** 的成員資格或同等權限。  
  
### <a name="to-view-dns-resource-records-for-a-zone"></a>若要查看區域的 DNS 資源記錄  
  
1.  在伺服器管理員中，按一下 [ **IPAM**]。 IPAM 用戶端主控台隨即出現。  
  
2.  在流覽窗格的 [**監視及管理**] 中，按一下 [ **DNS 區域**]。  流覽窗格會分割成上方流覽窗格和較低的流覽窗格。  
  
3.  在下方流覽窗格中，按一下 [**正向**對應]，然後展開 [網域和區域] 清單，以找出並選取您想要查看的區域。 例如，如果您有一個名為都柏林的區域，請按一下 [**都柏林**]。  
  
    ![選取您想要查看的區域](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01a.jpg)  

  
4.  在 [顯示] 窗格中，預設的視圖是區域的 DNS 伺服器。 若要變更此視圖，請按一下 [**目前的視圖**]，然後按一下 [**資源記錄**]。  
  
    ![將視圖變更為資源記錄](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_Zone_RR_02.jpg)  
  
5.  區域的 DNS 資源記錄隨即顯示。 若要篩選記錄，請在 [**篩選**] 中輸入您想要尋找的文字。  
  
    ![輸入文字以篩選記錄](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01c.jpg)  
  
6.  若要依記錄類型、存取領域或其他準則篩選資源記錄，請按一下 [**新增條件**]，然後從 [準則] 清單中選取，再按一下 [**新增**]。  
  
    ![使用準則來篩選記錄](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01d.jpg)  
  
## <a name="see-also"></a>另請參閱  
[DNS 區域管理](DNS-Zone-Management.md)  
[管理 IPAM](Manage-IPAM.md)  
  


