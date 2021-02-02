---
title: 新增 DNS 資源記錄
description: 瞭解如何使用 IPAM 用戶端主控台新增一或多個新的 DNS 資源記錄。
manager: brianlic
ms.topic: article
ms.assetid: 5379373f-a3d9-4f51-b6fc-bf0f6df1d244
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 99cad9f3ad2e746c1f3a530c89c67e2bb39b80a6
ms.sourcegitcommit: 84b97d34d606b6bf4b6ec8760a93107f1b311428
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/02/2021
ms.locfileid: "99245457"
---
# <a name="add-a-dns-resource-record"></a>新增 DNS 資源記錄

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題，使用 IPAM 用戶端主控台新增一或多個新的 DNS 資源記錄。

若要執行此程序，至少需要 **Administrators** 的成員資格或同等權限。

### <a name="to-add-a-dns-resource-record"></a>新增 DNS 資源記錄

1.  在伺服器管理員中，按一下 [  **IPAM**]。 IPAM 用戶端主控台隨即出現。

2.  在流覽窗格的 [ **監視和管理**] 中，按一下 [ **DNS 區域**]。  流覽窗格會分成上方流覽窗格和下方流覽窗格。

3.  在下方流覽窗格中，按一下 [ **向前查閱**]。 所有 IPAM 管理的 DNS 正向對應區域都會顯示在顯示窗格搜尋結果中。 以滑鼠右鍵按一下您要新增資源記錄的區域，然後按一下 [ **新增 DNS 資源記錄**]。

    ![新增 DNS 資源記錄](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_01.jpg)

4.  [ **新增 DNS 資源記錄** ] 對話方塊隨即開啟。 在 [ **資源記錄** 內容] 中，按一下 [ **dns 伺服器** ]，然後選取您要新增一或多個新資源記錄的 dns 伺服器。 在 [ **設定 DNS 資源記錄**] 中，按一下 [ **新增**]。

    ![設定 DNS 資源記錄](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_02.jpg)

5.  此對話方塊會展開以顯示 **新的資源記錄**。 按一下 [ **資源記錄類型**]。

    ![資源記錄類型](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_03.jpg)

6.  資源記錄類型的清單隨即顯示。 按一下您要新增的資源記錄類型。

    ![選取要新增的記錄類型](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_04.jpg)

7.  在 [ **新資源記錄** ] 的 [ **名稱**] 中，輸入資源記錄名稱。 在 [ **Ip 位址**] 中輸入 ip 位址，然後選取適合您部署的資源記錄屬性。 按一下 [ **新增資源記錄**]。

    ![[新增資源記錄] 頁面的 [資源記錄] 頁面的螢幕擷取畫面，其中顯示 [新增資源記錄] 區段。](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_06.jpg)

8.  如果您不想要建立其他新的資源記錄，請按一下 **[確定]**。 如果您想要建立其他新的資源記錄，請按一下 [ **新增**]。

    ![按一下 [確定] 或 [新增]](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_01.jpg)

9. 此對話方塊會展開以顯示 **新的資源記錄**。 按一下 [ **資源記錄類型**]。 資源記錄類型的清單隨即顯示。 按一下您要新增的資源記錄類型。

10. 在 [ **新資源記錄** ] 的 [ **名稱**] 中，輸入資源記錄名稱。 在 [ **Ip 位址**] 中輸入 ip 位址，然後選取適合您部署的資源記錄屬性。 按一下 [ **新增資源記錄**]。

    ![[新增資源記錄] 頁面的 [資源記錄] 頁面的螢幕擷取畫面，其中已醒目提示 [新增資源記錄] 選項。](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_02.jpg)

11. 如果您想要新增更多資源記錄，請重複建立記錄的程式。 當您完成建立新的資源記錄時， **請按一下 [** 套用]。

    ![完成建立資源記錄](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_03.jpg)

12. 當 IPAM 在您指定的 DNS 伺服器上建立資源記錄時，[ **新增資源記錄** ] 對話方塊會顯示資源記錄摘要。 成功建立記錄之後，記錄的 **狀態** 會是 [ **成功**]。

    ![記錄新增狀態](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_04.jpg)

13. 按一下 [確定]  。

## <a name="see-also"></a>另請參閱
[DNS 資源記錄管理](DNS-Resource-Record-Management.md) 
[管理 IPAM](Manage-IPAM.md)



