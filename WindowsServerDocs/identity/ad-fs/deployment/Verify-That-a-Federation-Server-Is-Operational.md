---
ms.assetid: ad61c586-ba8a-4534-8824-b45994d60c6b
title: 驗證同盟伺服器運作正常
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: d33c4d6b7c674e8a1029f47eb4cfffdfc2d61f7e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969725"
---
# <a name="verify-that-a-federation-server-is-operational"></a>驗證同盟伺服器運作正常


您可以使用下列程序來確認同盟伺服器可以運作，也就是說，相同網路上的所有用戶端都可以連線到新的同盟伺服器。

本機電腦上 **Users**、**Backup Operators**、**Power Users**、**Administrators** 或具有同等權限的成員資格，至少需要完成此程序。  請參閱在[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中使用適當帳戶和群組成員資格的詳細資料。

### <a name="procedure-1-to-verify-that-a-federation-server-is-operational"></a>程式1：驗證同盟伺服器是否可運作

1.  若要確認已 \( 在 \) 同盟伺服器上正確設定 Internet Information Services IIS，請登入與同盟伺服器位於相同樹系中的用戶端電腦。

2.  開啟瀏覽器視窗，在網址列中輸入同盟伺服器的 DNS 主機名稱，然後為新的同盟伺服器附加/adfs/fs/federationserverservice.asmx，例如：

    **https://fs1.fabrikam.com/adfs/fs/federationserverservice.asmx**

3.  按 ENTER 鍵，然後在同盟伺服器電腦上完成下一個程序。 如果您看到 [**此網站的安全性憑證有問題**] 訊息，請按一下 [**繼續流覽此網站**]。

    預期的輸出是顯示 XML 以及服務描述文件。 如果出現此頁面，表示同盟伺服器上的 IIS 可以運作，而且成功提供頁面服務。

若要完成此程序，至少需要本機電腦之 **Administrators** 群組的成員資格或同等權限。  請參閱在[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中使用適當帳戶和群組成員資格的詳細資料。

### <a name="procedure-2-to-verify-that-a-federation-server-is-operational"></a>程式2：驗證同盟伺服器是否可運作

1.  以管理員身分登入新的同盟伺服器。

2.  在 [開始]**** 畫面上輸入 **Event Viewer**，然後按 ENTER 鍵。

3.  在詳細資料窗格中，按兩下 [ \- **應用程式及服務記錄**檔]，再按兩下 [ \- **AD FS 事件**]，然後按一下 [系統**管理員**]。

4.  在 [**事件識別碼**] 欄中，尋找事件識別碼100。 如果同盟伺服器的設定正確，您將會看見事件識別碼為 100 的新事件 (位於事件檢視器的 [應用程式] 記錄檔中)。 此事件可驗證同盟伺服器已能夠與 Federation Service 進行通訊。

## <a name="additional-references"></a>其他參考資料
[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)


