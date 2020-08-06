---
title: Windows Server Essentials 中的電腦與伺服器連線問題疑難排解
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: e45b3d89-c057-4c70-a627-86fb06dd22aa
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e832957a5d44851131cb622e4c3bf9d99d4e4a7f
ms.sourcegitcommit: 04637054de2bfbac66b9c78bad7bf3e7bae5ffb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87838267"
---
# <a name="troubleshoot-connecting-computers-to-the-server-in-windows-server-essentials"></a>Windows Server Essentials 中的電腦與伺服器連線問題疑難排解

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

 本主題包含將電腦連線到執行 Windows Server Essentials 或 Windows Server Essentials 的伺服器時，可能會遇到之問題的疑難排解指引。

> [!NOTE]
>  如需 Windows Server Essentials 和 Windows Server Essentials 社區中最新的疑難排解資訊，建議您造訪[Windows Server Essentials 論壇](/answers/topics/windows-server-essentials.html)。 Windows Server Essentials 論壇是一個搜尋協助或詢問問題的好地方。

 本主題提供下列問題的解決方法：


-   問題1：[問題 1](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BMRK_Package)

-   問題2：[問題 2](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2)

-   問題3：[問題 3](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2a)

-   問題4：[問題 4](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueNetFramework)

-   問題5：[問題 5](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_Time)

-   問題6：[問題 6](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ServiceStopped)

-   問題7：[問題 7](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueReconnect)

-   問題8：[問題 8](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_JoinWin7)

-   問題9：[問題 9](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueAutologon)

-   問題10：[問題 10](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueOldLogs)

-   問題11：[問題 11](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_UpgradeClientOS)


##  <a name="issue-1"></a><a name="BMRK_Package"></a>問題1
 **問題**

 我取得套件安裝失敗。 請嘗試重新安裝 Windows Server Essentials 連接器。 如果問題持續發生，請在將電腦連接到伺服器時，請洽詢網路系統管理員的錯誤

 **描述**

 如果您將電腦連線到執行 Windows Server Essentials 的伺服器，而其他 Windows 更新或應用程式安裝處於擱置狀態，而且已取消連接器安裝，則可能會發生此問題。

 **方案**

 完成所有其他更新或軟體安裝。 出現提示時，請重新啟動電腦。

##  <a name="issue-2"></a><a name="BKMK_ConnectorIssue2"></a>問題2
 **問題**

 無法將電腦加入 Windows Server Essentials

 **描述**

 電腦名稱稱中有非 ASCII 字元的電腦不能加入 Windows Server Essentials。 如果電腦名稱包含非 ASCII 字元，您會收到錯誤訊息「發生意外的錯誤」。

 **方案**

 請將您的用戶端電腦重新命名為僅包含 ASCII 字元的名稱，然後再次嘗試將電腦新增至 Windows Server Essentials。

##  <a name="issue-3"></a><a name="BKMK_ConnectorIssue2a"></a>問題3
 **問題**

 當我將電腦連線到伺服器時，出現 [連接器軟體安裝已取消] 錯誤

 **描述**

 若要能夠將電腦連線到伺服器，系統帳戶必須具有 [Windows Server Essentials 儀表板] 上所顯示伺服器資料夾的 [完全控制] 許可權。 如果尚未取得必要的權限，您會收到「連接器軟體安裝已取消」的錯誤訊息。

 **方案**

 將每個伺服器資料夾的 [完全控制]**** 權限授與 SYSTEM 帳戶。

#### <a name="to-grant-the-system-account-full-control-permissions-on-a-server-folder"></a>將伺服器資料夾的 [完全控制] 權限授與 SYSTEM 帳戶

1.  開啟 [Windows Server Essentials 儀表板]。

2.  按一下 [存放裝置]****，然後按一下 [伺服器資料夾]****。

3.  以滑鼠右鍵按一下伺服器資料夾，然後按一下 [開啟資料夾]**** (如果您看不到 [開啟資料夾]**** 選項，就不需要設定資料夾的權限)。

4.  在 Windows 檔案總管頂端的網路路徑中，按一下伺服器共用，以顯示伺服器上的共用資料夾。 例如，如果路徑是 [網路] > [Server01] > [File History Backups]****，請按一下 [Server01]****。

5.  以滑鼠右鍵按一下伺服器資料夾，然後按一下 [內容]****。

6.  按一下 [安全性]**** 索引標籤。

7.  如果不允許 SYSTEM 帳戶具有 [完全控制]**** 權限，請按一下 [編輯]****，然後按一下 [SYSTEM]****。 在 [SYSTEM 的權限]**** 之下，選取 [完全控制]**** 旁的 [允許]**** 核取方塊。

8.  按兩次 [確定]**** 更新權限，然後關閉 [內容]****。

