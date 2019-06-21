---
title: 編輯 DNS 區域
description: 本主題是 Windows Server 2016 中的 IP 位址管理 (IPAM) 管理指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a35164e1-11ad-47c8-9843-580d30c70d07
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d688790828cb58b9fca2a17c95212c064532bb22
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282172"
---
# <a name="edit-a-dns-zone"></a>編輯 DNS 區域

>適用於：Windows Server （半年通道），Windows Server 2016

若要編輯 IPAM 用戶端主控台中的 DNS 區域，您可以使用本主題。  
  
若要執行此程序，至少需要 **Administrators** 的成員資格或同等權限。  
  
### <a name="to-edit-a-dns-zone"></a>若要編輯 DNS 區域  
  
1.  在 [伺服器管理員] 中，按一下**IPAM**。 IPAM 用戶端主控台隨即出現。  
  
2.  在 [導覽] 窗格中，在**監視與管理**，按一下**DNS 區域**。 瀏覽窗格會分割成上方瀏覽窗格和下方的瀏覽窗格中。  
  
3.  在下方瀏覽窗格中，進行下列選擇其中一項：  
  
    -   正向對應  
  
    -   IPv4 反向對應  
  
    -   IPv6 反向對應  
  
4.  例如，選取 IPv4 反向對應。  
  
    ![選取 區域類型](../../media/Edit-a-DNS-Zone/ipam_EditZone_01.jpg)  
  
5.  在 顯示 窗格中，以滑鼠右鍵按一下您要編輯，然後按一下 區域**編輯 DNS 區域**。  
  
    ![編輯 DNS 區域](../../media/Edit-a-DNS-Zone/ipam_EditZone_02.jpg)  
  
6.  **編輯 DNS 區域** 對話方塊隨即開啟與**一般**選取頁面。 如有需要編輯您在一般的區域內容：**DNS 伺服器**，**區域分類**，和**區域類型**，然後按一下 **套用**或者，如果您的編輯都已完成，**確定**。  
  
    ![編輯區域內容和儲存](../../media/Edit-a-DNS-Zone/ipam_EditZone_03a.jpg)  
  
7.  在 [**編輯 DNS 區域**] 對話方塊中，按一下**進階**。 **進階**區域屬性頁面隨即開啟。 如有需要編輯您想要變更，然後再按一下屬性**套用**或者，如果您的編輯都已完成，則**確定**。  
  
    ![編輯進階的區域屬性](../../media/Edit-a-DNS-Zone/ipam_EditZone_04a.jpg)  
  
8.  如果需要請選取其他區域的屬性頁面名稱 （名稱伺服器，SOA，區域傳輸），進行編輯，然後按一下**套用**或是**確定**。 若要檢閱所有您區域的編輯，請按一下**摘要**，然後按一下**確定**。  
  
## <a name="see-also"></a>另請參閱  
[DNS 區域管理](DNS-Zone-Management.md)  
[管理 IPAM](Manage-IPAM.md)  
  


