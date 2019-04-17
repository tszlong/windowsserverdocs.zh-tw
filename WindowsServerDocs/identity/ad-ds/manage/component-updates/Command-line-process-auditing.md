---
ms.assetid: c8597cc8-bdcb-4e59-a09e-128ef5ebeaf8
title: "命令列處理程序稽核"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 61e7a5ca2b9c00c9976e6032bb10adb1974020b0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="command-line-process-auditing"></a>命令列處理程序稽核

>適用於：Windows Server 2016、Windows Server 2012 R2

**作者**: Justin Turner 資深支援工程師視窗群組  
  
> [!NOTE]  
> 本文由 Microsoft 客戶支援工程師撰寫，以及適用於系統管理員經驗和系統設計師超過參考 TechNet 上的主題通常會提供深入的技術解釋的功能與 Windows Server 2012 R2 方案正在尋找。 不過，尚未經歷相同編輯行程，以便某些語言的似乎比哪些通常位於 TechNet 較少的外觀。  
  
## <a name="overview"></a>概觀  
  
-   現有處理程序建立稽核事件 ID 4688 現在將會包含稽核資訊的命令列處理程序。  
  
-   它也可執行檔 SHA1 2 月 hash 登入 Applocker 事件登入  
  
    -   應用程式與服務 Logs\Microsoft\Windows\AppLocker  
  
-   您可以透過 GPO，但預設停用  
  
    -   「在 [處理程序建立事件包含命令列]  
  
![稽核命令列](media/Command-line-process-auditing/GTR_ADDS_Event4688.gif)  
  
**圖 7 圖 \\\ * 阿拉伯文 16 事件 4688**  
  
檢視已更新的事件編號 4688 參考 _Ref366427278 \h 圖 16 中。  之前此更新的資訊適用於**處理程序命令列**取得登入。  因為這其他登入我們現在會看到不只開始使用 wscript.exe 程序，但也是用來執行描述。  
  
## <a name="configuration"></a>設定  
若要查看這項更新的效果，您必須支援兩個原則設定。  
  
### <a name="you-must-have-audit-process-creation-auditing-enabled-to-see-event-id-4688"></a>您必須建立稽核程序稽核查看事件 ID 4688 支援。  
若要讓的建立程序的稽核原則，編輯下列群組原則：  
  
**原則的位置：**電腦設定 > 原則 > Windows 設定 > 安全性設定 > 進階稽核設定 > 詳細追蹤  
  
**原則的名稱︰**稽核建立程序  
  
**支援的：** Windows 7 及以上  
  
**描述日協助：**  
  
此安全性原則設定判斷是否作業系統產生稽核事件處理程序建立（開始）時，使用者建立該程式的名稱。  
  
這些稽核事件可協助您了解如何使用電腦以及追蹤使用者活動。  
  
事件磁碟區：低根據使用量系統中，  
  
**預設：**未設定  
  
### <a name="in-order-to-see-the-additions-to-event-id-4688-you-must-enable-the-new-policy-setting-include-command-line-in-process-creation-events"></a>您必須以查看新增事件 4688 編號的項目，讓新的原則設定：在 [處理程序建立事件包含命令列  
**表格 7 表格 \\\ * 阿拉伯文 19 命令列處理程序原則設定**  
  
|原則設定|詳細資料|  
|------------------------|-----------|  
|**路徑**|建立系統 Templates\System\Audit 程序|  
|**設定**|**在 [處理程序建立事件包含命令列**|  
|**預設設定**|不已設定（不支援）|  
|**支援：**|?|  
|**描述**|這項原則設定判斷資訊登入安全性稽核事件時已建立新的處理程序。<br /><br />這個設定只適用於時支援的建立程序的稽核原則。 如果您可讓每個程序的命令列資訊將會登入一般的安全性事件木頭中的一部分稽核建立程序事件 4688 這項原則設定，「新的處理程序已建立，「工作站和的伺服器上套用這項原則設定。<br /><br />如果您可以停用或未設定這項原則設定，將不會稽核建立程序活動中包含處理程序的命令列的資訊。<br /><br />未設定預設值：<br /><br />注意：所有使用者存取朗讀的安全性事件將能順利讀取任何的命令列引數當這個原則設定時，都建立程序。 命令列引數可能包含例如密碼或使用者資料的機密或私人資訊。|  
  
![稽核命令列](media/Command-line-process-auditing/GTR_ADDS_IncludeCLISetting.gif)  
  
當您使用進階稽核原則設定時，您需要確認，這些設定不會覆寫基本稽核原則設定。  事件 4719 登時就會覆寫設定。  
  
![稽核命令列](media/Command-line-process-auditing/GTR_ADDS_Event4719.gif)  
  
下列程序如何防止衝突封鎖任何基本稽核原則設定的應用程式。  
  
### <a name="to-ensure-that-advanced-audit-policy-configuration-settings-are-not-overwritten"></a>若要確保不會覆寫進階稽核原則設定  
![稽核命令列](media/Command-line-process-auditing/GTR_ADDS_AdvAuditPolicy.gif)  
  
1.  打開群組原則管理主控台  
  
2.  預設網域原則，以滑鼠右鍵按一下，然後按一下 [編輯。  
  
3.  按兩下 [電腦設定，按兩下原則]，然後按兩下 [Windows 設定。  
  
4.  按兩下 [安全性設定，按兩下 [本機原則]，然後按一下安全性選項。  
  
5.  按兩下稽核：推動稽核原則子分類設定 (Windows Vista 或更新版本) 要覆寫稽核原則分類設定，然後按一下 [此原則。  
  
6.  按兩下、，然後按一下 [確定]  
  
## <a name="additional-resources"></a>其他資源  
[稽核建立程序](https://technet.microsoft.com/library/dd941613(v=WS.10).aspx)  
  
[進階安全性稽核原則 Step-by-Step 指南](https://technet.microsoft.com/library/dd408940(v=WS.10).aspx)  
  
[AppLocker：常見問題集](https://technet.microsoft.com/library/ee619725(v=ws.10).aspx)  
  
## <a name="try-this-explore-command-line-process-auditing"></a>試試看：探索命令列處理程序稽核  
  
1.  讓**稽核建立程序**事件，確保您不會覆寫進階稽核原則設定  
  
2.  建立指令碼，將會產生感興趣的一些事件執行指令碼。  觀察到的事件。  事件產生課程中使用的指令碼看起來像這樣：  
  
    ```  
    mkdir c:\systemfiles\temp\commandandcontrol\zone\fifthward  
    copy \\192.168.1.254\c$\hidden c:\systemfiles\temp\hidden\commandandcontrol\zone\fifthward  
    start C:\systemfiles\temp\hidden\commandandcontrol\zone\fifthward\ntuserrights.vbs  
    del c:\systemfiles\temp\*.* /Q  
    ```  
  
3.  讓命令列稽核處理程序  
  
4.  執行之前，請先相同做為指令碼，並觀察到的事件  
  


