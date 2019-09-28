---
ms.assetid: 8be8c48d-790c-4199-b9d3-9f4a07d5ced2
title: 收集網路資訊
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 5642963caee47ac48f841a13b47852b6b7d9b241
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408987"
---
# <a name="collecting-network-information"></a>收集網路資訊

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在 Active Directory Domain Services （AD DS）中設計有效網站拓撲的第一個步驟，就是洽詢貴組織的網路群組來收集資訊，並定期與實體網路拓撲進行通訊。  
  
## <a name="creating-a-location-map"></a>建立位置圖

建立一個位置圖，以代表您組織的實體網路基礎結構。 在 [位置] 地圖上，找出包含電腦群組的地理位置，其內部連線為每秒 10 mb （Mbps）或更高（區域網路（LAN）速度或更佳）。  
  
## <a name="listing-communication-links-and-available-bandwidth"></a>列出通訊連結和可用的頻寬

建立位置對應之後，請記錄通訊連結類型、其連結速度，以及每個位置之間的可用頻寬。 從您的網路群組取得廣域網路（WAN）拓撲。 如需常見的 WAN 線路類型和其頻寬的清單，請參閱[建立站台連結設計](../../ad-ds/plan/Creating-a-Site-Link-Design.md)中的「判斷成本」一節。 您將需要這項資訊，以便稍後在網站拓撲設計程式中建立網站連結。  
  
頻寬是指您可以在指定的一段時間內，透過通道傳輸的資料量。 可用的頻寬是指實際可供 AD DS 使用的頻寬量。 您可以從網路群組取得可用的頻寬資訊，也可以使用通訊協定分析器（例如網路監視器）分析每個連結上的流量。 如需有關安裝網路監視器的詳細資訊，請參閱[監視網路流量](https://go.microsoft.com/fwlink/?LinkId=107058)一文。  
  
記錄每個位置和連結到它的其他位置。 此外，請記錄通訊連結的類型及其可用的頻寬。 如需協助您列出通訊連結和可用頻寬的工作表，請參閱[適用于 Windows Server 2003 部署套件的工作輔助](https://go.microsoft.com/fwlink/?LinkID=102558)工具、下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services，以及開放的地理位置位置和通訊連結」（DSSTOPO_1）。  
  
## <a name="listing-ip-subnets-within-each-location"></a>列出每個位置中的 IP 子網

在您記載通訊連結和每個位置之間的可用頻寬之後，請記錄每個位置中的 IP 子網。 如果您還不知道每個位置中的子網路遮罩和 IP 位址，請查閱您的網路群組。  
  
AD DS 會藉由比較工作站的 IP 位址與與每個網站相關聯的子網，讓工作站與網站產生關聯。 當您將網域控制站新增至網域時，AD DS 也會檢查其 IP 位址，並將它們放在最適當的網站中。  
  
如需協助您列出每個位置中的 IP 子網的工作表，請參閱[Windows Server 2003 部署套件的工作輔助](https://go.microsoft.com/fwlink/?LinkID=102558)工具、下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services，以及開啟「位置和子網」（DSSTOPO_2）。  
  
> [!NOTE]  
> 除了 IP 第4版（IPv4）位址，Windows Server 也支援 IP 版本6（IPv6）子網首碼。 如需協助您列出 IPv6 子網首碼的工作表，請參閱 [Appendix A：位置和子網首碼 @ no__t-0。  

## <a name="listing-domains-and-number-of-users-for-each-location"></a>列出每個位置的網域和使用者數目

在位置中表示的每個地區網域的使用者數目，都是判斷地區網域控制站和通用類別目錄伺服器位置的因素之一，這是網站拓朴設計程式的下一個步驟。 例如，規劃將區域網域控制站放在包含超過100個地區網域使用者的位置，如此一來，如果 WAN 連結失敗，他們仍然可以登入網域。  
  
記錄位置、每個位置中表示的網域，以及每個位置中表示之每個網域的使用者數目。 若要協助您列出的網域和每個位置中的使用者數目為工作表，請參閱[工作輔助工具的 Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558)，下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，並開啟 [網域和使用者在每個位置]\(DSSTOPO_3.doc)。  
