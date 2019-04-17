---
title: 建立 DNS 區域
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
ms.assetid: a030ff51-a815-4fc4-b26d-aae41c3e4ce5
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e532837e6c98694fa040a6d47a8e536eecb4c3da
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="create-a-dns-zone"></a>建立 DNS 區域

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題以使用 IPAM client 主機建立一個 DNS 區域。  
  
資格在**系統管理員**，或相當於，才能執行此程序最小值。  
  
### <a name="to-create-a-dns-zone"></a>若要建立 DNS 區域  
  
1.  在伺服器管理員中，按一下**IPAM**。 顯示 IPAM client 主機。  
  
2.  在瀏覽窗格中，在**監視和管理**，按一下 [ **DNS 與 DHCP 伺服器]**。 在 [顯示] 窗格中，按一下**伺服器類型**，然後按一下 [ **DNS**。 搜尋結果中列出的所有受 IPAM DNS 伺服器。  
  
3.  找出您想要新增的區域，的伺服器，並以滑鼠右鍵按一下伺服器。  按一下**建立 DNS 區域**。  
  
    ![建立 DNS 區域](../../media/Create-a-DNS-Zone/ipam_CreateDNSZone_01a.jpg)  
  
4.  **建立 DNS 區域**對話方塊。 在**一般屬性**、選取區域分類，區域類型，然後輸入名稱中的**區域名稱**。 也選取 [適用於中部署值**進階屬性**，然後按一下 [ **[確定]**。  
  
    ![進階的屬性](../../media/Create-a-DNS-Zone/ipam_CreateDNSZone_02a.jpg)  
  
## <a name="see-also"></a>也了  
[管理 DNS 區域](DNS-Zone-Management.md)  
[管理 IPAM](Manage-IPAM.md)  
  


