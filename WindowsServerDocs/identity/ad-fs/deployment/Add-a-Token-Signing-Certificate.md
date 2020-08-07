---
ms.assetid: bbb84ea6-7e31-4442-85ab-a9447e7c19e8
title: 新增權杖簽署憑證
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 31a624b85d68611be0661d6efcc8f33d24efdc54
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87947619"
---
# <a name="add-a-token-signing-certificate"></a>新增權杖簽署憑證


Active Directory 同盟服務中的同盟伺服器 \( AD FS \) 需要權杖 \- 簽署憑證，以防止攻擊者在嘗試未經授權存取同盟資源時改變或偽造安全性權杖。 每個權杖 \- 簽署憑證都包含密碼編譯私密金鑰和公開金鑰，這些金鑰是用來以私密金鑰做為 \( 安全性權杖，以數位方式簽署 \) 。 之後，在夥伴同盟伺服器收到這些金鑰之後，他們會藉 \( 由加密安全性權杖的公開金鑰來驗證其真實性 \) 。

> [!CAUTION]
> 用於權杖簽署的憑證對於 \- 同盟服務的穩定性而言非常重要。 由於遺失或未計畫移除任何針對此用途所設定的憑證可能會中斷服務，因此您應該備份為此用途所設定的任何憑證。

權杖 \- 簽署憑證應連結至同盟服務中受信任的根。 您可以使用下列程式，從您已匯出的檔案中，將權杖 \- 簽署憑證新增至 [AD FS 管理] 嵌入式管理單元 \- 。

若要完成此程序，至少需要本機電腦之 **Administrators** 群組的成員資格或同等權限。  如需使用適當帳戶和群組成員資格的詳細資料，請參閱[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477) \( HTTP： \/ \/ go.microsoft.com \/ fwlink \/ ？LinkId \= 83477 \) 。

### <a name="to-add-a-token-signing-certificate"></a>新增權杖 \- 簽署憑證

1.  在 [**開始**] 畫面上，輸入**AD FS 管理**]，然後按 enter。

2.  在主控台樹中，按兩下 \- [**服務**]，然後按一下 [**憑證**]。

3.  在 [**動作**] 窗格中，按一下 [**新增權杖 \- 簽署憑證**] 連結。

4.  在 [**流覽憑證**檔案] 對話方塊中，流覽至您要新增的憑證檔案，選取憑證檔案，然後按一下 [**開啟**]。

## <a name="additional-references"></a>其他參考資料
[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)

[同盟伺服器的憑證需求](../design/certificate-requirements-for-federation-servers.md)

