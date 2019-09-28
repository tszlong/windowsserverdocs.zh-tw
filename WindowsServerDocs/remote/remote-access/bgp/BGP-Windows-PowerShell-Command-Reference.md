---
title: BGP Windows PowerShell 命令參考
description: 在 Windows Server 2016 中撰寫 Windows PowerShell 腳本時，您可以使用本主題做為參考，以新增、設定和移除 RAS 閘道和遠端存取區域網路（LAN）路由器的 BGP 功能。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4b0240a3-b927-4a1e-b241-5f8f29a9552f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2da5bc3008b9b686ddb2bce460ced39f98183f28
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394623"
---
# <a name="bgp-windows-powershell-command-reference"></a>BGP Windows PowerShell 命令參考

>適用於：Windows Server (半年度管道)、Windows Server 2016

在撰寫 Windows PowerShell 腳本時，您可以使用本主題做為參考，以新增、設定和移除 RAS 閘道和遠端存取區域網路（LAN）路由器的 BGP 功能。  
  
這些 BGP 命令是 Windows Server 2016 的遠端存取 Windows PowerShell 命令集的一部分。 本主題可協助您快速找出您想要在腳本中使用的 BGP 命令。  
  
如需所有遠端存取命令的詳細資訊，請參閱[遠端存取 Cmdlet](https://technet.microsoft.com/library/hh918399.aspx)。  
  
## <a name="bgp-command-reference"></a>BGP 命令參考  
下列各節提供每個 BGP 命令的命令名稱、用途和語法，以及遠端存取參考中命令的連結，其中包含每個命令的詳細資訊。  
  
此參考包含下列各節。  
  
-   [新增命令](#bkmk_add)  
  
-   [清除命令](#bkmk_clear)  
  
-   [停用和啟用命令](#bkmk_disable)  
  
-   [取得命令](#bkmk_get)  
  
-   [安裝命令](#bkmk_install)  
  
-   [移除命令](#bkmk_remove)  
  
-   [設定命令](#bkmk_set)  
  
-   [啟動和停止命令](#bkmk_start)  
  
-   [卸載命令](#bkmk_uninstall)  
  
### <a name="bkmk_add"></a>新增命令  
以下是 BGP Add 命令。  
  
[新增-Add-bgpcustomroute](https://technet.microsoft.com/library/dn262684.aspx)  
  
將自訂路由新增至 BGP 路由表。  
  
```  
Add-BgpCustomRoute [-CimSession <CimSession[]> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-Interface <String[]> ] [-Network <String[]> ] [-PassThru] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[新增-Bgp](https://technet.microsoft.com/library/dn262687.aspx)  
  
加入新的 BGP 對等。  
  
```  
Add-BgpPeer [-Name] <String> -LocalIPAddress <IPAddress> -PeerASN <UInt32> -PeerIPAddress <IPAddress> [-CimSession <CimSession[]> ] [-HoldTimeSec <UInt16> ] [-IdleHoldTimeSec <UInt16> ] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-LocalASN <UInt32> ] [-MaxAllowedPrefix <UInt32> ] [-OperationMode <OperationMode> {Mixed | Server} ] [-PassThru] [-PeeringMode <PeeringMode> {Automatic | Manual} ] [-RouteReflectorClient <Boolean> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Weight <UInt16> ] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[新增-BgpRouteAggregate](https://technet.microsoft.com/library/mt463113.aspx)  
  
新增特定 BGP 路由的新匯總路由。  
  
```  
Add-BgpRouteAggregate -Prefix <String> [-AttributePolicy <String[]> ] [-CimSession <CimSession[]> ] [-Force] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-PassThru] [-PreserveASPath <PreserveASPath> ] [-RoutingDomain <String> ] [-SummaryOnly <SummaryOnly> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[新增-BgpRouter](https://technet.microsoft.com/library/dn262665.aspx)  
  
為指定的租使用者識別碼新增 BGP 路由器。  
  
```  
Add-BgpRouter -BgpIdentifier <IPAddress> -LocalASN <UInt32> [-CimSession <CimSession[]> ] [-ClientToClientReflection <ClientToClientReflection> ] [-ClusterId <UInt32> ] [-CompareMEDAcrossASN <Boolean> ] [-DefaultGatewayRouting <Boolean> ] [-Force] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-IPv6Routing <IPv6RoutingState> {Disabled | Enabled} ] [-LocalIPv6Address <IPAddress> ] [-PassThru] [-RouteReflector <RouteReflector> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-TransitRouting <TransitRouting> ] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[新增-BgpRoutingPolicy](https://technet.microsoft.com/library/dn262662.aspx)  
  
將 BGP 路由原則新增至原則存放區。  
  
```  
Add-BgpRoutingPolicy [-Name] <String> [-PolicyType] <PolicyType> {Deny | Allow | ModifyAttribute} [-AddCommunity <String[]> ] [-CimSession <CimSession[]> ] [-ClearMED] [-Force] [-IgnorePrefix <String[]> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-MatchASNRange <UInt32[]> ] [-MatchCommunity <String[]> ] [-MatchNextHop <IPAddress[]> ] [-MatchPrefix <String[]> ] [-NewLocalPref <UInt32]> ] [-NewMED <UInt32]> ] [-NewNextHop <IPAddress> ] [-PassThru] [-RemoveAllCommunities] [-RemoveCommunity <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[新增-BgpRoutingPolicyForPeer](https://technet.microsoft.com/library/dn262680.aspx)  
  
將 BGP 路由原則新增至 BGP 對等體。  
  
```  
Add-BgpRoutingPolicyForPeer -Direction <PolicyDirection> {Ingress | Egress} -PolicyName <String[]> [-CimSession <CimSession[]> ] [-Force] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-PeerName <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
### <a name="bkmk_clear"></a>清除命令  
以下是 BGP 的清除命令  
  
[清除-BgpRouteFlapDampening](https://technet.microsoft.com/library/mt463114.aspx)  
  
針對指定的一組 BGP 路由清除路由擺動抑制資訊。  
  
```  
Clear-BgpRouteFlapDampening [-CimSession <CimSession[]> ] [-Force] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-Prefix <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
### <a name="bkmk_disable"></a>停用和啟用命令  
以下是 BGP 的停用和啟用命令  
  
[停用-BgpRouteFlapDampening](https://technet.microsoft.com/library/mt463100.aspx)  
  
停用 flapping BGP 路由的路由抑制。  
  
```  
Disable-BgpRouteFlapDampening [-CimSession <CimSession[]> ] [-Force] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[啟用-BgpRouteFlapDampening](https://technet.microsoft.com/library/mt463102.aspx)  
  
啟用 flapping BGP 路由的路由抑制。  
  
```  
Enable-BgpRouteFlapDampening [-CimSession <CimSession[]> ] [-Force] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-PassThru] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
### <a name="bkmk_get"></a>取得命令  
以下是 BGP 的 Get 命令。  
  
[Add-bgpcustomroute](https://technet.microsoft.com/library/dn262664.aspx)  
  
從 BGP 路由器取得自訂路由資訊。  
  
```  
Get-BgpCustomRoute [-CimSession <CimSession[]> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[Bgp](https://technet.microsoft.com/library/dn262659.aspx)  
  
取得 BGP 對等的設定資訊。  
  
```  
Get-BgpPeer [[-Name] <String[]> ] [-CimSession <CimSession[]> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[BgpRouteAggregate](https://technet.microsoft.com/library/mt463103.aspx)  
  
取得系統管理員所設定的所有匯總 BGP 路由。  
  
```  
Get-BgpRouteAggregate [-CimSession <CimSession[]> ] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-Prefix <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[BgpRouteFlapDampening](https://technet.microsoft.com/library/mt463108.aspx)  
  
抓取 BGP 路由抑制引擎的設定。  
  
```  
Get-BgpRouteFlapDampening [-CimSession <CimSession[]> ] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[BgpRouteInformation](https://technet.microsoft.com/library/dn262667.aspx)  
  
從 BGP 路由表中抓取一或多個網路首碼的 BGP 路由資訊。  
  
```  
Get-BgpRouteInformation [-CimSession <CimSession[]> ] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-Network <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Type <RouteType> ] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[BgpRouter](https://technet.microsoft.com/library/dn262660.aspx)  
  
取得 BGP 路由器的設定資訊。  
  
```  
Get-BgpRouter [-CimSession <CimSession[]> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-RoutingDomain <String[]> ] [-ThrottleLimit <Int32> ] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[BgpRoutingPolicy](https://technet.microsoft.com/library/dn262672.aspx)  
  
取得 BGP 路由原則的設定資訊。  
  
```  
Get-BgpRoutingPolicy [[-Name] <String[]> ] [-CimSession <CimSession[]> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-PolicyType <PolicyType> {Deny | Allow | ModifyAttribute} ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[Get-bgpstatistics](https://technet.microsoft.com/library/dn262685.aspx)  
  
抓取 BGP 對等互連相關訊息和路由通告統計資料。  
  
```  
Get-BgpStatistics [-CimSession <CimSession[]> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-PeerName <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
### <a name="bkmk_install"></a>安裝命令  
以下是 RAS 閘道和 BGP 的安裝命令。  
  
[安裝-RemoteAccess](https://technet.microsoft.com/library/hh918408.aspx)  
  
會執行 DirectAccess （DA）的必要條件檢查，以確保它可以安裝、安裝適用于遠端存取（RA）的 DA （包括遠端用戶端的管理），或僅管理遠端用戶端、安裝 VPN （遠端存取 VPN 和站對站 VPN）。和會安裝 BGP 路由。  
  
```  
Parameter Set: MultiTenant  
Install-RemoteAccess [-MultiTenancy] [-CapacityKbps <UInt64> ] [-CimSession <CimSession[]> ] [-ComputerName <String> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-MsgAuthenticator <String> {Enabled | Disabled} ] [-PassThru] [-RadiusPort <UInt16> ] [-RadiusScore <Byte> ] [-RadiusServer <String> ] [-RadiusTimeout <UInt32> ] [-SharedSecret <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
  
Parameter Set: Vpn  
Install-RemoteAccess [-VpnType] <String> {Vpn | VpnS2S | SstpProxy | RoutingOnly} [-CimSession <CimSession[]> ] [-ComputerName <String> ] [-EntrypointName <String> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-IPAddressRange <String[]> ] [-IPv6Prefix <String> ] [-Legacy] [-MsgAuthenticator <String> {Enabled | Disabled} ] [-PassThru] [-RadiusPort <UInt16> ] [-RadiusScore <Byte> ] [-RadiusServer <String> ] [-RadiusTimeout <UInt32> ] [-SharedSecret <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
> [!IMPORTANT]  
> 當您在多租使用者模式中安裝 RAS 閘道時，您必須使用**Enable-remoteaccessroutingdomain** Windows PowerShell 命令搭配 **-Type**參數值**All**，指定是否要為每個租使用者啟用 BGP。 下列範例程式碼說明如何在多租使用者模式中安裝 RAS，其中包含兩個租使用者 Contoso 和 Fabrikam 的所有 RAS 功能（點對站 VPN、站對站 VPN 和 BGP 路由）。  
  
```  
$Contoso_RoutingDomain = "ContosoTenant"  
$Fabrikam_RoutingDomain = "FabrikamTenant"  
  
Install-RemoteAccess -MultiTenancy  
  
Enable-RemoteAccessRoutingDomain -Name $Contoso_RoutingDomain -Type All -PassThru  
Enable-RemoteAccessRoutingDomain -Name $Fabrikam_RoutingDomain -Type All -PassThru  
```  
  
如果您使用遠端存取作為 LAN 路由器，而不是閘道，您仍然可以使用 BGP，這可提供在內部網路上具有動態路由的優點。 若要將遠端存取安裝為 BGP LAN 路由器，請在 Windows PowerShell 提示字元中輸入下列命令，然後按 ENTER。  
  
```  
Install-RemoteAccess -VpnType RoutingOnly  
```  
  
### <a name="bkmk_remove"></a>移除命令  
以下是 BGP 的移除命令。  
  
[移除-Add-bgpcustomroute](https://technet.microsoft.com/library/dn262669.aspx)  
  
從 BGP 路由器移除自訂路由。  
  
```  
Remove-BgpCustomRoute [-CimSession <CimSession[]> ] [-Force] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-Interface <String[]> ] [-Network <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[移除-Bgp](https://technet.microsoft.com/library/dn262675.aspx)  
  
從路由器移除 BGP 對等。  
  
```  
Remove-BgpPeer [-Name] <String[]> [-CimSession <CimSession[]> ] [-Force] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[移除-BgpRouteAggregate](https://technet.microsoft.com/library/mt463110.aspx)  
  
移除一組指定的匯總 BGP 路由。  
  
```  
Remove-BgpRouteAggregate [-CimSession <CimSession[]> ] [-Force] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-Prefix <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[移除-BgpRouter](https://technet.microsoft.com/library/dn262678.aspx)  
  
移除 BGP 路由器。  
  
```  
Remove-BgpRouter [-CimSession <CimSession[]> ] [-Force] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-RoutingDomain <String[]> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[移除-BgpRoutingPolicy](https://technet.microsoft.com/library/dn262656.aspx)  
  
從原則存放區移除路由原則。  
  
```  
Remove-BgpRoutingPolicy [-Name] <String[]> [-CimSession <CimSession[]> ] [-Force] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[移除-BgpRoutingPolicyForPeer](https://technet.microsoft.com/library/dn262681.aspx)  
  
從 BGP 對等移除路由原則。  
  
```  
Parameter Set: Remove1  
Remove-BgpRoutingPolicyForPeer [-CimSession <CimSession[]> ] [-Direction <PolicyDirection> {Ingress | Egress} ] [-Force] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-PeerName <String[]> ] [-PolicyName <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
### <a name="bkmk_set"></a>設定命令  
以下是 BGP 的 Set 命令。  
  
[設定-Bgp](https://technet.microsoft.com/library/dn262673.aspx)  
  
更新指定之 BGP 對等的設定。  
  
```  
Set-BgpPeer [-Name] <String> [-CimSession <CimSession[]> ] [-ClearPrefixLimit] [-Force] [-HoldTimeSec <UInt16> ] [-IdleHoldTimeSec <UInt16> ] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-LocalASN <UInt32> ] [-LocalIPAddress <IPAddress> ] [-MaxAllowedPrefix <UInt32> ] [-OperationMode <OperationMode> {Mixed | Server} ] [-PassThru] [-PeerASN <UInt32> ] [-PeeringMode <PeeringMode> {Automatic | Manual} ] [-PeerIPAddress <IPAddress> ] [-RouteReflectorClient <Boolean> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Weight <UInt16> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[設定-BgpRouteAggregate](https://technet.microsoft.com/library/mt463115.aspx)  
  
更新指定匯總 BGP 路由的屬性。  
  
```  
Set-BgpRouteAggregate -Prefix <String> [-AttributePolicy <String[]> ] [-CimSession <CimSession[]> ] [-Force] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-PassThru] [-PreserveASPath <PreserveASPath> ] [-RoutingDomain <String> ] [-SummaryOnly <SummaryOnly> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[設定-BgpRouteFlapDampening](https://technet.microsoft.com/library/mt463116.aspx)  
  
設定 BGP 路由抑制引擎。  
  
```  
Set-BgpRouteFlapDampening [-CimSession <CimSession[]> ] [-Force] [-HalfLife <UInt32> ] [-HalfLifeUnreachable <UInt32> ] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-MaxSuppressTime <UInt32> ] [-PassThru] [-ReuseThreshold <UInt32> ] [-RoutingDomain <String> ] [-SuppressThreshold <UInt32> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[設定-BgpRouter](https://technet.microsoft.com/library/dn262652.aspx)  
  
針對指定的租使用者識別碼，更新本機 BGP 路由器的設定。  
  
```  
Set-BgpRouter [-BgpIdentifier <IPAddress> ] [-CimSession <CimSession[]> ] [-ClientToClientReflection <ClientToClientReflection> ] [-ClusterId <UInt32> ] [-CompareMEDAcrossASN <Boolean> ] [-DefaultGatewayRouting <Boolean> ] [-Force] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-IPv6Routing <IPv6RoutingState> {Disabled | Enabled} ] [-LocalASN <UInt32> ] [-LocalIPv6Address <IPAddress> ] [-PassThru] [-RouteReflector <RouteReflector> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-TransitRouting <TransitRouting> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[設定-BgpRoutingPolicy](https://technet.microsoft.com/library/dn262670.aspx)  
  
修改路由原則設定。  
  
```  
Set-BgpRoutingPolicy [-Name] <String> [-AddCommunity <String[]> ] [-CimSession <CimSession[]> ] [-ClearMED] [-Force] [-IgnorePrefix <String[]> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-MatchASNRange <UInt32[]> ] [-MatchCommunity <String[]> ] [-MatchNextHop <IPAddress[]> ] [-MatchPrefix <String[]> ] [-NewLocalPref <UInt32]> ] [-NewMED <UInt32]> ] [-NewNextHop <IPAddress> ] [-PassThru] [-PolicyType <PolicyType> {Deny | Allow | ModifyAttribute} ] [-RemoveAllCommunities] [-RemoveCommunity <String[]> ] [-RemovePolicyClause <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[設定-BgpRoutingPolicyForPeer](https://technet.microsoft.com/library/dn262674.aspx)  
  
修改 BGP 對等的 BGP 路由原則。  
  
```  
Set-BgpRoutingPolicyForPeer -Direction <PolicyDirection> {Ingress | Egress} -PolicyName <String[]> [-CimSession <CimSession[]> ] [-Force] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-PeerName <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
### <a name="bkmk_start"></a>啟動和停止命令  
以下是 BGP 的開始和停止命令。  
  
[開始-Bgp](https://technet.microsoft.com/library/dn262683.aspx)  
  
開始 BGP 對等的路由會話。  
  
```  
Start-BgpPeer [-Name] <String[]> [-CimSession <CimSession[]> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[停止-Bgp](https://technet.microsoft.com/library/dn262661.aspx)  
  
停止 BGP 對等的路由會話。  
  
```  
Stop-BgpPeer [-Name] <String[]> [-CimSession <CimSession[]> ] [-Force] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
### <a name="bkmk_uninstall"></a>卸載命令  
以下是 RAS 閘道和 BGP 的卸載命令。  
  
[卸載-RemoteAccess](https://technet.microsoft.com/library/hh918390.aspx)  
  
從電腦卸載遠端存取，包括所有遠端存取功能（RAS 閘道、BGP 等等）。  
  
```  
Uninstall-RemoteAccess [-CimSession <CimSession[]> ] [-ComputerName <String> ] [-EntrypointName <String> ] [-Force] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-ThrottleLimit <Int32> ] [-VpnType <String> {Vpn | VpnS2S} ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  


