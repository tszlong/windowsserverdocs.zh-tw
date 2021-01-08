---
title: 監視「遠端存取」伺服器及其元件的操作狀態
description: 瞭解如何使用遠端存取服務器的管理主控台來監視其操作狀態。
manager: brianlic
ms.topic: article
ms.assetid: 077a3a64-2fa3-4994-9711-ec1fbdc081ba
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 74d43d4f1feacdeb10eb5e1ee55462187cf628f5
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98039778"
---
# <a name="monitor-the-operations-status-of-the-remote-access-server-and-its-components"></a>監視「遠端存取」伺服器及其元件的操作狀態

>適用於：Windows Server (半年度管道)、Windows Server 2016

**注意：** Windows Server 2012 將 DirectAccess 與「路由及遠端存取服務」(RRAS) 整合成一個遠端存取角色。

您可以使用遠端存取服務器的管理主控台來監視其作業狀態。

> [!NOTE]
> 您必須以 Domain Admins 群組成員或每部電腦上 Administrators 群組成員的身分登入，才能完成本主題中所述的工作。 如果您是以 Administrators 群組成員的帳戶登入，而無法完成工作，請在使用 Domain Admins 群組成員的帳戶登入時，嘗試執行工作。

#### <a name="to-monitor-the-remote-access-server-operations-status"></a>監視遠端存取服務器操作狀態

1.  在 [伺服器管理員] 中，按一下 [工具]，然後按一下 [遠端存取管理]。

2.  按一下 [**儀表板**]，以流覽至 [**遠端存取管理] 主控台** 中的 [**遠端存取報告**]。

3.  在 [監視] 儀表板上，注意 [**伺服器狀態**] 磚內的 [**操作狀態**] 磚。 此磚會列出伺服器操作狀態，以及所有伺服器元件的狀態。

4.  在右窗格中 **，按一下 [** 工作] 底下的 [重新整理]， **重載作業狀態** 。 作業狀態會每隔五分鐘自動重新整理，這是預設的重新整理間隔。 若要變更預設的重新整理間隔，請按一下 [設定重新整理 **間隔**]。

![Windows PowerShell ](../../../media/Monitor-the-operations-status-of-the-Remote-Access-server-and-its-components/PowerShellLogoSmall.gif)**_<em>Windows PowerShell 對等命令</em>_**

下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。

> [!NOTE]
> 包含叢集作業狀態的命令以供參考。

```
PS> Get-RemoteAccessHealth
PS> Get-RemoteAccessHealth -Cluster
```



