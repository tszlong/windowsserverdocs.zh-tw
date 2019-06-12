---
title: 監視「遠端存取」伺服器及其元件的操作狀態
description: 本主題是適用於遠端存取 」 監視和指南 Windows Server 2016 中的帳戶處理的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 077a3a64-2fa3-4994-9711-ec1fbdc081ba
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7f4c7e1418e541e1f913c8a20cbda3456c1c3802
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446605"
---
# <a name="monitor-the-operations-status-of-the-remote-access-server-and-its-components"></a>監視「遠端存取」伺服器及其元件的操作狀態

>適用於：Windows Server （半年通道），Windows Server 2016

**注意：** Windows Server 2012 將 DirectAccess 以及「路由及遠端存取服務」(RRAS) 合併成一個遠端存取角色。  
  
在遠端存取伺服器的 [管理] 主控台可用來監視其作業狀態。  
  
> [!NOTE]  
> 您必須登入以 Domain Admins 群組的成員或每一部電腦上的 Administrators 群組的成員才能完成本主題中所述的工作。 如果您在登入是系統管理員群組成員的帳戶，您無法完成工作，請嘗試執行工作，您在登入的帳戶是 Domain Admins 群組的成員。  
  
#### <a name="to-monitor-the-remote-access-server-operations-status"></a>若要監視遠端存取伺服器操作狀態  
  
1.  在 [伺服器管理員]  中，按一下 [工具]  ，然後按一下 [遠端存取管理]  。  
  
2.  按一下 **儀表板**瀏覽至**遠端存取報告**中**遠端存取管理主控台**。  
  
3.  在監視儀表板，請注意**操作狀態**圖格內**伺服器狀態**圖格。 此圖格會列出伺服器操作狀態和所有伺服器元件的狀態。  
  
4.  按一下 **重新整理**下方**工作**重新載入的操作狀態的右窗格中。 操作狀態會自動重新整理每隔五分鐘，這是預設重新整理間隔。 若要變更預設重新整理間隔，請按一下**設定重新整理間隔**。  
  
![Windows PowerShell](../../../media/Monitor-the-operations-status-of-the-Remote-Access-server-and-its-components/PowerShellLogoSmall.gif)***<em>Windows PowerShell 對等的命令</em>***  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
> [!NOTE]  
> 如需參考包含叢集的作業狀態的命令。  
  
```  
PS> Get-RemoteAccessHealth  
PS> Get-RemoteAccessHealth -Cluster  
```  
  


