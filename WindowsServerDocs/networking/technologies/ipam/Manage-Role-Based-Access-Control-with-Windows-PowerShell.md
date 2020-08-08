---
title: 使用 Windows PowerShell 管理角色型存取控制
description: 本主題是 Windows Server 2016 中的 IP 位址管理 (IPAM) 管理指南的一部分。
manager: brianlic
ms.topic: article
ms.assetid: 4f13f78e-0114-4e41-9a28-82a4feccecfc
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 61daa0e76fdd5f8db5d81a9709d3bc8ee31c78e2
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87997582"
---
# <a name="manage-role-based-access-control-with-windows-powershell"></a>使用 Windows PowerShell 管理角色型存取控制

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來瞭解如何使用 IPAM 來管理以 Windows PowerShell 為基礎的角色型存取控制。

>[!NOTE]
>如需 IPAM Windows PowerShell 命令參考，請參閱[Windows powershell 中的 IpamServer Cmdlet](/powershell/module/ipamserver/?view=win10-ps)。

新的 Windows PowerShell IPAM 命令可讓您取得和變更 DNS 和 DHCP 物件的存取範圍。 下表說明要用於每個 IPAM 物件的正確命令。

|IPAM 物件|Command|描述|
|---------------|-----------|---------------|
|DNS 伺服器|IpamDnsServer|此 Cmdlet 會傳回 IPAM 中的 DNS 伺服器物件|
|DNS 區域|IpamDnsZone|此 Cmdlet 會傳回 IPAM 中的 DNS 區域物件|
|DNS 資源記錄|IpamResourceRecord|此 Cmdlet 會傳回 IPAM 中的 DNS 資源記錄物件|
|DNS 條件轉寄站|IpamDnsConditionalForwarder|此 Cmdlet 會傳回 IPAM 中的 DNS 條件轉寄站物件|
|DHCP 伺服器|IpamDhcpServer|此 Cmdlet 會傳回 IPAM 中的 DHCP 伺服器物件|
|DHCP 超級領域|IpamDhcpSuperscope|此 Cmdlet 會傳回 IPAM 中的 DHCP 超級領域物件|
|DHCP 領域|IpamDhcpScope|此 Cmdlet 會傳回 IPAM 中的 DHCP 領域物件|

在下列命令輸出範例中，Cmdlet 會抓取 `Get-IpamDnsZone` **dublin.contoso.com** DNS 區域。

```
PS C:\Users\Administrator.CONTOSO> Get-IpamDnsZone -ZoneType Forward -ZoneName dublin.contoso.com

ZoneName             : dublin.contoso.com
ZoneType             : Forward
AccessScopePath      : \Global\Dublin
IsSigned             : False
DynamicUpdateStatus  : None
ScavengeStaleRecords : False
```

## <a name="setting-access-scopes-on-ipam-objects"></a>設定 IPAM 物件的存取範圍
您可以使用命令，在 IPAM 物件上設定存取範圍 `Set-IpamAccessScope` 。 您可以使用這個命令，將存取領域設定為物件的特定值，或讓物件繼承父物件的存取範圍。 以下是您可以使用此命令來設定的物件。

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

-   IP 位址子網

以下是命令的語法 `Set-IpamAccessScope` 。

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

在下列範例中，DNS 區域**dublin.contoso.com**的存取領域已從**都柏林**變更為**歐洲**。

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