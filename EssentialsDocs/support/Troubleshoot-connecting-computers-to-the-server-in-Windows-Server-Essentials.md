---
title: "若要在 Windows Server Essentials 的伺服器連接電腦的疑難排解"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e45b3d89-c057-4c70-a627-86fb06dd22aa
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 9b0d11be08840ecedabab6fd4e96f5d453ea4857
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-connecting-computers-to-the-server-in-windows-server-essentials"></a>若要在 Windows Server Essentials 的伺服器連接電腦的疑難排解

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集 
  
 本主題提供的疑難排解，當您執行的 Windows Server Essentials server 或 Windows Server Essentials 連接電腦時，可能會遇到的問題。  
  
> [!NOTE]
>  Windows Server Essentials 社群與 Windows Server Essentials 的最新的疑難排解資訊，我們建議您瀏覽[Windows Server Essentials 論壇](https://social.technet.microsoft.com/Forums/windowsserver/home?forum=winserveressentials)。 Windows Server Essentials 論壇是一個很好的協助，或是詢問問題。  
  
 本主題提供方案下列問題：  
  

-   問題 1:[第 1 期](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BMRK_Package)  
  
-   問題 2:[發出 2](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2)  
  
-   問題 3:[發出 3](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2a)  
  
-   問題 4:[發出 4](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueNetFramework)  
  
-   問題 5:[發出 5](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_Time)  
  
-   問題 6:[發出 6](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ServiceStopped)  
  
-   問題 7:[發出 7](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueReconnect)  
  
-   8 的問題：[發出 8](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_JoinWin7)  
  
-   9 的問題：[發出 9](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueAutologon)  
  
-   10 的問題：[發出 10](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueOldLogs)  
  
-   11 的問題：[發出 11](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_UpgradeClientOS)  

-   問題 1:[第 1 期](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BMRK_Package)  
  
-   問題 2:[發出 2](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2)  
  
-   問題 3:[發出 3](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2a)  
  
-   問題 4:[發出 4](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueNetFramework)  
  
-   問題 5:[發出 5](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_Time)  
  
-   問題 6:[發出 6](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ServiceStopped)  
  
-   問題 7:[發出 7](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueReconnect)  
  
-   8 的問題：[發出 8](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_JoinWin7)  
  
-   9 的問題：[發出 9](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueAutologon)  
  
-   10 的問題：[發出 10](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueOldLogs)  
  
-   11 的問題：[發出 11](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_UpgradeClientOS)  

  
##  <a name="BMRK_Package"></a>1 問題  
 **問題**  
  
 取得的套件安裝未成功。 請嘗試重新安裝 Windows Server Essentials 連接器。 如果問題持續存在，請連絡您的網路系統管理員錯誤伺服器連接電腦時  
  
 **描述**  
  
 如果您執行 Windows Server Essentials 的其他 Windows 更新或安裝的應用程式的擱置中並取消連接器安裝時伺服器連接電腦時，可能會發生此問題。  
  
 **方案**  
  
 完成所有其他更新或安裝的應用程式。 如果出現提示，請重新電腦。  
  
##  <a name="BKMK_ConnectorIssue2"></a>問題 2  
 **問題**  
  
 無法將電腦加入 Windows Server Essentials  
  
 **描述**  
  
 Windows Server Essentials 不能加入電腦已在 [電腦名稱非 ASCII 字元。 如果您的電腦名稱包含非 ASCII 字元，您收到錯誤訊息」已發生意外的錯誤」。  
  
 **方案**  
  
 重新命名 client 電腦包含僅 ASCII 字元名稱，然後再試一次將電腦新增到 Windows Server Essentials。  
  
##  <a name="BKMK_ConnectorIssue2a"></a>3 問題  
 **問題**  
  
 取得連接器安裝的軟體已取消的錯誤伺服器連接電腦時  
  
 **描述**  
  
 若要無法電腦連接到伺服器，系統 account 必須完整控制權限伺服器顯示在 Windows Server Essentials 儀表板上的資料夾。 如果尚未獲得所需的權限，您會收到」取消連接器軟體安裝「錯誤訊息。  
  
 **方案**  
  
 授與系統 account**完全控制**伺服器的每個資料夾的權限。  
  
#### <a name="to-grant-the-system-account-full-control-permissions-on-a-server-folder"></a>若要伺服器資料夾完全控制權限授與系統 account  
  
1.  打開 Windows Server Essentials 儀表板。  
  
2.  按一下**存放裝置**，然後按一下 [**伺服器資料夾**。  
  
3.  [伺服器] 資料夾上按一下滑鼠右鍵，然後按一下**資料夾的開放**。 (如果您看不到**打開資料夾**選項，您不需要設定權限] 資料夾中。)  
  
4.  在網路路徑頂端的 [Windows 檔案總管] 中，按一下 [顯示在伺服器上的共用的資料夾伺服器分享。 例如，如果路徑**網路 > Server01 > 檔案歷史備份**，按一下 [ **Server01**。  
  
5.  伺服器資料夾上按一下滑鼠右鍵，然後按一下**屬性**。  
  
6.  按一下**安全性**索引標籤。  
  
7.  如果**完全控制**不是允許系統帳號，按一下 [**編輯**，然後按一下 [**系統**。 在**系統權限]**，請選取**允許**旁邊的核取方塊**完全控制**。  
  
8.  按一下**[確定]**兩次來更新權限，關閉 [**屬性**。  
  
##  <a name="BKMK_ConnectorIssueNetFramework"></a>4 的問題  
 **問題**  
  
 取得執行此應用程式到，您必須安裝其中一個版本的.NET framework 下列：V4.5.50709」錯誤伺服器連接電腦時  
  
 **描述**  
  
 當您執行的 Windows Server Essentials server 或 Windows Server Essentials 連接電腦時，該精靈將會在電腦上安裝.NET Framework 4.5.50709 版本。 不過，如果舊版本的.NET Framework 4.5 版本，請將無法安裝更新的版本，並連接失敗的錯誤訊息：若要執行此應用程式，您必須安裝其中一個版本的.NET framework 下列：V4.5.50709。 如需有關如何取得適當版本的.NET Framework 的指示，請連絡您的配置發行者。  
  
 **方案**  
  
 從電腦中解除安裝.NET Framework 4.5，然後將電腦連接到伺服器。  
  
#### <a name="to-uninstall-net-framework-45"></a>若要解除安裝.NET Framework 4.5  
  
1.  在**[開始]**頁面 client 的開放電腦的**[控制台]**。  
  
2.  在 [控制台]，在**程式**，按一下 [**解除安裝程式**。  
  
3.  以滑鼠右鍵按一下**Microsoft.NET Framework 4.5**，然後按**解除安裝**。  
  
4.  .NET Framework 4.5 成功解除安裝之後，將電腦連接到伺服器。 安裝正確的版本的.NET Framework 4.5 加上的連接器軟體。  
  
##  <a name="BKMK_Time"></a>5 的問題  
 **問題**  
  
 取得伺服器不提供。 修正此問題的相關，請連絡您的網路負責人。 錯誤伺服器連接電腦時  
  
 **描述**  
  
 如果這情形的日期和時間，在連接的電腦不會與同步的日期和時間伺服器上。  Windows Server Essentials 與 Windows Server Essentials 同步處理的日期和時間的電腦執行的 Windows Server Essentials 或 Windows Server Essentials 網路使用時間同步服務。 同步處理的時間為預設驗證通訊協定使用時間伺服器的驗證程序的一部分。 例如時鐘 client 的電腦上無法同步到正確的日期和時間，Windows Server Essentials 或 Windows Server Essentials 驗證可能錯誤如果解譯為入侵嘗試登入要求和拒絕的存取權的使用者。  
  
 發生這個錯誤的伺服器 s 可用記憶體是否低於 5%的電量。  
  
 如果您已經有以 Windows Essentials Server 的 VPN 連接，並在您嘗試設定連接器軟體關閉為前提使用網域位址，這可能會發生。  
  
 **方案**  
  
1.  同步處理的日期和時間 client 電腦上，使用這些伺服器上。 然後將電腦連接到伺服器。  
  
2.  關閉伺服器端，在某些應用程式，然後將電腦連接到伺服器。  
  
3.  關閉 VPN 連接，，然後將電腦連接到伺服器。  
  
#### <a name="to-change-the-date-and-time-on-the-client-computer"></a>若要變更的日期和時間 client 電腦上  
  
1.  在 [開始] 畫面 client 的電腦，請打開**[控制台]**。  
  
2.  在 [控制台] 中，按一下 [**時鐘、語言和地區**，然後按**的日期和時間**。  
  
3.  按一下**變更日期和時間**，將的日期和時間設定為正確的日期和時間，然後按一下 [ **[確定]**。  
  
4.  按一下**[確定]**，然後關閉 [控制台]。  
  
5.  再試一次 client 電腦連接到伺服器。 如的指示，請連接電腦的伺服器。  
  
 如果您仍然無法 client 電腦連接到伺服器，請確定您是正確的日期和時間伺服器上。 如果不正確的日期和時間的變更。  
  
#### <a name="to-change-the-date-and-time-on-the-server"></a>若要變更的日期和時間伺服器上  
  
1.  使用您設定 Windows Server Essentials 或 Windows Server Essentials 安裝和設定期間的密碼登入。  
  
    > [!NOTE]
    >  如果您從遠端管理的伺服器，您必須登入以使用遠端桌面連接的伺服器。  
  
2.  在**[開始]**頁面上，開放**[控制台]**。  
  
3.  在 [控制台] 中，按一下 [**時鐘、語言和地區**，然後按**的日期和時間**。  
  
4.  按一下**變更日期和時間**，將的日期和時間設定為正確的日期和時間，然後按一下 [ **[確定]**。  
  
5.  按一下**[確定]**，然後關閉 [控制台]。  
  
6.  Client 在電腦上，再試一次 client 電腦連接到伺服器。 如的指示，請連接電腦的伺服器。  
  
##  <a name="BKMK_ServiceStopped"></a>6 的問題  
 **問題**  
  
 取得 An 發生意外的錯誤。 修正此問題的相關，請連絡您的網路負責人。 錯誤伺服器連接電腦時  
  
 **描述**  
  
 如果您收到這個錯誤訊息，可能無法在執行 WSS 憑證 Web 服務。  
  
 **方案**  
  
 開始 WSS 憑證 Web 服務。  
  
#### <a name="to-start-the-wss-certificate-web-service"></a>若要開始 WSS 憑證 Web 服務  
  
1.  在伺服器的**[開始]**頁面上，開放**系統管理工具]**。  
  
     在**系統管理工具**，以滑鼠右鍵按一下**網際網路服務 (IIS) 管理員**，然後按一下 [**開放**。  
  
2.  在瀏覽窗格中，按一下**WSS 憑證 Web 服務**。  
  
3.  在**動作**窗格中，按**[開始]**。  
  
##  <a name="BKMK_ConnectorIssueReconnect"></a>7 問題  
 **問題**  
  
 嘗試一直無法成功連接後，再伺服器連接電腦時，我會收到的警告已經伺服器連接的電腦名稱  
  
 **描述**  
  
 如果先前嘗試伺服器連接的電腦已取消或中斷，您可能會收到下列警告，當您嘗試重新連接：電腦名稱已連接到伺服器。 這是因為您連接至伺服器第一次嘗試時發行憑證。  
  
 **方案**如果您不確定您具有相同名稱不另一部電腦已連接伺服器，按一下 [**下一步**，並依照指示完成**我的電腦連接到伺服器**精靈。  
  
##  <a name="BKMK_JoinWin7"></a>8 的問題  
 **問題**  
  
 當我嘗試連接 client 的電腦是執行 Windows 7 家用伺服器時，開啟網頁執行連接器軟體，但 client 電腦無法連接到伺服器  
  
 **描述**  
  
 如果您網路上的路由器支援多點傳送，伺服器與執行 Windows 7 家用入門版或 Windows 7 家用進階版 client 電腦間通訊無法正確運作。  
  
 **方案**  
  
 停用您的路由器。 在某些路由器，這可能會包含停用擷取-2 M 路由通訊協定。 如需詳細資訊，請參考路由器製造商所提供的文件。  
  
##  <a name="BKMK_ConnectorIssueAutologon"></a>9 的問題  
 **問題**  
  
 自動登入停止運作我伺服器連接電腦之後  
  
 **描述**  
  
 自動登入的使用者 account 設定 Windows Server Essentials 連接電腦時，如果連接器軟體在電腦上安裝時，會覆寫設定。  
  
 **方案**修正這個問題，請的相關伺服器連接電腦時，請注意，適用於帳號密碼。 已安裝的連接器軟體之後，來設定使用該 account 自動登入。  
  
> [!NOTE]
>  Windows Server Essentials 網域需要符合預設密碼原則需求的密碼。  
  
##  <a name="BKMK_ConnectorIssueOldLogs"></a>10 問題  
 **問題**  
  
 解除安裝的連接器軟體搶鮮版不會移除現有登  
  
 **描述**  
  
 您搶鮮版（Beta 或 RC）版本的 Windows Server Essentials 的更新的發行版本之後，您必須移除每一部電腦已連接到 server 連接器軟體，然後將電腦重新安裝軟體連接器發行的版本連接。  
  
 不過，當您從網路的電腦移除的連接器軟體，不會刪除現有登入資料夾中的檔案 %ProgramData%\Microsoft\Windows Server\Logs\ 該電腦上。 如果您請勿登的資料夾，登入檔案可以損壞您的電腦連接到 Windows Server Essentials 的發行版本時。  
  
 **方案**以避免損壞的登入檔案，以手動方式 delete 登資料夾再重新連接 client 的電腦已更新的伺服器。  
  
#### <a name="to-reconnect-a-computer-after-a-server-update-without-corrupting-the-log-files"></a>若要重新連接電腦伺服器更新損壞登入之後檔案  
  
1.  解除安裝的 client 電腦上的連接器軟體。  
  
2.  從 %ProgramData%\Microsoft\Windows Server\ 資料夾 delete 登的資料夾。  
  
3.  再試一次將電腦連接到伺服器。 這會安裝連接器軟體，建立新的登資料夾和檔案登入的發行的版本。  
  
##  <a name="BKMK_UpgradeClientOS"></a>11 的問題  
 **問題**  
  
 我想要升級 client 的電腦上的作業系統  
  
 **描述**  
  
 在安裝期間的連接器軟體，確保 client 符合所有的連接器必要條件 client 作業系統針對執行許多檢查。 如果您安裝的連接器軟體之後，您可以升級 client 作業系統，部分必要條件可能不會出現，並 client 連接器可能會失敗。  
  
 **方案**  
  
 升級到另一個版本 client 作業系統之前（例如，您升級 Windows XP Windows Vista 或 Windows 7 的 Windows Vista），您必須解除安裝的連接器軟體。 使用**新增或移除程式**在 [控制台] 中。 Client 作業系統升級完成後，您可以重新安裝 client 連接器打開 http://<*伺服器*> 網頁瀏覽器連接 / 位置 <*伺服器*> 是 Windows Server Essentials 伺服器的名稱。  
  
 如果您已經安裝的連接器軟體升級 client，使用**新增/移除程式]**或**的程式和功能**解除安裝連接器軟體。 然後再試一次安裝連接器軟體。  
  
## <a name="see-also"></a>也了  
  
-   [管理 Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)  
  
-   [Windows 2012 Server Essentials ConnectComputer 疑難排解 (TechNet Wiki)](https://social.technet.microsoft.com/wiki/contents/articles/14370.windows-2012-server-essentials-connectcomputer-troubleshooting.aspx)
