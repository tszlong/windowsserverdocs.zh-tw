---
title: 監視「遠端存取」伺服器上現有的負載
description: 本主題是 Windows Server 2016 中遠端存取監視和帳戶處理指南的一部分。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 62fa2895-62ae-42cf-817c-53e06ac2a26c
ms.author: lizross
author: eross-msft
ms.openlocfilehash: ba9aa4ea0e8031601ce04fd25814785e4b739f51
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860531"
---
# <a name="monitor-the-existing-load-on-the-remote-access-server"></a>監視「遠端存取」伺服器上現有的負載

>適用於：Windows Server (半年通道)、Windows Server 2016

**注意：** Windows Server 2012 將 DirectAccess 與「路由及遠端存取服務」(RRAS) 整合成一個遠端存取角色。  
  
「**載入**」一詞指的是與「遠端存取」伺服器上的連接數目相關的統計資料。 以下是在遠端存取服務器上追蹤負載所需的步驟。  
  
您可以使用遠端存取服務器上的管理主控台所提供的 [監視] 儀表板來查看伺服器的負載統計資料，也可以使用效能監視器計數器來追蹤統計資料。  
  
> [!NOTE]  
> 您必須以 Domain Admins 群組成員或每部電腦上 Administrators 群組成員的身分登入，才能完成本主題中所述的工作。 如果您使用 Administrators 群組成員的帳戶登入時無法完成工作，請嘗試在使用屬於 Domain Admins 群組成員的帳戶登入時執行此項工作。  
  
#### <a name="to-use-the-monitoring-dashboard-to-monitor-the-remote-access-server-load"></a>使用監視儀表板來監視遠端存取服務器負載  
  
1.  在 [伺服器管理員] 中，按一下 [工具]，然後按一下 [遠端存取管理]。  
  
2.  按一下 [儀表板] 以瀏覽到 [遠端存取管理主控台] 中的 [遠端存取儀表板]。  
  
3.  在 [監視] 儀表板上，請注意 [**伺服器狀態**] 磚中的 [**遠端用戶端狀態**] 磚。 此磚會列出統計資料，例如連線的遠端用戶端總數、連線的 DirectAccess 用戶端總數，以及過去24小時內連線的最大使用者數目。  
  
4.  您**可以按一下右**窗格**中 [** 工作] 底下的 [重新整理]，重載健全狀況狀態。 若要變更預設重新整理間隔，請按一下 [工作 **] 下的**[設定重新整理**間隔**]。  
  
#### <a name="to-use-the-performance-monitor-tool-to-monitor-performance-counters-on-the-remote-access-server"></a>使用效能監視器工具監視遠端存取服務器上的效能計數器  
  
1.  依序按一下 [**開始**] 和 [系統**管理工具**]，然後按兩下 [**效能監視器**]。  
  
2.  在 [**效能**] 底下，按一下 [**效能監視器**]。  
  
3.  按一下 [**效能監視器**] 工具列中的 [**新增**] 按鈕（以綠色十字圖示表示）。  
  
4.  從**可用的計數器**清單中，選取 [ **RAS**和**RAmgmtsvc** ] 類別中的所有計數器，然後按一下 [**新增 > >** ]。  
  
5.  同樣地，從可用的**計數器**清單中，選取 [ **IPsec**連線] 類別目錄中的所有計數器，然後按一下 [**新增 > >]。**  
  
6.  按一下 **[確定]** ，在 [**效能監視器**] 主控台中新增選取的計數器以進行追蹤。  
  
**效能監視器**現在會以圖形方式顯示所選取的伺服器負載統計資料。  
  
![Windows PowerShell](../../../media/Monitor-the-existing-load-on-the-Remote-Access-server/PowerShellLogoSmall.gif)***<em>windows powershell 對等命令</em>***  
  
下列 Windows PowerShell 指令程式會執行與前述程序相同的功能。 請逐行各輸入一個指令程式，儘管有些指令程式可能因為受制於內文格式而自動換行拆成好幾行。  
  
```  
PS> Get-RemoteAccessConnectionStatisticsSummary  
```  
  


