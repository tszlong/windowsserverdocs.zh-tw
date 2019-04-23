---
ms.assetid: c8597cc8-bdcb-4e59-a09e-128ef5ebeaf8
title: 命令列程序稽核
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 66ae6992775319cf614b0cb4c21f864150746687
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859549"
---
# <a name="command-line-process-auditing"></a>命令列程序稽核

>適用於：Windows Server 2016, Windows Server 2012 R2

**作者**:Justin Turner 資深支援高階工程師，與 Windows 群組  
  
> [!NOTE]  
> 本內容由 Microsoft 客戶支援工程師編寫，適用對象為經驗豐富的系統管理員和系統架構​​師，如果 TechNet 提供的主題已無法滿足您，您要找的是 Windows Server 2012 R2 中功能和解決方案的更深入技術講解，則您是本文的適用對象。 不過，本文未經過相同的編輯階段，因此部分語句也許不如 TechNet 文章那樣洗鍊。  
  
## <a name="overview"></a>總覽  
  
-   預先存在的處理程序建立稽核事件識別碼 4688 現在將包含命令列程序的稽核資訊。  
  
-   它也會在 Applocker 事件記錄檔中記錄的可執行檔/2 的 SHA1 雜湊  
  
    -   應用程式和服務 Logs\Microsoft\Windows\AppLocker  
  
-   您啟用透過 GPO，但是它預設會停用  
  
    -   [包含在程序建立事件的命令列]  
  
![命令列稽核](media/Command-line-process-auditing/GTR_ADDS_Event4688.gif)  
  
**圖 SEQ Figure \\ \*阿拉伯文 16 事件 4688**  
  
檢閱更新的事件識別碼 4688 中 REF _Ref366427278 \h 圖 16。  在此之前更新的資訊沒有任何**程序的命令列**取得記錄。  這個額外的記錄功能因為我們現在可以看到，不僅 wscript.exe 程序已啟動，但也是用來執行 Vbscript 指令碼。  
  
## <a name="configuration"></a>組態  
若要查看此更新的影響，您必須啟用兩個原則設定。  
  
### <a name="you-must-have-audit-process-creation-auditing-enabled-to-see-event-id-4688"></a>您必須啟用才能看到事件識別碼 4688 的稽核程序建立稽核。  
若要啟用稽核程序建立原則，請編輯下列群組原則：  
  
**原則的位置：** 電腦設定 > 原則 > Windows 設定 > 安全性設定 > 進階稽核設定 > 詳細的追蹤  
  
**原則名稱：** 建立稽核程序  
  
**支援的作業：** Windows 7 和更新版本  
  
**描述/Help:**  
  
此安全性原則設定會決定作業系統是否會產生稽核事件時建立 （啟動） 的處理序和程式或建立它的使用者名稱。  
  
這些稽核事件可協助您了解如何使用電腦時，並追蹤使用者活動。  
  
事件的磁碟區：低到中等，根據系統的使用方式  
  
**預設值：** 未設定  
  
### <a name="in-order-to-see-the-additions-to-event-id-4688-you-must-enable-the-new-policy-setting-include-command-line-in-process-creation-events"></a>若要查看事件識別碼 4688 的新增項目，您必須啟用新的原則設定：在 程序建立事件中包含命令列  
**資料表 SEQ 表格\\\*阿拉伯文 19 命令列程序原則設定**  
  
|原則設定|詳細資料|  
|------------------------|-----------|  
|**路徑**|建立系統管理 Templates\System\Audit 處理程序|  
|**設定**|**在 程序建立事件中包含命令列**|  
|**預設設定**|設定 （未啟用）|  
|**支援的作業：**|?|  
|**描述**|此原則設定會決定在建立新的處理序時，安全性稽核事件中記錄哪些資訊。<br /><br />啟用稽核程序建立原則時，僅適用於這項設定。 如果您啟用此原則設定的命令列資訊的每個處理序將會記錄在安全性事件記錄在稽核程序建立事件 4688 中的純文字時，「 新的處理序已建立，「 工作站和這個伺服器上原則套用設定。<br /><br />如果您停用或未設定此原則設定時，處理程序的命令列資訊將不會包含在稽核程序建立事件。<br /><br />Default：未設定<br /><br />注意：啟用此原則設定時，存取任何使用者，讀取安全性事件都能夠成功讀取的任何命令列引數就會建立處理程序。 命令列引數可以包含敏感或私人的資訊，例如密碼或使用者資料。|  
  
![命令列稽核](media/Command-line-process-auditing/GTR_ADDS_IncludeCLISetting.gif)  
  
當您使用 [進階稽核原則設定] 設定時，必須確認這些設定不會被基本稽核原則設定覆寫。  設定會覆寫時，會記錄事件 4719。  
  
![命令列稽核](media/Command-line-process-auditing/GTR_ADDS_Event4719.gif)  
  
下列程序顯示如何透過封鎖任一基本稽核原則設定的套用，以防止發生衝突。  
  
### <a name="to-ensure-that-advanced-audit-policy-configuration-settings-are-not-overwritten"></a>確定進階稽核原則設定的設定不會被覆寫  
![命令列稽核](media/Command-line-process-auditing/GTR_ADDS_AdvAuditPolicy.gif)  
  
1.  開啟 群組原則管理主控台  
  
2.  以滑鼠右鍵按一下 [預設網域原則]，然後按一下 [編輯]。  
  
3.  連按兩下 電腦設定中，按兩下原則，然後按兩下 Windows 設定。  
  
4.  按兩下 安全性設定，按兩下 本機原則，然後按一下 安全性選項。  
  
5.  按兩下 稽核：強制執行稽核原則子類別設定 (Windows Vista 或更新版本) 以覆寫稽核原則類別設定，然後按一下 定義這個原則設定。  
  
6.  按一下 [已啟用]，然後按一下 [確定]。  
  
## <a name="additional-resources"></a>其他資源  
[稽核程序建立](https://technet.microsoft.com/library/dd941613(v=WS.10).aspx)  
  
[進階安全性稽核原則逐步指南](https://technet.microsoft.com/library/dd408940(v=WS.10).aspx)  
  
[AppLocker:常見問題集](https://technet.microsoft.com/library/ee619725(v=ws.10).aspx)  
  
## <a name="try-this-explore-command-line-process-auditing"></a>試試看吧：瀏覽命令列程序稽核  
  
1.  啟用**稽核程序建立**事件，並確定進階稽核原則設定不會覆寫  
  
2.  建立會產生一些感興趣的事件，並執行指令碼的指令碼。  查看的事件。  用來在課程中產生事件的指令碼看起來像這樣：  
  
    ```  
    mkdir c:\systemfiles\temp\commandandcontrol\zone\fifthward  
    copy \\192.168.1.254\c$\hidden c:\systemfiles\temp\hidden\commandandcontrol\zone\fifthward  
    start C:\systemfiles\temp\hidden\commandandcontrol\zone\fifthward\ntuserrights.vbs  
    del c:\systemfiles\temp\*.* /Q  
    ```  
  
3.  啟用命令列程序稽核  
  
4.  執行前的相同指令碼，並查看的事件  
  


