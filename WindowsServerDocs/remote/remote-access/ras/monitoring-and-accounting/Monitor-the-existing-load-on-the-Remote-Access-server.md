---
title: 監視「遠端存取」伺服器上現有的負載
description: 本主題是適用於遠端存取 」 監視和指南 Windows Server 2016 中的帳戶處理的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 62fa2895-62ae-42cf-817c-53e06ac2a26c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dc232a52e82f3b66164d30a134ed9e422db0964a
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282677"
---
# <a name="monitor-the-existing-load-on-the-remote-access-server"></a>監視「遠端存取」伺服器上現有的負載

>適用於：Windows Server （半年通道），Windows Server 2016

**注意：** Windows Server 2012 將 DirectAccess 以及「路由及遠端存取服務」(RRAS) 合併成一個遠端存取角色。  
  
詞彙**負載**指的是遠端存取伺服器上的連接數目與相關的統計資料。 以下是追蹤遠端存取伺服器上的負載所需的步驟。  
  
您可以使用位於遠端存取伺服器，以檢視伺服器的負載統計資料的 [管理] 主控台的監視儀表板，或您可以使用效能監視器計數器來追蹤統計資料。  
  
> [!NOTE]  
> 您必須登入以 Domain Admins 群組的成員或每一部電腦上的 Administrators 群組的成員才能完成本主題中所述的工作。 如果您在登入是系統管理員群組成員的帳戶，您無法完成工作，請嘗試執行工作，您在登入的帳戶是 Domain Admins 群組的成員。  
  
#### <a name="to-use-the-monitoring-dashboard-to-monitor-the-remote-access-server-load"></a>若要使用監視儀表板監視遠端存取伺服器負載  
  
1.  在 [伺服器管理員]  中，按一下 [工具]  ，然後按一下 [遠端存取管理]  。  
  
2.  按一下 [儀表板]  以瀏覽到 [遠端存取管理主控台]  中的 [遠端存取儀表板]  。  
  
3.  在監視儀表板，請注意**遠端用戶端狀態**圖格內**伺服器狀態**圖格。 此圖格會列出連接的遠端用戶端總數、 已連線的 DirectAccess 用戶端總數等的連線過去 24 小時內的使用者數目上限的統計資料。  
  
4.  您可以按一下**重新整理**下方**工作**重新載入健全狀況狀態的右窗格中。 若要變更預設重新整理間隔，請按一下**設定重新整理間隔**下方**工作**。  
  
#### <a name="to-use-the-performance-monitor-tool-to-monitor-performance-counters-on-the-remote-access-server"></a>若要使用效能監視器工具監視遠端存取伺服器上的效能計數器  
  
1.  按一下 **開始**，按一下**系統管理工具**，然後按兩下**效能監視器**。  
  
2.  底下**效能**，按一下**效能監視器**。  
  
3.  按一下 **新增**中 （以綠色的字形圖示表示） 的按鈕**效能監視器**工具列。  
  
4.  從清單中的**可用的計數器**，選取中的所有計數器**RAS**並**RAmgmtsvc**類別目錄，然後再按一下**新增 >>** .  
  
5.  同樣地，從清單**可用的計數器**，選取中的所有計數器**IPsec 連線**分類，，然後按一下**新增 >>。**  
  
6.  按一下  **確定**將在所選的計數器**效能監視器**主控台進行追蹤。  
  
**效能監視器**以圖形方式現在會顯示所選取的伺服器負載統計資料。  
  
![Windows PowerShell](../../../media/Monitor-the-existing-load-on-the-Remote-Access-server/PowerShellLogoSmall.gif)***<em>Windows PowerShell 對等的命令</em>***  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
```  
PS> Get-RemoteAccessConnectionStatisticsSummary  
```  
  


