---
title: 管理角色為基礎存取控制使用 Windows PowerShell
description: 本主題是在 Windows Server 2016 的 IP 位址管理 (IPAM) 管理組節目表的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4f13f78e-0114-4e41-9a28-82a4feccecfc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: df6fa423a4ec891f1ad3faefad6c6054519542c4
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="manage-role-based-access-control-with-windows-powershell"></a>管理角色為基礎存取控制使用 Windows PowerShell

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題以了解如何使用 IPAM 管理 Windows PowerShell 中的角色為基礎存取控制。  
  
>[!NOTE]
>針對 IPAM Windows PowerShell 命令參考資料，請查看[Windows PowerShell 中的 IP 位址管理 (IPAM) 伺服器 Cmdlet](https://technet.microsoft.com/library/jj553807.aspx)。  
  
新的 Windows PowerShell IPAM 命令為您提供擷取和變更的 DNS 與 DHCP 物件存取範圍的功能。 下表顯示使用每個 IPAM 物件正確的命令。  
  
|IPAM 物件|命令|描述|  
|---------------|-----------|---------------|  
|DNS 伺服器|取得-IpamDnsServer|這個 cmdlet 傳回 IPAM DNS 伺服器物件|  
|DNS 區域|取得-IpamDnsZone|這個 cmdlet 傳回 IPAM DNS 區物件|  
|DNS 資源記錄|取得-IpamResourceRecord|這個 cmdlet 傳回 IPAM DNS 資源記錄物件|  
|DNS 條件轉寄|取得-IpamDnsConditionalForwarder|這個 cmdlet 傳回 IPAM DNS 條件轉寄物件|  
|DHCP 伺服器|取得-IpamDhcpServer|這個 cmdlet 傳回 IPAM DHCP 伺服器物件|  
|DHCP 超級|取得-IpamDhcpSuperscope|這個 cmdlet 傳回 IPAM DHCP 超級物件|  
|DHCP 領域|取得-IpamDhcpScope|這個 cmdlet 傳回 IPAM DHCP 範圍物件|  
  
下列範例命令的輸出， `Get-IpamDnsZone` cmdlet 擷取**dublin.contoso.com** DNS 區域。  
  
```  
PS C:\Users\Administrator.CONTOSO> Get-IpamDnsZone -ZoneType Forward -ZoneName dublin.contoso.com  
  
ZoneName             : dublin.contoso.com  
ZoneType             : Forward  
AccessScopePath      : \Global\Dublin  
IsSigned             : False  
DynamicUpdateStatus  : None  
ScavengeStaleRecords : False  
```  
  
## <a name="setting-access-scopes-on-ipam-objects"></a>設定存取範圍 IPAM 物件  
您可以設定存取範圍 IPAM 物件使用`Set-IpamAccessScope`命令。 您可以使用此命令存取範圍設特定物件的值，或讓繼承家長物件範圍存取物件。 以下是您可以設定此命令的物件。  
  
-   DHCP 領域  
  
-   DHCP 伺服器  
  
-   DHCP 超級  
  
-   DNS 條件轉寄  
  
-   DNS 資源記錄  
  
-   DNS 伺服器  
  
-   DNS 區域  
  
-   IP 位址封鎖  
  
-   IP 位址  
  
-   IP 位址空間  
  
-   IP 位址子網路  
  
下列是語法`Set-IpamAccessScope`命令。  
  
```  
NAME  
    Set-IpamAccessScope  
  
SYNTAX  
    Set-IpamAccessScope [-IpamRange] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDnsServer] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDhcpServer] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDhcpSuperscope] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDhcpScope] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDnsConditionalForwarder] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDnsResourceRecord] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDnsZone] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamAddressSpace] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamSubnet] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamBlock] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  [<CommonParameters>]  
```  
  
下例 DNS 區域存取範圍在**dublin.contoso.com**變更的**都柏林**以**歐洲**。  
  
```  
PS C:\Users\Administrator.CONTOSO> Get-IpamDnsZone -ZoneType Forward -ZoneName dublin.contoso.com  
  
ZoneName             : dublin.contoso.com  
ZoneType             : Forward  
AccessScopePath      : \Global\Dublin  
IsSigned             : False  
DynamicUpdateStatus  : None  
ScavengeStaleRecords : False  
  
PS C:\Users\Administrator.CONTOSO> $a = Get-IpamDnsZone -ZoneType Forward -ZoneName dublin.contoso.com  
PS C:\Users\Administrator.CONTOSO> Set-IpamAccessScope -IpamDnsZone -InputObject $a -AccessScopePath \Global\Europe -PassThru  
  
ZoneName             : dublin.contoso.com  
ZoneType             : Forward  
AccessScopePath      : \Global\Europe  
IsSigned             : False  
DynamicUpdateStatus  : None  
ScavengeStaleRecords : False  
```  
  


