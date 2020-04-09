---
title: 建立 DNS 區域
description: 本主題是 Windows Server 2016 中 IP 位址管理（IPAM）管理指南的一部分。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ipam
ms.topic: article
ms.assetid: a030ff51-a815-4fc4-b26d-aae41c3e4ce5
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 0ae9869b95cfa1da04e0103b5a824ff1fc21568f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80814751"
---
# <a name="create-a-dns-zone"></a>建立 DNS 區域

>適用於：Windows Server (半年通道)、Windows Server 2016

您可以使用此主題，透過 IPAM 用戶端主控台來建立 DNS 區域。  
  
若要執行此程序，至少需要 **Administrators** 的成員資格或同等權限。  
  
### <a name="to-create-a-dns-zone"></a>若要建立 DNS 區域  
  
1.  在伺服器管理員中，按一下 [ **IPAM**]。 IPAM 用戶端主控台隨即出現。  
  
2.  在流覽窗格的 [**監視與管理**] 中，按一下 [ **DNS 和 DHCP 伺服器**]。 在 [顯示] 窗格中，按一下 [**伺服器類型**]，然後按一下 [ **DNS**]。 IPAM 管理的所有 DNS 伺服器都會列在搜尋結果中。  
  
3.  找出您要新增區域的伺服器，然後在伺服器上按一下滑鼠右鍵。  按一下 [**建立 DNS 區域**]。  
  
    ![建立 DNS 區域](../../media/Create-a-DNS-Zone/ipam_CreateDNSZone_01a.jpg)  
  
4.  [**建立 DNS 區域**] 對話方塊隨即開啟。 在 **[一般**內容] 中，選取 [區域] 類別、[區欄位型別]，然後在 [**區功能變數名稱稱**] 中輸入名稱。 也請在 [ **Advanced Properties**] 中選取適合您的部署的值，然後按一下 **[確定]** 。  
  
    ![Advanced 屬性](../../media/Create-a-DNS-Zone/ipam_CreateDNSZone_02a.jpg)  
  
## <a name="see-also"></a>另請參閱  
[DNS 區域管理](DNS-Zone-Management.md)  
[管理 IPAM](Manage-IPAM.md)  
  


