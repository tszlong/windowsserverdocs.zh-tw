---
title: 新增 DNS 資源記錄
description: 本主題是 Windows Server 2016 中的 IP 位址管理 (IPAM) 管理指南的一部分。
manager: brianlic
ms.topic: article
ms.assetid: 5379373f-a3d9-4f51-b6fc-bf0f6df1d244
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 66137f2797d04d9a6e3888b7d73187b66177195d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87962122"
---
# <a name="add-a-dns-resource-record"></a>新增 DNS 資源記錄

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用此主題，利用 IPAM 用戶端主控台新增一或多個新的 DNS 資源記錄。

若要執行此程序，至少需要 **Administrators** 的成員資格或同等權限。

### <a name="to-add-a-dns-resource-record"></a>新增 DNS 資源記錄

1.  在伺服器管理員中，按一下 [ **IPAM**]。 IPAM 用戶端主控台隨即出現。

2.  在流覽窗格的 [**監視及管理**] 中，按一下 [ **DNS 區域**]。  流覽窗格會分割成上方流覽窗格和較低的流覽窗格。

3.  在下方的流覽窗格中，按一下 [**正向查閱**]。 所有 IPAM 管理的 DNS 正向對應區域都會顯示在顯示窗格的搜尋結果中。 在您要新增資源記錄的區域上按一下滑鼠右鍵，然後按一下 [**新增 DNS 資源記錄**]。

    ![新增 DNS 資源記錄](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_01.jpg)

4.  [**新增 DNS 資源記錄**] 對話方塊隨即開啟。 在 [**資源記錄**內容] 中，按一下 [ **dns 伺服器**]，然後選取您要新增一或多個新資源記錄的 dns 伺服器。 在 [**設定 DNS 資源記錄**] 中，按一下 [**新增**]。

    ![設定 DNS 資源記錄](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_02.jpg)

5.  對話方塊會展開以顯示**新的資源記錄**。 按一下 [**資源記錄類型**]。

    ![資源記錄類型](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_03.jpg)

6.  資源記錄類型的清單隨即顯示。 按一下您要新增的資源記錄類型。

    ![選取要新增的記錄類型](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_04.jpg)

7.  在 [**新增資源記錄**] 的 [**名稱**] 中，輸入資源記錄名稱。 在 [ **Ip 位址**] 中，輸入 ip 位址，然後選取適用于您的部署的資源記錄屬性。 按一下 [**新增資源記錄**]。

    ![新增資源記錄](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_06.jpg)

8.  如果您不想要建立其他新的資源記錄，請按一下 **[確定]**。 如果您想要建立其他新的資源記錄，請按一下 [**新增**]。

    ![按一下 [確定] 或 [新增]](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_01.jpg)

9. 對話方塊會展開以顯示**新的資源記錄**。 按一下 [**資源記錄類型**]。 資源記錄類型的清單隨即顯示。 按一下您要新增的資源記錄類型。

10. 在 [**新增資源記錄**] 的 [**名稱**] 中，輸入資源記錄名稱。 在 [ **Ip 位址**] 中，輸入 ip 位址，然後選取適用于您的部署的資源記錄屬性。 按一下 [**新增資源記錄**]。

    ![新增資源記錄](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_02.jpg)

11. 如果您想要新增更多資源記錄，請重複建立記錄的程式。 當您完成建立新的資源記錄時，**按一下 [** 套用]。

    ![完成建立資源記錄](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_03.jpg)

12. [**新增資源記錄**] 對話方塊會顯示資源記錄摘要，而 IPAM 會在您指定的 DNS 伺服器上建立資源記錄。 成功建立記錄時，記錄的**狀態**會是 [**成功**]。

    ![記錄新增狀態](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_04.jpg)

13. 按一下 [確定]  。

## <a name="see-also"></a>另請參閱
[DNS 資源記錄管理](DNS-Resource-Record-Management.md) 
[管理 IPAM](Manage-IPAM.md)



