---
title: 將 Windows Small Business Server 2008 移轉到 Windows Server Essentials
description: 描述如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 71e3243e-2da9-409a-ae1f-813d4c9062e1
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: afae2e7290d29454eb5041064bec671a66a2076a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851629"
---
# <a name="migrate-windows-small-business-server-2008-to-windows-server-essentials"></a>將 Windows Small Business Server 2008 移轉到 Windows Server Essentials

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

本指南說明如何將現有的 Windows SBS 2008 網域移轉到 Windows Server® 2012 Essentials，在新硬體上，然後再移轉設定和資料。 本指南也說明如何在完成移轉之後，從 Windows Server Essentials 網路移除現有的伺服器。  
  
> [!NOTE]
>  若要避免發生問題，在移轉期間，Windows Server Essentials 產品開發團隊強烈建議您閱讀這份文件，再開始移轉。  
  
> [!NOTE]

>  若要將伺服器資料移轉至 Windows Server Essentials 的最新版本，請參閱[移轉至 Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。  

>  若要將伺服器資料移轉至 Windows Server Essentials 的最新版本，請參閱[移轉至 Windows Server Essentials](../migrate/Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。  

  
## <a name="additional-resources"></a>其他資源  
 如需可協助引導您進行移轉程序的其他資訊、工具和社群資源的連結，請造訪 [Windows Small Business Server 移轉](https://go.microsoft.com/fwlink/?LinkId=217520) 網站。  
  
## <a name="terms-and-definitions"></a>術語與定義  
 **來源伺服器：** 您要移轉設定和資料的現有伺服器。  
  
 **目的地伺服器：** 您要將設定和資料移轉到這裡的新伺服器。  
  
## <a name="migration-process-summary"></a>移轉程序摘要  
 本移轉指南包含下列步驟：  
  

1.  [準備好來源伺服器的 Windows Server Essentials 移轉的](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md)。  您必須確定您的來源伺服器和網路已經準備就緒可以進行移轉。 本節引導您備份來源伺服器、評估來源伺服器系統健康情況、安裝最新的 Service Pack 和修正程式，以及確認網路設定。  
  
2.  [以移轉模式安裝 Windows Server Essentials](Install-Windows-Server-Essentials-in-migration-mode.md)。  本節說明以移轉模式在目的地伺服器上安裝 Windows Server Essentials 時，應該採取的步驟。  
  
3.  [將電腦加入新的 Windows Server Essentials 網路](Join-computers-to-the-new-Windows-Server-Essentials-network.md)。  本章節涵蓋用戶端電腦加入新的 Windows Server Essentials 網路，以及更新群組原則設定。  
  
4.  [將 SBS 2008 設定和資料移到目的地伺服器](Move-Windows-SBS-2008-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md)。  本節提供從來源伺服器移轉資料和設定的相關資訊。  
  
5.  [啟用 Windows Server Essentials 目的地伺服器上的資料夾重新導向](Enable-folder-redirection-on-the-Windows-Server-Essentials-Destination-Server.md)。  如果來源伺服器上已啟用資料夾重新導向，您可以在目的地伺服器啟用資料夾重新導向，然後刪除舊的資料夾重新導向群組原則設定。  
  
6.  [降級和移除來源伺服器從新的 Windows Server Essentials 網路](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md)。  從網路移除來源伺服器之前，您必須強制執行群組原則更新並降級來源伺服器。  
  
7.  [執行 Windows Server Essentials 移轉的移轉後工作](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md)。  完成所有的設定和資料移轉到 Windows Server Essentials 之後，您可能想要將許可的電腦對應到使用者帳戶。  
  
8.  [執行 Windows Server Essentials Best Practices Analyzer](Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md)。  設定和資料到 Windows Server Essentials 移轉完成之後，您應該執行 Windows Server Essentials BPA。  

1.  [準備好來源伺服器的 Windows Server Essentials 移轉的](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md)。  您必須確定您的來源伺服器和網路已經準備就緒可以進行移轉。 本節引導您備份來源伺服器、評估來源伺服器系統健康情況、安裝最新的 Service Pack 和修正程式，以及確認網路設定。  
  
2.  [以移轉模式安裝 Windows Server Essentials](../migrate/Install-Windows-Server-Essentials-in-migration-mode.md)。  本節說明以移轉模式在目的地伺服器上安裝 Windows Server Essentials 時，應該採取的步驟。  
  
3.  [將電腦加入新的 Windows Server Essentials 網路](../migrate/Join-computers-to-the-new-Windows-Server-Essentials-network.md)。  本章節涵蓋用戶端電腦加入新的 Windows Server Essentials 網路，以及更新群組原則設定。  
  
4.  [將 SBS 2008 設定和資料移到目的地伺服器](../migrate/Move-Windows-SBS-2008-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md)。  本節提供從來源伺服器移轉資料和設定的相關資訊。  
  
5.  [啟用 Windows Server Essentials 目的地伺服器上的資料夾重新導向](../migrate/Enable-folder-redirection-on-the-Windows-Server-Essentials-Destination-Server.md)。  如果來源伺服器上已啟用資料夾重新導向，您可以在目的地伺服器啟用資料夾重新導向，然後刪除舊的資料夾重新導向群組原則設定。  
  
6.  [降級和移除來源伺服器從新的 Windows Server Essentials 網路](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md)。  從網路移除來源伺服器之前，您必須強制執行群組原則更新並降級來源伺服器。  
  
7.  [執行 Windows Server Essentials 移轉的移轉後工作](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md)。  完成所有的設定和資料移轉到 Windows Server Essentials 之後，您可能想要將許可的電腦對應到使用者帳戶。  
  
8.  [執行 Windows Server Essentials Best Practices Analyzer](../migrate/Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md)。  設定和資料到 Windows Server Essentials 移轉完成之後，您應該執行 Windows Server Essentials BPA。  

  
 有幾個移轉程序需要您以系統管理員的身分開啟 [命令提示字元] 視窗。  
  
###  <a name="BKMK_OpenACommandPromptAsAdmin"></a> 以系統管理員身分開啟命令提示字元 視窗，在來源伺服器上  
  
1.  按一下 [開始]。  
  
2.  在搜尋方塊中，輸入 cmd。  
  
3.  在結果清單中，以滑鼠右鍵按一下 cmd，然後按一下 [以系統管理員身分執行]。  
  
#### <a name="to-open-a-command-prompt-window-on-the-destination-server-as-an-administrator"></a>以系統管理員身分在目的地伺服器上開啟命令提示字元視窗  
  
1.  在 [開始] 畫面的搜尋方塊中，輸入 **cmd**。  
  
2.  在結果清單中，以滑鼠右鍵按一下 cmd，然後按一下 [以系統管理員身分執行]。
