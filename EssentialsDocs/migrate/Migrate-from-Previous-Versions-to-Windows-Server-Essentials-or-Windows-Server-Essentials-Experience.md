---
title: 從舊版移轉到 Windows Server Essentials 或 Windows Server Essentials 體驗
description: 瞭解如何從舊版的 Windows Small Business Server 和 Windows Server Essentials 遷移。
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 2974fb3a-5150-43fd-a73f-3e5074eb5d03
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: e25e8407e960264387ba946d11b721b3b22014bf
ms.sourcegitcommit: 9e19436bd8b20af60284071ab512405aebfbec83
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/29/2020
ms.locfileid: "97810755"
---
# <a name="migrate-from-previous-versions-to-windows-server-essentials-or-windows-server-essentials-experience"></a>從舊版移轉到 Windows Server Essentials 或 Windows Server Essentials 體驗

>適用于： Windows Server 2012 R2 Essentials

本指南說明如何從舊版的 Windows Small Business Server 和 Windows Server Essentials 進行遷移 (包括 Windows Server Essentials、Windows Small Business Server 2011 Standard、Windows Small Business Server 2011 Essentials、Windows small business Server 2008，以及 Windows Small Business Server 2003) 升級至 Windows Server Essentials 或已安裝 Windows server Essentials 體驗角色的 Windows Server 2012 R2。

 **針對最多25位使用者和50裝置的環境**，您可以依照本指南中的步驟，從舊版的 windows SBS 遷移至 Windows Server Essentials。

 **針對高達100使用者和200裝置的環境**，您可以遵循相同的指導方針，遷移至已安裝 Windows Server Essentials 體驗角色的 windows Server 2012 R2 Standard 或 Datacenter 版本。

> [!NOTE]
>  為了避免在移轉期間發生問題， Windows Server Essentials 產品開發團隊強烈建議您在開始移轉之前先閱讀此文件。

## <a name="terms-and-definitions"></a>詞彙和定義
 **來源伺服器** 您要從這裡移轉設定和資料的現有伺服器。

 **目的地伺服器** 您要將設定和資料移轉到這裡的新伺服器。

## <a name="migration-process-summary"></a>移轉程序摘要
 本移轉指南包含下列步驟：

1. [步驟1：準備您的來源伺服器以進行 Windows Server Essentials 遷移](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md)。  您必須確定您的來源伺服器和網路已經準備就緒可以進行移轉。 本節引導您備份來源伺服器、評估來源伺服器系統健康情況、安裝最新的 Service Pack 和修正程式，以及確認網路設定。

2. [步驟2：將 Windows Server Essentials 安裝為新的複本網域控制站](Step-2--Install-Windows-Server-Essentials-as-a-new-replica-domain-controller.md)。 本節說明如何安裝 windows server Essentials 或 Windows Server 2012 R2 Standard (，並啟用 Windows Server Essentials 體驗角色) 做為網域控制站。

3. [步驟3：將電腦加入新的 Windows Server Essentials 伺服器](Step-3--Join-computers-to-the-new-Windows-Server-Essentials-server.md)。  本節說明如何將用戶端電腦加入至執行 Windows Server Essentials 的新伺服器，以及更新群組原則設定。

4. [步驟4：將設定和資料移至目的地伺服器以進行 Windows Server Essentials 遷移](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md)。  本節提供從來源伺服器移轉資料和設定的相關資訊。

5. [步驟5：在目的地伺服器上啟用資料夾重新導向，以進行 Windows Server Essentials 遷移](Step-5--Enable-folder-redirection-on-the-Destination-Server-for-Windows-Server-Essentials-migration.md)。  如果來源伺服器上已啟用資料夾重新導向，您可以在目的地伺服器啟用資料夾重新導向，然後刪除舊的資料夾重新導向群組原則設定。

6. [步驟6：從新的 Windows Server Essentials 網路降級和移除來源伺服器](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md)。  從網路移除來源伺服器之前，您必須強制執行群組原則更新並降級來源伺服器。

7. [步驟7：執行 Windows Server Essentials 遷移的遷移後工作](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md)。  將所有設定和資料移轉到 Windows Server Essentials 之後，您可能會想要將允許的電腦對應到使用者帳戶。

8. [步驟8：執行 Windows Server Essentials 最佳做法分析](Step-8--Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md)程式。  完成將設定和資料移轉到 Windows Server Essentials 之後，您應該執行 Windows Server Essentials 最佳做法分析程式 (BPA) 。

   有幾個移轉程序需要您以系統管理員的身分開啟 [命令提示字元] 視窗。 下列程序說明如何執行這項操作。

###  <a name="to-open-a-command-prompt-window-on-the-source-server-as-an-administrator"></a><a name="BKMK_OpenACommandPromptAsAdmin"></a> 以系統管理員身分在來源伺服器上開啟命令提示字元視窗

1.  按一下 [啟動]。

2.  在搜尋方塊中，輸入 **cmd**。

3.  在結果清單中，以滑鼠右鍵按一下 cmd，然後按一下 [以系統管理員身分執行]。

#### <a name="to-open-a-command-prompt-window-on-the-destination-server-as-an-administrator"></a>以系統管理員身分在目的地伺服器上開啟命令提示字元視窗

1.  在 [開始] 畫面的搜尋方塊中，輸入 **cmd**。

2.  在結果清單中，以滑鼠右鍵按一下 cmd，然後按一下 [以系統管理員身分執行]。

## <a name="additional-references"></a>其他參考資料

-   [移轉伺服器資料到 Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

