---
ms.assetid: 33b80a3f-67f3-4da7-ac4a-7fd2232fbd5d
title: 使用 WID 的獨立同盟伺服器
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 7253691cff4cc83032e4a345682ca1e43bafddc4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858771"
---
# <a name="stand-alone-federation-server-using-wid"></a>使用 WID 的獨立同盟伺服器

Active Directory 同盟服務 \(AD FS 中的獨立\-獨立同盟伺服器是由裝載\) 設定為使用 Windows 內部資料庫同盟服務 WID \(的單一伺服器所組成。\) 此 AD FS 的拓撲適用于測試實驗室。 我們不建議在生產環境中使用它，因為它的限制只有一部同盟伺服器，而且不能用來相應增加至更多伺服器。  
  
如果您想要在測試實驗室中新增其他同盟伺服器，您必須部署本節稍後所述的任何其他拓撲，從頭重建同盟服務。 因此，我們建議您在私人測試網路中，針對測試實驗室或\-概念環境的證明\-使用此拓撲，如下圖所示。  
  
![使用 WID 的伺服器](media/FedServerWID.gif)  
  
## <a name="test-lab-considerations"></a>測試實驗室考慮  
本節描述與此適用于測試實驗室環境之拓撲相關聯的目標物件、優點和限制的各種考慮。  
  
### <a name="who-should-use-this-topology"></a>誰應該使用此拓撲？  
  
-   資訊技術 \(IT\) 專業人員或 IT 架構人員，想要評估或開發這項技術的概念證明  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>使用此拓撲的優點有哪些？  
  
-   在測試實驗室環境中輕鬆設定  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>使用此拓撲的限制為何？  
  
-   每個同盟服務只有一部同盟伺服器 \(無法相應增加至伺服器陣列\)  
  
-   不是多餘的 \(只有 AD FS 設定資料庫的單一實例存在\)  
  

## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
