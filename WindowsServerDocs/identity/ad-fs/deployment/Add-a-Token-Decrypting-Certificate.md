---
ms.assetid: 27e1e299-0beb-4e86-8143-1ba031dc3502
title: 新增權杖解密憑證
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 0c6e2fc3913a6a483d71441a5e2aa1f0aabb7cbb
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87947629"
---
# <a name="add-a-token-decrypting-certificate"></a>新增權杖解密憑證

\-當信賴憑證者同盟伺服器在新的憑證設定為主要解密憑證之後，必須將使用較舊憑證所簽發的權杖解密時，同盟伺服器會使用權杖解密憑證。 Active Directory 同盟服務 \( AD FS 會 \) 使用 \( Internet Information Services IIS 的安全通訊端層 SSL \) 憑證 \( \) 作為預設解密憑證。

> [!CAUTION]
> 用於權杖解密的憑證對於 \- 同盟服務的穩定性而言至關重要。 由於遺失或未計畫移除任何針對此用途所設定的憑證可能會中斷服務，因此您應該備份為此用途所設定的任何憑證。

您可以使用下列程式，將權杖 \- 解密憑證新增至您已匯出的檔案中的 [AD FS 管理] 嵌入式管理單元 \- 。

若要完成此程序，至少需要本機電腦之 **Administrators** 群組的成員資格或同等權限。  如需使用適當帳戶和群組成員資格的詳細資料，請參閱[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477) \( HTTP： \/ \/ go.microsoft.com \/ fwlink \/ ？LinkId \= 83477 \) 。

### <a name="to-add-a-token-decrypting-certificate"></a>新增權杖 \- 解密憑證

1.  在 [**開始**] 畫面上，輸入**AD FS 管理**]，然後按 enter。

2.  在主控台樹中，按兩下 \- [**服務**]，然後按一下 [**憑證**]。

3.  在 [**動作**] 窗格中，按一下 [**新增權杖 \- 解密憑證**] 連結。

4.  在 [**流覽憑證**檔案] 對話方塊中，流覽至您要新增的憑證檔案，選取憑證檔案，然後按一下 [**開啟**]。

## <a name="additional-references"></a>其他參考資料
[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)

[同盟伺服器的憑證需求](../design/certificate-requirements-for-federation-servers.md)

