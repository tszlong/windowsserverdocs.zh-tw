---
title: "Winlogon 自動重新登入 (ARSO)"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: na
ms.suite: na
ms.technology: security-auditing
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 15cddcfa-8a8e-45e4-bb76-b8e1a14ceac0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 172eb34fbfdb8a91adf55e35f888e90f5688d0e7
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="winlogon-automatic-restart-sign-on-arso"></a>Winlogon 自動重新登入 (ARSO)

>適用於：Windows Server（以每年次管道）、Windows Server 2016

**作者**: Justin Turner 資深支援工程師視窗群組  
  
> [!NOTE]  
> 本文由 Microsoft 客戶支援工程師撰寫，以及適用於系統管理員經驗和系統設計師超過參考 TechNet 上的主題通常會提供深入的技術解釋的功能與 Windows Server 2012 R2 方案正在尋找。 不過，尚未經歷相同編輯行程，以便某些語言的似乎比哪些通常位於 TechNet 較少的外觀。  
  
## <a name="overview"></a>概觀  
鎖定畫面應用程式引進了 Windows 8。  以下是執行，並顯示鎖定使用者的工作階段時的通知的應用程式（行事曆約會、電子郵件和簡訊等）。。  裝置重新啟動因為的 Windows 更新程序，會顯示在重新開機時這些鎖定畫面通知失敗。  部分使用者這些鎖定畫面應用程式而定。  
  
## <a name="whats-changed"></a>變更的項目？  
當使用者登入時在 Windows 8.1 的裝置上時，LSA 會儲存使用者的認證加密記憶體無障礙只要 lsass.exe。 Windows Update 自動重新開機，而使用者卡時，這些認證會用來設定自動登入的使用者。 執行系統 TCB 權限的 Windows Update 將會起始 RPC 通話，若要這樣做。  
  
在重新開機一次，使用者會自動透過自動登入機制登入，此外鎖定保護使用者的工作階段。 鎖定初始化透過 Winlogon 而 LSA 來管理 credential 完成。  來自動登鎖定使用者在主機上，使用者的鎖定畫面應用程式將會重新啟動和可用。  
  
> [!NOTE]  
> Windows Update 引入重新開機最後一個的使用者互動自動已該工作階段已被鎖定後，因此可以執行使用者的鎖定畫面應用程式。  
  
![顯示在鎖定畫面的螢幕擷取畫面](../media/winlogon-automatic-restart-sign-on-arso/GTR_ADDS_LockScreenApp.gif)  
  
![顯示鎖定畫面應用程式的螢幕擷取畫面](../media/winlogon-automatic-restart-sign-on-arso/GTR_ADDS_LockScreen.gif)  
  
**概觀**  
  
-   Windows Update 需要重新開機  
  
-   電腦重新開機無法 (*正在執行的任何應用程式會遺失資料*)？  
  
    -   適用於您重新開機  
  
    -   重新登入  
  
    -   鎖定電腦  
  
-   功能或群組原則來停用  
  
    -   在 [伺服器 Sku 預設停用  
  
-   為何？  
  
    -   部分更新後使用者登入，才能完成。  
  
    -   更好的使用者體驗：不需要等候 15 分鐘，完成安裝更新  
  
-   如何？ 自動登入  
  
    -   儲存的密碼，您登入會使用該認證  
  
    -   為 LSA 秘密分頁記憶體中儲存認證  
  
    -   僅限如果支援 BitLocker 已無法使用  
  
## <a name="group-policy-sign-in-last-interactive-user-automatically-after-a-system-initiated-restart"></a>群組原則中︰ 登入一個互動式使用者系統車載機起始重新後自動  
在 Windows 8.1 / Windows Server 2012 R2，自動登入 Windows Update 重新開機之後的鎖定畫面使用者加入伺服器 sku，Client sku 退出。  
  
**原則的位置：**電腦設定 > 原則 > 系統管理範本] > Windows 元件 > Windows 登入選項  
  
**原則的名稱︰**登入一個互動式使用者系統車載機起始重新後自動  
  
