---
title: 監視連線的遠端用戶端以查看其活動和狀態
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
ms.assetid: beb94475-b21f-46a9-ac51-bf2bb28ca94e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 468854d6e03bbc2abee3b9fce7f7960c7c25cfd3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879709"
---
# <a name="monitor-connected-remote-clients-for-activity-and-status"></a>監視連線的遠端用戶端以查看其活動和狀態

>適用於：Windows Server （半年通道），Windows Server 2016

**注意：** Windows Server 2012 將 DirectAccess 和遠端存取服務 (RAS) 結合成單一的遠端存取角色中。  
  
您可以使用遠端存取伺服器上的 [管理] 主控台來監視遠端用戶端活動和狀態。  
  
> [!NOTE]  
> 您必須登入以 Domain Admins 群組的成員或每一部電腦上的 Administrators 群組的成員才能完成本主題中所述的工作。 如果您在登入是系統管理員群組成員的帳戶，您無法完成工作，請嘗試執行工作，您在登入的帳戶是 Domain Admins 群組的成員。  
  
#### <a name="to-monitor-remote-client-activity-and-status"></a>若要監視遠端用戶端活動和狀態  
  
1.  在 [伺服器管理員] 中，按一下 [工具]，然後按一下 [遠端存取管理]。  
  
2.  按一下  **REPORTING**瀏覽至**遠端存取報告**中**遠端存取管理主控台**。  
  
3.  按一下 **遠端用戶端狀態**瀏覽至中的遠端用戶端活動和狀態使用者介面**遠端存取管理主控台**。  
  
4.  您會看到的使用者會連線到遠端存取伺服器，以及詳細的清單及其相關的統計資料。 按一下清單中，對應至用戶端的第一個資料列。 當您選取一個資料列時，遠端使用者活動會顯示在 [預覽] 窗格中。  
  
![Windows PowerShell](../../../media/Monitor-connected-remote-clients-for-activity-and-status/PowerShellLogoSmall.gif)Windows PowerShell 對等命令 * * *  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
```  
PS> Get-RemoteAccessConnectionStatistics  
```  
  
可篩選使用者統計資料，根據條件選取項目，使用下表中的欄位。  
  
|欄位名稱|值|  
|-------|-----|  
|Username|遠端使用者的使用者名稱或別名。 萬用字元可以用來選取一群使用者，例如 contoso\\* 或\*\administrator。|  
|主機名稱|遠端使用者的電腦帳戶名稱。 也可以指定 IPv4 或 IPv6 位址。|  
|類型|DirectAccess 或 VPN。 如果選取 DirectAccess，則會列出所有遠端使用者可以透過 DirectAccess 連線。 如果選取 VPN，則會列出所有遠端使用者使用 VPN 連線。|  
|ISP 位址|遠端使用者的 IPv4 或 IPv6 位址。|  
|IPv4 位址|遠端使用者連線到公司網路的通道內部 IPv4 位址。|  
|IPv6 位址|將遠端使用者連接到公司網路之通道的內部 IPv6 位址|  
|通訊協定/通道|遠端用戶端會使用轉換技術。 這是 Teredo、 6to4 或 IP-HTTPS 對於 DirectAccess 使用者，而且它是供 VPN 使用者的 PPTP L2TP、 PPTP、L2TP、SSTP 或 IKEv2。|  
|已存取資源|存取特定公司資源或端點的所有使用者。 對應至這個欄位的值是伺服器的主機名稱/IP 位址。|  
|Server|用戶端連線的遠端存取伺服器。 這僅與叢集和多站台部署相關。|  
  
  
  


