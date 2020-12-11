---
description: 深入瞭解：新增 Token-Signing 憑證
ms.assetid: bbb84ea6-7e31-4442-85ab-a9447e7c19e8
title: 新增權杖簽署憑證
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 16db3238d8d3dcac55f228a5a9cca80ccf3ca132
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97048136"
---
# <a name="add-a-token-signing-certificate"></a>新增權杖簽署憑證


Active Directory 同盟服務 AD FS 中的同盟伺服器 \( \) 需要權杖 \- 簽署憑證，以防止攻擊者在嘗試取得同盟資源的未經授權存取時，改變或偽造安全性權杖。 每個權杖 \- 簽署憑證都包含密碼編譯私密金鑰和公開金鑰，這些金鑰是用來透過 \( 私密金鑰以安全性權杖來以數位方式簽署 \) 。 之後，合作夥伴同盟伺服器收到這些金鑰之後，就會透過 \( 加密安全性權杖的公開金鑰來驗證真實性 \) 。

> [!CAUTION]
> 用於權杖簽署的憑證對 \- 同盟服務的穩定性而言是不可或缺的。 因為遺失或未計畫移除任何針對此用途設定的憑證可能會中斷服務，所以您應該備份任何針對此用途設定的憑證。

權杖 \- 簽署憑證應該會連結至同盟服務中的受根信任。 您可以使用下列程式，將權杖 \- 簽署憑證新增至已匯出的檔案中的 AD FS 管理嵌入式管理單元 \- 。

若要完成此程序，至少需要本機電腦之 **Administrators** 群組的成員資格或同等權限。  請參閱[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477) \( HTTP： \/ \/ go.microsoft.com \/ fwlink \/ 使用適當帳戶和群組成員資格的詳細資料：LinkId \= 83477 \) 。

### <a name="to-add-a-token-signing-certificate"></a>新增權杖 \- 簽署憑證

1.  在 [ **開始** ] 畫面上，輸入 **AD FS 管理**]，然後按 enter。

2.  在主控台樹中，按兩下 \- [ **服務**]，然後按一下 [ **憑證**]。

3.  在 [ **動作** ] 窗格中，按一下 [ **新增權杖 \- 簽署憑證** ] 連結。

4.  在 [ **流覽憑證** 檔案] 對話方塊中，流覽至您要新增的憑證檔案，選取憑證檔案，然後按一下 [ **開啟**]。

## <a name="additional-references"></a>其他參考資料
[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)

[同盟伺服器的憑證需求](../design/certificate-requirements-for-federation-servers.md)

