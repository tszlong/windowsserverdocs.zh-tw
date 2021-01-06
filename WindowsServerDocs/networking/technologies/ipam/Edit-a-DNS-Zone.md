---
title: 編輯 DNS 區域
description: 本主題是 Windows Server 2016 中的 IP 位址管理 (IPAM) 管理指南的一部分。
manager: brianlic
ms.topic: article
ms.assetid: a35164e1-11ad-47c8-9843-580d30c70d07
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 3c411dc560f8fed2d5138bcb8acc68a9b5f03567
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97948724"
---
# <a name="edit-a-dns-zone"></a>編輯 DNS 區域

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題，在 IPAM 用戶端主控台中編輯 DNS 區域。

若要執行此程序，至少需要 **Administrators** 的成員資格或同等權限。

### <a name="to-edit-a-dns-zone"></a>編輯 DNS 區域

1.  在伺服器管理員中，按一下 [ **IPAM**]。 IPAM 用戶端主控台隨即出現。

2.  在流覽窗格的 [ **監視和管理**] 中，按一下 [ **DNS 區域**]。 流覽窗格會分成上方流覽窗格和下方流覽窗格。

3.  在下方流覽窗格中，選取下列其中一個選項：

    -   向前查閱

    -   IPv4 反向對應

    -   IPv6 反向對應

4.  例如，選取 [IPv4 反向對應]。

    ![選取區欄位型別](../../media/Edit-a-DNS-Zone/ipam_EditZone_01.jpg)

5.  在 [顯示] 窗格中，以滑鼠右鍵按一下您要編輯的區域，然後按一下 [ **編輯 DNS 區域**]。

    ![編輯 DNS 區域](../../media/Edit-a-DNS-Zone/ipam_EditZone_02.jpg)

6.  [ **編輯 DNS 區域** ] 對話方塊隨即開啟，並選取 [ **一般** ] 頁面。 如有需要，請編輯 [一般區域內容： **DNS 伺服器**、 **區域類別目錄**] 和 [ **區欄位型別**]， **然後按一下 [** 套用]，如果您的編輯已完成，請按一下 **[確定]**。

    ![編輯區域屬性並儲存](../../media/Edit-a-DNS-Zone/ipam_EditZone_03a.jpg)

7.  在 [ **編輯 DNS 區域** ] 對話方塊中，按一下 [ **Advanced**]。 [ **Advanced** zone properties] 頁面隨即開啟。 如有需要，請編輯您要變更的屬性， **然後按一下 [** 套用]，如果您的編輯已完成，請按一下 **[確定]**。

    ![編輯 advanced zone 屬性](../../media/Edit-a-DNS-Zone/ipam_EditZone_04a.jpg)

8.  如有需要，請選取 [其他區域內容] 頁面名稱 (名稱伺服器、SOA、區域傳輸) 、進行編輯， **然後** 按一下 **[套用] 或 [確定]**。 若要檢查您所有的區域編輯，請按一下 [ **摘要**]，然後按一下 **[確定]**。

## <a name="see-also"></a>另請參閱
[DNS 區域管理](DNS-Zone-Management.md) 
[管理 IPAM](Manage-IPAM.md)



