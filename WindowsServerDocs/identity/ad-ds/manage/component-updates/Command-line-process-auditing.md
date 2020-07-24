---
ms.assetid: c8597cc8-bdcb-4e59-a09e-128ef5ebeaf8
title: 命令列程序稽核
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 5ca29f1eef61bd11b2ceede4f335c029412e7331
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966400"
---
# <a name="command-line-process-auditing"></a>命令列程序稽核

>適用于： Windows Server 2016、Windows Server 2012 R2

**作者**： Justin Turner，具備 Windows 群組的資深支援提升工程師  
  
> [!NOTE]  
> 本內容由 Microsoft 客戶支援工程師編寫，適用對象為經驗豐富的系統管理員和系統架構​​師，如果 TechNet 提供的主題已無法滿足您，您要找的是 Windows Server 2012 R2 中功能和解決方案的更深入技術講解，則您是本文的適用對象。 不過，本文未經過相同的編輯階段，因此部分語句也許不如 TechNet 文章那樣洗鍊。  
  
## <a name="overview"></a>概觀  
  
-   預先存在的進程建立 audit 事件識別碼4688現在會包含命令列處理常式的審核資訊。  
  
-   它也會在 Applocker 事件記錄檔中記錄可執行檔的 SHA1/2 雜湊  
  
    -   應用程式和服務 Logs\Microsoft\Windows\AppLocker  
  
-   您可透過 GPO 啟用，但預設為停用  
  
    -   「在進程建立事件中包含命令列」  
  
![命令列的審核](media/Command-line-process-auditing/GTR_ADDS_Event4688.gif)  
  
**圖 SEQ 圖 \\ \* 阿拉伯文16事件4688**  
  
在 REF _Ref366427278 \h [圖 16] 中查看更新的事件識別碼4688。  在此更新之前，不會記錄**處理常式命令列**的任何資訊。  基於這項額外的記錄，我們現在可以看到，這不僅是啟動 wscript.exe 程式，也用來執行 VB 腳本。  
  
## <a name="configuration"></a>組態  
若要查看此更新的效果，您必須啟用兩個原則設定。  
  
### <a name="you-must-have-audit-process-creation-auditing-enabled-to-see-event-id-4688"></a>您必須啟用「Audit 進程建立」審核，才能看到事件識別碼4688。  
若要啟用 Audit 進程建立原則，請編輯下列群組原則：  
  
**原則位置：** 電腦設定 > 原則 > Windows 設定 > 安全性設定 > Advanced Audit Configuration > 詳細追蹤  
  
**原則名稱：** 建立 Audit 進程  
  
**支援于：** Windows 7 和更新版本  
  
**描述/說明：**  
  
此安全性原則設定可決定作業系統是否會在建立進程時產生 audit 事件（啟動），以及建立程式的程式或使用者名稱。  
  
這些 audit 事件可協助您瞭解電腦的使用方式，以及追蹤使用者活動的方式。  
  
事件量：低到中，視系統使用量而定  
  
**預設：** 未設定  
  
### <a name="in-order-to-see-the-additions-to-event-id-4688-you-must-enable-the-new-policy-setting-include-command-line-in-process-creation-events"></a>為了查看事件識別碼4688的新增專案，您必須啟用新的原則設定： [在進程建立事件中包含命令列]。  
**資料表 SEQ 資料表 \\ \* 阿拉伯文19命令列程式原則設定**  
  
|原則設定|詳細資料|  
|------------------------|-----------|  
|**路徑**|系統管理 Templates\System\Audit 進程的建立|  
|**設定**|**在進程建立事件中包含命令列**|  
|**預設設定**|未設定（未啟用）|  
|**支援於：**|?|  
|**描述**|此原則設定可決定在建立新進程時，哪些資訊會記錄在安全性 audit 事件中。<p>此設定只適用于已啟用 Audit 進程建立原則的情況。 如果您啟用此原則設定，在套用此原則設定的工作站和伺服器上，每個處理常式的命令列資訊將會以純文字記錄在安全性事件記錄檔中，做為「建立新進程」的4688一部分。<p>如果您停用或未設定此原則設定，處理常式的命令列資訊將不會包含在 Audit 進程建立事件中。<p>預設值：未設定<p>注意：啟用此原則設定時，任何具有讀取安全性事件存取權的使用者都可以讀取任何已成功建立之進程的命令列引數。 命令列引數可以包含機密或私用資訊，例如密碼或使用者資料。|  
  
![命令列的審核](media/Command-line-process-auditing/GTR_ADDS_IncludeCLISetting.gif)  
  
當您使用 [進階稽核原則設定] 設定時，必須確認這些設定不會被基本稽核原則設定覆寫。  覆寫設定時，會記錄事件4719。  
  
![命令列的審核](media/Command-line-process-auditing/GTR_ADDS_Event4719.gif)  
  
下列程序顯示如何透過封鎖任一基本稽核原則設定的套用，以防止發生衝突。  
  
### <a name="to-ensure-that-advanced-audit-policy-configuration-settings-are-not-overwritten"></a>確定進階稽核原則設定的設定不會被覆寫  
![命令列的審核](media/Command-line-process-auditing/GTR_ADDS_AdvAuditPolicy.gif)  
  
1.  開啟群組原則管理主控台  
  
2.  在 [預設網域原則] 上按一下滑鼠右鍵，然後按一下 [編輯]。  
  
3.  依序按兩下 [電腦設定]、[原則] 及 [Windows 設定]。  
  
4.  按兩下 [安全性設定]，按兩下 [本機原則]，然後按一下 [安全性選項]。  
  
5.  按兩下 [稽核: 強制執行稽核原則子類別設定 (Windows Vista 或更新的版本) 以覆寫稽核原則類別設定]，然後按一下 [定義這個原則設定]。  
  
6.  按一下 [已啟用] ，然後按一下 [確定] 。  
  
## <a name="additional-resources"></a>其他資源  
[稽核程序建立](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941613(v=ws.10))  
  
[進階安全性稽核原則逐步指南](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd408940(v=ws.10))  
  
[AppLocker：常見問題](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee619725(v=ws.10))  
  
## <a name="try-this-explore-command-line-process-auditing"></a>試試看：探索命令列處理常式  
  
1.  啟用**Audit 進程建立**事件，並確保不會覆寫預先審核原則設定  
  
2.  建立腳本，以產生一些感的事件並執行腳本。  觀察事件。  在課程中用來產生事件的腳本看起來像這樣：  
  
    ```  
    mkdir c:\systemfiles\temp\commandandcontrol\zone\fifthward  
    copy \\192.168.1.254\c$\hidden c:\systemfiles\temp\hidden\commandandcontrol\zone\fifthward  
    start C:\systemfiles\temp\hidden\commandandcontrol\zone\fifthward\ntuserrights.vbs  
    del c:\systemfiles\temp\*.* /Q  
    ```  
  
3.  啟用命令列進程審核  
  
4.  執行與之前相同的腳本，並觀察事件  
  
