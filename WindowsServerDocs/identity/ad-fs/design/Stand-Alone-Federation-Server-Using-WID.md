---
description: 深入瞭解：使用 WID Stand-Alone 同盟伺服器
ms.assetid: 33b80a3f-67f3-4da7-ac4a-7fd2232fbd5d
title: 使用 WID 的獨立同盟伺服器
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 2bb6425d77da8e39f35a84df19d1fd19c12a55f9
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049376"
---
# <a name="stand-alone-federation-server-using-wid"></a>使用 WID 的獨立同盟伺服器

\-Active Directory 同盟服務 AD FS 中的獨立同盟伺服器 \( 是 \) 由裝載設定為使用 Windows 內部資料庫 WID 之同盟服務的單一伺服器所組成 \( \) 。 此 AD FS 拓撲適用于測試實驗室。 我們不建議在生產環境中使用它，因為它只能有一部同盟伺服器，而且無法用來擴大至更多伺服器。

如果您想要將其他同盟伺服器新增至您的測試實驗室，您必須部署本節稍後所述的任何其他拓撲，從頭重建同盟服務。 因此，我們建議您在 \- \- 私人測試網路中，針對單一同盟伺服器已足夠的測試實驗室或概念證明環境使用此拓撲，如下圖所示。

![使用 WID 的伺服器](media/FedServerWID.gif)

## <a name="test-lab-considerations"></a>測試實驗室考慮
本節說明與測試實驗室環境的此拓撲相關聯之目標物件、優點和限制的各種考慮。

### <a name="who-should-use-this-topology"></a>誰應該使用此拓撲？

-   \( \) 想要評估或開發這項技術概念證明的資訊技術 IT 專業人員或 it 架構設計師

### <a name="what-are-the-benefits-of-using-this-topology"></a>使用此拓撲的優點為何？

-   在測試實驗室環境中輕鬆設定

### <a name="what-are-the-limitations-of-using-this-topology"></a>使用此拓撲有哪些限制？

-   每同盟服務只有一部同盟伺服器 \( 無法擴大至伺服器陣列\)

-   \(只有 AD FS 設定資料庫的單一實例存在時，才不會重複\)


## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
