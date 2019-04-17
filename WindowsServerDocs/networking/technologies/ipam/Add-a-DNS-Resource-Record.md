---
title: 新增 DNS 資源記錄
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
ms.assetid: 5379373f-a3d9-4f51-b6fc-bf0f6df1d244
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e14a59e9f172b20e85a34d2299e3733a796adafc
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="add-a-dns-resource-record"></a>新增 DNS 資源記錄

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題加入一或多個新的 DNS 資源資料使用 IPAM client 主機。  
  
資格在**系統管理員**，或相當於，才能執行此程序最小值。  
  
### <a name="to-add-a-dns-resource-record"></a>若要新增的 DNS 資源記錄  
  
1.  在伺服器管理員中，按一下**IPAM**。 顯示 IPAM client 主機。  
  
2.  在瀏覽窗格中，在**監視和管理**，按一下 [ **DNS 區域**。  瀏覽窗格中分為左上角的瀏覽窗格中，較低的瀏覽窗格。  
  
3.  在較低的瀏覽窗格中，按一下 [**向前查詢**。 顯示窗格搜尋結果中，會顯示 IPAM 管理 DNS 向前搜尋的所有區域。 以滑鼠右鍵按一下您要新增的資源記錄，然後再按的區域**新增 DNS 資源記錄**。  
  
    ![新增 DNS 資源記錄](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_01.jpg)
  
4.  **DNS 資源記錄**對話方塊。 在**使用碼表進行的資源屬性**，按一下 [**的 DNS 伺服器**，然後選取您想要加入一或多個新的資源資料的 DNS 伺服器。 在**設定 DNS 資源記錄**，按一下 [**新增]**。  
  
    ![設定 DNS 資源記錄](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_02.jpg)  
  
5.  以顯示的對話方塊展開**新資源記錄**。 按一下**資源使用碼表進行類型**。  
  
    ![使用碼表進行輸入的資源](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_03.jpg)  
  
6.  顯示資源記錄類型的清單。 按一下您想要新增的資源記錄類型。  
  
    ![選取要新增的使用碼表進行類型](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_04.jpg)  
  
7.  在**新增資源記錄]**在**名稱**，輸入資源記錄名稱。 在**的 IP 位址**，輸入 IP 位址，然後選取 [適用於您的部署的資源記錄屬性。 按一下**新增資源記錄**。  
  
    ![新增記錄資源](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_06.jpg)  
  
8.  如果您不想要建立新的其他資源資訊，請按一下**[確定]**。 如果您想要建立新的其他資源資訊，請按一下**新增]**。  
  
    ![按一下 [確定] 或 [新增](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_01.jpg)
  
9. 以顯示的對話方塊展開**新資源記錄**。 按一下**資源使用碼表進行類型**。 顯示資源記錄類型的清單。 按一下您想要新增的資源記錄類型。  
  
10. 在**新增資源記錄]**在**名稱**，輸入資源記錄名稱。 在**的 IP 位址**，輸入 IP 位址，然後選取 [適用於您的部署的資源記錄屬性。 按一下**新增資源記錄**。  
  
    ![新增記錄資源](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_02.jpg)  
  
11. 如果您想要加入更多資源資料、建立記錄重複進行程序。 當您建立新的資源記錄完成後時，按一下**套用]**。  
  
    ![使用碼表進行建立的完整的資源](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_03.jpg)  
  
12. **新增資源記錄**對話方塊中 IPAM 指定 DNS 伺服器上建立的資源記錄時，顯示資源記錄摘要。 記錄成功地建立時，**狀態**的記錄為**成功**。  
  
    ![記錄除了狀態](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_04.jpg)  
  
13. 按一下**[確定]**。  
  
## <a name="see-also"></a>也了  
[DNS 資源記錄管理](DNS-Resource-Record-Management.md)  
[管理 IPAM](Manage-IPAM.md)  
  


