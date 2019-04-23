---
ms.assetid: 33b80a3f-67f3-4da7-ac4a-7fd2232fbd5d
title: 使用 WID 的獨立同盟伺服器
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9ec4150a7d3adfaac786219d253e1d0898c18204
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876519"
---
# <a name="stand-alone-federation-server-using-wid"></a>使用 WID 的獨立同盟伺服器

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

獨立\-單獨的同盟伺服器在 Active Directory Federation Services \(AD FS\)裝載 Federation Service 設定為使用 Windows 內部資料庫的單一伺服器所組成\(WID\). 此 AD FS 拓撲是測試實驗室。 我們不建議您這麼做為生產環境因為它有限制為只有一部同盟伺服器，而且它不能相應增加為更多的伺服器。  
  
如果您想要將其他同盟伺服器新增到您的測試實驗室，您必須重建從頭 Federation Service 部署任何其他本節稍後說明的拓撲。 因此，我們建議您在測試實驗室或證明使用此拓撲\-的\-中您所在的單一同盟伺服器已足夠，如下圖所示的私人測試網路環境。  
  
![使用 WID 的伺服器](media/FedServerWID.gif)  
  
## <a name="test-lab-considerations"></a>測試實驗室的考量  
本章節描述各種考量相關的適用對象、 權益和相關聯的測試實驗室環境使用此拓撲的限制。  
  
### <a name="who-should-use-this-topology"></a>誰應該使用此拓撲？  
  
-   資訊科技\(IT\)專業人員或 IT 架構設計人員想要評估或開發這項技術的概念證明  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>使用此拓撲的優點有哪些？  
  
-   輕鬆地在測試實驗室環境中設定  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>使用此拓撲的限制有哪些？  
  
-   只有一部同盟伺服器，每個同盟服務\(相應增加至伺服陣列沒有功能\)  
  
-   不是多餘\(AD FS 設定資料庫中的單一執行個體存在\)  
  

## <a name="see-also"></a>另請參閱
[Windows Server 2012 中 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
