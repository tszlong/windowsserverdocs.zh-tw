---
title: 從舊版移轉到 Windows Server Essentials 或 Windows Server Essentials 體驗
description: 描述如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/20116
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2974fb3a-5150-43fd-a73f-3e5074eb5d03
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 213ee4304d9d4ebdb7580f7f78fdaca78aa454c9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883869"
---
# <a name="migrate-from-previous-versions-to-windows-server-essentials-or-windows-server-essentials-experience"></a>從舊版移轉到 Windows Server Essentials 或 Windows Server Essentials 體驗

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

本指南說明如何從舊版的 Windows Small Business Server 和 Windows Server Essentials （包括 Windows Server Essentials、 Windows Small Business Server 2011 Standard、 Windows Small Business Server 2011 Essentials、 Windows 移轉Small Business Server 2008 和 Windows Small Business Server 2003） 到 Windows Server Essentials 或已安裝的 Windows Server Essentials 體驗角色的 Windows Server 2012 R2。  
  
 **使用多達 25 名使用者及 50 台裝置的環境**，您可以遵循本指南，以從舊版的 Windows SBS 移轉到 Windows Server Essentials 中的步驟。  
  
 **使用最多 100 位使用者和 200 台裝置的環境**，您可以遵循相同的指引移轉到 Windows Server 2012 r2 Standard 或 Datacenter edition 已安裝的 Windows Server Essentials 體驗角色。  
  
> [!NOTE]
>  為了避免在移轉期間發生問題， Windows Server Essentials 產品開發團隊強烈建議您在開始移轉之前先閱讀此文件。  
  
## <a name="terms-and-definitions"></a>術語與定義  
 **來源伺服器** 您要從這裡移轉設定和資料的現有伺服器。  
  
 **目的地伺服器** 您要將設定和資料移轉到這裡的新伺服器。  
  
## <a name="migration-process-summary"></a>移轉程序摘要  
 本移轉指南包含下列步驟：  
  
1.  [步驟 1：準備好來源伺服器的 Windows Server Essentials 移轉的](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md)。  您必須確定您的來源伺服器和網路已經準備就緒可以進行移轉。 本節引導您備份來源伺服器、評估來源伺服器系統健康情況、安裝最新的 Service Pack 和修正程式，以及確認網路設定。  
  
2.  [步驟 2：Windows Server Essentials 安裝為新的複本網域控制站](Step-2--Install-Windows-Server-Essentials-as-a-new-replica-domain-controller.md)。 本節說明如何使用來安裝 Windows Server Essentials 或 Windows Server 2012 R2 Standard （已啟用 「 Windows Server Essentials 體驗角色） 為網域控制站。  
  
3.  [步驟 3：將電腦加入新的 Windows Server Essentials 伺服器](Step-3--Join-computers-to-the-new-Windows-Server-Essentials-server.md)。  本節說明如何將用戶端電腦加入新的伺服器執行 Windows Server Essentials，以及更新群組原則設定。  
  
4.  [步驟 4：將設定和資料移至目的地伺服器的 Windows Server Essentials 移轉的](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md)。  本節提供從來源伺服器移轉資料和設定的相關資訊。  
  
5.  [步驟 5：啟用移轉的目的地伺服器的 Windows Server Essentials 上的資料夾重新導向](Step-5--Enable-folder-redirection-on-the-Destination-Server-for-Windows-Server-Essentials-migration.md)。  如果來源伺服器上已啟用資料夾重新導向，您可以在目的地伺服器啟用資料夾重新導向，然後刪除舊的資料夾重新導向群組原則設定。  
  
6.  [步驟 6：降級和移除來源伺服器從新的 Windows Server Essentials 網路](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md)。  從網路移除來源伺服器之前，您必須強制執行群組原則更新並降級來源伺服器。  
  
7.  [步驟 7：執行 Windows Server Essentials 移轉的移轉後工作](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md)。  完成所有的設定和資料移轉到 Windows Server Essentials 之後，您可能想要將許可的電腦對應到使用者帳戶。  
  
8.  [步驟 8：執行 Windows Server Essentials Best Practices Analyzer](Step-8--Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md)。  設定和資料到 Windows Server Essentials 移轉完成之後，您應該執行 Windows Server Essentials Best Practices Analyzer (BPA)。  
  
 有幾個移轉程序需要您以系統管理員的身分開啟 [命令提示字元] 視窗。 下列程序說明如何執行這項操作。  
  
###  <a name="BKMK_OpenACommandPromptAsAdmin"></a> 以系統管理員身分開啟命令提示字元 視窗，在來源伺服器上  
  
1.  按一下 [開始] 。  
  
2.  在搜尋方塊中，輸入 **cmd**。  
  
3.  在結果清單中，以滑鼠右鍵按一下 cmd，然後按一下 [以系統管理員身分執行]。  
  
#### <a name="to-open-a-command-prompt-window-on-the-destination-server-as-an-administrator"></a>以系統管理員身分在目的地伺服器上開啟命令提示字元視窗  
  
1.  在 [開始] 畫面的搜尋方塊中，輸入 **cmd**。  
  
2.  在結果清單中，以滑鼠右鍵按一下 cmd，然後按一下 [以系統管理員身分執行]。  
  
## <a name="see-also"></a>另請參閱  
  
-   [將伺服器資料移轉到 Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [將伺服器資料移轉到 Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

