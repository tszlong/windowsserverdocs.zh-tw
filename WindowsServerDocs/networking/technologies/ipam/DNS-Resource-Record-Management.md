---
title: DNS 資源記錄管理
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
ms.assetid: 7b66c09d-e401-4f70-9a2a-6047dd629bfa
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2edf0fef80cfcfb2c754cacd53df3b38c3881733
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844609"
---
# <a name="dns-resource-record-management"></a>DNS 資源記錄管理

>適用於：Windows Server （半年通道），Windows Server 2016

本主題提供有關使用 IPAM 來管理 DNS 資源記錄的資訊。  
  
> [!NOTE]  
> 本主題中，除了下列的 DNS 資源記錄管理主題有這一節。  
>   
> -   [新增 DNS 資源記錄](../../technologies/ipam/Add-a-DNS-Resource-Record.md)  
> -   [刪除 DNS 資源記錄](../../technologies/ipam/Delete-DNS-Resource-Records.md)  
> -   [篩選 DNS 資源記錄的檢視](../../technologies/ipam/Filter-the-View-of-DNS-Resource-Records.md)  
> -   [檢視特定 IP 位址的 DNS 資源記錄](../../technologies/ipam/View-DNS-Resource-Records-for-a-Specific-IP-Address.md)  
  
## <a name="resource-record-management-overview"></a>資源記錄管理概觀  
當您部署 Windows Server 2016 中的 IPAM 時，您可以執行伺服器探索 來將 DHCP 和 DNS 伺服器新增到 IPAM 伺服器管理主控台。 IPAM 伺服器然後動態 DNS 每 6 小時會從收集資料的 DNS 伺服器，將它設定為管理。 IPAM 會維護本機資料庫，它會儲存這個 DNS 資料。 IPAM 為您提供的日期和伺服器資料收集，以及告知您在下一天的時間和時間會從 DNS 伺服器的資料收集時的通知。  
  
下圖中的黃色狀態列會顯示 IPAM 通知使用者介面位置。  
  
![IPAM 通知](../../media/DNS-Resource-Record-Management/ipam_DataCollection_01.jpg)  
  
收集 DNS 資料包含 DNS 區域和資源記錄的資訊。 您可以設定 IPAM 收集來自您慣用的 DNS 伺服器的區域資訊。  IPAM 會收集檔案為基礎和 Active Directory 區域。  
  
> [!NOTE]  
> IPAM 會完全從已加入網域的 Microsoft DNS 伺服器收集資料。 IPAM 不支援協力廠商 DNS 伺服器和加入非網域的伺服器。  
  
以下是會由 IPAM 所收集的 DNS 資源記錄類型的清單。  
  
-   AFS 資料庫  
  
-   ATM 位址  
  
-   CNAME  
  
-   DHCID  
  
-   DNAME  
  
-   主機 A 或 AAAA  
  
-   主機資訊  
  
-   ISDN  
  
-   MX  
  
-   名稱伺服器  
  
-   指標 (PTR)  
  
-   負責人員  
  
-   透過路由傳送  
  
-   服務位置  
  
-   SOA  
  
-   SRV  
  
-   文字  
  
-   已知的服務  
  
-   WINS  
  
-   WINS-R  
  
-   X.25  
  
在 Windows Server 2016 中，IPAM 會提供 IP 位址清查、 DNS 區域和 DNS 資源記錄之間的整合：  
  
-   您可以使用 IPAM 來自動建立 DNS 資源記錄的 IP 位址詳細目錄。  
  
-   您可以手動建立 DNS A 和 AAAA 資源記錄的 IP 位址詳細目錄。  
  
-   您可以檢視特定的 DNS 區域的 DNS 資源記錄，並篩選依類型、 IP 位址、 資源記錄資料和其他篩選選項的記錄。  
  
-   IPAM 會自動建立 IP 位址範圍與 DNS 反向查閱區域之間的對應。  
  
-   IPAM 建立 IP 位址的 PTR 記錄，其中包含該 IP 位址範圍和反向查閱區域中。 如有需要您可以手動修改此對應。  
  
IPAM 可讓您能夠從 IPAM 主控台執行下列作業的資源記錄。  
  
-   建立 DNS 資源記錄  
  
-   編輯 DNS 資源記錄  
  
-   刪除 DNS 資源記錄  
  
-   建立相關聯的資源記錄  
  
IPAM 會自動記錄所有的 DNS 組態變更，請務必使用 IPAM 主控台。  
  
## <a name="see-also"></a>另請參閱  
[管理 IPAM](Manage-IPAM.md)  
  


