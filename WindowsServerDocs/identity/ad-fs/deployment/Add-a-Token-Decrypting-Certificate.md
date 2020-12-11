---
description: 深入瞭解：新增 Token-Decrypting 憑證
ms.assetid: 27e1e299-0beb-4e86-8143-1ba031dc3502
title: 新增權杖解密憑證
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 86f45f534567b88ad07c1334c0536ebcfe05524e
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97047886"
---
# <a name="add-a-token-decrypting-certificate"></a>新增權杖解密憑證

\-當信賴憑證者同盟伺服器必須在新的憑證設為主要解密憑證之後，解密以較舊憑證發行的權杖時，同盟伺服器會使用權杖解密憑證。 Active Directory 同盟服務 \( AD FS \) 使用 \( Internet Information Services IIS 的安全通訊端層 SSL \) 憑證 \( \) 作為預設解密憑證。

> [!CAUTION]
> 用於權杖解密的憑證對 \- 同盟服務的穩定性而言是不可或缺的。 因為遺失或未計畫移除任何針對此用途設定的憑證可能會中斷服務，所以您應該備份任何針對此用途設定的憑證。

您可以使用下列程式，將權杖 \- 解密憑證新增至已匯出的檔案中的 AD FS 管理嵌入式管理單元 \- 。

若要完成此程序，至少需要本機電腦之 **Administrators** 群組的成員資格或同等權限。  請參閱[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477) \( HTTP： \/ \/ go.microsoft.com \/ fwlink \/ 使用適當帳戶和群組成員資格的詳細資料：LinkId \= 83477 \) 。

### <a name="to-add-a-token-decrypting-certificate"></a>新增權杖 \- 解密憑證

1.  在 [ **開始** ] 畫面上，輸入 **AD FS 管理**]，然後按 enter。

2.  在主控台樹中，按兩下 \- [ **服務**]，然後按一下 [ **憑證**]。

3.  在 [ **動作** ] 窗格中，按一下 [ **新增權杖 \- 解密憑證** ] 連結。

4.  在 [ **流覽憑證** 檔案] 對話方塊中，流覽至您要新增的憑證檔案，選取憑證檔案，然後按一下 [ **開啟**]。

## <a name="additional-references"></a>其他參考資料
[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)

[同盟伺服器的憑證需求](../design/certificate-requirements-for-federation-servers.md)

