---
title: '管理資料中心橋接 (DCB) '
description: 本主題提供有關如何使用 Windows PowerShell 命令來管理 Windows Server 2016 中的資料中心橋接的指示。
ms.topic: article
ms.assetid: 1575cc7c-62a7-4add-8f78-e5d93effe93f
manager: brianlic
ms.author: lizross
author: eross-msft
ms.date: 12/08/2020
ms.openlocfilehash: 51f4b70dfc81010bfb2989f925a9f6508b6eb1c1
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949924"
---
# <a name="manage-data-center-bridging-dcb"></a>管理資料中心橋接 (DCB) 

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題提供有關如何在執行 \( \) \- Windows Server 2016 或 Windows 10 的電腦上安裝的 DCB 相容網路介面卡上，使用 Windows PowerShell 命令設定資料中心橋接 DCB 的指示。

## <a name="install-dcb-in-windows-server-2016-or-windows-10"></a>在 Windows Server 2016 或 Windows 10 中安裝 DCB

如需使用及如何安裝 DCB 之必要條件的詳細資訊，請參閱 [Windows Server 2016 或 Windows 10 中的安裝資料中心橋接 (DCB) ](dcb-install.md)。


## <a name="dcb-configurations"></a>DCB 設定

在 Windows Server 2016 之前，所有 DCB 設定都已廣泛套用至所有支援 DCB 的網路介面卡。

在 Windows Server 2016 中，您可以將 DCB 設定套用至全域原則存放區或個別的原則存放區 \( \) 。 套用個別原則時，它們會覆寫所有的全域原則設定。

在您執行下列動作之前，系統層級上的流量類別、PFC 和應用程式優先順序指派的設定不會套用到網路介面卡。

1. 將 DCBX 的位轉換成 false

