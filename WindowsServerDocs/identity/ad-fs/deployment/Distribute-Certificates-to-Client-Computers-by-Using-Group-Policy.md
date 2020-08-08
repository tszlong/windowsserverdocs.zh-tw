---
ms.assetid: cf32926a-2083-408b-a264-2cad179ed18a
title: 使用群組原則將憑證散發到用戶端電腦
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 35b543a7a249130d1fd66a0d86424b5bcb0b8d96
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87962892"
---
# <a name="distribute-certificates-to-client-computers-by-using-group-policy"></a>使用群組原則將憑證散發到用戶端電腦


您可以使用下列程式，將適當的安全通訊端層 SSL 憑證或對等憑證，使用群組原則，向下推送 \( \) 至帳戶 \( \) 夥伴樹系中的每部用戶端電腦的帳戶同盟伺服器、資源同盟伺服器和網頁伺服器的受根信任。

**Domain Admins** **Enterprise Admins** \( \) 若要完成此程式，至少需要 Domain admins 或 Enterprise Admins 的成員資格，或 Active Directory Domain Services AD DS 中的同等許可權。  如需使用適當帳戶和群組成員資格的詳細資料，請參閱[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477) \( HTTP： \/ \/ go.microsoft.com \/ fwlink \/ ？LinkId \= 83477 \) 。

### <a name="to-distribute-certificates-to-client-computers-by-using-group-policy"></a>若要使用群組原則將憑證散發到用戶端電腦

1.  在帳戶夥伴組織樹系中的網域控制站上，啟動 [**群組原則管理**] 嵌入式管理單元 \- 。

2.  尋找現有的群組原則物件 \( gpo \) ，或建立新的 gpo 以包含憑證設定。 確定 GPO 與 \( \) 適當使用者和電腦帳戶所在的網域、網站或組織單位 OU 相關聯。

3.  以滑鼠右鍵 \- 按一下 GPO，然後按一下 [**編輯**]。

4.  在主控台樹中，開啟 [電腦設定原則] [ ** \\ Windows 設定] [安全性設定] [ \\ \\ \\ 公開金鑰原則**]，以滑鼠右鍵 \- 按一下 [**信任的根憑證授權**]，然後按一下 [匯**入**

5.  在 [**歡迎使用憑證匯入嚮導]** 頁面上，按 **[下一步]**。

6.  在 [**要匯入**的檔案] 頁面上，輸入適當憑證檔案的路徑， \( 例如 \\ \\ fs1 \\ c $ \\ Fs1 .Cer \) ，然後按 **[下一步]**。

7.  在 [**憑證存放區**] 頁面上，按一下 **[將所有憑證放入下列存放區**]，然後按 **[下一步]**。

8.  在 [**正在完成憑證匯入嚮導]** 頁面上，確認您提供的資訊正確無誤，然後按一下 **[完成]**。

9. 重複步驟2到6，為伺服器陣列中的每部同盟伺服器新增額外的憑證。
