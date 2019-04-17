---
title: 使用 Windows PowerShell 來設定非網域成員 Client 的電腦
description: 本主題是 BranchCache 部署節目表適用於 Windows Server 2016 的示範如何將 BranchCache 部署最佳化分公司 WAN 頻寬分散與裝載快取模式中的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 1b511e1a-686d-441f-a1c7-d4d029e1a061
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a5415d9fa5a4af806f23a1af9c907b1f02e9627b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="use-windows-powershell-to-configure-non-domain-member-client-computers"></a>使用 Windows PowerShell 來設定非網域成員 Client 的電腦

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以手動設定 BranchCache client 電腦上的快取分散式的模式使用此程序，或裝載快取模式。  
  
> [!NOTE]  
> 如果您已經設定 BranchCache client 電腦使用群組原則、 群組原則設定覆寫任何手動 client 電腦套用原則設定。  
  
資格在**系統管理員**，或相當於的最低需求才能執行此程序。  
  
### <a name="to-enable-branchcache-distributed-or-hosted-cache-mode"></a>若要讓 BranchCache 分散式或裝載快取模式  
  
1.  BranchCache client 在電腦上您想要設定，以系統管理員的身分執行 Windows PowerShell，然後執行下列其中一項。  
  
    -   若要設定 client 電腦 BranchCache 分散式快取模式，輸入下列命令，，然後按 ENTER 鍵。  
  
        `Enable-BCDistributed`  
  
    -   若要設定 client 電腦 BranchCache 裝載快取模式，輸入下列命令，，然後按 ENTER 鍵。  
  
        `Enable-BCHostedClient`  
  
        > [!TIP]  
        > 如果您想要指定的提供裝載快取的伺服器，使用`-ServerNames`參數以逗號分隔讓參數值為裝載快取伺服器清單。 例如，如果您有兩個裝載快取伺服器名 HCS1 和 HCS2，設定 client 電腦裝載快取模式使用下列命令。  
        >   
        > `Enable-BCHostedClient -ServerNames HCS1,HCS2`  
  


