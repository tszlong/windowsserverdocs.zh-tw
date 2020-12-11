---
description: '深入瞭解： Winlogon 自動重新開機 Sign-On (ARSO) '
title: Winlogon 自動重新啟動登入 (ARSO)
ms.topic: article
ms.assetid: 15cddcfa-8a8e-45e4-bb76-b8e1a14ceac0
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: 4a9aec72bd91bc28975dea1c9ba6c28cbb512c6c
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97050216"
---
# <a name="winlogon-automatic-restart-sign-on-arso"></a>Winlogon 自動重新啟動登入 (ARSO)

>適用於：Windows Server (半年度管道)、Windows Server 2016

**作者**： Justin Turner，與 Windows 群組的資深支援擴大工程師

> [!NOTE]
> 本內容由 Microsoft 客戶支援工程師編寫，適用對象為經驗豐富的系統管理員和系統架構​​師，如果 TechNet 提供的主題已無法滿足您，您要找的是 Windows Server 2012 R2 中功能和解決方案的更深入技術講解，則您是本文的適用對象。 不過，本文未經過相同的編輯階段，因此部分語句也許不如 TechNet 文章那樣洗鍊。

## <a name="overview"></a>概觀
Windows 8 引進了鎖定畫面應用程式。  這些應用程式會在使用者的會話鎖定 (行事曆約會、電子郵件和訊息等 ) 的情況下，執行和顯示通知。  由於 Windows Update 進程而重新開機的裝置無法在重新開機時顯示這些鎖定畫面通知。  有些使用者相依于這些鎖定畫面應用程式。

## <a name="whats-changed"></a>有哪些變更？
當使用者登入 Windows 8.1 裝置時，LSA 會將使用者認證儲存在加密的記憶體中，而且只能由 lsass.exe 存取。 當 Windows Update 起始自動重新開機而沒有使用者狀態時，這些認證將用來設定使用者的自動登入。 以 TCB 許可權執行為系統的 Windows Update 將會起始 RPC 呼叫來執行此動作。

在重新開機時，使用者會透過自動登入機制自動登入，然後再另外鎖定以保護使用者的會話。 鎖定將會透過 Winlogon 起始，而認證管理則由 LSA 進行。  藉由自動登入並鎖定主控台上的使用者，將會重新開機並提供使用者的鎖定畫面應用程式。

> [!NOTE]
> 在 Windows Update 引發重新開機後，會自動登入最後一個互動式使用者，並鎖定會話，讓使用者的鎖定畫面應用程式可以執行。

![顯示鎖定畫面的螢幕擷取畫面](../media/winlogon-automatic-restart-sign-on-arso/GTR_ADDS_LockScreenApp.gif)

![顯示鎖定畫面應用程式的螢幕擷取畫面](../media/winlogon-automatic-restart-sign-on-arso/GTR_ADDS_LockScreen.gif)

**快速總覽**

-   Windows Update 需要重新開機

-   電腦是否能夠重新開機 (*沒有任何執行的應用程式會遺失資料*) ？

    -   為您重新開機

    -   重新登入

    -   鎖定電腦

-   群組原則啟用或停用

    -   預設在伺服器 Sku 中停用

-   為什麼？

    -   某些更新在使用者重新登入之前無法完成。

    -   更好的使用者體驗：不需要等待15分鐘，更新才能完成安裝

-   怎麼做？ Autologon

    -   儲存密碼，使用該認證將您登入

    -   將認證儲存為分頁記憶體中的 LSA 秘密

    -   只有在啟用 BitLocker 時才能啟用

## <a name="group-policy-sign-in-last-interactive-user-automatically-after-a-system-initiated-restart"></a>群組原則：在系統起始重新開機後自動登入上一個互動式使用者
在 Windows 8.1/Windows Server 2012 R2 中，重新開機 Windows Update 之後鎖定畫面使用者的自動登入，就是選擇使用伺服器 Sku 並退出用戶端 Sku。

**原則位置：** 電腦設定 > 原則 > 系統管理範本 > Windows 元件 > Windows 登入選項