##  <a name="issue-4"></a><a name="BKMK_ConnectorIssueNetFramework"></a>問題4
 **問題**

 我收到執行此應用程式的，您必須在將電腦連線到伺服器時，安裝下列其中一個版本的 .NET Framework： V 4.5.50709 "錯誤

 **描述**

 當您將電腦連線到執行 Windows Server Essentials 或 Windows Server Essentials 的伺服器時，嚮導會嘗試在電腦上安裝 .NET Framework 版本4.5.50709。 不過，如果舊版的 .NET Framework 版本4.5 存在，則無法安裝更新的版本，而且連線嘗試會失敗並出現下列錯誤訊息：若要執行此應用程式，您必須安裝下列其中一個版本的 .NET Framework： V 4.5.50709。 請洽詢您的配置發行者，以取得取得適當 .NET Framework 版本的指示。

 **方案**

 從電腦解除安裝 .NET Framework 4.5，再將電腦連線到伺服器。

#### <a name="to-uninstall-net-framework-45"></a>解除安裝 .NET Framework 4.5

1.  在用戶端電腦的 [開始]**** 頁面上，開啟 [控制台]****。

2.  在 [控制台] 的 [程式集]**** 之下，按一下 [解除安裝程式]****。

3.  以滑鼠右鍵按一下 [Microsoft.NET Framework 4.5]****，然後按一下 [解除安裝]****。

4.  成功解除安裝 .NET Framework 4.5 之後，將電腦連線到伺服器。 .NET Framework 4.5 的正確版本會隨連接器軟體一併安裝。

##  <a name="issue-5"></a><a name="BKMK_Time"></a>問題5
 **問題**

 我收到伺服器無法使用。 若要解決此問題，請洽詢負責您網路的人員。 的錯誤

 **描述**

 如果已連線電腦上的日期和時間與伺服器上的日期和時間不同步，就可能發生這種情況。  Windows Server Essentials 和 Windows Server Essentials 會使用時間同步處理服務，將在 Windows Server Essentials 或 Windows Server Essentials 網路中執行之電腦的日期和時間同步。 時間同步處理很重要，因為預設的驗證通訊協定會在驗證程序中使用伺服器時間。 例如，如果用戶端電腦上的時鐘未同步處理到正確的日期和時間，Windows Server Essentials 或 Windows Server Essentials 驗證可能會將登入要求誤認為入侵嘗試，並拒絕使用者存取。

 如果伺服器的可用記憶體低於5%，就會發生這種情況。

 如果您已經有 Windows Essentials Server 的 VPN 連線，並嘗試使用網域位址設定外部部署的連接器軟體，就可能發生這種情況。

 **方案**

1.  同步處理用戶端電腦和伺服器上的日期和時間。 然後將電腦連線到伺服器。

2.  關閉伺服器端的一些應用程式，然後將電腦連線到伺服器。

3.  關閉 VPN 連線，然後將電腦連線到伺服器。

#### <a name="to-change-the-date-and-time-on-the-client-computer"></a>變更用戶端電腦上的日期和時間

1. 在用戶端電腦的 [開始] 頁面上，開啟 [控制台]****。

2. 在 [控制台] 中，按一下 [時鐘、語言和區域]****，然後按一下 [日期和時間]****。

3. 按一下 [變更日期和時間]****，將日期和時間設定為正確的日期和時間，然後按一下 [確定]****。

4. 按一下 [確定]****，然後關閉 [控制台]。

5. 再次嘗試將用戶端電腦連線到伺服器。 如需指示，請參閱＜將電腦連線到伺服器＞。

   如果您仍然無法將用戶端電腦連線到伺服器，請確定伺服器上的日期和時間正確。 如果日期和時間不正確，請加以變更。

#### <a name="to-change-the-date-and-time-on-the-server"></a>變更伺服器上的日期和時間

1.  使用您在 Windows Server Essentials 或 Windows Server Essentials 安裝和設定期間所設定的密碼登入伺服器。

    > [!NOTE]
    >  如果您從遠端管理伺服器，您必須使用「遠端桌面連線」登入伺服器。

2.  在 [開始]**** 頁面上，開啟 [控制台]****。

3.  在 [控制台] 中，按一下 [時鐘、語言和區域]****，然後按一下 [日期和時間]****。

4.  按一下 [變更日期和時間]****，將日期和時間設定為正確的日期和時間，然後按一下 [確定]****。

5.  按一下 [確定]****，然後關閉 [控制台]。

6.  在用戶端電腦上，再次嘗試將用戶端電腦連線到伺服器。 如需指示，請參閱＜將電腦連線到伺服器＞。

##  <a name="issue-6"></a><a name="BKMK_ServiceStopped"></a>問題6
 **問題**

 發生未預期的錯誤。 若要解決此問題，請洽詢負責您網路的人員。 的錯誤

 **描述**

 如果您收到這個錯誤訊息，WSS 憑證 Web 服務可能並未執行。

 **方案**

 啟動 WSS 憑證 Web 服務。

#### <a name="to-start-the-wss-certificate-web-service"></a>啟動 WSS 憑證 Web 服務

1.  在伺服器的 [開始]**** 頁面上，開啟 [系統管理工具]****。

     在 [系統管理工具]**** 中，以滑鼠右鍵按一下 [Internet Information Services (IIS) 管理員]****，然後按一下 [開啟]****。

