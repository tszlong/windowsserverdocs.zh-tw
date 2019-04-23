---
title: 管理資料中心橋接 (DCB)
description: 本主題為您提供有關如何使用 Windows PowerShell 命令來管理資料中心橋接在 Windows Server 2016 中的指示。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 1575cc7c-62a7-4add-8f78-e5d93effe93f
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3912bb6048a06a4656b5b27ccec8f8fb3f5b114b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847409"
---
# <a name="manage-data-center-bridging-dcb"></a>管理資料中心橋接 (DCB)

>適用於：Windows Server （半年通道），Windows Server 2016

本主題提供有關如何使用 Windows PowerShell 命令來設定資料中心橋接\(DCB\) DCB 上\-中正在執行的電腦已安裝相容的網路介面卡Windows Server 2016 或 Windows 10。

## <a name="install-dcb-in-windows-server-2016-or-windows-10"></a>在 Windows Server 2016 或 Windows 10 安裝 DCB

如需使用和如何安裝 DCB 必要條件的資訊，請參閱[安裝的資料中心橋接 (DCB) 在 Windows Server 2016 或 Windows 10](dcb-install.md)。


## <a name="dcb-configurations"></a>DCB 組態 

在 Windows Server 2016 之前, 所有 DCB 設定已都套用通用至所有支援 DCB 的網路介面卡。 

在 Windows Server 2016 中，您可以套用 DCB 設定全域原則存放區或個別的原則存放區\(s\)。 套用個別的原則時它們會覆寫所有的全域原則設定。

在系統層級的流量類別，PFC 和應用程式優先順序指派的組態不會套用網路介面卡上，直到您執行下列作業。

1. 開啟 DCBX 願意位元為 false

