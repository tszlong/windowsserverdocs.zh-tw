---
title: 確認用戶端電腦設定
description: 本主題是 Windows Server 2016 的 BranchCache 部署指南的一部分，示範如何在分散式和託管快取模式中部署 BranchCache，以優化分公司的 WAN 頻寬使用量
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 31ea58b0-d407-4f62-8ec6-6a1b19174042
ms.author: lizross
author: eross-msft
ms.openlocfilehash: e4bd3c4d4b2998f5c4faea22887bdef8663587cb
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319171"
---
# <a name="verify-client-computer-settings"></a>確認用戶端電腦設定

>適用於：Windows Server (半年通道)、Windows Server 2016

您可以使用此程式來驗證是否已正確設定 BranchCache 的用戶端電腦。  
  
> [!NOTE]  
> 此套裝程式含手動更新群組原則和重新開機 BranchCache 服務的步驟。 如果您重新開機電腦，就不需要執行這些動作，因為在這種情況下，它們會自動發生。  
  
您必須是系統**管理員**的成員或同等許可權，才能執行此程式。  
  
### <a name="to-verify-branchcache-client-computer-settings"></a>驗證 BranchCache 用戶端電腦設定  
  
1.  若要在您想要驗證其 BranchCache 設定的用戶端電腦上重新整理群組原則，請以系統管理員身分執行 Windows PowerShell，輸入下列命令，然後按 ENTER。  
  
    `gpupdate /force`  
  
2.  若用戶端電腦設定為託管快取模式，並已設定為透過服務連接點自動探索託管快取伺服器，請執行下列命令來停止並重新啟動 BranchCache 服務。  
  
    `net stop peerdistsvc`  
  
    `net start peerdistsvc`  
  
3.  執行下列命令來檢查目前的 BranchCache 操作模式。  
  
    `Get-BCStatus`  
  
4.  在 Windows PowerShell 中，檢查**BCStatus**命令的輸出。  
  
    **BranchCacheIsEnabled**的值應為**True**。  
  
    在**ClientSettings**中， **CurrentClientMode**的值應該是**DistributedClient**或**HostedCacheClient**，視您使用本指南所設定的模式而定。  
  
    在**ClientSettings**中，如果您設定了託管快取模式，並在設定期間提供託管快取伺服器的名稱，或是用戶端已使用服務連接點自動找出託管快取伺服器，則**HostedCacheServerList**應該具有與託管快取伺服器名稱相同的值。 例如，如果您的託管快取伺服器名為 HCS1，而您的網域是 corp.contoso.com，則**HostedCacheServerList**的值會是**HCS1.corp.contoso.com**。  
  
5.  如果以上列出的任何 BranchCache 設定沒有正確的值，請使用本指南中的步驟來確認群組原則或本機電腦原則設定，以及您所設定的防火牆例外狀況，並確認它們是否正確。 此外，請重新開機電腦，或遵循此程式中的步驟重新整理群組原則並重新啟動 BranchCache 服務。  
  


