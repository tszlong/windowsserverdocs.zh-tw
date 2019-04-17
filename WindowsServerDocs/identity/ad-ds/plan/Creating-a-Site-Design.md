---
ms.assetid: 83f746e5-81db-4610-9977-1d5c57699f50
title: "建立一個網站設計"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2bcbf30159721e1fc2e12af103ca6f3e79f3ec97
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="creating-a-site-design"></a>建立一個網站設計

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

建立網站設計包含決定的位置將會變成網站、建立網站物件、建立物件子網路，以及子網路關聯的網站。  
  
## <a name="deciding-which-locations-will-become-sites"></a>決定即將網站的位置。  
選擇的位置建立網站的方式如下：  
  
-   建立網站的所有的計劃放置網域控制站的位置。 請參考」網域控制站的位置」(DSSTOPO_4.doc) 試算表找出加入網域控制站的位置所述的資訊。  
  
-   建立這些位置，包括執行需要建立網站的應用程式的伺服器的網站。 某些應用程式，例如散發檔案系統命名空間 (DFSN)，使用網站物件找出最近戶端伺服器。  
  
    > [!NOTE]  
    > 如果您的組織會有多個網路相近快速且可靠的連接，您可以在單一 Active Directory 網站包含所有的網路的子網路。 例如，如果來回退貨網路之間兩部伺服器在不同的延遲子網路 10 ms 或較少，您可以在相同的 Active Directory 網站包含這兩個子網路。 如果有兩個位置間網路延遲超過 10 ms，您不應包含子網路中單一 Active Directory 網站。 即使在延遲 10 ms 或較少，您可以選擇要部署不同的地點，如果您想要分割網站的 Active Directory 應用程式間的流量。  
  
-   如果網站不需要的位置，新增的網站的位置已的最大的寬形區域 (wan) 速度和可用的頻寬位置的子網路。  
  
將會變成網站及網路位址，以及在每個位置的子網路遮罩文件位置。 試算表中列出的網站來協助您，會看到工作協助工具的 Windows Server 2003 部署套件 ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558))，下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，並左」關聯子網路與網站」(DSSTOPO_6.doc)。  
  
## <a name="creating-a-site-object-design"></a>建立一個網站物件設計  
每個您有要建立的網站的位置，計劃在 Active Directory Domain Services (AD DS) 中建立網站物件。 將會變成網站的 [關聯子網路與網站「工作表的文件位置。  
  
如需如何建立網站物件的詳細資訊，請建立一個網站 ([https://go.microsoft.com/fwlink/?LinkId=107067](https://go.microsoft.com/fwlink/?LinkId=107067))。  
  
## <a name="creating-a-subnet-object-design"></a>建立子網路物件設計  
針對每一個的 IP 子網路和相關的每個位置的子網路遮罩，計劃中 AD DS 代表在網站中的 IP 位址建立子網路物件。  
  
在建立時 Active Directory 物件子網路，網路的 IP 子網路和子網路遮罩有關的資訊會自動被翻譯成網路首碼長度記號格式<IP address>/<prefix length>。 例如，網路 IP 版本 4 (IPv4) 位址 172.16.4.0 子網路遮罩 255.255.252.0 顯示為 172.16.4.0 月 22。 除了 IPv4 位址，Windows Server 2008 也支援 6 (IPv6) 版本的 IP 子網路首碼，例如 3FFE:FFFF:0:C000: 110:: / 64。 如需在每個位置的 IP 子網路，查看」的位置，子網路「(DSSTOPO_2.doc) 試算表中[收集網路資訊](../../ad-ds/plan/Collecting-Network-Information.md)和[附錄答位置和子網路首碼](Appendix-A--Locations-and-Subnet-Prefixes.md)。  
  
將網站物件所指的「建立關聯子網路的網站] (DSSTOPO_6.doc) 試算表」決定的位置將會變成網站」一節，以判斷哪一個子網路中的每個子網路物件就是相關聯的網站。 相關的 [關聯子網路與網站」(DSSTOPO_6.doc) 工作表中每個位置的子網路 Active Directory 物件的文件。  
  
如需如何建立物件子網路，請建立一個子網路 ([https://go.microsoft.com/fwlink/?LinkId=107068](https://go.microsoft.com/fwlink/?LinkId=107068))。  
  