2.  在瀏覽窗格中，按一下 [WSS 憑證 Web 服務]****。

3.  在 [動作]**** 窗格中，按一下 [啟動]****。

##  <a name="issue-7"></a><a name="BKMK_ConnectorIssueReconnect"></a>問題7
 **問題**

 當我嘗試在連線嘗試失敗後再次將電腦連接到伺服器時，我收到警告，表示此名稱的電腦已經連接到伺服器

 **描述**

 如果先前嘗試將電腦連線到伺服器已取消或中斷，您可能會在嘗試重新連線時收到下列警告：此名稱的電腦已經連接到伺服器。 由於第一次嘗試連線到伺服器時會發出憑證，因此會發生這種情況。

 **解決方法**：如果您確定沒有其他同名的電腦已連線到伺服器，請按 [下一步]****，然後遵循指示，完成 [將我的電腦連線到伺服器精靈]****。

##  <a name="issue-8"></a><a name="BKMK_JoinWin7"></a>問題8
 **問題**

 當我嘗試將執行 Windows 7 Home 的用戶端電腦連線到伺服器時，會開啟執行連接器軟體的網頁，但用戶端電腦無法連線到伺服器

 **描述**

 如果您網路上的路由器已啟用多點傳送，伺服器與執行 Windows 7 Home Basic 或 Windows 7 Home Premium 的用戶端電腦之間的通訊將無法正常運作。

 **方案**

 停用您路由器上的多點傳送。 在某些路由器上，可能還需要停用 RIP-2M 路由通訊協定。 如需詳細資訊，請參閱路由器廠商所提供的文件。

##  <a name="issue-9"></a><a name="BKMK_ConnectorIssueAutologon"></a>問題9
 **問題**

 當我將電腦連線到伺服器之後，自動登入已停止運作。

 **描述**

 當您將電腦連線到 Windows Server Essentials 時，如果已為使用者帳戶設定自動登入，則會在電腦上安裝連接器軟體時覆寫此設定。

 **解決方法**：若要解決這個問題，請在您將電腦連線到伺服器時，記下使用者帳戶所使用的密碼。 安裝連接器軟體之後，設定自動登入以使用該帳戶。

> [!NOTE]
>  Windows Server Essentials 網域帳戶需要符合預設密碼原則需求的密碼。

##  <a name="issue-10"></a><a name="BKMK_ConnectorIssueOldLogs"></a>問題10
 **問題**

 解除安裝連接器軟體的發行前版本並不會移除現有的記錄檔

 **描述**

 從 Windows Server Essentials 的發行前版本 (Beta 或 RC) 版本更新為發行版本之後，您必須從連線到伺服器的每部電腦移除連接器軟體，然後再次連接電腦以安裝連接器軟體的發行版本。

 不過，當您從網路電腦移除連接器軟體時，並不會刪除該電腦上 %ProgramData%\Microsoft\Windows Server\Logs\ 資料夾中的現有記錄檔。 如果您未刪除 Logs 資料夾，當您將電腦連線到 Windows Server Essentials 的發行版本時，記錄檔可能會損毀。

 **解決方法**：若要避免記錄檔損毀，請手動刪除 Logs 資料夾，再將用戶端電腦重新連線到更新的伺服器。

#### <a name="to-reconnect-a-computer-after-a-server-update-without-corrupting-the-log-files"></a>在伺服器更新之後重新連線電腦，而不損毀記錄檔

1.  從用戶端電腦解除安裝連接器軟體

2.  從 %ProgramData%\Microsoft\Windows Server\ 資料夾刪除 Logs 資料夾。

3.  再次將電腦連線到伺服器。 這會安裝連接器軟體的發行版本，並建立新的 Logs 資料夾和記錄檔。

##  <a name="issue-11"></a><a name="BKMK_UpgradeClientOS"></a>問題11
 **問題**

 我想要升級用戶端電腦上的作業系統

 **描述**

 在安裝連接器軟體期間，會對用戶端作業系統執行許多檢查，以確保用戶端符合連接器的所有必要條件。 如果您在安裝連接器軟體之後升級用戶端作業系統，可能會缺少其中一些必要元件，而導致用戶端連接器失敗。

 **方案**

 將用戶端作業系統升級為不同版本之前 (例如將 Windows XP 升級為 Windows Vista 或將 Windows Vista 升級為 Windows 7)，您應該先解除安裝連接器軟體。 請使用 [控制台] 中的 [新增或移除程式]****。 用戶端作業系統升級完成之後，您可以在網頁瀏覽器中開啟 HTTP://<*server*>/connect，以重新安裝用戶端連接器，其中 <*Server*> 是 Windows server Essentials 伺服器的名稱。

 如果您已經升級安裝了連接器軟體的用戶端，請使用 [新增/移除程式]**** 或 [程式和功能]**** 解除安裝連接器軟體。 然後重新安裝連接器軟體。

## <a name="additional-references"></a>其他參考資料

- [管理 Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)

- [Windows 2012 Server Essentials ConnectComputer 疑難排解]()