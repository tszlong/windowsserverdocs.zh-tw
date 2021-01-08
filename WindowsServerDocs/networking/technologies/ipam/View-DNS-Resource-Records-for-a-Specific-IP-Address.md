---
title: 檢視特定 IP 位址的 DNS 資源記錄
description: 瞭解如何查看與您所選 IP 位址相關聯的 DNS 資源記錄。
manager: brianlic
ms.topic: article
ms.assetid: f590fb86-4195-4f90-98cb-e90459d4c1e3
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 9ad301081ae286c72e26a567970ac4f8a1a99aa2
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98040118"
---
# <a name="view-dns-resource-records-for-a-specific-ip-address"></a>檢視特定 IP 位址的 DNS 資源記錄

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來查看與所選 IP 位址相關聯的 DNS 資源記錄。

若要執行此程序，至少需要 **Administrators** 的成員資格或同等權限。

### <a name="to-view-resource-records-for-an-ip-address"></a>若要查看 IP 位址的資源記錄

1.  在伺服器管理員中，按一下 [  **IPAM**]。 IPAM 用戶端主控台隨即出現。

2.  在流覽窗格的 [ **IP 位址空間**] 中，按一下 [ **ip 位址清查**]。 在下方流覽窗格中，按一下 [ **IPv4** ] 或 [ **IPv6**]。 IP 位址清查會出現在顯示窗格的搜尋視圖中。 找出並選取您想要查看其 DNS 資源記錄的 IP 位址。

    ![查看 IP 位址清查](../../media/View-DNS-Resource-Records-for-a-Specific-IP-Address/ipam_IPInventory_01.jpg)

3.  在顯示窗格的 **詳細資料檢視** 中，按一下 [ **DNS 資源記錄**]。 會顯示與所選 IP 位址相關聯的資源記錄。

    ![查看 DNS 資源記錄](../../media/View-DNS-Resource-Records-for-a-Specific-IP-Address/ipam_IPInventory_02.jpg)

## <a name="see-also"></a>另請參閱
[DNS 資源記錄管理](DNS-Resource-Record-Management.md) 
[管理 IPAM](Manage-IPAM.md)



