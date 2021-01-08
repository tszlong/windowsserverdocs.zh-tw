---
title: 監視「遠端存取」伺服器上現有的負載
description: 瞭解如何使用 [監視] 儀表板來查看伺服器的負載統計資料，或者您可以使用效能監視器計數器來追蹤統計資料。
manager: brianlic
ms.topic: article
ms.assetid: 62fa2895-62ae-42cf-817c-53e06ac2a26c
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 999a4e2a0f660368e0d7f5cfce4e1f7217029311
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98039838"
---
# <a name="monitor-the-existing-load-on-the-remote-access-server"></a>監視「遠端存取」伺服器上現有的負載

>適用於：Windows Server (半年度管道)、Windows Server 2016

**注意：** Windows Server 2012 將 DirectAccess 與「路由及遠端存取服務」(RRAS) 整合成一個遠端存取角色。

「 **載入** 」一詞指的是與遠端存取服務器上的連接數目相關的統計資料。 以下是追蹤遠端存取服務器上的負載所需的步驟。

您可以使用遠端存取服務器上管理主控台中的 [監視] 儀表板來查看伺服器的負載統計資料，也可以使用效能監視器計數器來追蹤統計資料。

> [!NOTE]
> 您必須以 Domain Admins 群組成員或每部電腦上 Administrators 群組成員的身分登入，才能完成本主題中所述的工作。 如果您是以 Administrators 群組成員的帳戶登入，而無法完成工作，請在使用 Domain Admins 群組成員的帳戶登入時，嘗試執行工作。

#### <a name="to-use-the-monitoring-dashboard-to-monitor-the-remote-access-server-load"></a>使用監視儀表板來監視遠端存取服務器負載

1.  在 [伺服器管理員] 中，按一下 [工具]，然後按一下 [遠端存取管理]。

2.  按一下 [儀表板] 以瀏覽到 [遠端存取管理主控台] 中的 [遠端存取儀表板]。

3.  在 [監視] 儀表板上，注意 [**伺服器狀態**] 磚內的 [**遠端用戶端狀態**] 磚。 此磚會列出統計資料，例如已連線的遠端用戶端總數、連線的 DirectAccess 用戶端總數，以及過去24小時內連接的最大使用者數目。

4.  您 **可以在右** 窗格中，按一下 [工作] **底下的 [** 重新整理]，以重載健全狀況狀態。 若要變更預設重新整理間隔，請按一下 [工作]**底下的 [** 設定重新整理 **間隔**]。

#### <a name="to-use-the-performance-monitor-tool-to-monitor-performance-counters-on-the-remote-access-server"></a>使用效能監視器工具監視遠端存取服務器上的效能計數器

1.  按一下 [ **開始**]，按一下 [系統 **管理工具**]，然後按兩下 [ **效能監視器**]。

2.  在 [ **效能**] 底下，按一下 [ **效能監視器**]。

3.  按一下 [ **新增** ] 按鈕， (**效能監視器** 工具列中的綠色交叉圖示) 表示。

4.  從 **可用計數器** 的清單中，選取 [ **RAS** ] 和 [ **RAmgmtsvc** ] 類別中的所有計數器，然後按一下 [ **新增>>**]。

5.  同樣地，從 **可用計數器** 的清單中，選取 [ **IPsec 連接** ] 類別中的所有計數器，然後按一下 [ **新增>>]。**

6.  按一下 **[確定]** ，在 **效能監視器** 主控台中新增選取的計數器以進行追蹤。

**效能監視器** 現在會以圖形方式顯示選取的伺服器負載統計資料。

![Windows PowerShell ](../../../media/Monitor-the-existing-load-on-the-Remote-Access-server/PowerShellLogoSmall.gif)**_<em>Windows PowerShell 對等命令</em>_**

下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。

```
PS> Get-RemoteAccessConnectionStatisticsSummary
```



