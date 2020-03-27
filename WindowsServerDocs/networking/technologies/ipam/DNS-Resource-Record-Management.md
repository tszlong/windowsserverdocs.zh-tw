---
title: DNS 資源記錄管理
description: 本主題是 Windows Server 2016 中 IP 位址管理（IPAM）管理指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7b66c09d-e401-4f70-9a2a-6047dd629bfa
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 3aa20913a01a23291879b98d6f53fe60a7138670
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312456"
---
# <a name="dns-resource-record-management"></a>DNS 資源記錄管理

>適用於：Windows Server (半年通道)、Windows Server 2016

本主題提供使用 IPAM 管理 DNS 資源記錄的相關資訊。  
  
> [!NOTE]  
> 除了本主題之外，本節還提供下列 DNS 資源記錄管理主題。  
>   
> -   [新增 DNS 資源記錄](../../technologies/ipam/Add-a-DNS-Resource-Record.md)  
> -   [刪除 DNS 資源記錄](../../technologies/ipam/Delete-DNS-Resource-Records.md)  
> -   [篩選 DNS 資源記錄的視圖](../../technologies/ipam/Filter-the-View-of-DNS-Resource-Records.md)  
> -   [查看特定 IP 位址的 DNS 資源記錄](../../technologies/ipam/View-DNS-Resource-Records-for-a-Specific-IP-Address.md)  
  
## <a name="resource-record-management-overview"></a>資源記錄管理總覽  
當您在 Windows Server 2016 中部署 IPAM 時，可以執行伺服器探索，將 DHCP 和 DNS 伺服器新增至 IPAM 伺服器管理主控台。 接著，IPAM 伺服器會從其設定要管理的 DNS 伺服器每隔六個小時動態地收集 DNS 資料。 IPAM 會維護儲存此 DNS 資料的本機資料庫。 IPAM 會通知您收集伺服器資料的日期和時間，以及告訴您從 DNS 伺服器進行資料收集的下一天和時間。  
  
下圖中的黃色狀態列顯示 IPAM 通知的使用者介面位置。  
  
![IPAM 通知](../../media/DNS-Resource-Record-Management/ipam_DataCollection_01.jpg)  
  
所收集的 DNS 資料包含 DNS 區域和資源記錄資訊。 您可以設定 IPAM 從慣用的 DNS 伺服器收集區域資訊。  IPAM 會收集以檔案為基礎和 Active Directory 的區域。  
  
> [!NOTE]  
> IPAM 只會從已加入網域的 Microsoft DNS 伺服器收集資料。 IPAM 不支援協力廠商 DNS 伺服器和未加入網域的伺服器。  
  
以下是 IPAM 所收集的 DNS 資源記錄類型清單。  
  
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
  
-   指標（PTR）  
  
-   負責的人員  
  
-   路由傳送  
  
-   服務位置  
  
-   SOA  
  
-   SRV  
  
-   Text  
  
-   知名的服務  
  
-   WINS  
  
-   WINS-R  
  
-   X. 25  
  
在 Windows Server 2016 中，IPAM 會提供 IP 位址清查、DNS 區域和 DNS 資源記錄之間的整合：  
  
-   您可以使用 IPAM，從 DNS 資源記錄自動建立 IP 位址清查。  
  
-   您可以從 DNS A 和 AAAA 資源記錄手動建立 IP 位址清查。  
  
-   您可以查看特定 DNS 區域的 DNS 資源記錄，並根據類型、IP 位址、資源記錄資料和其他篩選選項來篩選記錄。  
  
-   IPAM 會自動建立 IP 位址範圍與 DNS 反向查詢區域之間的對應。  
  
-   IPAM 會針對出現在反向查詢區域中的 PTR 記錄建立 IP 位址，並將其包含在該 IP 位址範圍內。 您也可以視需要手動修改此對應。  
  
IPAM 可讓您從 IPAM 主控台對資源記錄執行下列作業。  
  
-   建立 DNS 資源記錄  
  
-   編輯 DNS 資源記錄  
  
-   刪除 DNS 資源記錄  
  
-   建立相關聯的資源記錄  
  
IPAM 會自動記錄您使用 IPAM 主控台進行的所有 DNS 設定變更。  
  
## <a name="see-also"></a>另請參閱  
[管理 IPAM](Manage-IPAM.md)  
  


