---
title: 使用 Windows PowerShell 管理角色型存取控制
description: 本主題是 Windows Server 2016 中的 IP 位址管理 (IPAM) 管理指南的一部分。
manager: brianlic
ms.topic: article
ms.assetid: 4f13f78e-0114-4e41-9a28-82a4feccecfc
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 99859a1e9f43baa969475628e743975c78ef8bff
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96866467"
---
# <a name="manage-role-based-access-control-with-windows-powershell"></a>使用 Windows PowerShell 管理角色型存取控制

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來瞭解如何使用 IPAM 來管理 Windows PowerShell 的角色型存取控制。

>[!NOTE]
>如需 IPAM Windows PowerShell 命令參考，請參閱 [Windows PowerShell 中的 IpamServer Cmdlet](/powershell/module/ipamserver/)。

新的 Windows PowerShell IPAM 命令可讓您取得及變更 DNS 和 DHCP 物件的存取範圍。 下表說明每個 IPAM 物件所要使用的正確命令。

|IPAM 物件|命令|描述|
|---------------|-----------|---------------|
|DNS 伺服器|Get-IpamDnsServer|此 Cmdlet 會傳回 IPAM 中的 DNS 伺服器物件|
|DNS 區域|Get-IpamDnsZone|此 Cmdlet 會傳回 IPAM 中的 DNS 區域物件|
|DNS 資源記錄|Get-IpamResourceRecord|此 Cmdlet 會傳回 IPAM 中的 DNS 資源記錄物件|
|DNS 條件轉寄站|Get-IpamDnsConditionalForwarder|此 Cmdlet 會傳回 IPAM 中的 DNS 條件轉寄站物件|
|DHCP 伺服器|Get-IpamDhcpServer|此 Cmdlet 會傳回 IPAM 中的 DHCP 伺服器物件|
|DHCP 超級領域|Get-IpamDhcpSuperscope|此 Cmdlet 會傳回 IPAM 中的 DHCP 超級領域物件|
|DHCP 領域|Get-IpamDhcpScope|此 Cmdlet 會傳回 IPAM 中的 DHCP 領域物件|

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
您可以使用命令來設定 IPAM 物件的存取範圍 `Set-IpamAccessScope` 。 您可以使用此命令將存取範圍設定為物件的特定值，或讓物件繼承父物件的存取範圍。 以下是您可以使用此命令來設定的物件。

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

在下列範例中，DNS 區域 **dublin.contoso.com** 的存取範圍已從 **都柏林** 變更為 **歐洲**。

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
