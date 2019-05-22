---
ms.assetid: 8be8c48d-790c-4199-b9d3-9f4a07d5ced2
title: 收集網路資訊
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d88cc2fafd9aa4fc221efc901c48fa41ef007a21
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867039"
---
# <a name="collecting-network-information"></a>收集網路資訊

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

設計有效的站台拓樸，Active Directory 網域服務 (AD DS) 中的第一個步驟是，請參閱您的組織網路群組收集資訊，並與它們定期溝通有關您實體網路拓樸。  
  
## <a name="creating-a-location-map"></a>建立位置地圖

建立代表您組織的實體網路基礎結構的位置對應。 在位置地圖上，找出包含的電腦群組的內部連線 10 mb / 秒 (Mbps) 或更高版本 （區域網路 (LAN) 速度或更新版本） 的地理位置。  
  
## <a name="listing-communication-links-and-available-bandwidth"></a>列出通訊連結和可用的頻寬

建立之後的位置對應，文件類型通訊連結，連結速度，以及每個位置之間的可用頻寬。 從您網路的群組中取得的廣域網路 (wan) 拓撲。 如需常見的 WAN 線路類型和其頻寬的清單，請參閱中的 < 判斷成本 > 一節[建立站台連結設計](../../ad-ds/plan/Creating-a-Site-Link-Design.md)。 您將需要這項資訊來建立站台連結，稍後在站台拓撲設計程序。  
  
頻寬是指您可以在指定的時間之間的通訊通道傳輸的資料量。 可用的頻寬是指實際適用於 AD DS 的頻寬總量。 您可以取得可用的頻寬資訊從您網路的群組，或您可以使用通訊協定分析器，例如網路監視器來分析每個連結上的流量。 如需安裝網路監視器的資訊，請參閱文章[監視網路流量](https://go.microsoft.com/fwlink/?LinkId=107058)。  
  
記錄每個位置與連結到它的位置。 此外，記錄類型的通訊連結和其可用的頻寬。 可協助您列出通訊連結和可用的頻寬為工作表，請參閱[工作輔助工具的 Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558)，下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，及開啟 「 地理位置和通訊連結 」 (DSSTOPO_1.doc)。  
  
## <a name="listing-ip-subnets-within-each-location"></a>列出每個位置中的 IP 子網路

您的文件的通訊連結和每個位置之間可用的頻寬之後，記錄每個位置內的 IP 子網路。 如果您不會已經知道子網路遮罩以及各個位置內的 IP 位址，請參閱您網路的群組。  
  
AD DS 站台與關聯的工作站，藉由比較工作站的 IP 位址與每個站台相關聯的子網路。 將網域控制站新增到網域時，AD DS 也會檢查其 IP 位址，並將它們放在最適當的站台。  
  
可協助您列出每個位置內的 IP 子網路為工作表，請參閱[工作輔助工具的 Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558)，下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，並開啟 」位置和子網路 」 (DSSTOPO_2.doc)。  
  
> [!NOTE]  
> 除了 IP 第 4 版 (IPv4) 位址，Windows Server 也支援 IP 版本 6 (IPv6) 子網路首碼。 若要協助您列出的 IPv6 子網路首碼為工作表，請參閱[附錄 a:位置和子網路首碼](../../ad-ds/plan/Appendix-A--Locations-and-Subnet-Prefixes.md)。  

## <a name="listing-domains-and-number-of-users-for-each-location"></a>列出網域和每個地點的使用者數目

表示在某個位置中的每個地區網域的使用者數目是其中一個因素會決定設置地區網域控制站和通用類別目錄伺服器，也就是站台拓撲設計程序的下一個步驟。 比方說，規劃放置區域網域控制站在包含 100 個以上區域的網域使用者，因此他們可以仍登入加入網域的 WAN 連結失敗時的位置。  
  
記錄的位置，會在每個位置，以及代表每個位置中的每個網域的使用者數目表示的網域。 若要協助您列出的網域和每個位置中的使用者數目為工作表，請參閱[工作輔助工具的 Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558)，下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，並開啟 [網域和使用者在每個位置]\(DSSTOPO_3.doc)。  
