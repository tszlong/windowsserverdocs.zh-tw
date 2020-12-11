---
description: 深入瞭解：將伺服器驗證憑證匯入至預設的網站
ms.assetid: e1f2ce2d-b24f-4ccd-8add-9e69419fc6c1
title: 將伺服器驗證憑證匯入到預設的網站
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 390e7dcaf3277d092509a15b6c3c60363454ab7d
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97048416"
---
# <a name="import-a-server-authentication-certificate-to-the-default-web-site"></a>將伺服器驗證憑證匯入到預設的網站

從憑證授權單位單位 CA 取得伺服器驗證憑證之後 \( \) ，您必須在伺服器陣列中的每部同盟伺服器或同盟伺服器 Proxy 的預設網站上手動安裝該憑證。

對於網頁伺服器，您必須手動在適當的網站上或您的同盟應用程式所在的虛擬目錄中安裝該伺服器驗證憑證。

如果您正在設定陣列，請務必一樣要使用相同的設定，在陣列中的每部伺服器上執行此程序。

> [!NOTE]
> AD FS 管理嵌入式管理單元會 \- 將同盟伺服器的伺服器驗證憑證視為服務通訊憑證。

若要完成此程序，至少需要本機電腦之 **Administrators** 群組的成員資格或同等權限。  請參閱 [本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)使用適當帳戶和群組成員資格的詳細資料。

### <a name="to-import-a-server-authentication-certificate-to-the-default-web-site"></a>將伺服器驗證憑證匯入到預設的網站

1.  在 [ **開始** ] 畫面上，輸入 **Internet Information Services \( IIS \) 管理員**]，然後按 enter。

2.  在主控台樹狀目錄中，按一下 [ComputerName]。

3.  在中央窗格中，按兩下 \- [ **伺服器憑證**]。

4.  按一下 [動作] 窗格中的 [匯入]。

5.  在 [匯 **入憑證**] 對話方塊中，**按一下 [...** ] 按鈕。

6.  瀏覽至 pfx 憑證檔案的位置、將其醒目提示，然後按一下 [開啟]。

7.  輸入憑證的密碼，然後按一下 [確定]。

## <a name="additional-references"></a>其他參考資料
[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)

[檢查清單：設定同盟伺服器 Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)

[同盟伺服器的憑證需求](../design/certificate-requirements-for-federation-servers.md)

[同盟伺服器 Proxy 的憑證需求](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807054(v=ws.11))


