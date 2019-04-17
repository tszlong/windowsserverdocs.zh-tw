---
ms.assetid: 2a2f493a-9796-454a-9721-e223b799dfa7
title: "規劃森林根網域控制站的位置"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 374ce31c61c666302e2b00a8f365c289cb8799f8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="planning-forest-root-domain-controller-placement"></a>規劃森林根網域控制站的位置

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

森林根網域控制站的需要建立信任路徑利用存取自己以外的網域中的資源。 放置森林根網域控制站中樞位置和位置裝載的資料中心。 如果使用者在指定的位置需要資源存取來自於同一個位置，其他網域，而且不可靠網路可用性 datacenter 之間的使用者位置，您可以將新增的樹系根網域控制站的位置，或建立捷徑在兩個網域信任。 它是更多成本有效率除非您有其他的理由放置森林根網域控制站在這個位置中建立捷徑之間的網域信任。  
  
快顯信任最佳化驗證位於網域中的使用者要求協助。 為 [快顯信任的網域之間的相關詳細資訊，了解時看到建立信任快顯 ([https://go.microsoft.com/fwlink/?LinkId=107061](https://go.microsoft.com/fwlink/?LinkId=107061))。  
  
試算表中列出的樹系根網域控制站位置協助您，會看到工作協助工具的 Windows Server 2003 部署套件 ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558))，下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，以及打開 (DSSTOPO_4.doc)」網域控制站位置]。  
  
您將需要建立森林根網域時參考此資訊。 如需部署的樹系根網域的相關資訊，請查看[部署 Windows Server 2008 森林根網域](https://technet.microsoft.com/library/cc731174.aspx)。  
  


