---
ms.assetid: 10d6723e-c857-43da-9d2d-acb5641d3da8
title: 將電腦加入網域
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: edb02a623998f2bac3621f201267e76fdcf3e1dd
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969745"
---
# <a name="join-a-computer-to-a-domain"></a>將電腦加入網域

\(若要讓 Active Directory 同盟服務 AD FS \) 運作，每一部做為同盟伺服器的電腦都必須加入網域。 同盟伺服器 proxy 可能已加入網域，但這不是必要條件。

如果網頁伺服器只裝載宣告感知應用程式，您就不需要將網頁伺服器加入網域 \- 。

若要完成此程序，至少需要本機電腦之 **Administrators** 群組的成員資格或同等權限。  請參閱在[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中使用適當帳戶和群組成員資格的詳細資料。

### <a name="to-join-a-computer-to-a-domain"></a>將電腦加入網域

1.  在 [**開始**] 畫面上，輸入 [**控制台**]，然後按 enter 鍵。

2.  流覽至 [**系統及安全性**]，然後按一下 [**系統**]。

3.  在 [電腦名稱、網域及工作群組設定]**** 底下，按一下 [變更設定]****。

4.  在 [電腦名稱]**** 索引標籤中，按一下 [變更]****。

5.  在 [**成員隸屬**] 底下，按一下 [**網域**]，輸入您希望這部電腦加入的功能變數名稱，然後按一下 **[確定]**。

6.  按一下 [確定]****，然後重新啟動電腦。

## <a name="additional-references"></a>其他參考資料
[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)

[檢查清單：設定同盟伺服器 Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)


