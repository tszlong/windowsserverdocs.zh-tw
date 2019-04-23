---
title: 使用 Windows PowerShell 來設定非網域成員用戶端電腦
description: 本主題是 BranchCache 部署指南的 Windows Server 2016 中，示範如何以最佳化 WAN 頻寬使用量，在分公司的分散式和裝載式快取模式部署 BranchCache 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 1b511e1a-686d-441f-a1c7-d4d029e1a061
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9abb77e573d7b3f144ab831c655c81370a4a6af1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839949"
---
# <a name="use-windows-powershell-to-configure-non-domain-member-client-computers"></a>使用 Windows PowerShell 來設定非網域成員用戶端電腦

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用此程序，手動設定 BranchCache 分散式快取模式用戶端電腦或託管快取模式。  
  
> [!NOTE]  
> 如果您已設定 BranchCache 用戶端電腦使用群組原則，群組原則設定覆寫任何手動設定来套用原則的用戶端電腦。  
  
中的成員資格**系統管理員**，或同等權限才能執行此程序的最小值。  
  
### <a name="to-enable-branchcache-distributed-or-hosted-cache-mode"></a>若要啟用 BranchCache 分散式或託管快取模式  
  
1.  在您想要設定 BranchCache 用戶端電腦，以系統管理員身分執行 Windows PowerShell，然後執行下列其中一項。  
  
    -   若要設定 BranchCache 分散式快取模式用戶端電腦，請輸入下列命令，並再按 ENTER 鍵。  
  
        `Enable-BCDistributed`  
  
    -   若要設定為 BranchCache 託管快取模式用戶端電腦，請輸入下列命令，，然後按 ENTER 鍵。  
  
        `Enable-BCHostedClient`  
  
        > [!TIP]  
        > 如果您想要指定可用的託管快取伺服器，使用`-ServerNames`參數以逗號分隔清單的託管快取伺服器做為參數值。 比方說，如果您有兩個名為 HCS1 和 HCS2 的託管快取伺服器，請使用下列命令設定託管快取模式的用戶端電腦。  
        >   
        > `Enable-BCHostedClient -ServerNames HCS1,HCS2`  
  


