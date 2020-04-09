---
title: Windows Server Essentials 中的電腦監視問題疑難排解
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: f1e6b377-4a24-4d28-9b25-05910914826b
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 5e2c8706c29e63a74d637e4a2c072d0ffa0a56d5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852201"
---
# <a name="troubleshoot-computer-monitoring-in-windows-server-essentials"></a>Windows Server Essentials 中的電腦監視問題疑難排解

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

本主題提供在 [警示檢視器] 中監視電腦的健全狀況狀態，以及透過 Windows Server Essentials 中的電子郵件通知，所遇到的問題疑難排解。  
  
> [!NOTE]
>  如需 Windows Server Essentials 社區最新的疑難排解資訊，建議您流覽[Windows Server Essentials 論壇](https://social.technet.microsoft.com/Forums/winserveressentials/threads)。 Windows Server Essentials 論壇是一個搜尋協助或詢問問題的好地方。  
  
##  <a name="troubleshooting-email-notifications-for-alerts"></a><a name="BKMK_TS"></a>疑難排解警示的電子郵件通知  
 本節列出使用警示的電子郵件通知時，可能會遇到的各種問題。  
  
### <a name="cannot-send-the-test-email-for-the-alert"></a>無法傳送警示的測試電子郵件  
 **問題**您會收到錯誤訊息，指出「無法傳送警示的測試電子郵件」。  
  
 **原因** 這個錯誤可能是因為警示通知的設定中有下列問題：  
  
- 不正確的 SMTP 伺服器名稱或連接埠號碼。  
  
- 不正確指定 SMTP 伺服器需要單一通訊端層 (SSL) 連線。  
  
- SMTP 伺服器需要驗證，但卻輸入不正確的認證。  
  
  **解決方式** 修正電子郵件通知設定中的任何錯誤。  
  
##### <a name="to-identify-issues-in-your-email-notification-settings"></a>找出電子郵件通知設定的問題  
  
-   向您的網際網路服務提供者 (ISP) 詢問正確的 SMTP 伺服器名稱、連接埠號碼以及 SSL 用法。  
  
-   檢閱伺服器上警示之電子郵件通知的記錄檔，位置在：  
  
     %ProgramData%\Microsoft\Windows Server\Logs\SharedServiceHost-AlertServiceConfig.log  
  
    > [!TIP]
    >  若要查看 [ProgramData] 資料夾，您必須顯示隱藏的項目。 如果您沒有看到 [ProgramData] 資料夾，請在功能區的 [**視圖**] 索引標籤上，于 [**顯示/隱藏**] 群組中選取 [**隱藏專案**] 文字方塊。  
  
##### <a name="to-update-your-email-notification-setup-for-alerts"></a>更新警示的電子郵件通知設定  
  
1.  在儀表板上，按一下右上角的任何警示圖示以開啟警示檢視器。  
  
2.  在警示檢視器底部，按一下 [設定警示的電子郵件通知]。  
  
3.  在 [設定警示的電子郵件通知] 對話方塊中，按一下 [啟用]。  
  
4.  在 [SMTP 設定] 對話方塊中，更新 SMTP 設定，然後按一下 [確定]。  
  
5.  若要測試更新的設定，請按一下 [套用及傳送電子郵件]。  
  
6.  確認測試電子郵件已成功後，按一下 [確定] 以儲存更新的設定。  
  
### <a name="test-email-notification-does-not-list-any-alerts"></a>測試電子郵件通知沒有列出任何警示  
 **問題** 警示的測試電子郵件通知沒有顯示任何警示，即使警示檢視器中有列出警示時亦同。  
  
 **解決方式** 警示檢視器中並非所有警示都會產生電子郵件通知。 只有在其健康定義檔內設定為以電子郵件通知呈報的警示，才會以電子郵件傳送給指定的電子郵件收件者。  
  
 當您按一下 [套用及傳送電子郵件] 時，通常您會收到沒有列出任何健康情況警示的範例電子郵件通知。 不過，如果此測試程序期間已確認有設定為傳送電子郵件通知的健康警示，測試電子郵件中就會包含該警示。  
  
### <a name="active-alerts-are-displayed-for-an-uninstalled-application"></a>已解除安裝的應用程式顯示作用中警示  
 **問題** 即使應用程式及其健康定義檔已解除安裝，仍然會顯示應用程式的作用中警示。  
  
 **解決方式** 您必須手動刪除已解除安裝之應用程式的作用中警示。 若要刪除警示，請執行下列動作。  
  
##### <a name="to-delete-an-alert-from-the-server-by-using-the-dashboard"></a>使用儀表板從伺服器刪除警示  
  
1.  從伺服器開啟 [儀表板]。  
  
2.  在瀏覽窗格中，按一下任何顯示的警示圖示 (嚴重、警告或資訊)。 這會啟動警示檢視器。  
  
3.  在 [警示檢視器] 中，以滑鼠右鍵按一下您想要刪除的警示，然後按一下 [刪除此警示]。
