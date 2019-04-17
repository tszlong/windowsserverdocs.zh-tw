---
title: 管理資料中心橋接 (DCB)
description: 本主題提供您如何使用 Windows PowerShell 命令來管理資料中心橋接在 Windows Server 2016 上的指示操作。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 1575cc7c-62a7-4add-8f78-e5d93effe93f
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dbe9e5e4af2ddd834b5b8f38e9ffd1b403e92793
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="manage-data-center-bridging-dcb"></a>管理資料中心橋接 (DCB)

>適用於：Windows Server（以每年次管道）、Windows Server 2016

本主題提供您如何使用 Windows PowerShell 命令設定的資料中心橋接 \(DCB\) 已安裝在電腦上執行 Windows 10 或 Windows Server 2016 DCB\ 相容網路介面卡上的指示操作。

## <a name="install-dcb-in-windows-server-2016-or-windows-10"></a>Windows Server 2016 或 Windows 10 中安裝 DCB

有關使用及如何安裝 DCB 必要條件資訊，請查看[安裝資料中心橋接 (DCB) 的 Windows Server 2016 或 Windows 10 在](dcb-install.md)。


## <a name="dcb-configurations"></a>DCB 設定 

Windows Server 2016 之前所有 DCB 設定已都適用以支援 DCB 所有網路介面卡。 

在 Windows Server 2016，您可以在通用原則網上商店，或是個人原則 Store\(s\) 套用 DCB 設定。 當個人原則已經套用他們覆寫所有全球原則設定。

之前，請執行下列網路介面卡不被套用的系統層級流量課程、PFC 和應用程式的優先順序設定的設定。

1. 將 \ [false\] DCBX 願意位元

