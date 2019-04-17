---
title: "從舊版移轉到 Windows Server Essentials 或 Windows Server Essentials 的體驗"
description: "告訴您如何使用 Windows Server Essentials"
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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="migrate-from-previous-versions-to-windows-server-essentials-or-windows-server-essentials-experience"></a>從舊版移轉到 Windows Server Essentials 或 Windows Server Essentials 的體驗

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

本指南告訴您如何移轉舊版 Windows 小型企業 Server 與 Windows Server Essentials（包括 Windows Server Essentials、Windows 小型企業 Server 2011 標準，Windows 小型企業 Server 2011 Essentials、Windows 小型企業 Server 2008 和 Windows 小型企業 Server 2003）的 Windows Server Essentials 或 Windows Server 2012 R2 已安裝 Windows Server Essentials 體驗角色。  
  
 **最多 25 位使用者與 50 裝置環境**，您可以依照本指南從舊版的 Windows SBS 移轉到 Windows Server Essentials 的步驟。  
  
 **環境 100 最多使用者與 200 裝置的**，您可以遵循相同的指導方針與 Windows Server Essentials 體驗角色安裝移轉到標準或 Datacenter 版本的 Windows Server 2012 R2。  
  
> [!NOTE]
>  若要避免移轉的問題，Windows Server Essentials product 開發團隊非常建議您朗讀這份文件，才能開始移轉。  
  
## <a name="terms-and-definitions"></a>條款及定義  
 **來源伺服器**設定和資料，您的移轉現有的伺服器。  
  
 **目的地伺服器**您的移轉您的設定和資料新的伺服器。  
  
## <a name="migration-process-summary"></a>移轉處理程序摘要  
 此移轉指南包含下列步驟：  
  
1.  [步驟 1：準備來源 Windows Server Essentials 伺服器移轉](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md)。  您必須先確定您的網路和來源伺服器，可供移轉。 本章節逐步引導您備份來源伺服器、評估來源伺服器系統健康、安裝的最新的 service pack 和修正程式，以及驗證網路設定。  
  
2.  [步驟 2：安裝 Windows Server Essentials 的全新複本網域控制站為](Step-2--Install-Windows-Server-Essentials-as-a-new-replica-domain-controller.md)。 本節為網域控制站告訴您如何安裝 Windows Server Essentials，或 Windows Server 2012 標準 R2（與 Windows Server Essentials 體驗角色功能）。  
  
3.  [步驟 3：電腦加入新的 Windows Server Essentials 伺服器](Step-3--Join-computers-to-the-new-Windows-Server-Essentials-server.md)。  本章節如何 client 電腦加入新的伺服器執行 Windows Server Essentials 和更新的群組原則設定。  
  
4.  [步驟 4：前往設定和資料目的 Windows Server Essentials 伺服器移轉](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md)。  本章節從來源伺服器提供移轉資料和設定的相關資訊。  
  
5.  [步驟 5：讓資料夾重新導向目的地 Windows Server Essentials 伺服器移轉上的](Step-5--Enable-folder-redirection-on-the-Destination-Server-for-Windows-Server-Essentials-migration.md)。  如果來源伺服器上為支援資料夾重新導向，您可以讓資料夾重新導向目的伺服器，並再 delete 舊的資料夾重新導向群組原則設定。  
  
6.  [步驟 6：降級並移除新的 Windows Server Essentials 網路來源伺服器](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md)。  之前來源伺服器移除網路，您必須強迫「群組原則」更新，並降級來源伺服器。  
  
7.  [步驟 7：執行 Windows Server Essentials 移轉後的移轉工作](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md)。  將所有設定和資料都移轉到 Windows Server Essentials 完成之後，您可能想要對應帳號允許的電腦。  
  
8.  [步驟 8：執行 Windows Server Essentials 最佳做法分析](Step-8--Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md)。  移轉設定與 Windows Server Essentials 的資料完成之後，您應該先執行 Windows Server Essentials 最佳做法分析 (BPA)。  
  
 有幾個移轉程序需要您開放以系統管理員身分命令提示字元視窗。 下列程序如何執行此動作。  
  
###  <a name="BKMK_OpenACommandPromptAsAdmin"></a>以系統管理員身分打開來源伺服器上的命令提示字元視窗  
  
1.  按一下**[開始]**。  
  
2.  在搜尋方塊中，輸入**cmd**。  
  
3.  在結果清單中，以滑鼠右鍵按一下**cmd**，然後按**以系統管理員身分執行**。  
  
#### <a name="to-open-a-command-prompt-window-on-the-destination-server-as-an-administrator"></a>以系統管理員身分打開目的伺服器上的命令提示字元視窗  
  
1.  在**[開始]**畫面上的，在搜尋方塊中，輸入**cmd**。  
  
2.  在結果清單中，以滑鼠右鍵按一下**cmd**，然後按**以系統管理員身分執行**。  
  
## <a name="see-also"></a>也了  
  
-   [伺服器資料移轉到 Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [伺服器資料移轉到 Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

