---
title: 確認用戶端電腦設定
description: 本主題是 BranchCache 部署指南的 Windows Server 2016 中，示範如何以最佳化 WAN 頻寬使用量，在分公司的分散式和裝載式快取模式部署 BranchCache 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 31ea58b0-d407-4f62-8ec6-6a1b19174042
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d628886186474d3f05d7961ca3d3b45b8bf12e73
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834049"
---
# <a name="verify-client-computer-settings"></a>確認用戶端電腦設定

>適用於：Windows Server （半年通道），Windows Server 2016

若要確認用戶端電腦已正確設定 branchcache，您可以使用此程序。  
  
> [!NOTE]  
> 此程序包括手動更新 群組原則和重新啟動 BranchCache 服務的步驟。 您不需要執行這些動作，如果您重新啟動電腦，因為它們會在此情況下自動發生。  
  
您必須是隸屬**系統管理員**，或同等權限才能執行此程序。  
  
### <a name="to-verify-branchcache-client-computer-settings"></a>若要確認 BranchCache 用戶端電腦設定  
  
1.  若要設定您想要確認其 BranchCache 用戶端電腦上，重新整理群組原則，系統管理員身分執行 Windows PowerShell，輸入下列命令，，然後按 ENTER。  
  
    `gpupdate /force`  
  
2.  用戶端電腦設定為託管快取模式，並設定自動探索託管快取伺服器的服務連接點，執行下列命令來停止並重新啟動的 BranchCache 服務。  
  
    `net stop peerdistsvc`  
  
    `net start peerdistsvc`  
  
3.  執行下列命令來檢查目前的 BranchCache 作業模式。  
  
    `Get-BCStatus`  
  
4.  在 Windows PowerShell 中，檢閱的輸出**Get BCStatus**命令。  
  
    值**BranchCacheIsEnabled**應該 **，則為 True**。  
  
    在  **ClientSettings**，值**CurrentClientMode**應該**DistributedClient**或**HostedCacheClient**，取決於您已設定使用本指南的模式。  
  
    在  **ClientSettings**，如果您設定託管快取模式並提供您的託管快取伺服器的名稱，在設定期間，或如果用戶端會自動位於裝載快取伺服器使用服務連接點，請**HostedCacheServerList**應該有相同的名稱或託管快取伺服器的名稱的值。 例如，如果 HCS1 和您的網域名稱為託管快取伺服器是 corp.contoso.com，值**HostedCacheServerList**是**HCS1.corp.contoso.com**。  
  
5.  任何設定，則上面所列的 BranchCache 並沒有正確的值，使用本指南中的步驟，確認群組原則或本機電腦原則設定，以及防火牆例外，在您的設定，並確定它們正確。 此外，請重新啟動電腦，或遵循此程序來重新整理群組原則，然後重新啟動 BranchCache 服務中的步驟。  
  


