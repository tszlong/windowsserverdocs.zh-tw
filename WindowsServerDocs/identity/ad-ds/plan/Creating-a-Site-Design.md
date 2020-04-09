---
ms.assetid: 83f746e5-81db-4610-9977-1d5c57699f50
title: 建立站台設計
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 0464510b498296570bdef5edb122c6c26e1fe18f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822771"
---
# <a name="creating-a-site-design"></a>建立站台設計

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

建立網站設計牽涉到決定哪些位置會變成網站、建立網站物件、建立子網物件，以及將子網與網站產生關聯。  
  
## <a name="deciding-which-locations-will-become-sites"></a>決定哪些位置會變成網站

決定要建立網站的位置，如下所示：  
  
- 針對您打算放置網域控制站的所有位置建立網站。 請參閱「網域控制站位置」（DSSTOPO_4 .doc）工作表中記載的資訊，以識別包含網域控制站的位置。  
- 建立包含執行需要建立網站之應用程式的伺服器之位置的網站。 某些應用程式（例如分散式檔案系統命名空間（DFSN））會使用網站物件，找出最接近用戶端的伺服器。  

   > [!NOTE]  
   > 如果您的組織有多個網路以快速、可靠的連線進行近近的連線，您可以在單一 Active Directory 網站中包含這些網路的所有子網。 例如，如果在不同子網中的兩部伺服器之間來回傳回的網路延遲為10毫秒或更少，您可以將這兩個子網包含在相同的 Active Directory 網站中。 如果兩個位置之間的網路延遲大於10毫秒，則不應在單一 Active Directory 網站中包含子網。 即使延遲時間小於或等於10毫秒，如果您想要將 Active Directory 應用程式的網站之間的流量分割，則可以選擇部署個別的網站。  

- 如果某個位置不需要網站，請將位置的子網新增至位置具有最大廣域網路（WAN）速度和可用頻寬的網站。  
  
將成為網站的檔位置，以及每個位置中的網路位址和子網路遮罩。 如需協助您記錄網站的工作表，請參閱[Windows Server 2003 部署套件的工作輔助工具](https://go.microsoft.com/fwlink/?LinkID=102558)、下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services .zip，然後開啟「將子網與網站建立關聯」（DSSTOPO_6 .doc）。  
  
## <a name="creating-a-site-object-design"></a>建立網站物件設計

針對您已決定建立網站的每個位置，規劃在 Active Directory Domain Services （AD DS）中建立網站物件。 將在「將子網與網站建立關聯」工作表中成為網站的檔位置。  
  
如需如何建立網站物件的詳細資訊，請參閱[建立網站一](https://go.microsoft.com/fwlink/?LinkId=107067)文。  
  
## <a name="creating-a-subnet-object-design"></a>建立子網物件設計

針對每個與每個位置相關聯的 IP 子網和子網路遮罩，規劃在 AD DS 中建立子網物件，以代表網站內的所有 IP 位址。  
  
建立 Active Directory 子網物件時，系統會自動將有關網路 IP 子網和子網路遮罩的資訊轉譯為網路前置長度標記法格式，<IP address>/<prefix length>。 例如，具有子網路遮罩255.255.252.0 的網路 IP 第4版（IPv4）位址172.16.4.0 會顯示為 172.16.4.0/22。 除了 IPv4 位址之外，Windows Server 2008 也支援 IP 版本6（IPv6）子網首碼，例如3FFE： FFFF：0： C000：：/64。 如需有關每個位置中 IP 子網的詳細資訊，請參閱[收集網路資訊](../../ad-ds/plan/Collecting-Network-Information.md)和[附錄 A：位置和子網首碼](Appendix-A--Locations-and-Subnet-Prefixes.md)中的「位置和子網」（DSSTOPO_2 .doc）工作表。  
  
將每個子網物件與網站物件產生關聯，方法是參考「決定哪些位置會變成網站」一節中的「建立子網與網站之間的關聯」（DSSTOPO_6 .doc）工作表，判斷哪個子網要與哪個網站相關聯。 記錄與「將子網與網站建立關聯」（DSSTOPO_6 .doc）工作表中每個位置相關聯的 Active Directory 子網物件。  
  
如需如何建立子網物件的詳細資訊，請參閱[建立子網一](https://go.microsoft.com/fwlink/?LinkId=107068)文。
