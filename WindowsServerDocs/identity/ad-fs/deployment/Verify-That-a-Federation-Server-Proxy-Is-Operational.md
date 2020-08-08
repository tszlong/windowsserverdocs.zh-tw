---
ms.assetid: d555041a-709e-42c7-920a-8dfd2c7e0488
title: 驗證同盟伺服器 Proxy 運作正常
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 4550934c56b4406ea7aff9b90509a205014dd95d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87940957"
---
# <a name="verify-that-a-federation-server-proxy-is-operational"></a>驗證同盟伺服器 Proxy 運作正常


您可以使用下列程式來確認同盟伺服器 proxy 可以與 Active Directory 同盟服務 AD FS 中的同盟服務進行通訊 \( \) 。 執行此程式之後，請執行此程式**AD FS 同盟伺服器 proxy 設定向導]** ，將電腦設定為在同盟伺服器 proxy 角色中執行。 如需有關如何執行此嚮導的詳細資訊，請參閱[設定電腦的同盟伺服器 Proxy 角色](Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md)。

> [!IMPORTANT]
> 此測試的結果是在同盟伺服器 Proxy 電腦上的事件檢視器中成功產生特定事件。

若要完成此程序，至少需要本機電腦之 **Administrators** 群組的成員資格或同等權限。  請參閱在[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中使用適當帳戶和群組成員資格的詳細資料。

### <a name="to-verify-that-a-federation-server-proxy-is-operational"></a>若要驗證同盟伺服器 proxy 是否可運作

1.  以系統管理員身分登入同盟伺服器 proxy。

2.  在 [**開始**] 畫面上，輸入**事件檢視器**，然後按 enter。

3.  在詳細資料窗格中，按兩下 [ \- **應用程式及服務記錄**檔]，再按兩下 [ \- **AD FS 事件**]，然後按一下 [系統**管理員**]。

4.  在 [事件識別碼]**** 欄位中，搜尋事件識別碼 198。

    如果同盟伺服器 proxy 設定正確，您會在事件檢視器的應用程式記錄檔中看到新的事件，且事件識別碼為198。 這個事件會確認同盟伺服器 proxy 服務是否已成功啟動，且現在已上線。

## <a name="additional-references"></a>其他參考資料
[檢查清單：設定同盟伺服器 Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)


