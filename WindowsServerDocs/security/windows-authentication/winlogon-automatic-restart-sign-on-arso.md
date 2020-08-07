---
title: Winlogon 自動重新啟動登入 (ARSO)
ms.topic: article
ms.assetid: 15cddcfa-8a8e-45e4-bb76-b8e1a14ceac0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 3e709c76bb1ae8c3557748d3a1e14f80fce89525
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87936458"
---
# <a name="winlogon-automatic-restart-sign-on-arso"></a>Winlogon 自動重新啟動登入 (ARSO)

>適用於：Windows Server (半年度管道)、Windows Server 2016

**作者**： Justin Turner，具備 Windows 群組的資深支援提升工程師

> [!NOTE]
> 本內容由 Microsoft 客戶支援工程師編寫，適用對象為經驗豐富的系統管理員和系統架構​​師，如果 TechNet 提供的主題已無法滿足您，您要找的是 Windows Server 2012 R2 中功能和解決方案的更深入技術講解，則您是本文的適用對象。 不過，本文未經過相同的編輯階段，因此部分語句也許不如 TechNet 文章那樣洗鍊。

## <a name="overview"></a>概觀
Windows 8 引進了鎖定畫面應用程式。  這些應用程式會在使用者的會話鎖定時執行並顯示通知 (行事曆約會、電子郵件和訊息等 ) 。  因為 Windows Update 程式而重新開機的裝置無法在重新開機時顯示這些鎖定畫面通知。  有些使用者相依于這些鎖定畫面應用程式。

## <a name="whats-changed"></a>有哪些變更？
當使用者登入 Windows 8.1 裝置時，LSA 會將使用者認證儲存在只能由 lsass.exe 存取的加密記憶體中。 當 Windows Update 起始自動重新開機而沒有使用者目前狀態時，將會使用這些認證來設定使用者的自動登入。 Windows Update 以 TCB 許可權執行為系統時，將會起始 RPC 呼叫來進行此動作。

重新開機時，使用者會自動透過自動登入機制登入，然後另外鎖定以保護使用者的會話。 鎖定會透過 Winlogon 起始，而認證管理則是由 LSA 來完成。  藉由在主控台上自動登入和鎖定使用者，使用者的鎖定畫面應用程式將會重新開機並可供使用。

> [!NOTE]
> 在 Windows Update 引發重新開機之後，最後一個互動式使用者會自動登入，而會話會被鎖定，讓使用者的鎖定畫面應用程式可以執行。

![顯示鎖定畫面的螢幕擷取畫面](../media/winlogon-automatic-restart-sign-on-arso/GTR_ADDS_LockScreenApp.gif)

![顯示鎖定畫面應用程式的螢幕擷取畫面](../media/winlogon-automatic-restart-sign-on-arso/GTR_ADDS_LockScreen.gif)

**快速概覽**

-   Windows Update 需要重新開機

-   電腦是否能夠重新開機 (*沒有執行的應用程式會遺失資料*) ？

    -   重新開機您的

    -   重新登入

    -   鎖定電腦

-   已啟用或停用群組原則

    -   伺服器 Sku 中預設為停用

-   原因為何？

    -   在使用者重新登入之前，部分更新無法完成。

    -   較佳的使用者體驗：不需要等候15分鐘的時間來完成安裝更新

-   怎麼做？ Autologon

    -   儲存密碼，使用該認證將您登入

    -   將認證儲存為分頁記憶體中的 LSA 秘密

    -   只有在啟用 BitLocker 時才能啟用

## <a name="group-policy-sign-in-last-interactive-user-automatically-after-a-system-initiated-restart"></a>群組原則：在系統起始重新開機之後自動登入最後一個互動式使用者
在 Windows 8.1/Windows Server 2012 R2 中，Windows Update 重新開機後的鎖定畫面使用者自動登入是選擇伺服器 Sku，並退出宣告用戶端 Sku。

**原則位置：** 電腦設定 > 原則 > 系統管理範本 > Windows 元件 > Windows 登入選項

