---
title: 設定 DNS 資源記錄的存取範圍
description: 本主題是 Windows Server 2016 中 IP 位址管理（IPAM）管理指南的一部分。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ipam
ms.topic: article
ms.assetid: a96a8752-5678-49c5-b069-d2cce8042a51
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 613796c933498d104db4895733c9a9b1957cb952
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860611"
---
# <a name="set-access-scope-for-dns-resource-records"></a>設定 DNS 資源記錄的存取範圍

>適用於：Windows Server (半年通道)、Windows Server 2016

您可以使用此主題，使用 IPAM 用戶端主控台來設定 DNS 資源記錄的存取領域。  
  
若要執行此程序，至少需要 **Administrators** 的成員資格或同等權限。  
  
### <a name="to-set-access-scope-for-dns-resource-records"></a>設定 DNS 資源記錄的存取領域  
  
1.  在伺服器管理員中，按一下 [ **IPAM**]。 IPAM 用戶端主控台隨即出現。  
  
2.  在流覽窗格中，按一下 [ **DNS 區域**]。  在下方流覽窗格中，展開 [**正向**對應]，然後流覽至包含您想要變更其存取領域之資源記錄的區域並加以選取。  
  
3.  在 [顯示] 窗格中，找出並選取您想要變更其存取領域的資源記錄。  
  
    ![選取資源記錄](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_02.jpg)  
  
4.  以滑鼠右鍵按一下選取的 DNS 資源記錄，然後按一下 [**設定存取領域**]。  
  
    ![設定存取領域](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_03.jpg)  
  
5.  [**設定存取領域**] 對話方塊隨即開啟。 如果您的部署需要，請按一下以取消選取 [**從父系繼承存取領域**]。 在 **[選取存取領域**] 中選取一個專案，然後按一下 **[確定]** 。  
  
    ![選取存取領域](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_04.jpg)  
  
## <a name="see-also"></a>另請參閱  
[以角色為基礎的存取控制](Role-based-Access-Control.md)  
[管理 IPAM](Manage-IPAM.md)  
  


