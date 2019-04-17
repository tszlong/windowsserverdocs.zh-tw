---
title: 請確認 Client 電腦設定
description: 本主題是 BranchCache 部署節目表適用於 Windows Server 2016 的示範如何將 BranchCache 部署最佳化分公司 WAN 頻寬分散與裝載快取模式中的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 31ea58b0-d407-4f62-8ec6-6a1b19174042
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d507f2097a9349cbefba520ad0dc143e0dd7452e
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="verify-client-computer-settings"></a>請確認 Client 電腦設定

>適用於：Windows Server（以每年次管道）、Windows Server 2016

若要確認 client 電腦 BranchCache 針對正確的設定，您可以使用此程序。  
  
> [!NOTE]  
> 此程序包含步驟手動更新群組原則和重新 BranchCache 服務。 您不需要執行這些動作，如果您重新開機，將會自動發生這種情況下。  
  
您必須成員的**系統管理員**，或相當於執行此程序。  
  
### <a name="to-verify-branchcache-client-computer-settings"></a>若要確認 BranchCache client 電腦設定  
  
1.  若要重新整理 client 您想要確認其 BranchCache 設定電腦群組原則，系統管理員身分執行 Windows PowerShell，輸入下列命令，，然後按 ENTER。  
  
    `gpupdate /force`  
  
2.  Client 電腦裝載快取模式中的設定，並設定為自動探索裝載快取伺服器服務連接點，執行下列命令停止並開機 BranchCache 服務。  
  
    `net stop peerdistsvc`  
  
    `net start peerdistsvc`  
  
3.  執行下列命令查看目前 BranchCache 操作模式。  
  
    `Get-BCStatus`  
  
4.  Windows PowerShell 中, 檢視的輸出**取得-BCStatus**命令。  
  
    值為**BranchCacheIsEnabled**應該**，則為 True**。  
  
    在**ClientSettings**的值為**CurrentClientMode**應該**DistributedClient**或**HostedCacheClient**，根據您在設定使用本指南模式。  
  
    在**ClientSettings**，如果您設定裝載快取模式，並在設定期間，提供您裝載快取伺服器的名稱或是否有自動位於 client 裝載快取伺服器使用服務連接點， **HostedCacheServerList**應該會有相同的名稱或裝載快取伺服器的名稱為值。 例如，您裝載快取的伺服器稱為 HCS1 與您的網域是否 corp.contoso.com 的值為**HostedCacheServerList**是**HCS1.corp.contoso.com**。  
  
5.  如果任何設定上述 BranchCache 不具有正確的值，本指南使用的步驟驗證的群組原則 」 或 「 本機電腦原則設定，以及防火牆例外，在您的設定，並確認它們正確。 此外，重新開機，或是遵循的步驟來重新整理群組原則和重新開機 BranchCache 服務此程序。  
  


