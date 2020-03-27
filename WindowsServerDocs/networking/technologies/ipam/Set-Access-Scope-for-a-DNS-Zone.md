---
title: 設定 DNS 區域的存取範圍
description: 本主題是 Windows Server 2016 中 IP 位址管理（IPAM）管理指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a211dde-80eb-4888-b5bb-4e28fe8dc7df
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 870acde822fb5c8c038139710facb71208df3387
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309568"
---
# <a name="set-access-scope-for-a-dns-zone"></a>設定 DNS 區域的存取範圍

>適用於：Windows Server (半年通道)、Windows Server 2016

您可以使用此主題來設定 DNS 區域的存取領域，方法是使用 IPAM 用戶端主控台。  
  
若要執行此程序，至少需要 **Administrators** 的成員資格或同等權限。  
  
### <a name="to-set-the-access-scope-for-a-dns-zone"></a>設定 DNS 區域的存取領域  
  
1.  在伺服器管理員中，按一下 [ **IPAM**]。 IPAM 用戶端主控台隨即出現。  
  
2.  在流覽窗格中，按一下 [ **DNS 區域**]。 在 [顯示] 窗格中，以滑鼠右鍵按一下您要變更存取領域的 DNS 區域，然後按一下 [**設定存取領域**]。  
  
    ![設定存取領域](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_02.jpg)  
  
3.  [**設定存取領域**] 對話方塊隨即開啟。 如果您的部署需要，請按一下以取消選取 [**從父系繼承存取領域**]。 在 **[選取存取領域**] 中選取一個專案，然後按一下 **[確定]** 。  
  
    ![選取存取領域](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_03.jpg)  
  
4.  在 IPAM 用戶端主控台的 [顯示] 窗格中，確認已變更區域的存取領域。  
  
    ![確認區域的存取領域已變更](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_04.jpg)  
  
## <a name="see-also"></a>另請參閱  
[以角色為基礎的存取控制](Role-based-Access-Control.md)  
[管理 IPAM](Manage-IPAM.md)  
  


