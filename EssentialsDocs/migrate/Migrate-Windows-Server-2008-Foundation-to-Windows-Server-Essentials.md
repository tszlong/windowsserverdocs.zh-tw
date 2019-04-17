---
title: "移轉到 Windows Server Essentials 的 Windows Server 2008 基本知識"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f22fc0a4-cb82-4e60-afe6-2d03145745e7
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 22721833d3ba97c7860c949764d565415bbc7cab
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="migrate-windows-server-2008-foundation-to-windows-server-essentials"></a>移轉到 Windows Server Essentials 的 Windows Server 2008 基本知識

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

本指南告訴您如何將現有的 Windows Server 2008 基本知識網域移轉到新的硬體，在 Windows Server® 2012 年基本資訊，然後設定和資料移轉。 本指南以及如何從 Windows Server Essentials 網路移除您現有的伺服器，當您完成移轉。  
  
> [!NOTE]
>  若要避免發生問題移轉期間，Windows Server Essentials product 開發團隊非常建議您朗讀這份文件，才能開始移轉。  
  
## <a name="additional-resources"></a>其他資源  
 如需詳細資訊，工具和社群資源協助引導您完成此移轉程序的連結，請查看[Windows 小型企業伺服器移轉](https://go.microsoft.com/fwlink/?LinkId=217520)。  
  
## <a name="terms-and-definitions"></a>條款及定義  
 **來源 Server:**設定和資料，您的移轉現有的伺服器。  
  
 **目的地 Server:**您的移轉您的設定和資料新的伺服器。  
  
## <a name="migration-process-summary"></a>移轉處理程序摘要  
 此移轉指南包含下列步驟：  
  

1.  [準備來源 Windows Server Essentials 伺服器移轉](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md)。  您必須先確定您的網路和來源伺服器，可供移轉。 本章節逐步引導您備份來源伺服器、評估來源伺服器系統健康、安裝的最新的 service pack 和修正程式，以及驗證網路設定。  
  
2.  [安裝 Windows Server Essentials 移轉模式](Install-Windows-Server-Essentials-in-migration-mode.md)。  本章節告訴您，您應該帶到 Windows Server Essentials 的伺服器上安裝目的地移轉模式中的步驟。  
  
3.  [電腦加入新的 Windows Server Essentials 網路](Join-computers-to-the-new-Windows-Server-Essentials-network.md)。  本章節鍵盤保護蓋 client 電腦加入新的 Windows Server Essentials 網路和更新的群組原則設定。  
  
4.  [Windows Server 2008 基本知識設定和資料前往目的地伺服器](Move-Windows-Server-2008-Foundation-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md)。  本章節從來源伺服器提供移轉資料和設定的相關資訊。  
  
5.  [降級並移除新的 Windows Server Essentials 網路來源伺服器](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md)。  之前來源伺服器移除網路，您必須強迫「群組原則」更新，並降級來源伺服器。  
  
6.  [針對 Windows Server Essentials 移轉執行後的移轉工作](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md)。  將所有設定和資料都移轉到 Windows Server Essentials 完成之後，您可能想要對應帳號允許的電腦。  
  
7.  [執行 Windows Server Essentials 最佳做法分析](Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md)。  移轉設定與 Windows Server Essentials 的資料完成之後，您應該先執行 Windows Server Essentials BPA。  

1.  [準備來源 Windows Server Essentials 伺服器移轉](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md)。  您必須先確定您的網路和來源伺服器，可供移轉。 本章節逐步引導您備份來源伺服器、評估來源伺服器系統健康、安裝的最新的 service pack 和修正程式，以及驗證網路設定。  
  
2.  [安裝 Windows Server Essentials 移轉模式](../migrate/Install-Windows-Server-Essentials-in-migration-mode.md)。  本章節告訴您，您應該帶到 Windows Server Essentials 的伺服器上安裝目的地移轉模式中的步驟。  
  
3.  [電腦加入新的 Windows Server Essentials 網路](../migrate/Join-computers-to-the-new-Windows-Server-Essentials-network.md)。  本章節鍵盤保護蓋 client 電腦加入新的 Windows Server Essentials 網路和更新的群組原則設定。  
  
4.  [Windows Server 2008 基本知識設定和資料前往目的地伺服器](../migrate/Move-Windows-Server-2008-Foundation-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md)。  本章節從來源伺服器提供移轉資料和設定的相關資訊。  
  
5.  [降級並移除新的 Windows Server Essentials 網路來源伺服器](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md)。  之前來源伺服器移除網路，您必須強迫「群組原則」更新，並降級來源伺服器。  
  
6.  [針對 Windows Server Essentials 移轉執行後的移轉工作](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md)。  將所有設定和資料都移轉到 Windows Server Essentials 完成之後，您可能想要對應帳號允許的電腦。  
  
7.  [執行 Windows Server Essentials 最佳做法分析](../migrate/Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md)。  移轉設定與 Windows Server Essentials 的資料完成之後，您應該先執行 Windows Server Essentials BPA。  

  
 有幾個移轉程序需要您開放以系統管理員身分命令提示字元視窗。  
  
###  <a name="BKMK_OpenACommandPromptAsAdmin"></a>以系統管理員身分打開來源伺服器上的命令提示字元視窗  
  
1.  按一下**[開始]**。  
  
2.  在搜尋方塊中，輸入**cmd**。  
  
3.  在結果清單中，以滑鼠右鍵按一下**cmd**，然後按**以系統管理員身分執行**。  
  
#### <a name="to-open-a-command-prompt-window-on-the-destination-server-as-an-administrator"></a>以系統管理員身分打開目的伺服器上的命令提示字元視窗  
  
1.  在**[開始]**畫面上的，在搜尋方塊中，輸入**cmd**。  
  
2.  在結果清單中，以滑鼠右鍵按一下**cmd**，然後按**以系統管理員身分執行**。
