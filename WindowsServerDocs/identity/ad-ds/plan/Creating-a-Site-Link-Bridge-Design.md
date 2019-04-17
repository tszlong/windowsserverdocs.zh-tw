---
ms.assetid: 64142026-07b5-4601-840a-c8dcf6ab9814
title: "建立一個網站連結橋接器設計"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 58dda7c1f56fa3799b902ab5458e71323047ec73
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="creating-a-site-link-bridge-design"></a>建立一個網站連結橋接器設計

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

網站連結橋接器連接兩個或更多的網站連結，並讓轉移之間的網站連結。 每個橋接器中的網站連結必須與其他網站連結中橋接器網站。 知識一致性檢查程式 (KCC) 搭配使用的資訊每個網站連結來計算複寫一個網站連結中的網站和橋接器其他網站連結網站之間的費用。 常見的網站的網站連結之間存在，而 KCC 也無法建立網域控制站在連接的相同的網站連結橋接器網站之間直接連接。  
  
根據預設，所有網站連結都的轉移。 我們建議您將不會變更的預設值，支援轉移**所有網站的連結，ios 都橋接器**（預設功能）。 不過，您將需要停用**所有網站的連結，ios 都橋接器**，如果完成網站連結都橋接器設計：  
  
-   完全不路由傳送您的 IP 網路。 當您停用**所有網站的連結，ios 都橋接器**和所有網站連結被都視為非轉移，以及您可以建立設定來建立您的網路的實際路由行為模型網站連結都橋接器物件。  
  
-   您需要掌控複寫 Active Directory Domain Services (AD DS) 中所做的變更。 停用**所有網站的連結，ios 都橋接器**的網站連結 IP 傳輸並設定一個網站連結都橋接器，網站連結都橋接器變成相當於斷續網路。 在網站連結橋接器所有網站連結可以都路由間接，，但不是以外的網站連結橋接器路由都傳送。  
  
如需詳細資訊，了解如何使用 Active Directory 網站和服務] 嵌入式管理單元，來停用**所有網站的連結，ios 都橋接器**設定，請讓或停用網站連結橋樑 ([https://go.microsoft.com/fwlink/?LinkId=107073](https://go.microsoft.com/fwlink/?LinkId=107073))。  
  
## <a name="controlling-ad-ds-replication-flow"></a>控制 AD DS 複寫工作流程  
有兩個案例中，您需要的網站連結橋接器設計複寫流程包含控制複寫錯誤移轉以及控制從防火牆複寫。  
  
### <a name="controlling-replication-failover"></a>控制複寫錯誤移轉  
如果您的組織已經拓撲中樞支點網路，您通常不想衛星網站，以建立複寫連接到其他衛星網站，如果中樞網站的所有網域控制站都失敗。 在這些案例中，您必須停用**所有網站的連結，ios 都橋接器**，並建立網站連結橋接器使複寫連接建立衛星網站之間只有一個或兩個躍點原位衛星網站的另一個中樞網站。  
  
### <a name="controlling-replication-through-a-firewall"></a>透過防火牆控制複寫  
如果有兩個網域控制站代表相同的網域中的兩個不同的網站專門允許彼此只是透過防火牆，您可以停用**所有網站的連結，ios 都橋接器**，並建立網站的網站連結橋接器一端相同的防火牆。 因此，如果您的網路分隔防火牆，我們建議您停用轉移的網站連結，並一方防火牆上建立的網站連結橋接網路。 管理透過防火牆複寫相關資訊，會看到網路分段防火牆在 Active Directory ([https://go.microsoft.com/fwlink/?LinkId=107074](https://go.microsoft.com/fwlink/?LinkId=107074))。  
  


