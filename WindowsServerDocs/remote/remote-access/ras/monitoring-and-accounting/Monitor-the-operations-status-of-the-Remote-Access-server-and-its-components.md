---
title: 監視「遠端存取」伺服器及其元件的操作狀態
description: 本主題是 Windows Server 2016 中遠端存取監視和帳戶處理指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 077a3a64-2fa3-4994-9711-ec1fbdc081ba
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 64471ba81842fb91a7f6ef765e171949294102fa
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314186"
---
# <a name="monitor-the-operations-status-of-the-remote-access-server-and-its-components"></a>監視「遠端存取」伺服器及其元件的操作狀態

>適用於：Windows Server (半年通道)、Windows Server 2016

**注意：** Windows Server 2012 將 DirectAccess 與「路由及遠端存取服務」(RRAS) 整合成一個遠端存取角色。  
  
您可以使用遠端存取服務器中的管理主控台來監視其作業狀態。  
  
> [!NOTE]  
> 您必須以 Domain Admins 群組成員或每部電腦上 Administrators 群組成員的身分登入，才能完成本主題中所述的工作。 如果您使用 Administrators 群組成員的帳戶登入時無法完成工作，請嘗試在使用屬於 Domain Admins 群組成員的帳戶登入時執行此項工作。  
  
#### <a name="to-monitor-the-remote-access-server-operations-status"></a>監視遠端存取服務器的作業狀態  
  
1.  在 [伺服器管理員] 中，按一下 [工具]，然後按一下 [遠端存取管理]。  
  
2.  按一下 [**儀表板**]，流覽至 [**遠端存取管理] 主控台**中的 [**遠端存取報告**]。  
  
3.  在 [監視] 儀表板上，請注意 [**伺服器狀態**] 磚中的 [**作業狀態**] 磚。 此圖格會列出伺服器作業狀態和所有伺服器元件的狀態。  
  
4.  在右窗格中 **，按一下 [** 工作] 下方的 [重新整理] 以**重載作業狀態**。 作業狀態會每隔五分鐘自動重新整理一次，這是預設的重新整理間隔。 若要變更預設的重新整理間隔，請按一下 [設定重新整理**間隔**]。  
  
![Windows PowerShell](../../../media/Monitor-the-operations-status-of-the-Remote-Access-server-and-its-components/PowerShellLogoSmall.gif)***<em>windows powershell 對等命令</em>***  
  
下列 Windows PowerShell 指令程式會執行與前述程序相同的功能。 請逐行各輸入一個指令程式，儘管有些指令程式可能因為受制於內文格式而自動換行拆成好幾行。  
  
> [!NOTE]  
> 包含叢集作業狀態的命令，以供參考。  
  
```  
PS> Get-RemoteAccessHealth  
PS> Get-RemoteAccessHealth -Cluster  
```  
  


