---
ms.assetid: c8597cc8-bdcb-4e59-a09e-128ef5ebeaf8
title: 命令列程序稽核
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: d08032a01a1c3bf2fd03ba302d6eaba5162c317d
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93070740"
---
# <a name="command-line-process-auditing"></a>命令列程序稽核

>適用于： Windows Server 2016、Windows Server 2012 R2

**作者** ： Justin Turner，與 Windows 群組的資深支援擴大工程師

> [!NOTE]
> 本內容由 Microsoft 客戶支援工程師編寫，適用對象為經驗豐富的系統管理員和系統架構​​師，如果 TechNet 提供的主題已無法滿足您，您要找的是 Windows Server 2012 R2 中功能和解決方案的更深入技術講解，則您是本文的適用對象。 不過，本文未經過相同的編輯階段，因此部分語句也許不如 TechNet 文章那樣洗鍊。

## <a name="overview"></a>概觀

-   預先存在的進程建立審核事件識別碼4688現在會包含命令列程式的 audit 資訊。

-   它也會在 Applocker 事件記錄檔中記錄可執行檔的 SHA1/2 雜湊

    -   應用程式和服務 Logs\Microsoft\Windows\AppLocker

-   您可以透過 GPO 啟用，但預設為停用

    -   「在進程建立事件中包含命令列」

![命令列的審核](media/Command-line-process-auditing/GTR_ADDS_Event4688.gif)

**圖 SEQ 圖 \\ \* 阿拉伯文16事件4688**

請參閱 REF _Ref366427278，圖16中的更新事件識別碼4688。  在此更新之前，不會記錄 **進程命令列** 的任何資訊。  由於這項額外的記錄，我們現在可以看到，這並不只是 wscript.exe 的進程開始，還可以用來執行 VB 腳本。

## <a name="configuration"></a>組態
若要查看此更新的影響，您將需要啟用兩個原則設定。

### <a name="you-must-have-audit-process-creation-auditing-enabled-to-see-event-id-4688"></a>您必須啟用 Audit Process 建立審核才能看到事件識別碼4688。
若要啟用 Audit Process 建立原則，請編輯下列群組原則：

**原則位置：** 電腦配置 > 原則 > Windows 設定 > 安全性設定 > Advanced Audit Configuration > 詳細追蹤

**原則名稱：** 建立 Audit Process

**支援：** Windows 7 和更新版本

**Description/Help：**

這個安全性原則設定會決定作業系統是否會在建立進程時產生 audit 事件 (啟動) ，以及建立程式的程式或使用者的名稱。

這些 audit 事件可協助您瞭解電腦的使用方式，以及追蹤使用者活動。

事件磁片區：低至中，視系統使用狀況而定

**預設值：** 未設定

### <a name="in-order-to-see-the-additions-to-event-id-4688-you-must-enable-the-new-policy-setting-include-command-line-in-process-creation-events"></a>若要查看事件識別碼4688的新增專案，您必須啟用新的原則設定：在處理常式建立事件中包含命令列
**表 SEQ 資料表 \\ \* 阿拉伯文19命令列進程原則設定**

|原則設定|詳細資料|
|------------------------|-----------|
|**路徑**|系統管理 Templates\System\Audit 進程建立|
|**設定**|**在進程建立事件中包含命令列**|
|**預設設定**|未設定 (未啟用) |
|**支援於：**|?|
|**說明**|此原則設定會決定在建立新的進程時，哪些資訊會記錄在安全性審核事件中。<p>這項設定僅適用于啟用審核程式建立原則時。 如果您啟用此原則設定，每個處理常式的命令列資訊都會以純文字的形式記錄在安全性事件記錄檔中，做為審核程式建立事件4688的一部分，也就是套用此原則設定的工作站和伺服器上的「已建立新的進程」。<p>如果您停用或未設定此原則設定，進程的命令列資訊將不會包含在 Audit Process 建立事件中。<p>預設值：未設定<p>注意：啟用此原則設定時，具有讀取安全性事件之存取權的任何使用者都可以讀取任何已成功建立之進程的命令列引數。 命令列引數可以包含機密或私用資訊，例如密碼或使用者資料。|

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

## <a name="try-this-explore-command-line-process-auditing"></a>試試看：探索命令列進程審核

1.  啟用 **Audit Process 建立** 事件，並確保不會覆寫事先稽核原則設定

2.  建立會產生一些感興趣事件的腳本，並執行腳本。  觀察事件。  在課程中用來產生事件的腳本看起來像這樣：

    ```
    mkdir c:\systemfiles\temp\commandandcontrol\zone\fifthward
    copy \\192.168.1.254\c$\hidden c:\systemfiles\temp\hidden\commandandcontrol\zone\fifthward
    start C:\systemfiles\temp\hidden\commandandcontrol\zone\fifthward\ntuserrights.vbs
    del c:\systemfiles\temp\*.* /Q
    ```

3.  啟用命令列進程審核

4.  執行與之前相同的腳本，並觀察事件

