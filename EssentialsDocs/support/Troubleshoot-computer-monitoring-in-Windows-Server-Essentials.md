---
title: "疑難排解監視在 Windows Server Essentials 的電腦"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f1e6b377-4a24-4d28-9b25-05910914826b
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 72fe309e0e7ce6d7227cce8b7f2c5dbf018eb4a1
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-computer-monitoring-in-windows-server-essentials"></a>疑難排解監視在 Windows Server Essentials 的電腦

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

本主題提供時發生監控電腦，並透過 Windows Server Essentials 的電子郵件通知的警示檢視器中的健康狀態的問題進行疑難排解。  
  
> [!NOTE]
>  針對 Windows Server Essentials 社群貢獻的最新疑難排解資訊，我們建議您瀏覽[Windows Server Essentials 論壇](https://social.technet.microsoft.com/Forums/winserveressentials/threads)。 Windows Server Essentials 論壇是一個很好的協助，或是詢問問題。  
  
##  <a name="BKMK_TS"></a>疑難排解警示的電子郵件通知  
 本節列出使用警示的電子郵件通知時，可能會遇到的各種問題。  
  
### <a name="cannot-send-the-test-email-for-the-alert"></a>無法傳送測試警示的電子郵件  
 **問題**您收到錯誤訊息，無法傳送測試警示的電子郵件。  
  
 **原因**這個錯誤可能會造成任何下列設定警示通知的問題：  
  
-   不正確的 SMTP 伺服器名稱或連接埠號碼。  
  
-   它指定不正確 SMTP 伺服器需要連接單一通訊端層 (SSL)。  
  
-   SMTP 伺服器需要驗證而且輸入不正確的認證。  
  
 **方案**修正您的電子郵件通知的設定中的任何錯誤。  
  
##### <a name="to-identify-issues-in-your-email-notification-settings"></a>找出問題您的電子郵件通知設定  
  
-   詢問正確 SMTP 伺服器名稱、連接埠號碼，並使用 SSL 網際網路服務提供者 (ISP)。  
  
-   檢視警示的電子郵件通知的登入檔案伺服器，在這個位置中：  
  
     %ProgramData%\Microsoft\Windows Server\Logs\SharedServiceHost-AlertServiceConfig.log  
  
    > [!TIP]
    >  若要查看 ProgramData 資料夾，您必須隱藏顯示的項目。 如果您不 t s 功能區上看到 ProgramData 資料夾，**檢視**索引標籤的**顯示/隱藏**群組中，選取**隱藏的項目**文字方塊。  
  
##### <a name="to-update-your-email-notification-setup-for-alerts"></a>若要更新您的警示的電子郵件通知設定  
  
1.  儀表板，按一下右上角打開警示檢視器任何警示圖示。  
  
2.  底部的 [提醒] 檢視器，請按一下**設定電子郵件通知的警示**。  
  
3.  在**設定電子郵件通知的警示**對話方塊中，按**支援**。  
  
4.  在**SMTP 設定**對話方塊中，更新 SMTP 設定，然後按一下 [ **[確定]**。  
  
5.  若要測試的更新的設定，請按一下**套用並傳送電子郵件**。  
  
6.  您確認測試電子郵件已成功後，請按一下**[確定]**以儲存您的更新的設定。  
  
### <a name="test-email-notification-does-not-list-any-alerts"></a>測試電子郵件通知未列出任何警示  
 **問題**即使有警示檢視器中列出的警示測試電子郵件通知的警示不會顯示任何通知。  
  
 **方案**未警示報告的警示檢視器中的產生的電子郵件通知。 設定為會擴大為他們健康的定義檔案中的電子郵件通知的警示的電子郵件傳送給指定的電子郵件收件者。  
  
 當您按下**套用並傳送電子郵件**，通常是您會收到範例電子郵件通知未列出的健康警示。 不過，如果已傳送電子郵件通知健康警示找出這項測試程序期間，該警示包含測試電子郵件中。  
  
### <a name="active-alerts-are-displayed-for-an-uninstalled-application"></a>作用中警示會顯示為已解除安裝的應用程式  
 **問題**顯示即使應用程式，並其健康定義檔已被解除安裝的應用程式的作用中警示。  
  
 **方案**您必須手動 delete 解除安裝應用程式的作用中警示。 若要 delete 警示，執行下列動作。  
  
##### <a name="to-delete-an-alert-from-the-server-by-using-the-dashboard"></a>若要從伺服器的警示 delete 使用儀表板  
  
1.  伺服器，從左儀表板。  
  
2.  在瀏覽窗格中，按一下任何顯示警示圖示（重大、警告或資訊）。 這時限警示檢視器。  
  
3.  在警示檢視器，以滑鼠右鍵按一下您想要 delete，然後按一下 [提醒**Delete 此警示**。
