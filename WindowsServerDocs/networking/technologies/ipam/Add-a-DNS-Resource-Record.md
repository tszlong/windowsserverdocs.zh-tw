---
title: 新增 DNS 資源記錄
description: 本主題是 Windows Server 2016 中的 IP 位址管理 (IPAM) 管理指南的一部分。
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
ms.openlocfilehash: 868b27a2a2b2005c3cf54d544d2534ae66ae0d98
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849479"
---
# <a name="add-a-dns-resource-record"></a>新增 DNS 資源記錄

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題使用 IPAM 用戶端主控台，新增一或多個新的 DNS 資源記錄。  
  
若要執行此程序，至少需要 **Administrators** 的成員資格或同等權限。  
  
### <a name="to-add-a-dns-resource-record"></a>若要新增的 DNS 資源記錄  
  
1.  在 [伺服器管理員] 中，按一下**IPAM**。 IPAM 用戶端主控台隨即出現。  
  
2.  在 [導覽] 窗格中，在**監視與管理**，按一下**DNS 區域**。  瀏覽窗格會分割成上方瀏覽窗格和下方的瀏覽窗格中。  
  
3.  在下方瀏覽窗格中，按一下**正向對應**。 在顯示窗格搜尋結果中，會顯示所有受 IPAM 管理 DNS 正向對應區域。 以滑鼠右鍵按一下您要新增資源記錄，然後再按的區域**新增 DNS 資源記錄**。  
  
    ![新增 DNS 資源記錄](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_01.jpg)
  
4.  **新增 DNS 資源記錄**對話方塊隨即開啟。 在 **資源記錄的內容**，按一下**DNS 伺服器**選取您要新增一或多個新的資源記錄的 DNS 伺服器。 在 **設定的 DNS 資源記錄**，按一下**新增**。  
  
    ![設定 DNS 資源記錄](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_02.jpg)  
  
5.  此對話方塊會展開以顯示**新的資源記錄**。 按一下 **資源記錄類型**。  
  
    ![資源記錄類型](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_03.jpg)  
  
6.  資源記錄類型的清單隨即顯示。 按一下您想要新增的資源記錄類型。  
  
    ![選取要新增的記錄類型](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_04.jpg)  
  
7.  在 **新的資源記錄，** 中**名稱**，輸入資源記錄名稱。 在  **IP 位址**，輸入 IP 位址，然後選取 適用於您部署的資源記錄屬性。 按一下 **將資源記錄新增**。  
  
    ![新增資源記錄](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_06.jpg)  
  
8.  如果您不想要建立其他新的資源記錄，請按一下**確定**。 如果您想要建立其他新的資源記錄，請按一下**新增**。  
  
    ![按一下 OK 或 新增](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_01.jpg)
  
9. 此對話方塊會展開以顯示**新的資源記錄**。 按一下 **資源記錄類型**。 資源記錄類型的清單隨即顯示。 按一下您想要新增的資源記錄類型。  
  
10. 在 **新的資源記錄，** 中**名稱**，輸入資源記錄名稱。 在  **IP 位址**，輸入 IP 位址，然後選取 適用於您部署的資源記錄屬性。 按一下 **將資源記錄新增**。  
  
    ![新增資源記錄](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_02.jpg)  
  
11. 如果您想要新增更多的資源記錄，請重複此程序建立記錄。 當您完成建立新的資源記錄之後時，按一下**套用**。  
  
    ![完整的資源記錄的建立](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_03.jpg)  
  
12. **新增資源記錄** 對話方塊中顯示的資源記錄摘要，在 IPAM 建立您所指定的 DNS 伺服器上的資源記錄。 已成功建立記錄，當**狀態**記錄的已**成功**。  
  
    ![資料錄加入狀態](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_04.jpg)  
  
13. 按一下 [確定] 。  
  
## <a name="see-also"></a>另請參閱  
[DNS 資源記錄管理](DNS-Resource-Record-Management.md)  
[管理 IPAM](Manage-IPAM.md)  
  


