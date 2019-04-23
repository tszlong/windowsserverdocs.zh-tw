---
ms.assetid: cb834273-828a-4141-9387-37dd8270e932
title: Winlogon 自動重新啟動登入 (ARSO)
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4024a00c6c186aa929e88cb2aa86b0ec04a731b3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883619"
---
# <a name="winlogon-automatic-restart-sign-on-arso"></a>Winlogon 自動重新啟動登入 (ARSO)

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

**作者**:Justin Turner 資深支援高階工程師，與 Windows 群組

> [!NOTE]
> 本內容由 Microsoft 客戶支援工程師編寫，適用對象為經驗豐富的系統管理員和系統架構​​師，如果 TechNet 提供的主題已無法滿足您，您要找的是 Windows Server 2012 R2 中功能和解決方案的更深入技術講解，則您是本文的適用對象。 不過，本文未經過相同的編輯階段，因此部分語句也許不如 TechNet 文章那樣洗鍊。

## <a name="overview"></a>總覽
導入的 Windows 8 鎖定螢幕應用程式。  這些是應用程式執行，並在使用者工作階段鎖定時顯示通知 （行事曆約會、 電子郵件和訊息等）。  因為 Windows 更新程序會重新啟動的裝置將無法顯示重新啟動時這些鎖定螢幕通知。  某些使用者取決於這些鎖定螢幕的應用程式。

## <a name="whats-changed"></a>變更的功能有哪些？
當使用者登入 Windows 8.1 裝置上時，LSA 會只能由 lsass.exe 就可存取的加密記憶體中儲存的使用者認證。 Windows Update 會起始自動重新開機，而不需要使用者目前狀態，這些認證將用於設定使用者自動登入中。 以系統身分執行以 TCB 權限的 Windows Update 會起始 RPC 呼叫，若要這樣做。

在重新開機時，使用者會自動登入，透過自動登入機制，此外鎖定來保護使用者的工作階段。 鎖定將會起始透過 Winlogon 認證管理由 LSA 而。  藉由自動登入，並在主控台上的鎖定使用者，使用者的鎖定螢幕的應用程式將會重新啟動且可用。

> [!NOTE]
> Windows Update 引發重新開機後，在最後一個互動式的使用者自動登入工作階段已鎖定所以可以執行使用者的鎖定畫面應用程式。

![winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/GTR_ADDS_LockScreenApp.gif)

![winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/GTR_ADDS_LockScreen.gif)

**快速概觀**

-   Windows 更新需要重新啟動

-   是能夠重新啟動電腦 (*執行的任何應用程式會失去資料*)？

    -   為您的重新啟動

    -   重新登入

    -   鎖定電腦

-   啟用或停用群組原則

    -   在 伺服器 Sku 預設為停用

-   為什麼？

    -   某些更新無法完成，直到使用者登入。

    -   更好的使用者經驗： 不需要等待 15 分鐘才能完成安裝的更新

-   如何？ AutoLogon

    -   儲存密碼，會使用該認證來登入

    -   做為 LSA 密碼，在分頁式記憶體中儲存認證

    -   只有才可以啟用已啟用 BitLocker

## <a name="group-policy-sign-in-last-interactive-user-automatically-after-a-system-initiated-restart"></a>群組原則：登入的最後一個互動式使用者系統起始的重新啟動後，自動
在 Windows 8.1 / Windows Server 2012 R2，自動登入的 Windows Update 重新啟動後鎖定螢幕使用者參加伺服器 sku，並選擇用戶端 Sku。

**原則的位置：** 電腦設定 > 原則 > 系統管理範本 > Windows 元件 > Windows 登入選項

**原則名稱：** 登入的最後一個互動式使用者系統起始的重新啟動後，自動

**支援的作業：** 至少為 Windows Server 2012 R2 中，Windows 8.1 或 Windows RT 8.1

**描述/Help:**

此原則設定控制是否會自動登入裝置在最後一個互動式的使用者 Windows 更新之後重新啟動系統。

如果您啟用或未設定此原則設定，裝置安全地將使用者的認證 （包括使用者名稱、 網域及加密的密碼） 儲存至 Windows Update 重新啟動之後自動登入設定。 Windows Update 重新啟動之後，使用者會自動登入，並與該使用者所設定的所有鎖定畫面應用程式會自動鎖定工作階段。

如果您停用此原則設定，裝置不會儲存使用者的認證，來自動登入之後重新啟動 Windows Update。 系統重新啟動之後，不會重新啟動使用者的鎖定畫面應用程式。

**登錄編輯程式**

|值名稱|類型|資料|
|--------------|--------|--------|
|DisableAutomaticRestartSignOn|DWORD|0<br /><br />**範例:**<br /><br />0 （已啟用）<br /><br />1 （停用）|

**原則的登錄位置：** HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System

**類型：** DWORD

**登錄名稱：** DisableAutomaticRestartSignOn

值：0 或 1

0 = 啟用

1 = 已停用

![winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/GTR_ADDS_SignInPolicy.gif)

## <a name="troubleshooting"></a>疑難排解
當 WinLogon 會自動鎖定時，WinLogon 的狀態追蹤會儲存在 WinLogon 事件記錄檔。

設定自動登入嘗試的狀態會記錄

-   如果成功

    -   因此將其記錄

-   如果它失敗：

    -   記錄失敗的是

-   當 BitLocker 的狀態變更：

    -   將記錄移除認證

        -   這些會儲存在 LSA 作業記錄。

### <a name="reasons-why-autologon-might-fail"></a>為什麼自動登入可能會失敗的原因
有幾種情況下，無法用來達成使用者自動登入。  本章節被要擷取已知的情況下，這可能發生。

### <a name="user-must-change-password-at-next-login"></a>使用者必須變更密碼在下次登入時
需要在下次登入時變更密碼時，使用者登入可以進入封鎖的狀態。  這可能會在大部分情況下，重新啟動之前偵測到的但並非所有 (比方說，可以達到密碼到期之間關機及下一步 的登入。

### <a name="user-account-disabled"></a>使用者帳戶已停用
即使停用，則可以維護現有的使用者工作階段。  重新啟動已停用的帳戶可以偵測在本機在大部分情況下預先根據 gp 它可能不是網域 （某個網域快取的帳戶登入案例工作即使帳戶已停用在網域控制站）。

### <a name="logon-hours-and-parental-controls"></a>登入時數與家長監護
登入時數與家長監護可以禁止新的使用者工作階段所建立。  如果重新啟動時，此視窗，使用者不允許登入。  沒有額外的原則，使做為合規性動作的登出或鎖定。  尤其是在維護期間是通常在這段期間，這可能是有問題的可能平台的時間喚醒，之間會發生帳戶鎖定的許多子案例。

## <a name="additional-resources"></a>其他資源
**資料表 SEQ 表格\\\*阿拉伯文 3:ARSO 詞彙**

|詞彙|定義|
|--------|--------------|
|Autologon|自動登入是一項功能，已在 Windows 中的數個版本存在。  這是 Windows，甚至還有工具，例如自動登入的 Windows v3.01 記錄的功能 *[http:/technet.microsoft.com/sysinternals/bb963905.aspx](https://technet.microsoft.com/sysinternals/bb963905.aspx)*<br /><br />它可讓單一使用者的裝置會自動登入不需要輸入認證。 設定及儲存在登錄中做為加密的 LSA 密碼認證。|


