---
title: 將 Windows Small Business Server 2011 Essentials 移轉到 Windows Server Essentials
description: 瞭解如何將現有的 Windows Small Business Server 2011 Essentials 網域遷移到 Windows Server 2012 Essentials，然後遷移設定和資料。
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 32fc90d8-31c5-4c7e-9fe3-483cf3c35f78
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 7f59ee81edab99145628c5bc3d4c487a51408103
ms.sourcegitcommit: 9e19436bd8b20af60284071ab512405aebfbec83
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/29/2020
ms.locfileid: "97810855"
---
# <a name="migrate-windows-small-business-server-2011-essentials-to-windows-server-essentials"></a>將 Windows Small Business Server 2011 Essentials 移轉到 Windows Server Essentials

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

本指南說明如何將現有的 Windows Small Business Server 2011 Essentials 網域遷移到 Windows Server &reg; 2012 Essentials，然後遷移設定和資料。 本指南也會說明如何在完成遷移之後，從 Windows Server Essentials 網路移除現有的伺服器。

> [!NOTE]
>  為了避免在遷移期間發生問題，Windows Server Essentials 產品開發團隊強烈建議您在開始進行遷移之前，先閱讀這份檔。
>
> [!NOTE]
>
>  若要將您的伺服器資料移轉至最新版本的 Windows Server Essentials，請參閱 [遷移至 Windows Server essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。


## <a name="additional-resources"></a>其他資源
 如需可協助引導您進行移轉程序的其他資訊、工具和社群資源的連結，請參閱 [Windows Small Business Server 移轉](https://go.microsoft.com/fwlink/?LinkId=217520)。

## <a name="terms-and-definitions"></a>詞彙和定義
 **來源伺服器：** 您要從中遷移設定和資料的現有伺服器。

 **目的地伺服器：** 您要將設定和資料移轉到其中的新伺服器。

## <a name="migration-process-summary"></a>移轉程序摘要
 本移轉指南包含下列步驟：


1.  [準備您的來源伺服器以進行 Windows Server Essentials 遷移](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md)。  您必須確定您的來源伺服器和網路已經準備就緒可以進行移轉。 本節引導您備份來源伺服器、評估來源伺服器系統健康情況、安裝最新的 Service Pack 和修正程式，以及確認網路設定。

2.  [以移轉模式安裝 Windows Server Essentials](Install-Windows-Server-Essentials-in-migration-mode.md)。  本節說明以移轉模式在目的地伺服器上安裝 Windows Server Essentials 所應採取的步驟。

3.  [將電腦加入新的 Windows Server Essentials 伺服器](Join-computers-to-the-new-Windows-Server-Essentials-server.md)。  本節內容涵蓋將用戶端電腦加入新的 Windows Server Essentials 伺服器，以及更新群組原則設定。

4.  [將 SBS 2011 Essentials 設定和資料移至目的地伺服器](Move-Windows-SBS-2011-Essentials-to-the-Destination-Server-for-migration.md)。  本節提供從來源伺服器移轉資料和設定的相關資訊。

5.  [在 Windows Server Essentials 目的地伺服器上啟用資料夾](Enable-folder-redirection-on-the-Windows-Server-Essentials-Destination-Server.md)重新導向。  如果來源伺服器上已啟用資料夾重新導向，您可以在目的地伺服器啟用資料夾重新導向，然後刪除舊的資料夾重新導向群組原則設定。

6.  [從新的 Windows Server Essentials 網路降級和移除來源伺服器](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md)。  從網路移除來源伺服器之前，您必須強制執行群組原則更新並降級來源伺服器。

7.  [執行 Windows Server Essentials 遷移的遷移後](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md)工作。  將所有設定和資料移轉到 Windows Server Essentials 之後，您可能會想要將允許的電腦對應到使用者帳戶。

8.  [執行 Windows Server Essentials 最佳做法分析](Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md)程式。  完成將設定和資料移轉到 Windows Server Essentials 之後，您應該下載並執行 Windows Server Essentials BPA。

 有幾個移轉程序需要您以系統管理員的身分開啟命令提示字元視窗。

###  <a name="to-open-a-command-prompt-window-on-the-source-server-as-an-administrator"></a><a name="BKMK_OpenACommandPromptAsAdmin"></a> 以系統管理員身分在來源伺服器上開啟命令提示字元視窗

1.  按一下 [啟動]。

2.  在搜尋方塊中，輸入 **cmd**。

3.  在結果清單中，以滑鼠右鍵按一下 cmd，然後按一下 [以系統管理員身分執行]。

#### <a name="to-open-a-command-prompt-window-on-the-destination-server-as-an-administrator"></a>以系統管理員身分在目的地伺服器上開啟命令提示字元視窗

1.  在 [開始] 畫面的搜尋方塊中，輸入 **cmd**。

2.  在結果清單中，以滑鼠右鍵按一下 cmd，然後按一下 [以系統管理員身分執行]。