2. 讓 DCB 網路介面卡上。 查看[讓和 DCB 設定顯示在網路介面卡](#bkmk_enabledcb)。

>[!NOTE]
>如果您想要設定 DCB 透過 DCBX 切換控制從，請查看[DCBX 設定](#BKMK_DCBX_Settings)

DCBX 願意元是 DCB 規格中所述。 如果設願意元在裝置上的為 true，該裝置是接受從遠端透過 DCBX 裝置的設定。 如果在裝置上的願意位元設定為，此裝置拒絕所有設定嘗試從遠端裝置，並執行只本機設定。

如果 DCB 未安裝 Windows Server 2016 中的願意的位元值無關一樣作業系統，作業系統不本機設定套用到網路介面卡因為而言。 DCB 安裝之後，願意的位元的預設值為 true。 此設計，可讓網路介面卡，以讓它們可能會有收到遠端同事們任何設定。

如果網路介面卡不支援 DCBX，它將不會收到設定從遠端裝置。 它會收到作業系統設定，但 DCBX 後才願意元為 \ [false\]。

## <a name="set-the-willing-bit"></a>設定願意位元

若要執行作業系統設定的資料傳輸、PFC，以及應用程式的優先順序設定網路介面卡，或只是覆寫從遠端裝置的組態 \，如果有任何 \，您可以執行下列命令。

>[!NOTE]
>DCB Windows PowerShell 命令名稱包含「QoS」，而不是「DCB」中的名稱。 這是因為 QoS 和 DCB 提供順暢的 QoS 管理體驗 Windows Server 2016 中整合。

    
    Set-NetQosDcbxSetting -Willing $FALSE
    
    Confirm
    Are you sure you want to perform this action?
    Set-NetQosDcbxSetting -Willing $false
    [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"):
    

若要顯示設定的願意位元的狀態，您可以使用下列命令：

    
    Get-NetQosDcbxSetting
    
    Willing PolicySetIfIndex IfAlias
    ------- ---------------- -------
    False   Global  
    

## <a name="dcb-configuration-on-network-adapters"></a>DCB 網路介面卡設定

讓 DCB 網路介面卡，可讓您看到的切換控制從傳送到網路介面卡的設定。

DCB 設定包括下列步驟。

1.  設定 DCB 在系統層級，包括：

    a。 交通課程管理
    
    b。 優先順序流程控制項 (PFC) 設定
    
    c。 應用程式的優先順序設定
    
    d。 DCBX 設定

2. 設定 DCB 網路介面卡上。



##  <a name="dcb-traffic-class-management"></a>管理 DCB 流量課

以下是範例流量課程管理 Windows PowerShell 命令。

### <a name="create-a-traffic-class"></a>建立流量課

您可以使用**新-NetQosTrafficClass**來建立流量課程命令。

    
    New-NetQosTrafficClass -Name SMB -Priority 4 -BandwidthPercentage 30 -Algorithm ETS
    
    Name Algorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ---- --------- ------------ -------- ---------------- -------
    SMB  ETS   30   4Global
      

根據預設，所有 802.1 p 值對應至預設流量課程，它的實體連結頻寬 100%。 **新-NetQosTrafficClass**命令會建立新的交通課程，會附上 802.1 p 高優先順序的任何一封包值 4 對應。 傳輸選擇演算法 \(TSA\) ETS，30%的頻寬。

您可以建立 7 最新的交通類別。 包括預設流量課程，可能會有最 8 流量類別系統中。 不過，DCB 功能的網路介面卡可能不支援許多流量類別硬體中。 如果您建立可以在 [網路介面卡上可容納超過更多的流量類別，您的網路介面卡上讓 DCB 迷你連接埠驅動程式會作業系統來報告錯誤。 這個錯誤被登入事件登入。

適用於所有建立的流量類別頻寬保留總和可能不會超過 99%的頻寬。 預設流量課程永遠具有至少 1%的頻寬自行保留。

### <a name="display-traffic-classes"></a>顯示流量類別

您可以使用**取得-NetQosTrafficClass**檢視流量類別的命令。

    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   70   0-3,5-7  Global
    SMB ETS   30   4Global  
    
### <a name="modify-a-traffic-class"></a>修改流量課

您可以使用**設定為 NetQosTrafficClass**來建立流量課程命令。 

    Set-NetQosTrafficClass -Name SMB -BandwidthPercentage 50

您可以使用**取得-NetQosTrafficClass**命令，可檢視設定。

    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   50   0-3,5-7  Global
    SMB ETS   50   4Global   
    

建立流量課程之後，您可以變更其設定獨立。 您可以變更設定包括︰

1. 頻寬配置 \(-BandwidthPercentage\)

2. TSA (\-Algorithm\)

3. 優先順序對應 \(-Priority\)

### <a name="remove-a-traffic-class"></a>移除流量課

您可以使用**移除-NetQosTrafficClass**以 delete 流量課程命令。

>[!IMPORTANT]
>您無法移除預設流量課程。


    Remove-NetQosTrafficClass -Name SMB

您可以使用**取得-NetQosTrafficClass**命令，可檢視設定。
    
    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   100  0-7  Global
    

移除流量課程之後，802.1 p 值對應至的流量課程被對應至預設流量課程。 移除流量課程時保留流量課程給任何頻寬被返回預設流量課程配置。

## <a name="per-network-interface-policies"></a>每個網路介面原則

所有上述範例設定全球的原則。 以下是如何設定，並取得每個 NIC 原則的範例。 

在「PolicySet] 欄位通用從 AdapterSpecific 變更。 顯示 AdapterSpecific 原則時, 的介面索引 \(ifIndex\) 和介面名稱 \(ifAlias\) 也會顯示。

```
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

## <a name="priority-flow-control-settings"></a>優先順序流量控制的設定：

以下是命令範例優先順序流量控制的設定。 這些設定也可以指定個人介面卡。

### <a name="enable-and-display-priority-flow-control-for-global-and-interface-specific-use-cases"></a>讓與顯示優先順序 Flow 控制通用和介面特定使用案例

```
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


### <a name="disable-priority-flow-control-global-and-interface-specific"></a>停用優先順序流程控制項 (全球和介面特定)

```
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

##  <a name="application-priority-assignment"></a>應用程式的優先順序設定

以下是範例的優先順序設定。

### <a name="create-qos-policy"></a>建立 QoS 原則

```
PS C:\> New-NetQosPolicy -Name "SMB Policy" -PriorityValue8021Action 4

Name           : SMB Policy
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
JobObject      :
PriorityValue  : 4

```

前一個命令 smb 建立新的原則。 – SMB 是收件匣篩選器符合 445（保留 SMB）的 TCP 連接埠。 如果封包傳送的 445 它將會標記 802.1 p 值 4 之前的作業系統的 TCP 連接埠封包會被傳遞至網路迷你連接埠驅動程式。

– SMB，除了其他預設篩選包含 – iSCSI（相符的 TCP 連接埠 3260）、-NFS（相符的 TCP 連接埠 2049 年）、-LiveMigration（相符的 TCP 連接埠 6600）、-（符合 EtherType 0x8906）FCOE 和 – NetworkDirect。

NetworkDirect 為網路介面卡上的任何 RDMA 實作上方時，我們建立了抽象維度.層級。 – NetworkDirect 必須跟隨網路直接連接埠。

除了預設篩選器，您可以將分類流量應用程式的可執行檔名稱（第一次如範例所示下列），或 IP 位址、連接埠或通訊協定（在第二個範例所示）：

**透過可執行檔名稱**

```
PS C:\> New-NetQosPolicy -Name background -AppPathNameMatchCondition "C:\Program files (x86)\backup.exe" -PriorityValue8021Action 1

Name           : background
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
AppPathName    : C:\Program files (x86)\backup.exe
JobObject      :
PriorityValue  : 1

```


**通訊協定的 IP 位址連接埠**

```
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

```
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
JobObject      :
PriorityValue  : 4

```

### <a name="modify-qos-policy"></a>修改 QoS 原則

您可以修改 QoS 原則，如下所示。


```
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

```
PS C:\> Remove-NetQosPolicy -Name "Network Management"

Confirm
Are you sure you want to perform this action?
Remove-NetQosPolicy -Name "Network Management" -Store GPO:localhost
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"): y  

```

## <a name="dcb-configuration-on-network-adapters"></a>DCB 網路介面卡設定

網路介面卡 DCB 設定為上述的系統層級 DCB 組態的獨立。 

無論是否 DCB 已安裝 Windows Server 2016 中，您可以隨時執行下列命令。 

如果您從 [切換設定 DCB 依賴 DCBX 傳播它到網路介面卡設定，您可以檢查所設定的收到和之後，您可以 DCB 網路介面卡上的網路介面卡，從側邊的作業系統上執行。

###  <a name="bkmk_enabledcb"></a>讓和 DCB 設定顯示在網路介面卡

```
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

### <a name="disable-dcb-on-network-adapters"></a>停用 DCB 網路介面卡

```
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
## <a name="bkmk_wps"></a>Windows PowerShell 命令 DCB

有適用於 Windows Server 2016 和 Windows Server 2012 R2 DCB Windows PowerShell 命令。 您可以使用的命令所有的 Windows Server 2016 中的 Windows Server 2012 R2。

### <a name="windows-server-2016-windows-powershell-commands-for-dcb"></a>Windows Server 2016 Windows PowerShell 命令 DCB

下列主題適用於 Windows Server 2016 的所有的資料中心橋接 \(DCB\) 提供 Windows PowerShell cmdlet 描述和語法服務品質 \ (QoS\) \-specific cmdlet。 它會列出 cmdlet 依字母順序根據動詞 cmdlet 的開頭。

- [DcbQoS 模組](https://technet.microsoft.com/itpro/powershell/windows/dcbqos/dcbqos)

### <a name="windows-server-2012-r2-windows-powershell-commands-for-dcb"></a>Windows Server 2012 R2 Windows PowerShell 命令 DCB

下列主題適用於 Windows Server 2012 R2 的所有的資料中心橋接 \(DCB\) 提供 Windows PowerShell cmdlet 描述和語法服務品質 \ (QoS\) \-specific cmdlet。 它會列出 cmdlet 依字母順序根據動詞 cmdlet 的開頭。

- [資料中心橋接 (DCB) 品質的 Windows PowerShell 中的服務 (QoS) Cmdlet](https://technet.microsoft.com/library/hh967440.aspx)
