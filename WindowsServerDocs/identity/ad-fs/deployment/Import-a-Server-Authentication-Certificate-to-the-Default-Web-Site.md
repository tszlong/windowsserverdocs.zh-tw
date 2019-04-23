---
ms.assetid: e1f2ce2d-b24f-4ccd-8add-9e69419fc6c1
title: 將伺服器驗證憑證匯入預設的網站中
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 7bc890c744de5cd86d4e8b0418e75512518f656c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880939"
---
# <a name="import-a-server-authentication-certificate-to-the-default-web-site"></a>將伺服器驗證憑證匯入預設的網站中

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

您從憑證授權單位取得伺服器驗證憑證之後\(CA\)，您必須手動安裝該憑證預設網站上的每個同盟伺服器或伺服器陣列中的同盟伺服器 proxy。  
  
對於網頁伺服器，您必須手動在適當的網站上或您的同盟應用程式所在的虛擬目錄中安裝該伺服器驗證憑證。  
  
如果您正在設定陣列，請務必一樣要使用相同的設定，在陣列中的每部伺服器上執行此程序。  
  
> [!NOTE]  
> AD FS 管理嵌入式管理單元\-在參考同盟伺服器，做為服務通訊憑證的伺服器驗證憑證。  
  
若要完成此程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。   
  
### <a name="to-import-a-server-authentication-certificate-to-the-default-web-site"></a>將伺服器驗證憑證匯入到預設的網站  
  
1.  在 **開始**畫面上，輸入**Internet Information Services \(IIS\) Manager**，然後按 ENTER 鍵。  
  
2.  在主控台樹狀目錄中，按一下 [ComputerName]。  
  
3.  在中央窗格中，按兩下\-按一下 **伺服器憑證**。  
  
4.  在 [動作] 窗格中，按一下 [匯入]。  
  
5.  在 **匯入憑證** 對話方塊中，按一下  **...** 按鈕。  
  
6.  瀏覽到 pfx 憑證檔案的位置並選取該檔案，然後按一下 [開啟]。  
  
7.  輸入該憑證的密碼，然後按一下 [確定]。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)  
  
[檢查清單：設定同盟伺服器 Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[同盟伺服器的憑證需求](https://technet.microsoft.com/library/dd807040.aspx)  
  
[同盟伺服器 Proxy 的憑證需求](https://technet.microsoft.com/library/dd807054.aspx)  
   
  

