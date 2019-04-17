---
ms.assetid: 8be8c48d-790c-4199-b9d3-9f4a07d5ced2
title: "收集資訊的網路"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 174f11ca85a659f9d0c52e220d5ce37b1804f39b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="collecting-network-information"></a>收集資訊的網路

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

連絡您組織的網路群組收集的資訊，並與它們定期通訊關於您的實體網路拓撲是設計的有效網站拓撲 Active Directory Domain Services (AD DS) 中的第一個步驟。  
  
## <a name="creating-a-location-map"></a>建立位置對應  
建立位置對應，表示您的組織的實體網路基礎結構。 位置的地圖上找出包含群組的電腦與內部連接 10 mb 秒 (Mbps) 或更高版本（速度區域網路（區域網路）或更好）的地理位置。  
  
## <a name="listing-communication-links-and-available-bandwidth"></a>列出通訊連結和可用的頻寬  
建立位置對應之後, 文件通訊連結，連結速度，與每個位置間頻寬的類型。 取得寬區域 (wan) 拓撲從您網路的群組。 清單 WAN 賽道每一種與他們的頻寬，會看到「判斷成本」一節中的[建立網站連結設計](../../ad-ds/plan/Creating-a-Site-Link-Design.md)。 您將需要此資訊來建立網站連結稍後網站拓撲設計程序。  
  
您可以在一段指定時間的通訊通道上傳送的資料量是指頻寬。 可用的頻寬指的是實際可供使用 AD DS 頻寬。 您可以從您的網路群組，取得頻寬資訊或您可以使用通訊協定分析器，例如網路監視器，這是與 Windows Server 2008 出貨元件分析流量每個連結。 安裝監視器網路相關資訊，會看到監視網路流量 ([https://go.microsoft.com/fwlink/?LinkId=107058](https://go.microsoft.com/fwlink/?LinkId=107058))。  
  
每個位置和其他連結到該位置的文件。 此外，記錄通訊的連結和它提供的頻寬類型。 試算表中列出的通訊的連結和可用的頻寬協助您，會看到工作協助工具的 Windows Server 2003 部署套件 ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558))，下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，以及打開 (DSSTOPO_1.doc)」地理位置和通訊連結]。  
  
## <a name="listing-ip-subnets-within-each-location"></a>列出的 IP 子網路中每個位置  
您的文件的通訊連結和每個位置間頻寬之後，錄製中每個位置的 IP 子網路。 如果您不會已經知道子網路遮罩及每個位置的 IP 位址，請洽詢您的網路群組。  
  
AD DS 關聯網站比較工作站的 IP 位址的每個網站相關聯的子網路工作站。 在您的網域控制站加入網域，AD DS 也會檢查它們的 IP 位址，並放入最適合的網站。  
  
試算表中列出的 IP 子網路中每個位置協助您，會看到工作協助工具的 Windows Server 2003 部署套件 ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558))，下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip 和開放」的位置，子網路「(DSSTOPO_2.doc).  
  
> [!NOTE]  
> IP 版本除了 4 (IPv4) 地址、Windows Server 2008 也支援 6 (IPv6) 版本的 IP 子網路首碼。 適用於試算表中列出的 IPv6 子網路首碼協助您，請查看[附錄答位置和子網路首碼](../../ad-ds/plan/Appendix-A--Locations-and-Subnet-Prefixes.md)。  
  
## <a name="listing-domains-and-number-of-users-for-each-location"></a>列出網域並為每個位置的使用者人數  
使用者的位置在每個地區網域是其中一個比例，以判斷位置的區域網域控制站伺服器通用，也就是網站拓撲設計程序的下一個步驟。 例如，計畫中的位置，讓他們仍可登入網域如果 WAN 連結失敗包含超過 100 區域網域使用者放置地區網域控制站。  
  
錄製位置，都可以在每個位置，與每個網域都會在每個位置的使用者的網域。 試算表中列出的網域和的使用者都可以在每個位置的數目協助您，會看到工作協助工具的 Windows Server 2003 部署套件 ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558))，下載 < DICT__Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip>Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip</DICT__Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip >，並打開「網域和使用者在每一個位置](DSSTOPO_3.doc)。  
  


