---
title: 使用遠端存取監視和計量
description: 本主題是 Windows Server 2016 中遠端存取監視和帳戶處理指南的一部分。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 92519b49-0df4-43c1-9717-f13570644212
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 03d367f2f981ca3ed649f1ca4d5eca23967c9cfc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860511"
---
# <a name="use-remote-access-monitoring-and-accounting"></a>使用遠端存取監視和計量

>適用於：Windows Server (半年通道)、Windows Server 2016

「遠端存取」監視會回報 DirectAccess 和 VPN 連線的遠端使用者活動與狀態。 它會追蹤用戶端連線的數目和持續時間 (還有其他統計資料)，並監視伺服器的操作狀態。 便於使用的監視主控台可提供完整「遠端存取」基礎結構的檢視。 單一伺服器、叢集及多站台設定都可使用監視檢視。  
  
**注意：** Windows Server 2012 將 DirectAccess 與「路由及遠端存取服務」(RRAS) 整合成一個遠端存取角色。  
  
> [!NOTE]  
> 除了本主題，還有下列關於監視遠端存取的主題可使用。  
>   
> -   [監視「遠端存取」伺服器上現有的負載](Monitor-the-existing-load-on-the-Remote-Access-server.md)  
> -   [監視「遠端存取」伺服器的設定發佈狀態](Monitor-the-configuration-distribution-status-of-the-Remote-Access-server.md)  
> -   [監視遠端存取服務器及其元件的操作狀態](Monitor-the-operations-status-of-the-Remote-Access-server-and-its-components.md)  
> -   [識別並解決「遠端存取」伺服器操作問題](Identify-and-resolve-Remote-Access-server-operations-problems.md)  
> -   [監視連線的遠端用戶端以查看其活動和狀態](Monitor-connected-remote-clients-for-activity-and-status.md)  
> -   [使用歷程記錄資料來產生遠端用戶端的使用狀況報告](Generate-a-usage-report-for-remote-clients-using-historical-data.md)  

## <a name="in-this-guide"></a>在本指南中  
本文件包含透過 DirectAccess 管理主控台和對應的 Windows PowerShell Cmdlet (隨「遠端存取伺服器」角色提供) 來利用「遠端存取」之監視功能的指示。  
  
將說明下列監視和計量案例：  
  
1.  監視「遠端存取」伺服器上現有的負載  
  
2.  監視「遠端存取」伺服器的設定發佈狀態  
  
3.  監視「遠端存取」伺服器及其元件的操作狀態  
  
4.  識別並解決「遠端存取」伺服器操作問題  
  
5.  監視連線的遠端用戶端以查看其活動和狀態  
  
6.  使用歷程記錄資料來產生遠端用戶端的使用狀況報告  
  
### <a name="understand-monitoring-and-accounting"></a>了解監視和計量  
開始進行遠端用戶端的監視和計量工作之前，您需要了解兩者之間的差異。  
  
-   **監視**會顯示在指定的時間點主動連線的使用者。  
  
-   **計量**會保存已連線到公司網路之使用者的歷程記錄及其使用狀況詳細資料 (用於規範和稽核)。  
  
遠端用戶端監視是根據連線。 DirectAccess 用戶端所建立的通道連線有兩種：  
  
-   **電腦通道流量**連線：此通道是由電腦在系統內容中建立，用來存取名稱解析、驗證、補救更新等所需的伺服器。  
  
-   **使用者通道流量**連線：當使用者嘗試存取公司網路上的資源時，此通道是由電腦上的使用者帳戶（在使用者內容中）所建立。 根據部署需求，使用者可能必須提供強式認證 (例如使用智慧卡或提供單次密碼) 來存取公司網路資源。  
  
以 DirectAccess 來說，會以遠端用戶端的 IP 位址來唯一識別連線。 例如，如果電腦通道已針對用戶端電腦開啟，而且使用者是從該電腦連線，則會使用相同的連接。 在電腦通道仍然處於作用中的情況下，如果使用者中斷連線後又再次連線，則會視為單一連線。  
  


