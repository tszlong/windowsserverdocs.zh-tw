---
description: 深入瞭解：使用群組原則將憑證散發至用戶端電腦
ms.assetid: cf32926a-2083-408b-a264-2cad179ed18a
title: 使用群組原則將憑證散發至用戶端電腦
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: fc053f7afebcab52862f23340653f2e27a44ad51
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97043426"
---
# <a name="distribute-certificates-to-client-computers-by-using-group-policy"></a>使用群組原則將憑證散發至用戶端電腦


您可以使用下列程式，將適當的安全通訊端層 \( SSL \) 憑證 \( 或等同的憑證（使用群組原則連結到帳戶 \) 夥伴樹系中的每一部用戶端電腦），以傳送給帳戶同盟伺服器、資源同盟伺服器和 Web 服務器的根信任。

  \( \) 若要完成此程式，至少需要 Domain admins 或 Enterprise admins 的成員資格或 Active Directory Domain Services AD DS 中的同等許可權。  請參閱[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477) \( HTTP： \/ \/ go.microsoft.com \/ fwlink \/ 使用適當帳戶和群組成員資格的詳細資料：LinkId \= 83477 \) 。

### <a name="to-distribute-certificates-to-client-computers-by-using-group-policy"></a>使用群組原則將憑證散發至用戶端電腦

1.  在帳戶夥伴組織樹系中的網域控制站上，啟動 **群組原則管理** ] 嵌入式管理單元 \- 。

2.  尋找現有的群組原則物件 \( gpo \) ，或建立新的 gpo 以包含憑證設定。 確定 GPO 與 \( \) 適當的使用者和電腦帳戶所在的網域、網站或組織單位 OU 相關聯。

3.  \-在 GPO 上按一下滑鼠右鍵，然後按一下 [**編輯**]。

4.  在主控台樹中，開啟 [ **電腦 \\ \\ 設定原則 Windows 設定 \\ 安全性設定 \\ 公開金鑰原則**]，以滑鼠右鍵 \- 按一下 **[受信任的根憑證授權** 單位]，然後按一下 [匯 **入**]。

5.  在 [ **歡迎使用憑證匯入嚮導]** 頁面上，按 **[下一步**]。

6.  在 [**要匯入** 的檔案] 頁面上，輸入適當憑證檔案的路徑（ \( 例如， \\ \\ fs1 \\ c $ \\ Fs1 .Cer），然後 \) 按 **[下一步]**。

7.  在 [ **憑證存放區** ] 頁面上，按一下 **[將所有憑證放入以下的存放區**]，然後按 **[下一步]**。

8.  在 [ **完成憑證匯入嚮導]** 頁面上，確認您提供的資訊正確無誤，然後按一下 **[完成]**。

9. 重複步驟2到6，為伺服器陣列中的每部同盟伺服器新增額外的憑證。
