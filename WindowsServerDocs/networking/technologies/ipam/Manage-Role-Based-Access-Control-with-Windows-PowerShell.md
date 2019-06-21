---
title: 使用 Windows PowerShell 管理角色型存取控制
description: 本主題是 Windows Server 2016 中的 IP 位址管理 (IPAM) 管理指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4f13f78e-0114-4e41-9a28-82a4feccecfc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 11dd417be4720b09851fc03f111edaaf06b55e59
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282128"
---
# <a name="manage-role-based-access-control-with-windows-powershell"></a>使用 Windows PowerShell 管理角色型存取控制

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題以了解如何使用 IPAM 來管理角色型存取控制，使用 Windows PowerShell。  
  
>[!NOTE]
>如需 IPAM 的 Windows PowerShell 命令參考，請參閱[IpamServer cmdlet，在 Windows PowerShell 中的](https://docs.microsoft.com/powershell/module/ipamserver/?view=win10-ps)。  
  
新的 Windows PowerShell IPAM 命令為您提供擷取和變更的 DNS 和 DHCP 物件的存取範圍的能力。 下表說明了正確的命令，將 IPAM 中的每個物件。  
  
|IPAM 物件|命令|描述|  
|---------------|-----------|---------------|  
|DNS 伺服器|Get-IpamDnsServer|此 cmdlet 會傳回在 IPAM 中的 DNS 伺服器物件|  
|DNS 區域|Get-IpamDnsZone|此 cmdlet 會傳回在 IPAM 中的 DNS 區域物件|  
|DNS 資源記錄|Get-IpamResourceRecord|此 cmdlet 會傳回在 IPAM 中的 DNS 資源記錄物件|  
|DNS 條件轉寄站|Get-IpamDnsConditionalForwarder|此 cmdlet 會傳回在 IPAM 中的 DNS 條件轉寄站物件|  
|DHCP 伺服器|Get-IpamDhcpServer|此 cmdlet 會傳回在 IPAM 中的 DHCP 伺服器物件|  
|DHCP 超級領域|Get-IpamDhcpSuperscope|此 cmdlet 會傳回在 IPAM 中的 DHCP 超級領域物件|  
|DHCP 領域|Get-IpamDhcpScope|此 cmdlet 會傳回在 IPAM 中的 DHCP 範圍的物件|  
  
在下列範例中的命令輸出中，`Get-IpamDnsZone`指令程式可抓取**dublin.contoso.com** DNS 區域。  
  
```  
PS C:\Users\Administrator.CONTOSO> Get-IpamDnsZone -ZoneType Forward -ZoneName dublin.contoso.com  
  
ZoneName             : dublin.contoso.com  
ZoneType             : Forward  
AccessScopePath      : \Global\Dublin  
IsSigned             : False  
DynamicUpdateStatus  : None  
ScavengeStaleRecords : False  
```  
  
## <a name="setting-access-scopes-on-ipam-objects"></a>IPAM 物件上設定存取領域  
您也可以使用 IPAM 物件上設定存取領域`Set-IpamAccessScope`命令。 物件的特定值來設定存取領域，或造成要繼承自父物件的存取領域的物件，您可以使用此命令。 以下是您可以使用此命令來設定的物件。  
  
-   DHCP 領域  
  
-   DHCP 伺服器  
  
-   DHCP 超級領域  
  
-   DNS 條件轉寄站  
  
-   DNS 資源記錄  
  
-   DNS 伺服器  
  
-   DNS 區域  
  
-   IP 位址區塊  
  
-   IP 位址範圍  
  
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
  
在下列範例中，DNS 區域的存取範圍**dublin.contoso.com**已從**都柏林**來**歐洲**。  
  
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
  


