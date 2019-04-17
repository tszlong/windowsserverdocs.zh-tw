---
title: DNS 資源記錄管理
description: 本主題是在 Windows Server 2016 的 IP 位址管理 (IPAM) 管理組節目表的一部分。
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
ms.openlocfilehash: 5ed781ef37243b80ea9da8aad27a29046b8dc8c9
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="dns-resource-record-management"></a>DNS 資源記錄管理

>適用於：Windows Server（以每年次管道）、Windows Server 2016

本主題提供使用 IPAM 管理 DNS 資源記錄的相關資訊。  
  
> [!NOTE]  
> 本主題中，除了下列的 DNS 資源使用碼表進行管理主題可在本區段中。  
>   
> -   [新增 DNS 資源記錄](../../technologies/ipam/Add-a-DNS-Resource-Record.md)  
> -   [Delete DNS 資源記錄](../../technologies/ipam/Delete-DNS-Resource-Records.md)  
> -   [篩選 DNS 資源記錄檢視](../../technologies/ipam/Filter-the-View-of-DNS-Resource-Records.md)  
> -   [檢視 DNS 資源記錄指定 IP 位址](../../technologies/ipam/View-DNS-Resource-Records-for-a-Specific-IP-Address.md)  
  
## <a name="resource-record-management-overview"></a>資源使用碼表進行管理概觀  
在 Windows Server 2016 中的 IPAM 部署時，您可以執行伺服器探索 IPAM 伺服器管理主控台中加入 DHCP 和 DNS 伺服器。 IPAM 伺服器然後動態收集 DNS 資料每 6 個小時的 DNS 伺服器，它設定為從管理。 IPAM 維護本機資料庫它會儲存此 DNS 資料。 IPAM 為您提供和 server 資料收集，以及告知您的下一天的時間和時間的 DNS 伺服器收集資料時的通知。  
  
下圖黃色狀態列上顯示通知 IPAM 使用者介面位置。  
  
![IPAM 通知](../../media/DNS-Resource-Record-Management/ipam_DataCollection_01.jpg)  
  
DNS 資料收集，包括 DNS 區域和資源使用碼表進行資訊。 您可以設定 IPAM 收集的您的慣用 DNS 伺服器區資訊。  IPAM 收集檔案架構與 Active Directory 區域。  
  
> [!NOTE]  
> IPAM 只從加入網域的 Microsoft DNS 伺服器收集資料。 第三方 DNS 伺服器及非網域結合的伺服器 IPAM，不支援。  
  
以下是 DNS IPAM 所收集的資源記錄類型的清單。  
  
-   AFS 資料庫  
  
-   ATM 地址  
  
-   CNAME  
  
-   DHCID  
  
-   DNAME  
  
-   A 或 AAAA 主機  
  
-   主機資訊  
  
-   ISDN  
  
-   MX  
  
-   伺服器的名稱  
  
-   指標 (PTR)  
  
-   負責人  
  
-   透過傳送  
  
-   位置服務  
  
-   SOA  
  
-   SRV  
  
-   文字  
  
-   已知服務  
  
-   WINS  
  
-   WINS-R  
  
-   X.25  
  
Windows Server 2016 中 IPAM 提供整合 IP 位址庫存、DNS 區域和 DNS 資源記錄：  
  
-   您可以使用 IPAM 自動建置 IP 的 DNS 資源記錄地址清單。  
  
-   您可以從 DNS A 和 AAAA 資源記錄手動建立 IP 位址清單。  
  
-   您可以檢視 DNS 資源記錄特定 DNS 區域，以及篩選記錄根據類型、IP 位址、資源記錄資料，以及其他篩選的選項。  
  
-   IPAM 會自動建立的對應的 IP 位址範圍和 DNS 反向對應的區域。  
  
-   IPAM 建立 PTR 記錄位於反向對應區域，以及該 ip 中包含哪些 IP 位址。 如果需要您可以手動修改這個對應。  
  
IPAM 可讓您在執行從 IPAM 主機的資源記錄上的下列操作。  
  
-   建立 DNS 資源記錄  
  
-   編輯 DNS 資源記錄  
  
-   Delete DNS 資源記錄  
  
-   建立記錄相關的資源  
  
IPAM 自動登所有 DNS 設定變更，您將使用 IPAM 主機。  
  
## <a name="see-also"></a>也了  
[管理 IPAM](Manage-IPAM.md)  
  