2. 在網路介面卡上啟用 DCB。 請參閱 [啟用和顯示網路介面卡上的 DCB 設定](#bkmk_enabledcb)。

>[!NOTE]
>如果您想要透過 DCBX 設定來自交換器的 DCB，請參閱 [DCBX 設定](#dcb-configuration-on-network-adapters)。

DCB 規格中說明了 DCBX 的願意位。 如果裝置上的願意位設定為 true，則裝置願意透過 DCBX 接受來自遠端裝置的設定。 如果裝置上的願意位設為 false，裝置將會拒絕遠端裝置的所有設定嘗試，並只強制執行本機設定。

如果 DCB 不是安裝在 Windows Server 2016 中，則願意位的值與作業系統無關，因為作業系統沒有本機設定適用于網路介面卡。 安裝 DCB 之後，願意位的預設值為 true。 這項設計可讓網路介面卡保留他們的遠端對等端可能收到的任何設定。

如果網路介面卡不支援 DCBX，則永遠不會接收來自遠端裝置的設定。 它會從作業系統接收設定，但只有在 DCBX 願意位設為 false 之後。

## <a name="set-the-willing-bit"></a>設定願意的位

若要在網路介面卡上強制執行流量類別、PFC 和應用程式優先順序指派的作業系統設定，或直接覆寫遠端裝置的設定 \ —如果有任何 \ —您可以執行下列命令。

>[!NOTE]
>DCB Windows PowerShell 命令名稱包含 "QoS"，而不是名稱字串中的 "DCB"。 這是因為 QoS 和 DCB 已整合到 Windows Server 2016 中，以提供順暢的 QoS 管理體驗。

```powershell
    Set-NetQosDcbxSetting -Willing $FALSE

    Confirm
    Are you sure you want to perform this action?
    Set-NetQosDcbxSetting -Willing $false
    [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"):
```

若要顯示「願意」位設定的狀態，您可以使用下列命令：

```powershell
    Get-NetQosDcbxSetting

    Willing PolicySetIfIndex IfAlias
    ------- ---------------- -------
    False   Global
```

## <a name="dcb-configuration-on-network-adapters"></a>網路介面卡上的 DCB 設定

在網路介面卡上啟用 DCB，可讓您查看從交換器傳播到網路介面卡的設定。

DCB 設定包含下列步驟。

1.  在系統層級設定 DCB 設定，包括：

    a. 流量類別管理

    b. 優先順序流量控制 (PFC) 設定

    c. 應用程式優先順序指派

    d. DCBX 設定

2. 在網路介面卡上設定 DCB。

##  <a name="dcb-traffic-class-management"></a>DCB 流量類別管理

以下是流量類別管理的範例 Windows PowerShell 命令。

### <a name="create-a-traffic-class"></a>建立流量類別

您可以使用 **get-netqostrafficclass** 命令來建立流量類別。

```powershell
    New-NetQosTrafficClass -Name SMB -Priority 4 -BandwidthPercentage 30 -Algorithm ETS

    Name Algorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ---- --------- ------------ -------- ---------------- -------
    SMB  ETS   30   4Global
```

根據預設，所有802.1 的 p 值都會對應至預設的流量類別，其具有實體連結的100% 頻寬。 **Get-netqostrafficclass** 命令會建立新的流量類別，其會對應以 802.1 p priority priority 值4標記的任何封包。 傳輸選取演算法 \( TSA \) 為 ETS，且具有30% 的頻寬。

您最多可以建立7個新的流量類別。 包含預設的流量類別，系統中最多可以有8個流量類別。 不過，支援 DCB 的網路介面卡可能不支援硬體中的許多流量類別。 如果您建立的流量類別多於可在網路介面卡上容納的數量，而且您在該網路介面卡上啟用 DCB，則微型埠驅動程式會向作業系統報告錯誤。 錯誤會記錄在事件記錄檔中。

所有已建立流量類別的頻寬保留總和可能不會超過99% 的頻寬。 預設的流量類別一律會保留至少1% 的頻寬給自己。

### <a name="display-traffic-classes"></a>顯示交通類別

您可以使用 **get-netqostrafficclass** 命令來查看流量類別。

```powershell
    Get-NetQosTrafficClass

    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   70   0-3,5-7  Global
    SMB ETS   30   4Global
```

### <a name="modify-a-traffic-class"></a>修改流量類別

您可以使用 **get-netqostrafficclass** 命令建立流量類別。

```powershell
    Set-NetQosTrafficClass -Name SMB -BandwidthPercentage 50
```

然後，您可以使用 **get-netqostrafficclass** 命令來查看設定。

```powershell
    Get-NetQosTrafficClass

    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   50   0-3,5-7  Global
    SMB ETS   50   4Global
```

建立流量類別之後，您可以單獨變更其設定。 您可以變更的設定包括：

1. 頻寬配置 (-BandwidthPercentage) 

2. TSA (-演算法) 

3. 優先順序對應 (-優先順序) 

### <a name="remove-a-traffic-class"></a>移除流量類別

您可以使用 **get-netqostrafficclass** 命令來刪除流量類別。

>[!IMPORTANT]
>您無法移除預設的流量類別。

```powershell
    Remove-NetQosTrafficClass -Name SMB

You can then use the **Get-NetQosTrafficClass** command to view settings.

    Get-NetQosTrafficClass

    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   100  0-7  Global
```

移除流量類別之後，對應至該流量類別的 802.1 p 值會重新對應至預設的流量類別。 當流量類別被移除時，任何已保留給流量類別的頻寬都會傳回預設的流量類別配置。

## <a name="per-network-interface-policies"></a>Per-Network 介面原則

上述所有範例都會設定全域原則。 以下是您可以如何設定和取得每個 NIC 原則的範例。

"PolicySet" 欄位會從 Global 變更為 AdapterSpecific。 顯示 AdapterSpecific 原則時， \( 也會顯示介面索引 ifIndex \) 和介面名稱 \( ifAlias \) 。

```powershell
PS C:\> Get-NetQosTrafficClass

Name        Algorithm Bandwidth(%) Priority         PolicySet        IfIndex IfAlias
----        --------- ------------ --------         ---------        ------- -------
[Default]   ETS       100          0-7              Global


PS C:\> Get-NetQosTrafficClass -InterfaceAlias M1

Name        Algorithm Bandwidth(%) Priority         PolicySet        IfIndex IfAlias
----        --------- ------------ --------         ---------        ------- -------
[Default]   ETS       100          0-7              AdapterSpecific  4       M1


PS C:\> New-NetQosTrafficClass -Name SMBGlobal -BandwidthPercentage 30 -Priority 4 -Algorithm ETS

Name        Algorithm Bandwidth(%) Priority         PolicySet        IfIndex IfAlias
----        --------- ------------ --------         ---------        ------- -------
SMBGlobal   ETS       30           4                Global


PS C:\> New-NetQosTrafficClass -Name SMBforM1 -BandwidthPercentage 30 -Priority 4 -Algorithm ETS -Interfac


Name        Algorithm Bandwidth(%) Priority         PolicySet        IfIndex IfAlias
----        --------- ------------ --------         ---------        ------- -------
SMBforM1    ETS       30           4                AdapterSpecific  4       M1


PS C:\> Get-NetQosTrafficClass

Name        Algorithm Bandwidth(%) Priority         PolicySet        IfIndex IfAlias
----        --------- ------------ --------         ---------        ------- -------
[Default]   ETS       70           0-3,5-7          Global
SMBGlobal   ETS       30           4                Global


PS C:\> Get-NetQosTrafficClass -InterfaceAlias M1

Name        Algorithm Bandwidth(%) Priority         PolicySet        IfIndex IfAlias
----        --------- ------------ --------         ---------        ------- -------
[Default]   ETS       70           0-3,5-7          AdapterSpecific  4       M1
SMBforM1    ETS       30           4                AdapterSpecific  4       M1

```

## <a name="priority-flow-control-settings"></a>優先順序流量控制設定：

以下是優先順序流量控制設定的命令範例。 您也可以針對個別介面卡指定這些設定。

### <a name="enable-and-display-priority-flow-control-for-global-and-interface-specific-use-cases"></a>啟用和顯示全域和介面特定使用案例的優先順序流程式控制制

```powershell
PS C:\> Enable-NetQosFlowControl -Priority 4
PS C:\> Enable-NetQosFlowControl -Priority 3 -InterfaceAlias M1
PS C:\> Get-NetQosFlowControl

Priority   Enabled    PolicySet        IfIndex IfAlias
--------   -------    ---------        ------- -------
0          False      Global
1          False      Global
2          False      Global
3          False      Global
4          True       Global
5          False      Global
6          False      Global
7          False      Global

PS C:\> Get-NetQosFlowControl -InterfaceAlias M1

Priority   Enabled    PolicySet        IfIndex IfAlias
--------   -------    ---------        ------- -------
0          False      AdapterSpecific  4       M1
1          False      AdapterSpecific  4       M1
2          False      AdapterSpecific  4       M1
3          True       AdapterSpecific  4       M1
4          False      AdapterSpecific  4       M1
5          False      AdapterSpecific  4       M1
6          False      AdapterSpecific  4       M1
7          False      AdapterSpecific  4       M1
```

### <a name="disable-priority-flow-control-global-and-interface-specific"></a>停用全域和介面特定) 的優先順序流量控制 (

```powershell
PS C:\> Disable-NetQosFlowControl -Priority 4
PS C:\> Disable-NetQosFlowControl -Priority 3 -InterfaceAlias m1
PS C:\> Get-NetQosFlowControl

Priority   Enabled    PolicySet        IfIndex IfAlias
--------   -------    ---------        ------- -------
0          False      Global
1          False      Global
2          False      Global
3          False      Global
4          False      Global
5          False      Global
6          False      Global
7          False      Global

PS C:\> Get-NetQosFlowControl -InterfaceAlias M1

Priority   Enabled    PolicySet        IfIndex IfAlias
--------   -------    ---------        ------- -------
0          False      AdapterSpecific  4       M1
1          False      AdapterSpecific  4       M1
2          False      AdapterSpecific  4       M1
3          False      AdapterSpecific  4       M1
4          False      AdapterSpecific  4       M1
5          False      AdapterSpecific  4       M1
6          False      AdapterSpecific  4       M1
7          False      AdapterSpecific  4       M1
```

##  <a name="application-priority-assignment"></a>應用程式優先順序指派

以下是優先順序指派的範例。

### <a name="create-qos-policy"></a>建立 QoS 原則

```powershell
PS C:\> New-NetQosPolicy -Name "SMB Policy" -SMB -PriorityValue8021Action 4

Name           : SMB Policy
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
Template       : SMB
PriorityValue  : 4
```

先前的命令會建立新的 SMB 原則。 – SMB 是一個收件匣篩選器，可符合 (保留給 SMB) 的 TCP 埠445。 如果封包傳送至 TCP 埠445，則會在封包傳遞至網路微型埠驅動程式之前，使用 802.1 p 值4的作業系統標記。

除了– SMB 之外，其他預設篩選器還包括-iSCSI (符合 TCP 埠 3260) 、-NFS (符合 tcp 埠 2049) 、-LiveMigration (符合 TCP 埠 6600) 、-FCOE (符合 EtherType 0x8906) 和– NetworkDirect。

NetworkDirect 是我們在網路介面卡上的任何 RDMA 執行之上建立的抽象層。 – NetworkDirect 後面必須接著網路直接埠。

除了預設的篩選準則之外，您還可以依應用程式的可執行檔名稱來分類流量 (如以下第一個範例所) ，或是依 IP 位址、埠或通訊協定 (，如第二個範例) 所示：

**依可執行檔名稱**

```powershell
PS C:\> New-NetQosPolicy -Name background -AppPathNameMatchCondition "C:\Program files (x86)\backup.exe" -PriorityValue8021Action 1

Name           : background
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
AppPathName    : C:\Program files (x86)\backup.exe
JobObject      :
PriorityValue  : 1
```

**依 IP 位址埠或通訊協定**

```powershell
PS C:\> New-NetQosPolicy -Name "Network Management" -IPDstPrefixMatchCondition 10.240.1.0/24 -IPProtocolMatchCondition both -NetworkProfile all -PriorityValue8021Action 7

Name           : Network Management
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
JobObject      :
IPProtocol     : Both
IPDstPrefix    : 10.240.1.0/24
PriorityValue  : 7
```

### <a name="display-qos-policy"></a>顯示 QoS 原則

```powershell
PS C:\> Get-NetQosPolicy

Name           : background
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
AppPathName    : C:\Program files (x86)\backup.exe
JobObject      :
PriorityValue  : 1

Name           : Network Management
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
JobObject      :
IPProtocol     : Both
IPDstPrefix    : 10.240.1.0/24
PriorityValue  : 7

Name           : SMB Policy
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
Template       : SMB
JobObject      :
PriorityValue  : 4
```

### <a name="modify-qos-policy"></a>修改 QoS 原則

您可以修改 QoS 原則，如下所示。

```powershell
PS C:\> Set-NetQosPolicy -Name "Network Management" -IPSrcPrefixMatchCondition 10.235.2.0/24 -IPProtocolMatchCondition both -PriorityValue8021Action 7
PS C:\> Get-NetQosPolicy

Name           : Network Management
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
JobObject      :
IPProtocol     : Both
IPSrcPrefix    : 10.235.2.0/24
IPDstPrefix    : 10.240.1.0/24
PriorityValue  : 7
```

### <a name="remove-qos-policy"></a>移除 QoS 原則

```powershell
PS C:\> Remove-NetQosPolicy -Name "Network Management"

Confirm
Are you sure you want to perform this action?
Remove-NetQosPolicy -Name "Network Management" -Store GPO:localhost
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"): y
```

## <a name="dcb-configuration-on-network-adapters"></a>網路介面卡上的 DCB 設定

網路介面卡上的 DCB 設定與上面所述系統層級的 DCB 設定無關。

無論是否已在 Windows Server 2016 中安裝 DCB，您都可以執行下列命令。

如果您從交換器設定 DCB，並依賴 DCBX 將設定傳播至網路介面卡，您可以在啟用網路介面卡上的 DCB 之後，檢查網路介面卡上從作業系統端接收和強制執行的設定。

###  <a name="enable-and-display-dcb-settings-on--network-adapters"></a><a name="bkmk_enabledcb"></a>在網路介面卡上啟用和顯示 DCB 設定

```powershell
PS C:\> Enable-NetAdapterQos M1
PS C:\> Get-NetAdapterQos

Name                       : M1
Enabled                    : True
Capabilities               :                       Hardware     Current
                                                   --------     -------
                             MacSecBypass        : NotSupported NotSupported
                             DcbxSupport         : None         None
                             NumTCs(Max/ETS/PFC) : 8/8/8        8/8/8

OperationalTrafficClasses  : TC TSA    Bandwidth Priorities
                             -- ---    --------- ----------
                              0 ETS    70%       0-3,5-7
                              1 ETS    30%       4

OperationalFlowControl     : All Priorities Disabled
OperationalClassifications : Protocol  Port/Type Priority
                             --------  --------- --------
                             Default             1
```

### <a name="disable-dcb-on-network-adapters"></a>停用網路介面卡上的 DCB

```powershell
PS C:\> Disable-NetAdapterQos M1
PS C:\> Get-NetAdapterQos M1

Name         : M1
Enabled      : False
Capabilities :                       Hardware     Current
                                     --------     -------
               MacSecBypass        : NotSupported NotSupported
               DcbxSupport         : None         None
               NumTCs(Max/ETS/PFC) : 8/8/8        0/0/0
```

## <a name="windows-powershell-commands-for-dcb"></a><a name="bkmk_wps"></a>適用于 DCB 的 Windows PowerShell 命令

Windows Server 2016 和 Windows Server 2012 R2 都有 DCB Windows PowerShell 命令。 您可以在 Windows Server 2016 中使用 Windows Server 2012 R2 的所有命令。

### <a name="windows-server-2016-windows-powershell-commands-for-dcb"></a>適用于 DCB 的 Windows Server 2016 Windows PowerShell 命令

適用于 Windows Server 2016 的下列主題提供 Windows PowerShell 的 Cmdlet 說明和語法，適用于所有資料中心橋接 \( DCB \) 服務 \( QoS \) \- 特定 Cmdlet 的品質。 Cmdlet 清單以 Cmdlet 開頭動詞的字母順序排列。

- [DcbQoS 模組](/powershell/module/dcbqos/)

### <a name="windows-server-2012-r2-windows-powershell-commands-for-dcb"></a>適用于 DCB 的 Windows Server 2012 R2 Windows PowerShell 命令

下列適用于 Windows Server 2012 R2 的主題提供 Windows PowerShell 的 Cmdlet 說明和語法，適用于所有資料中心橋接 \( DCB \) 服務 \( QoS \) \- 特定 Cmdlet 的品質。 Cmdlet 清單以 Cmdlet 開頭動詞的字母順序排列。

- [Windows PowerShell 中的資料中心橋接 (DCB) 服務品質 (QoS) Cmdlet](/powershell/module/dcbqos/)