2. 網路介面卡上啟用 DCB。 請參閱[啟用，並顯示網路介面卡上的 DCB 設定](#bkmk_enabledcb)。

>[!NOTE]
>如果您想要從 DCBX 透過交換器設定 DCB，請參閱[DCBX 設定](#BKMK_DCBX_Settings)

DCBX 願意元是 DCB 規格所述。 如果在裝置上的願意位元會設為 true，該裝置是願意接受從透過 DCBX 遠端裝置的設定。 如果在裝置上的願意位元設為 false 時，裝置會拒絕所有的設定嘗試從遠端裝置，並強制執行只有的本機設定。

如果 DCB 未安裝 Windows Server 2016 中願意的位元值無關的作業系統是而言，因為作業系統已套用至網路介面卡沒有本機設定。 DCB 安裝之後，願意的位元的預設值為 true。 此設計可讓網路介面卡，將任何可能已收到來自其遠端對等的設定。

如果網路介面卡不支援 DCBX，它將不會從遠端裝置上收到組態。 它會接收組態來自作業系統，但 DCBX 後才有意的位元會設為 false。

## <a name="set-the-willing-bit"></a>設定願意的位元

若要強化的流量類別、 PFC 和應用程式優先順序指派網路介面卡上的作業系統設定，或只是覆寫來自遠端裝置的設定 \ — 如果有的話 \ — 您可以執行下列命令。

>[!NOTE]
>DCB 的 Windows PowerShell 命令名稱"QoS"，而不是"DCB"的字串中包含名稱。 這是因為在 Windows Server 2016 中提供無縫式的 QoS 管理體驗整合 QoS 和 DCB。

    
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
    

## <a name="dcb-configuration-on-network-adapters"></a>網路介面卡上的 DCB 組態

網路介面卡上啟用 DCB，可讓您查看與交換器傳播到網路介面卡的組態。

DCB 設定包括下列步驟。

1.  DCB 的設定系統層級，其中包括：

    a. 流量類別管理
    
    b. 優先順序流量控制 (PFC) 設定
    
    c.  指派應用程式優先權
    
    d. DCBX 設定

2. 網路介面卡上設定 DCB。



##  <a name="dcb-traffic-class-management"></a>DCB Traffic Class 管理

以下是範例 Windows PowerShell 命令的流量類別管理。

### <a name="create-a-traffic-class"></a>建立資料傳輸類別

您可以使用**新增 NetQosTrafficClass**命令來建立 「 流量 」 類別。

    
    New-NetQosTrafficClass -Name SMB -Priority 4 -BandwidthPercentage 30 -Algorithm ETS
    
    Name Algorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ---- --------- ------------ -------- ---------------- -------
    SMB  ETS   30   4Global
      

根據預設，所有的 802.1p 值會對應至預設的流量類別，具有 100%的實體連結的頻寬。 **新增 NetQosTrafficClass**命令會建立新的流量類別，任何的標記具有 802.1p 優先順序的封包至值 4 會對應。 傳輸選擇演算法\(TSA\) ETS，且具有 30%的頻寬。

您可以建立最多 7 新的流量類別。 預設的流量類別，包括可以有最多 8 的流量類別在系統中。 不過，DCB 功能的網路介面卡可能不支援許多流量在硬體中的類別中。 如果您建立比可以容納在網路介面卡的流量類別，而且您在該網路介面卡上啟用 DCB，miniport 驅動程式會向作業系統回報錯誤。 錯誤會記錄在事件記錄檔。

所有建立的流量類別的頻寬保留項目總和可能不會超過 99%的頻寬。 預設流量類別一律會有至少 1%的頻寬保留給本身。

### <a name="display-traffic-classes"></a>顯示的流量類別

您可以使用**Get-netqostrafficclass**命令來檢視流量類別。

    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   70   0-3,5-7  Global
    SMB ETS   30   4Global  
    
### <a name="modify-a-traffic-class"></a>修改的資料傳輸類別

您可以使用**組 NetQosTrafficClass**命令來建立 「 流量 」 類別。 

    Set-NetQosTrafficClass -Name SMB -BandwidthPercentage 50

然後您可以使用**Get-netqostrafficclass**命令來檢視設定。

    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   50   0-3,5-7  Global
    SMB ETS   50   4Global   
    

您建立的流量類別之後，您可以變更其設定獨立。 您可以變更的設定包括：

1. 頻寬配置\(-BandwidthPercentage\)

2. TSA (\-Algorithm\)

3. 優先順序對應\(-優先順序\)

### <a name="remove-a-traffic-class"></a>移除的資料傳輸類別

您可以使用**移除 NetQosTrafficClass**命令來刪除 「 流量 」 類別。

>[!IMPORTANT]
>您無法移除預設的流量類別。


    Remove-NetQosTrafficClass -Name SMB

然後您可以使用**Get-netqostrafficclass**命令來檢視設定。
    
    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   100  0-7  Global
    

移除流量類別之後，802.1p 值對應到流量類別會重新對應至預設流量類別。 移除流量類別時，任何已保留給 「 流量 」 類別的頻寬會回到預設流量類別配置。

## <a name="per-network-interface-policies"></a>每個網路介面原則

上述範例中的所有設定全域原則。 以下是如何設定和取得每個 NIC 原則的範例。 

從全域 AdapterSpecific，變更 「 PolicySet"欄位。 當 AdapterSpecific 原則會顯示，介面索引\(ifIndex\)和介面名稱\(ifAlias\)也會顯示。

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

以下是優先順序流量控制設定的命令範例。 這些設定也可以指定個別的配接器。

### <a name="enable-and-display-priority-flow-control-for-global-and-interface-specific-use-cases"></a>啟用和顯示優先順序流量控制的 Global 與介面特定使用案例

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


### <a name="disable-priority-flow-control-global-and-interface-specific"></a>停用優先順序流量控制 (全域和特定的介面)

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

##  <a name="application-priority-assignment"></a>指派應用程式優先權

以下是優先順序指派的範例。

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

前一個命令會建立新的原則，smb。 – SMB 會比對 TCP 通訊埠 445 （smb 保留） 的收件匣 篩選器。 如果封包傳送至 TCP 連接埠 445，將會標記具有 802.1p 值 4 之前的作業系統則會將封包傳遞給網路 miniport 驅動程式。

– 除了 SMB 之外，其他的預設篩選器會包含 – iSCSI （比對 TCP 連接埠 3260）、-NFS （比對 TCP 連接埠 2049年）、-LiveMigration （比對 TCP 連接埠 6600）、-FCOE （比對 EtherType 0x8906） 和 – NetworkDirect。

NetworkDirect 是我們會建立任何網路介面卡的 RDMA 實作之上的抽象層。 – NetworkDirect 會接上網路直接連接埠。

除了預設篩選器中，您可以將資料傳輸 （如下列第一個範例中），應用程式的可執行檔名稱或 IP 位址、 連接埠或通訊協定 （如第二個範例所示）：

**由可執行檔名稱**

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


**依 IP 位址的連接埠或通訊協定**

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

## <a name="dcb-configuration-on-network-adapters"></a>網路介面卡上的 DCB 組態

DCB 設定網路介面卡上的系統層級上面所述的 DCB 組態無關。 

不論是否已安裝 DCB，Windows Server 2016 中，您一律可以執行下列命令。 

如果您設定 DCB 的切換，並依賴 DCBX 傳播至網路介面卡的組態，您可以檢查哪些組態的接收和從作業系統端的網路介面卡上強制執行之後您的網路介面卡上啟用 DCB。

###  <a name="bkmk_enabledcb"></a>啟用並顯示 DCB 設定網路介面卡上

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

### <a name="disable-dcb-on-network-adapters"></a>停用網路介面卡上的 DCB

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
## <a name="bkmk_wps"></a>DCB 的 Windows PowerShell 命令

有 Windows Server 2016 和 Windows Server 2012 R2 的 DCB Windows PowerShell 命令。 您可以使用的所有命令的 Windows Server 2016 中的 Windows Server 2012 R2。

### <a name="windows-server-2016-windows-powershell-commands-for-dcb"></a>Windows Server 2016 Windows PowerShell 命令，DCB

Windows Server 2016 的下列主題提供 Windows PowerShell cmdlet 說明和語法的所有資料中心橋接\(DCB\)服務品質\(QoS\)\-特定 cmdlet。 Cmdlet 清單以 Cmdlet 開頭動詞的字母順序排列。

- [DcbQoS Module](https://technet.microsoft.com/itpro/powershell/windows/dcbqos/dcbqos)

### <a name="windows-server-2012-r2-windows-powershell-commands-for-dcb"></a>Windows Server 2012 R2 Windows PowerShell 命令，DCB

Windows Server 2012 R2 的下列主題提供 Windows PowerShell cmdlet 說明和語法的所有資料中心橋接\(DCB\)服務品質\(QoS\)\-特定 cmdlet。 Cmdlet 清單以 Cmdlet 開頭動詞的字母順序排列。

- [資料中心橋接 (DCB) 服務品質 (QoS) Cmdlet 在 Windows PowerShell](https://technet.microsoft.com/library/hh967440.aspx)