**原則名稱：** 在系統起始重新開機之後自動登入最後一個互動式使用者

**支援于：** 至少要有 Windows Server 2012 R2、Windows 8.1 或 Windows RT 8。1

**描述/說明：**

此原則設定可控制裝置在 Windows Update 重新開機系統後，是否會自動登入最後一個互動式使用者。

如果您啟用或未設定此原則設定，裝置會安全地儲存使用者的認證 (包括 [使用者名稱]、[網域] 和 [加密密碼]) 設定在 Windows Update 重新開機之後自動登入。 在 Windows Update 重新開機之後，使用者會自動登入，而且會話會自動鎖定並已針對該使用者設定的所有鎖定畫面應用程式。

如果您停用此原則設定，裝置將不會儲存使用者的認證，以在 Windows Update 重新開機之後自動登入。 在系統重新開機之後，使用者的鎖定畫面應用程式不會重新開機。

**登錄編輯器**

|值名稱|類型|資料|
|-------|----|----|
|DisableAutomaticRestartSignOn|DWORD|0<p>**範例︰**<p>0 (啟用) <p>已停用 1 () |

**原則登錄位置：** HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System

**類型：** 值

登錄**名稱：** DisableAutomaticRestartSignOn

值：0或1

0 = 已啟用

1 = 已停用

![顯示原則設定控制項 UI 的螢幕擷取畫面，您可以在其中指定裝置是否會在 Windows Update 重新開機系統後，自動登入最後一個互動式使用者](../media/winlogon-automatic-restart-sign-on-arso/GTR_ADDS_SignInPolicy.gif)

## <a name="troubleshooting"></a>疑難排解
當 WinLogon 自動鎖定時，WinLogon 的狀態追蹤將會儲存在 WinLogon 事件記錄檔中。

記錄自動登入設定嘗試的狀態

-   如果成功

    -   將其記錄為

-   如果失敗：

    -   記錄失敗的內容

-   當 BitLocker 的狀態變更時：

    -   將會記錄移除認證

        -   這些會儲存在 LSA 操作記錄中。

### <a name="reasons-why-autologon-might-fail"></a>自動登登可能失敗的原因
在幾種情況下，使用者無法自動登入。  本節的目的是要用來捕捉可能發生這種情況的已知案例。

### <a name="user-must-change-password-at-next-login"></a>使用者必須在下次登入時變更密碼
使用者登入可以在下次登入時需要密碼變更時進入封鎖狀態。  在大部分情況下，您可以在重新開機前偵測到這種情況，但並非所有 (例如，關閉和下一次登入時都可以達到密碼到期。

### <a name="user-account-disabled"></a>使用者帳戶已停用
即使停用了現有的使用者會話，也可以加以維護。  在大部分情況下，可以在本機偵測到已停用之帳戶的重新開機，視 gp 而定，可能不是網域帳戶 (某些網域快取的登入案例即使在 DC) 上已停用帳戶也能正常執行。

### <a name="logon-hours-and-parental-controls"></a>登入時數和家長監護
[登入時數] 和 [家長監護] 可以禁止建立新的使用者會話。  如果在此視窗期間發生重新開機，則不允許使用者登入。  還有其他原則會導致鎖定或登出做為合規性動作。  這可能會對許多子案例造成問題，其中的帳戶鎖定可能會在床時間和喚醒之間發生，特別是在這段期間通常是在維護期間。

## <a name="additional-resources"></a>其他資源
**資料表 SEQ 資料表 \\ \* 阿拉伯文3： ARSO 詞彙**

|詞彙|定義|
|----|-------|
|Autologon|自動登入功能是已在 Windows 中針對數個版本而呈現的功能。  這是 Windows 的已記載功能，甚至還具有 Windows v4.0 * [HTTP：/technet. .com/sysinternals/bb963905](https://technet.microsoft.com/sysinternals/bb963905.aspx)等工具的自動登出。*<p>它允許裝置的單一使用者自動登入，而不需要輸入認證。 認證會以加密的 LSA 秘密設定並儲存在登錄中。|