**支援的：**在至少 Windows Server 2012 R2、Windows 8.1 或 Windows RT 8.1  
  
**描述日協助：**  
  
這項原則是否裝置將會自動登入最後一個的使用者互動之後 Windows Update 設定會控制重新開機系統。  
  
如果您可以或未設定這項原則設定，裝置安全地儲存使用者的認證（包括的使用者名稱、網域及加密的密碼）來自動登入設定，Windows Update 重新開機之後。 Windows Update 重新開機之後，使用者會自動登入，並自動鎖定工作階段的所有鎖定畫面應用程式的使用者設定。  
  
如果您停用這個原則設定，請裝置不會儲存自動登入的使用者的認證後重新開機的 Windows 更新。 系統重新開機之後，不會重新啟動使用者的鎖定畫面應用程式。  
  
**作業系統**  
  
|值名稱|輸入|資料|  
|-------|----|----|  
|DisableAutomaticRestartSignOn|DWORD|0<br /><br />**範例：**<br /><br />0（啟用）<br /><br />1（停用）|  
  
**原則登錄位置：** HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System  
  
**輸入：** DWORD  
  
**登錄名稱：** DisableAutomaticRestartSignOn  
  
值：0 或 1  
  
0 = 啟用  
  
1 = 停用  
  
![顯示原則設定的螢幕擷取畫面控制您可以指定是否裝置將會自動登入最後一個的使用者互動之後，Windows Update 重新開機的系統 UI](../media/winlogon-automatic-restart-sign-on-arso/GTR_ADDS_SignInPolicy.gif)  
  
## <a name="troubleshooting"></a>疑難排解  
WinLogon 會自動鎖定，當 WinLogon 的狀態追蹤將會儲存在 WinLogon 事件登入。  
  
已登入設定自動登入嘗試的狀態  
  
-   如果成功  
  
    -   因此記錄  
  
-   如果失敗：  
  
    -   記錄失敗為何  
  
-   變更的 BitLocker 狀態時：  
  
    -   將會登入認證移除  
  
        -   這將會儲存 LSA 操作登入。  
  
### <a name="reasons-why-autologon-might-fail"></a>自動登入無法通過原因為何  
有幾個案例中無法達到使用者自動登入。  本節適擷取案例中，這可能會發生。  
  
### <a name="user-must-change-password-at-next-login"></a>使用者必須在變更密碼登入下一步  
使用者登入需要變更密碼在下次登入時時，可以進入封鎖的狀態。  這可以偵測到重新開機在大部分案例中之前，但並非所有 (，例如可連絡到密碼到期關機之間下次登入。  
  
### <a name="user-account-disabled"></a>使用者 Account 停用  
即使已停用，就可以維護現有的使用者工作階段。  適用於已停用 account 重新開機可以偵測到本機在大部分案例中事先，根據網域帳號（一些網域即使 account 已停用在網域控制站快登入案例工作）可能無法 gp。  
  
### <a name="logon-hours-and-parental-controls"></a>登入時間和家長監護  
登入小時和家長監護可以禁止建立的新使用者工作階段。  如果發生此項視窗重新開機，使用者會不會允許登入。  還有其他原則，會導致 compliance 控制項目登出或鎖定。  尤其是通常會在這段期間的 [維護] 視窗，這可能是有許多子女案例床時間和喚醒，之間 account 鎖定可能發生的問題。  
  
## <a name="additional-resources"></a>其他資源  
**表格 7 表格 \\\ * 阿拉伯文 3: ARSO 詞彙**  
  
|詞彙|解析度|  
|----|-------|  
|自動登入|自動登入是已經在 Windows 中的數個版本的功能。  它是記載的工具，例如適用於 Windows 的自動登入 v3.01 有的 Windows 功能*[http:/technet.microsoft.com/sysinternals/bb963905.aspx](https://technet.microsoft.com/sysinternals/bb963905.aspx)*<br /><br />它可以讓裝置的自動登入不需要輸入認證單一使用者。 設定及登錄為加密 LSA 密碼儲存認證。|  
  