**原則名稱：** 在系統起始重新開機後自動登入上一個互動式使用者

**支援：** 至少 Windows Server 2012 R2、Windows 8.1 或 Windows RT 8。1

**Description/Help：**

此原則設定可控制在 Windows Update 重新開機系統之後，裝置是否會自動登入最後一個互動式使用者。

如果您啟用或未設定此原則設定，裝置會安全地儲存使用者的認證 (包括使用者名稱、網域和加密密碼) ，以在 Windows Update 重新開機後設定自動登入。 Windows Update 重新開機之後，使用者會自動登入，並且會自動鎖定會話，並針對該使用者設定所有鎖定畫面應用程式。

如果您停用此原則設定，則裝置不會儲存使用者的認證，以在 Windows Update 重新開機後自動登入。 系統重新開機之後，不會重新開機使用者的鎖定畫面應用程式。

**登錄編輯器**

|值名稱|類型|資料|
|-------|----|----|
|DisableAutomaticRestartSignOn|DWORD|0<p>**範例︰**<p>已啟用 0 () <p>1 (停用) |

**原則登錄位置：** HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System

**輸入：** Dword

登錄 **名稱：** DisableAutomaticRestartSignOn

值：0或1

0 = 已啟用

1 = 已停用

![顯示原則設定控制 UI 的螢幕擷取畫面，您可以在其中指定裝置在 Windows Update 重新開機系統後，是否會自動登入最後的互動式使用者](../media/winlogon-automatic-restart-sign-on-arso/GTR_ADDS_SignInPolicy.gif)

## <a name="troubleshooting"></a>疑難排解
當 WinLogon 自動鎖定時，WinLogon 的狀態追蹤將會儲存在 WinLogon 事件記錄檔中。

系統會記錄自動登入設定嘗試的狀態

-   如果成功

    -   將其記錄為這類

-   如果失敗：

    -   記錄失敗的原因

-   當 BitLocker 的狀態變更時：

    -   將會記錄移除認證

        -   這些會儲存在 LSA 操作記錄檔中。

### <a name="reasons-why-autologon-might-fail"></a>自動登入可能會失敗的原因
在幾種情況下，無法達到使用者自動登入。  本節的目的是要捕捉可能發生這種情況的已知案例。

### <a name="user-must-change-password-at-next-login"></a>使用者必須在下次登入時變更密碼
當需要在下次登入時進行密碼變更時，使用者登入可能會進入封鎖狀態。  在大部分情況下都可以偵測到這種情況，但並非所有 (例如，在關機和下一次登入時都可以達到密碼到期。

### <a name="user-account-disabled"></a>使用者帳戶已停用
即使已停用，也可以維護現有的使用者會話。  您可以在大部分情況下，事先在大部分的情況下于本機偵測已停用帳戶的重新開機，這可能不是網域帳戶 (某些網域快取的登入案例仍可運作，即使 DC) 上已停用帳戶也是如此。

### <a name="logon-hours-and-parental-controls"></a>登入時間和家長監護
登入時間和家長監護可以防止建立新的使用者會話。  如果在此視窗期間重新開機，則不允許使用者登入。  有其他原則會導致鎖定或登出為合規性動作。  這可能會對許多子系的情況造成問題，也就是在床時間與喚醒時可能發生帳戶鎖定的情況，特別是在這段期間內的維護期間很普遍。

## <a name="additional-resources"></a>其他資源
**表 SEQ 資料表 \\ \* 阿拉伯文3： ARSO 詞彙**

|詞彙|定義|
|----|-------|
|Autologon|自動登入是 Windows 中已有數個版本的功能。  它是 Windows 的記載功能，甚至還有 Windows v4.0 *[HTTP：/technet. sysinternals/bb963905 .aspx](/sysinternals/downloads/autologon)等工具的自動登入*<p>它可讓裝置的單一使用者自動登入，而不需要輸入認證。 認證會以加密的 LSA 秘密的形式設定和儲存在登錄中。|
