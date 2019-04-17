---
ms.assetid: 33b80a3f-67f3-4da7-ac4a-7fd2232fbd5d
title: "使用 WID 獨立聯盟伺服器"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9ec4150a7d3adfaac786219d253e1d0898c18204
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="stand-alone-federation-server-using-wid"></a>使用 WID 獨立聯盟伺服器

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在 Active Directory 同盟服務 \(AD FS\) stand\ 只聯盟伺服器單一伺服器同盟服務主機設定為使用 Windows 內部資料庫 \(WID\) 所組成。 這個 AD FS 拓撲是實驗室測試。 我們不建議使用正式作業環境因為它會有數上限是一個聯盟 server，而且不能用於更多的伺服器來放大。  
  
如果您想要加入測試實驗室其他聯盟伺服器，您必須在任何其他拓撲本節所述稍後部署重建從頭同盟服務。 因此，我們建議您使用這個拓撲實驗室或 proof\ of\ 概念環境中私人測試網路中的單一聯盟伺服器足以，如下所示。  
  
![使用 WID 伺服器](media/FedServerWID.gif)  
  
## <a name="test-lab-considerations"></a>測試 lab 注意事項  
本節各種考量有關目標對象、 優點和這種拓撲實驗室測試環境中相關聯的限制。  
  
### <a name="who-should-use-this-topology"></a>誰應該使用此拓撲？  
  
-   資訊技術 \(IT\) 專業人員或想要評估或開發證明的概念，這項技術的人 IT 架構  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>使用這個拓撲的好處為何？  
  
-   輕鬆地在實驗室測試環境中的設定  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>使用這個拓撲限制為何？  
  
-   每個同盟服務只有一個聯盟伺服器 \ （不拓展 farm\ 至的功能）  
  
-   不備援 \ （僅限一個執行個體 AD FS 設定資料庫 exists\）  
  

## <a name="see-also"></a>也了
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
