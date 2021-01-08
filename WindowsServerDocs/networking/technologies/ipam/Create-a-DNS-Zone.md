---
title: 建立 DNS 區域
description: 瞭解如何使用 IPAM 用戶端主控台來建立 DNS 區域。
manager: brianlic
ms.topic: article
ms.assetid: a030ff51-a815-4fc4-b26d-aae41c3e4ce5
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 04e1e393a5f434ae171ed25da4324955bea4e595
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98039688"
---
# <a name="create-a-dns-zone"></a>建立 DNS 區域

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用此主題來建立使用 IPAM 用戶端主控台的 DNS 區域。

若要執行此程序，至少需要 **Administrators** 的成員資格或同等權限。

### <a name="to-create-a-dns-zone"></a>建立 DNS 區域

1.  在伺服器管理員中，按一下 [  **IPAM**]。 IPAM 用戶端主控台隨即出現。

2.  在流覽窗格的 [ **監視和管理**] 中，按一下 [ **DNS 和 DHCP 伺服器**]。 在 [顯示] 窗格中，按一下 [ **伺服器類型**]，然後按一下 [ **DNS**]。 受 IPAM 管理的所有 DNS 伺服器都會列在搜尋結果中。

3.  找出您要新增區域的伺服器，並在伺服器上按一下滑鼠右鍵。  按一下 [ **建立 DNS 區域**]。

    ![建立 DNS 區域](../../media/Create-a-DNS-Zone/ipam_CreateDNSZone_01a.jpg)

4.  [ **建立 DNS 區域** ] 對話方塊隨即開啟。 在 **[一般** 內容] 中，選取區域類別目錄、區欄位型別，然後在 [ **區功能變數名稱稱**] 中輸入名稱。 也請在 [ **Advanced Properties**] 中選取適合您部署的值，然後按一下 **[確定]**。

    ![進階屬性](../../media/Create-a-DNS-Zone/ipam_CreateDNSZone_02a.jpg)

## <a name="see-also"></a>另請參閱
[DNS 區域管理](DNS-Zone-Management.md) 
[管理 IPAM](Manage-IPAM.md)



