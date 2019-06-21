---
title: 建立 DNS 區域
description: 本主題是 Windows Server 2016 中的 IP 位址管理 (IPAM) 管理指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a030ff51-a815-4fc4-b26d-aae41c3e4ce5
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 83e3865308fd45e88b800753b20ab298f9a14c96
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282247"
---
# <a name="create-a-dns-zone"></a>建立 DNS 區域

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題使用 IPAM 用戶端主控台中建立 DNS 區域。  
  
若要執行此程序，至少需要 **Administrators** 的成員資格或同等權限。  
  
### <a name="to-create-a-dns-zone"></a>若要建立 DNS 區域  
  
1.  在 [伺服器管理員] 中，按一下**IPAM**。 IPAM 用戶端主控台隨即出現。  
  
2.  在 [導覽] 窗格中，在**監視與管理**，按一下**DNS 和 DHCP 伺服器**。 在 [顯示] 窗格中，按一下**伺服器類型**，然後按一下**DNS**。 在搜尋結果，詳列 IPAM 所管理的所有 DNS 伺服器。  
  
3.  找出您要新增的區域的伺服器，並以滑鼠右鍵按一下伺服器。  按一下 **建立 DNS 區域**。  
  
    ![建立 DNS 區域](../../media/Create-a-DNS-Zone/ipam_CreateDNSZone_01a.jpg)  
  
4.  **建立 DNS 區域**對話方塊隨即開啟。 在 **一般屬性**，選取 「 區域 」 類別目錄的區域類型，然後輸入中的名稱**e name&amp;** 。 也選取適合您的部署中的值**進階屬性**，然後按一下**確定**。  
  
    ![進階的屬性](../../media/Create-a-DNS-Zone/ipam_CreateDNSZone_02a.jpg)  
  
## <a name="see-also"></a>另請參閱  
[DNS 區域管理](DNS-Zone-Management.md)  
[管理 IPAM](Manage-IPAM.md)  
  


