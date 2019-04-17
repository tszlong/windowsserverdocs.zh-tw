---
title: 聚合型的 NIC 小組的 NIC 設定
description: 本主題提供如何在 Windows Server 2016 datacenter 設定匯集 NIC 指示。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: f01546f8-c495-4055-8492-8806eee99862
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1ac6c2915301b1cf64486f24c197ebbf22bb5e2e
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="converged-nic-teamed-nic-configuration"></a>聚合型的 NIC 小組的 NIC 設定

>適用於：Windows Server（以每年次管道）、Windows Server 2016

下列章節提供部署匯集 NIC 聯合 NIC 設定切換 Embedded 小組 \(SET\) 使用中的指示。 本指南範例設定描述兩個 HYPER-V 主機、HYPER-V 主機 1 和 HYPER-V 主機 2。

## <a name="test-connectivity-between-source-and-destination"></a>測試連接之間來源和目的地資訊

本節測試連接之間來源和目的地 HYPER-V 主機時所需的步驟。

下圖描述兩個 HYPER-V 主機，**HYPER-V 主機 1**和**HYPER-V 主機 2**。 

這兩個 HYPER-V 主機有兩個網路介面卡。 每個主機上, 一個介面卡連接到 192.168.1.x/24 子網路，並 192.168.2.x/24 子網路介面卡連接。

![HYPER-V 主機](../../media/Converged-NIC/1-datacenter-test-conn.jpg)

### <a name="test-nic-connectivity-to-the-hyper-v-virtual-switch"></a>測試 NIC 連接至 HYPER-V Virtual 開關切換至

您可以使用此步驟，來確保實體而的您稍後將會建立 HYPER-V Virtual 參數，可連接目的主機。 

這項測試層級 3 \(L3\)-或 IP 層級-以及層級 2 \(L2\) 區域網路使用示範連接 \(VLAN\)。

您可以使用下列 Windows PowerShell 命令，以取得的網路介面卡的屬性。

    Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
     
以下是此命令的範例結果。

|名稱|InterfaceDescription|ifIndex|狀態|MacAddress|LinkSpeed|
|-----|--------------------|-------|-----|----------|---------|
|
|--1 測試 40 G|Mellanox ConnectX-3 Pro 乙太網路卡|11|向上|E4-1D-2D-07-43-D0|40 Gbps|

您可以使用其中一項下列命令，以取得額外的介面卡屬性，包括的 IP 位址。

    Get-NetIPAddress -InterfaceAlias "Test-40G-1"

    Get-NetIPAddress -InterfaceAlias "TEST-40G-1" | Where-Object {$_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress
    
以下是範例結果的命令。

|參數|值。|
|---------|-----|
|
|IPAddress| 192.168.1.3|
|InterfaceIndex|11|
|InterfaceAlias|--1 測試 40 G|
|AddressFamily|IPv4|
|輸入| 單|
|PrefixLength|24|

### <a name="verify-that-nic-team-member-has-a-valid-ip-address"></a>請確認 NIC 小組成員具有有效的 IP 位址

您可以使用此步驟，以確認其他 NIC 團隊或 \(SET\) 切換 Embedded 小組成員實體 Nic \(pNICs\) 也有有效的 IP 位址。

針對此步驟，您可以使用不同的子網路，\ (xxx.xxx。**2**.xxx 與 xxx.xxx。**1**。xxx\)，以幫助傳送此介面卡的目的地。 

或者，如果您在相同的子網路中找到這兩個 pNICs，Windows TCP/IP 堆疊負載平衡介面之間和簡單驗證變得更複雜。

您可以使用下列的 Windows PowerShell 命令，以取得的第二個網路介面卡的屬性。

    Get-NetAdapter -Name "Test-40G-2" | ft -AutoSize

以下是此命令的範例結果。

|名稱 |InterfaceDescription |ifIndex |狀態 |MacAddress |LinkSpeed|
|----|--------------------|-------|------|----------|---------|
|
|測試 40 G 2 |Mellanox ConnectX-3 Pro 乙太網路...\ #2 |13 |向上 |E4-1D-2D-07-40-70 |40 Gbps|

您可以使用其中一項下列命令，以取得額外的介面卡屬性，包括的 IP 位址。

    Get-NetIPAddress -InterfaceAlias "Test-40G-2"

    Get-NetIPAddress -InterfaceAlias "Test-40G-2" | Where-Object {$\_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress

以下是範例結果的命令。

|參數|值。|
|---------|-----|
|
|IPAddress|192.168.2.3|
|InterfaceIndex|13|
|InterfaceAlias|測試 40 G 2|
|AddressFamily|IPv4|
|輸入|單|
|PrefixLength|24|

### <a name="verify-that-additional-nics-have-valid-ip-addresses"></a>請確認其他 Nic 有有效的 IP 位址

您可以使用此步驟，以確保其他而具有有效的 IP 位址。

針對此步驟，您可以使用不同的子網路，\ (xxx.xxx。**2**.xxx 與 xxx.xxx。**1**。xxx\)，以幫助傳送此介面卡的目的地。 

或者，如果您在相同的子網路中找到這兩個 pNICs，Windows TCP/IP 堆疊負載平衡介面之間和簡單驗證變得更複雜。

您可以使用下列的 Windows PowerShell 命令，以取得的第二個網路介面卡的屬性。

    Get-NetAdapter -Name "Test-40G-2" | ft -AutoSize

以下是此命令的範例結果。

|名稱 |InterfaceDescription |ifIndex |狀態 |MacAddress |LinkSpeed|
|----|--------------------|-------|------|----------|---------|
|
|測試 40 G 2 |Mellanox ConnectX-3 Pro 乙太網路...\ #2 |13 |向上 |E4-1D-2D-07-40-70 |40 Gbps|

您可以使用其中一項下列命令，以取得額外的介面卡屬性，包括的 IP 位址。
    
    Get-NetIPAddress -InterfaceAlias "Test-40G-2"

    Get-NetIPAddress -InterfaceAlias "Test-40G-2" | Where-Object {$_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress

以下是範例結果的命令。

|參數|值。|
|---------|-----|
|
|IPAddress|192.168.2.3|
|InterfaceIndex|13|
|InterfaceAlias|測試 40 G 2|
|AddressFamily|IPv4|
|輸入|單|
|PrefixLength|24|

### <a name="ensure-that-source-and-destination-can-communicate"></a>確保能通訊來源和目的地資訊

您可以使用此步驟以確認雙向通訊 \ (ping 來源目的地和這兩個 systems\ 上反之亦然)。  下列範例中，**測試-NetConnection**使用 Windows PowerShell 命令時，但如果您想要您可以使用**ping**命令。 

    Test-NetConnection 192.168.1.5

以下是此命令的範例結果。

|參數|值。|
|---------|-----|
|
|電腦名稱|192.168.1.5|
|RemoteAddress|192.168.1.5|
|InterfaceAlias|--1 測試 40 G|
|SourceAddress|192.168.1.3|
|PingSucceeded|False|
|PingReplyDetails \(RTT\)|0 ms|

有時候，您可能需要使用進階安全性順利執行這項測試 Windows 防火牆停用。 如果防火牆停用，記住安全性，請確定您的設定符合您組織的安全性的需求。

下列範例命令可讓您要停用所有防火牆設定檔。

    Set-NetFirewallProfile -All -Enabled False
    
停用防火牆之後，您可以使用下列命令以測試連接。

    Test-NetConnection 192.168.1.5

以下是此命令的範例結果。

|參數|值。|
|---------|-----|
|
|電腦名稱|192.168.1.5|
|RemoteAddress|192.168.1.5|
|InterfaceAlias|--1 測試 40 G|
|SourceAddress|192.168.1.3|
|PingSucceeded|False|
|PingReplyDetails \(RTT\)|0 ms|

### <a name="verify-connectivity-for-additional-nics"></a>連接其他 Nic 驗證

使用此步驟，您可以在所有後續 pNICs NIC 或一組小組中所包含的重複的上一個步驟。

您可以使用下列命令來測試連接。
    
    Test-NetConnection 192.168.2.5

以下是此命令的範例結果。

|參數|值。|
|---------|-----|
|
|電腦名稱|192.168.2.5|
|RemoteAddress|192.168.2.5|
|InterfaceAlias|測試 40 G 2|
|SourceAddress|192.168.2.3|
|PingSucceeded|False|
|PingReplyDetails \(RTT\)|0 ms|


## <a name="configure-vlans"></a>設定 Vlan

針對此步驟，Nic 可在**存取**模式。 但是當您在本文稍後建立 HYPER-V Virtual 切換 \(vSwitch\)，VLAN 屬性會套用到 vSwitch 連接埠層級。 

切換可以主機多個 Vlan，因為它是所需的最上方的架 \(ToR\) 實體切換，將其設定主幹模式中的連接埠。

如需如何設定主幹模式開關切換至上的指示，請洽詢您 ToR 切換文件。

下圖描述具有兩個每個的 VLAN 101 網路介面卡的兩個 HYPER-V 主機，而且 VLAN 102 設定中網路介面卡屬性。

![設定的區域網路](../../media/Converged-NIC/2-datacenter-configure-vlans.jpg)

### <a name="configure-the-vlan-ids-for-both-nics"></a>這兩個 Nic 設定的 VLAN Id

您可以使用此步驟來設定 VLAN Id Nic 安裝在您的 HYPER-V 主機。

協會，網路標準電子工程師 \(IEEE\) 根據實體的服務品質 \(QoS\) 屬性 NIC 處理嵌入 802.1Q 802.1 p 標頭 \(VLAN\) 標頭，當您設定的 VLAN id。

#### <a name="configure-nic-test-40g-1"></a>設定 NIC 測試 40 G 1

使用下列命令，設定 VLAN ID 的第一個 NIC、測試 40 G 1，然後檢視 [顯示設定。

    
    Set-NetAdapterAdvancedProperty -Name "Test-40G-1" -RegistryKeyword VlanID -RegistryValue "101"
    Get-NetAdapterAdvancedProperty -Name "Test-40G-1" | Where-Object {$_.RegistryKeyword -eq "VlanID"} | ft -AutoSize
    
以下是此命令的範例結果。

|名稱 |顯示名稱| DisplayValue| RegistryKeyword |RegistryValue|
|----|-----------|------------|---------------|-------------|
|
|--1 測試 40 G|VLAN ID|101|VlanID|{101}|


請確定該 VLAN ID 生效的網路介面卡實作獨立使用下列命令重新開機網路介面卡。

    Restart-NetAdapter -Name "Test-40G-1"

您可以使用下列命令，以確保您的網路介面卡狀態的是**上**再繼續。

    Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize

以下是此命令的範例結果。

|名稱|InterfaceDescription|ifIndex| 狀態|MacAddress|LinkSpeed|
|----|--------------------|-------|------|----------| ---------|
|
|--1 測試 40 G|Mellanox ConnectX-3 Pro 乙太網路草原...|11|向上|E4-1D-2D-07-43-D0|40 Gbps|

#### <a name="configure-nic-test-40g-2"></a>設定 NIC 測試 40 G 2

使用下列命令，設定 VLAN ID 的第二個 NIC、測試 40 G 2，然後檢視 [顯示設定。

    
    Set-NetAdapterAdvancedProperty -Name "Test-40G-2" -RegistryKeyword VlanID -RegistryValue "102"
    Get-NetAdapterAdvancedProperty -Name "Test-40G-2" | Where-Object {$_.RegistryKeyword -eq "VlanID"} | ft -AutoSize
    

以下是此命令的範例結果。

|名稱 |顯示名稱| DisplayValue| RegistryKeyword |RegistryValue|
|----|-----------|------------|---------------|-------------|
|
|測試 40 G 2|VLAN ID|102|VlanID|{102}|

請確定該 VLAN ID 生效的網路介面卡實作獨立使用下列命令重新開機網路介面卡。

`Restart-NetAdapter -Name "Test-40G-2"  `              

您可以使用下列命令，以確保您的網路介面卡狀態的是**上**再繼續。

    Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize

以下是此命令的範例結果。

|名稱|InterfaceDescription|ifIndex| 狀態|MacAddress|LinkSpeed|
|----|--------------------|-------|------|----------| ---------|
|
|測試 40 G 2 |Mellanox ConnectX-3 Pro 乙太網路草原... |11 |向上 |E4-1D-2D-07-43-D1 |40 Gbps|


### <a name="verify-connectivity"></a>請確認連接

您可以使用本節之後重新啟動網路介面卡，請確認連接。 您可以確認連接之後申請這兩個介面卡的 VLAN 標記。 如果連接失敗，您可以檢查 VLAN 設定或目的參與相同 VLAN 切換。 

>[!IMPORTANT]
>您上一節中執行步驟後，可能需要幾秒鐘的裝置重新開機，並在網路上推出。

#### <a name="verify-connectivity-for-nic-test-40g-1"></a>請確認連接的 NIC 測試-40 G-1

請確認連接的第一個而，您可以執行下列命令。

    Test-NetConnection 192.168.1.5
    
以下是此命令的範例結果。

|參數|值。|
|---------|-----|
|
|電腦名稱|192.168.1.5|
|RemoteAddress|192.168.1.5|
|InterfaceAlias|--1 測試 40 G|
|SourceAddress|192.168.1.5|
|PingSucceeded|為 true|
|PingReplyDetails \(RTT\)|0 ms|

#### <a name="verify-connectivity-for-nic-test-40g-2"></a>請確認連接的 NIC 測試-40 G-2

您可以使用本節適用於 NIC 測試-40 G-2 測試連接。

您可以使用下列命令的第二個 NIC 測試連接
    
    Test-NetConnection 192.168.2.5
    
以下是此命令的範例結果。

|參數|值。|
|---------|-----|
|
|電腦名稱|192.168.2.5|
|RemoteAddress|192.168.2.5|
|InterfaceAlias|測試 40 G 2|
|SourceAddress|192.168.2.3|
|PingSucceeded|為 true|
|PingReplyDetails \(RTT\)|0 ms|

A**測試-NetConnection**失敗或 ping 失敗，您執行之後，就會發生**重新開機-NetAdapter**並不常用，因此等網路介面卡完全初始化，然後再試一次。

如果 VLAN 101 連接運作，但無法運作的 VLAN 102 連接，可能是開關切換至該需要設定為在您想要的 VLAN 上允許連接埠流量的問題。 您可以檢查此暫時 VLAN 101，來設定失敗介面卡及重複連接測試。

## <a name="configure-quality-of-service-qos"></a>設定服務 \(QoS\) 品質

下一個步驟是設定服務品質 \(QoS\)，需要先安裝 Windows Server 2016 功能資料中心橋接 \(DCB\)。

下圖成功設定中的上一個步驟的 Vlan 後描述您的 HYPER-V 主機。

![設定品質服務](../../media/Converged-NIC/3-datacenter-configure-qos.jpg)

### <a name="install-data-center-bridging-dcb"></a>安裝資料中心橋接 \(DCB\)

您可以使用此步驟，安裝以及 DCB。 在大部分案例中，這個步驟是選擇性的 iWarp 實作，但必須 fabric 縮放，例如跨-架案例。

您可以在每個您的 HYPER-V 主機上安裝 DCB 使用下列命令。 

    Install-WindowsFeature Data-Center-Bridging

以下是此命令的範例結果。

|成功 |需要重新開機 |結束代碼|功能結果|
|------- |-------------- |--------- |-------------- |
|
|為 true |否] |成功| {資料中心橋接}|

### <a name="set-the-qos-policies-for-smb-direct"></a>設定 SMB 直接 QoS 原則 

您可以使用下列命令來設定 SMB 直接 QoS 原則。

    New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3

以下是此命令的範例結果。

|參數|值。|
|---------|-----|
|
|名稱 |SMB|
|擁有者|群組原則 \(Machine\)|
|NetworkProfile|所有|
|優先順序|127|
|JobObject|&nbsp;| 
|NetDirectPort|445
|PriorityValue|3

### <a name="set-policies-for-other-traffic-on-the-interface"></a>設定介面的其他資料傳輸原則 

您可以使用下列命令，將 QoS 額外的原則。

    New-NetQosPolicy "DEFAULT" -Default -PriorityValue8021Action 0
 
以下是此命令的範例結果。

|參數|值。|
|---------|-----|
|
|名稱 | 預設值|
|擁有者|群組原則 \(Machine\)|
|NetworkProfile|所有|
|優先順序|127|
|範本| 預設值|
|JobObject| &nbsp;|
|PriorityValue|0|

### <a name="turn-on-flow-control-for-smb"></a>打開 smb 流程控制項 

您可以使用下列命令，讓 SMB 流程控制項以及檢視結果。

    Enable-NetQosFlowControl -priority 3
    Get-NetQosFlowControl

以下是此命令的範例結果。

|高優先順序|支援|PolicySet|IfIndex|IfAlias|
|---------|-----|--------- |-------| -------|
|
|0 |False |全球|&nbsp;|&nbsp;|
|1 |False |全球|&nbsp;|&nbsp;|
|2 |False |全球|&nbsp;|&nbsp;|
|3 |為 true |全球|&nbsp;|&nbsp;|
|4 |False |全球|&nbsp;|&nbsp;|
|5 |False |全球|&nbsp;|&nbsp;|
|6 |False |全球|&nbsp;|&nbsp;|
|7 |False |全球|&nbsp;|&nbsp;|

如果您的結果不符這些結果因為 3 以外的項目 true Enabled 值，您必須執行下一個步驟。 如果結果相符這些範例結果唯一的項目 3 有 Enabled 的值為 true，不需要執行下一個步驟，並可以跳到下**讓 QoS 目標和目的地資訊的網路介面卡的**。

### <a name="ensure-flow-control-is-disabled-for-other-traffic-classes-optional"></a>確定已停用其他流量類別 \(Optional\) 流量控制

您不需要執行此步驟，如果**FlowControl**無法進行流量類別 0,1,2,4,5,6 和 7。

如果**FlowControl**已的任何資料傳輸類別 3 以外 \（0,1,2,4,5,6，並 7\），您必須停用**FlowControl**這些類別。 

>[!NOTE]
>在複雜的設定，其他流量類別可能需要流程控制項，不過在這些案例中已超出範圍本指南。

您可以使用下列命令，來停用流量類別 0,1,2,4,5,6，以及 7 SMB 流程控制項並檢視結果。

    Disable-NetQosFlowControl -priority 0,1,2,4,5,6,7
    Get-NetQosFlowControl

以下是此命令的範例結果。

|高優先順序|支援|PolicySet|IfIndex|IfAlias|
|---------|-----|--------- |-------| -------|
|
|0 |False |全球|&nbsp;|&nbsp;|
|1 |False |全球|&nbsp;|&nbsp;|
|2 |False |全球|&nbsp;|&nbsp;|
|3 |為 true |全球|&nbsp;|&nbsp;|
|4 |False |全球|&nbsp;|&nbsp;|
|5 |False |全球|&nbsp;|&nbsp;|
|6 |False |全球|&nbsp;|&nbsp;|
|7 |False |全球|&nbsp;|&nbsp;|

### <a name="enable-qos-for-the-local-and-destination-network-adapters"></a>讓 QoS 本機和目的地的網路介面卡 

這個步驟中，您可以讓 QoS 本機和目的地的網路介面卡。 請確定您執行下列命令，這兩個您的 HYPER-V 主機上。

#### <a name="enable-qos-for-nic-test-40g-1"></a>讓 QoS NIC 測試 40 G 1

您可以使用下列命令，讓 QoS 並檢視您的設定的結果。

    Enable-NetAdapterQos -InterfaceAlias "Test-40G-1"
    Get-NetAdapterQos -Name "Test-40G-1"

以下是此命令的範例結果。

**名稱**：測試-40 G-1**支援**: True**功能**:   

|參數|硬體|目前|
|---------|--------|-------|
|
|MacSecBypass|NotSupported|NotSupported|
|DcbxSupport|無|無|
|NumTCs(Max/ETS/PFC)|8/8/8|8/8/8|
 
**OperationalTrafficClasses**: 

|TC|TSA|頻寬|優先順序|
|----|-----|--------|-------|
|
|0| [嚴格]|&nbsp;|0-7|

**OperationalFlowControl**：優先順序 3 功能的**OperationalClassifications**:

|通訊協定|連接埠日類型|高優先順序|
|--------|---------|--------|
|
|預設值|&nbsp;|0|
|NetDirect| 445|3|

#### <a name="enable-qos-for-nic-test-40g-2"></a>讓 QoS NIC 測試 40 G 2

您可以使用下列命令，讓 QoS 並檢視您的設定的結果。

    Enable-NetAdapterQos -InterfaceAlias "Test-40G-2"
    Get-NetAdapterQos -Name "Test-40G-2"

 
以下是此命令的範例結果。

**名稱**：測試 40 G 2**支援**: True**功能**:   

|參數|硬體|目前|
|---------|--------|-------|
|
|MacSecBypass|NotSupported|NotSupported|
|DcbxSupport|無|無|
|NumTCs(Max/ETS/PFC)|8/8/8|8/8/8|
 
**OperationalTrafficClasses**: 

|TC|TSA|頻寬|優先順序|
|----|-----|--------|-------|
|
|0| [嚴格]|&nbsp;|0-7|

**OperationalFlowControl**：優先順序 3 功能的**OperationalClassifications**:

|通訊協定|連接埠日類型|高優先順序|
|--------|---------|--------|
|
|預設值|&nbsp;|0|
|NetDirect| 445|3|


### <a name="allocate-50-of-the-bandwidth-reservation-to-smb-direct-rdma"></a>配置 SMB 直接 \(RDMA\) 50%的頻寬保留的項目

您可以使用下列命令一半頻寬預定的指派給 SMB 直接存取。

    New-NetQosTrafficClass "SMB" -priority 3 -bandwidthpercentage 50 -algorithm ETS

以下是此命令的範例結果。

|名稱|演算法 |Bandwidth(%)| 高優先順序 |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|SMB | ETS     | 50 |3 |全球 |&nbsp;|&nbsp;|                                      
 
您可以使用下列命令，以檢視頻寬保留的資訊。

    Get-NetQosTrafficClass | ft -AutoSize

以下是此命令的範例結果。
 
|名稱|演算法 |Bandwidth(%)| 高優先順序 |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|[預設值]| ETS|50 |0-2,4-7|  全球|&nbsp;|&nbsp;| 
SMB |ETS|50 |3 |全球|&nbsp;|&nbsp;| 

### <a name="create-traffic-classes-for-tenant-ip-traffic-optional"></a>建立流量類別承租人 IP 傳輸 \(optional\)

您可以使用此步驟來建立兩個額外的流量類別承租人 IP 傳輸。 

當您執行以下的範例命令時，如果您想要您可以略過的「IP1」和「IP2「值。

    New-NetQosTrafficClass "IP1" -Priority 1 -bandwidthpercentage 10 -algorithm ETS

以下是此命令的範例結果。

|名稱|演算法 |Bandwidth(%)| 高優先順序 |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|IP1 |ETS |10 |1 |全球|&nbsp;|&nbsp;|


    New-NetQosTrafficClass "IP2" -Priority 2 -bandwidthpercentage 10 -algorithm ETS

以下是此命令的範例結果。

|名稱|演算法 |Bandwidth(%)| 高優先順序 |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|IP2 |ETS |10 |2 |全球|&nbsp;|&nbsp;|

您可以使用下列命令，以檢視 QoS 流量類別。

    Get-NetQosTrafficClass | ft -AutoSize

以下是此命令的範例結果。

|名稱|演算法 |Bandwidth(%)| 高優先順序 |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|[預設值] |ETS |30 |0,4-7 |全球|&nbsp;|&nbsp;|
|SMB |ETS |50 |3 |全球|&nbsp;|&nbsp;|
|IP1 |ETS |10 |1 |全球|&nbsp;|&nbsp;|
|IP2 |ETS |10 |2 |全球|&nbsp;|&nbsp;|

## <a name="configure-debugger-optional"></a>設定 \(Optional\) 偵錯工具

您可以使用此步驟進行偵錯工具。

根據預設，附加偵錯工具封鎖 NetQos。 您可以使用下列命令覆寫偵錯工具，以及檢視結果。

    Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 –Force
    
    Get-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" | ft AllowFlowControlUnderDebugger

以下是此命令的範例結果。

    AllowFlowControlUnderDebugger
    -----------------------------
    1

## <a name="test-rdma-mode-1"></a>測試 RDMA \(Mode 1\)

您可以使用此步驟，以確保建立 vSwitch 和轉換為 RDMA \(Mode 2\) 之前 fabric 已正確設定。

下圖描述目前 HYPER-V 主機的狀態。

![測試 RDMA](../../media/Converged-NIC/4-datacenter-test-rdma.jpg)

若要確認 RDMA 設定，您可以執行下列命令。

    Get-NetAdapterRdma | ft -AutoSize

以下是此命令的範例結果。

|名稱 |InterfaceDescription |支援|
|----|--------------------|-------|
|
|--1 測試 40 G| VPI Mellanox ConnectX-4 介面卡 #2 |為 true|
|測試 40 G 2| VPI Mellanox ConnectX-4 介面卡 |為 true|


### <a name="determine-the-ifindex-value-of-your-target-adapter"></a>判斷您的目標介面卡的 ifIndex 值

您可以使用下列命令來探索 ifIndex 目標介面卡的值。

    Get-NetIPConfiguration -InterfaceAlias "TEST*" | ft InterfaceAlias,InterfaceIndex,IPv4Address

以下是此命令的範例結果。

|InterfaceAlias |InterfaceIndex |IPv4Address|
|-------------- |-------------- |-----------|
|--1 測試 40 G |14 |{192.168.1.3}|
|測試 40 G 2 | 13 |{192.168.2.3}|

### <a name="download-diskspdexe-and-a-powershell-script"></a>下載 DiskSpd.exe 和 PowerShell 指令碼

若要繼續時，您必須先下載下列項目。

- 下載 DiskSpd.exe 公用程式，並擷取到 C:\TEST\ 公用程式[http://tinyurl.com/z68h3rc](http://tinyurl.com/z68h3rc)

- 若要 C:\TEST\ 下載 Test-RDMA powershell 指令碼 [https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)

### <a name="run-the-powershell-script"></a>執行 PowerShell 指令碼

當您執行的 Windows PowerShell 指令碼 Test-Rdma.ps1 時，您可以傳送 ifIndex 值指令碼，以及遠端相同的 VLAN 上介面卡的 IP 位址。

您可以使用下列命令範例為 14 ifIndex 指令碼執行的網路介面卡 192.168.1.5。
    
    C:\TEST\Test-RDMA.PS1 -IfIndex 14 -IsRoCE $true -RemoteIpAddress 192.168.1.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
    VERBOSE: The adapter TEST-40G-1 is a physical adapter
    VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
    VERBOSE: QoS/DCB/PFC configuration is correct.
    VERBOSE: RDMA configuration is correct.
    VERBOSE: Checking if remote IP address, 192.168.1.5, is reachable.
    VERBOSE: Remote IP 192.168.1.5 is reachable.
    VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
    VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 0 RDMA bytes sent per second
    VERBOSE: 662979201 RDMA bytes written per second
    VERBOSE: 37561021 RDMA bytes sent per second
    VERBOSE: 1023098948 RDMA bytes written per second
    VERBOSE: 8901349 RDMA bytes sent per second
    VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
    VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.1.5
    

>[!NOTE]
>如果 RDMA 流量失敗，RoCE 案例具體而言，洽詢您 ToR 切換設定適當的必須符合的主機設定 PFC 日 ETS 設定。 請參考 QoS 參考值本文件。

### <a name="determine-the-ifindex-value-of-your-target-adapter"></a>判斷您的目標介面卡的 ifIndex 值

您可以使用下列命令來探索 ifIndex 目標介面卡的值。

    Get-NetIPConfiguration -InterfaceAlias "TEST*" | ft InterfaceAlias,InterfaceIndex,IPv4Address

以下是此命令的範例結果。

|InterfaceAlias |InterfaceIndex |IPv4Address|
|-------------- |-------------- |-----------|
|--1 測試 40 G |14 |{192.168.1.3}|
|測試 40 G 2 | 13 |{192.168.2.3}|

您可以使用下列命令範例 ifIndex 13 的指令碼執行的網路介面卡 192.168.2.5。

    
    C:\TEST\Test-RDMA.PS1 -IfIndex 13 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
    VERBOSE: The adapter TEST-40G-2 is a physical adapter
    VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
    VERBOSE: QoS/DCB/PFC configuration is correct.
    VERBOSE: RDMA configuration is correct.
    VERBOSE: Checking if remote IP address, 192.168.2.5, is reachable.
    VERBOSE: Remote IP 192.168.2.5 is reachable.
    VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
    VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 0 RDMA bytes sent per second
    VERBOSE: 541185606 RDMA bytes written per second
    VERBOSE: 34821478 RDMA bytes sent per second
    VERBOSE: 954717307 RDMA bytes written per second
    VERBOSE: 35040816 RDMA bytes sent per second
    VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
    VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.2.5
    

## <a name="create-a-hyper-v-virtual-switch"></a>建立 HYPER-V Virtual 開關切換至

建立 HYPER-V Virtual 切換 \(vSwitch\) 在您的 HYPER-V 主機上，您可以使用此一節。

下圖與 vSwitch 描述 HYPER-V 主機 1。

![建立 HYPER-V Virtual 開關切換至](../../media/Converged-NIC/5-datacenter-create-vswitch.jpg)

### <a name="create-a-vswitch-in-switch-embedded-teaming-set-mode"></a>建立 vSwitch 切換 Embedded 小組 \(SET\) 模式

您可以使用下列範例命令來建立將開關切換至獨立團隊名為**VMSTEST** HYPER-V 主機 1。 這兩個測試-40 G 1 及測試 40 G 2，此電腦上的網路介面卡] 會新增至設定小組這個命令。
    
    New-VMSwitch –Name "VMSTEST" –NetAdapterName "TEST-40G-1","TEST-40G-2" -EnableEmbeddedTeaming $true -AllowManagementOS $true
    
以下是此命令的範例結果。

|名稱 |SwitchType |NetAdapterInterfaceDescription|
|---- |---------- |------------------------------|
|VMSTEST |外部 |聯合介面|

您可以使用此命令檢視實體介面卡小組中設定。

    Get-VMSwitchTeam -Name "VMSTEST" | fl

以下是此命令的範例結果。

**名稱**: VMSTEST **Id**: ad9bb542-dda2-4450-a00e-f96d44bdfbec **NetAdapterInterfaceDescription**: {Mellanox ConnectX-3 Pro 乙太網路卡、Mellanox ConnectX-3 Pro 乙太網路卡 #2} **TeamingMode**: SwitchIndependent **LoadBalancingAlgorithm**：動態

#### <a name="display-two-views-of-the-host-vnic"></a>顯示兩個主機但 vNIC 檢視

您可以使用下列兩個命令主機但 vNIC 的兩個不同的檢視。

若要顯示的主機但 vNIC 屬性，您可以使用這個命令。

    Get-NetAdapter

以下是此命令的範例結果。

|名稱 |InterfaceDescription | ifIndex |狀態 |MacAddress |LinkSpeed | |---|---|---|---|---| | | vEthernet (VMSTEST) |Hyper-VVirtual 乙太網路卡 #2 | 28 |向上 |E4-1D-2D-07-40-71|80 Gbps |

您可以使用此命令來顯示其他主機但 vNIC 屬性。

    Get-VMNetworkAdapter -ManagementOS

以下是此命令的範例結果。

|名稱 |IsManagementOs |VMName |SwitchName |MacAddress |狀態 |IPAddresses|
|----|--------------|------|----------|----------|------|-----------|
|
|VMSTEST|為 true |VMSTEST |E41D2D074071| {確定}|&nbsp;|

#### <a name="test-the-network-connection-to-the-remote-vlan-101-adapter"></a>網路連接到遠端的 VLAN 101 介面卡的測試

您可以使用此命令來連接遠端 VLAN 101 介面卡的測試。

`Test-NetConnection 192.168.1.5` 

以下是此命令的範例結果。

    WARNING: Ping to 192.168.1.5 failed -- Status: DestinationHostUnreachable
    
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (CORP-External-Switch)
    SourceAddress  : 10.199.48.170
    PingSucceeded  : False
    PingReplyDetails (RTT) : 0 ms
    
### <a name="reconfigure-vlans"></a>重新 Vlan 設定

若要移除實體而 VLAN 存取設定，並設定使用 vSwitch VLANID，您可以使用此步驟。

您必須移除 VLAN 存取設定，以防止這兩個自動-標記輸出流量不正確的 VLAN ID 使用與從篩選輸入流量的不符存取 VLAN 收到

若要移除存取 VLAN 設定，您可以使用下列命令。
    
    Set-NetAdapterAdvancedProperty -Name "Test-40G-1" -RegistryKeyword VlanID -RegistryValue "0"
    Set-NetAdapterAdvancedProperty -Name "Test-40G-2" -RegistryKeyword VlanID -RegistryValue "0"
    

您可以使用下列命令，來設定使用 vSwitch 特定 Windows PowerShell 命令 VLANID 並檢視這個動作的結果。

    
    Set-VMNetworkAdapterVlan -VMNetworkAdapterName "VMSTEST" -VlanId "101" -Access -ManagementOS
    Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName "VMSTEST"
    

以下是此命令的範例結果。

VMName VMNetworkAdapterName 模式 VlanList
------ -------------------- ----   --------
       VMSTEST              Access 101     

您可以使用下列命令測試網路連接到檢視結果。

    Test-NetConnection 192.168.1.5
    
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (VMSTEST)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    

如果您的結果不類似的範例結果且抓取失敗訊息」警告：無法 Ping 192.168.1.5-狀態：DestinationHostUnreachable，「確認 [vEthernet (VMSTEST)」適當的 IP 位址，執行下列命令。

    
    Get-NetIPAddress -InterfaceAlias "vEthernet (VMSTEST)"
    

如果未的 IP 位址設定，您可以使用下列命令以修正此問題，並檢視您的動作的結果。

    
    New-NetIPAddress -InterfaceAlias "vEthernet (VMSTEST)" -IPAddress 192.168.1.3 -PrefixLength 24
    
    IPAddress : 192.168.1.3
    InterfaceIndex: 37
    InterfaceAlias: vEthernet (VMSTEST)
    AddressFamily : IPv4
    Type  : Unicast
    PrefixLength  : 24
    PrefixOrigin  : Manual
    SuffixOrigin  : Manual
    AddressState  : Tentative
    ValidLifetime : Infinite ([TimeSpan]::MaxValue)
    PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
    SkipAsSource  : False
    PolicyStore   : ActiveStore
    
#### <a name="rename-the-management-nic"></a>重新命名管理 NIC

您可以使用此區段重新命名管理而，並 RDMA 之後使用不同的主機但 vNIC 執行個體。

您可以執行下列命令，而管理重新命名，並檢視某些 NIC 屬性。

    Rename-VMNetworkAdapter -ManagementOS -Name “VMSTEST” -NewName “MGT”
    Get-VMNetworkAdapter -ManagementOS

以下是此命令的範例結果。

|名稱 |IsManagementOs |VMName |SwitchName |MacAddress |狀態 |IPAddresses
|----|--------------|------|----------|----------|------|-----------|
|
|CORP-外部開關切換至 |為 true |&nbsp;|CORP-外部開關切換至 |001B785768AA |{確定}|&nbsp;|
|MGT |為 true |&nbsp;|VMSTEST |E41D2D074071 |{確定}|&nbsp;|

您可以執行下列命令，可檢視某些其他 NIC 屬性。

    Get-NetAdapter

以下是此命令的範例結果。

|名稱 |InterfaceDescription |ifIndex |狀態 |MacAddress |LinkSpeed|
|----|--------------------|------|----------|---------|------|
|
|vEthernet (MGT) |HYPER-V Virtual 乙太網路卡 #2 |28 |向上 | E4-1D-2D-07-40-71 |80 Gbps|

## <a name="test-hyper-v-virtual-switch-rdma"></a>測試 HYPER-V Virtual 切換 RDMA

下圖描述您的 HYPER-V 主機，包括 HYPER-V 主機 1 vSwitch 目前狀態。

![測試 HYPER-V Virtual 開關切換至](../../media/Converged-NIC/6-datacenter-test-vswitch-rdma.jpg)

### <a name="set-priority-tagging-on-the-host-vnic-to-complement-the-previous-vlan-settings"></a>設定優先順序主機但 vNIC 上標記補充先前的 VLAN 設定

您可以使用下列命令範例設定優先順序主機但 vNIC 上標記，並檢視您的動作的結果。
    
    Set-VMNetworkAdapter -ManagementOS -Name "MGT" -IeeePriorityTag on
    Get-VMNetworkAdapter -ManagementOS -Name "MGT" | fl Name,IeeePriorityTag
    

    Name: MGT
    IeeePriorityTag : On
    
### <a name="create-two-host-vnics-for-rdma"></a>有兩個主機 vNICs 建立 RDMA

您可以使用下列命令範例建立 RDMA 兩個主機 vNICs 和連接 vSwitch VMSTEST 它們。
    
    Add-VMNetworkAdapter –SwitchName "VMSTEST" –Name SMB1 –ManagementOS
    Add-VMNetworkAdapter –SwitchName "VMSTEST" –Name SMB2 –ManagementOS
    
若要檢視管理 NIC 屬性，您可以使用下列命令範例。
    
    Get-VMNetworkAdapter -ManagementOS
    
以下是此命令的範例結果。

|名稱 |IsManagementOs |VMName |SwitchName |MacAddress |狀態 |IPAddresses|
|----|--------------|------|----------|----------|------|-----------|
|
|CORP-外部開關切換至 |為 true |CORP-外部開關切換至 |001B785768AA|{確定} |&nbsp;| 
|Mgt |為 true |VMSTEST |E41D2D074071 |{確定} |&nbsp;|
|SMB1 |為 true |VMSTEST |00155D30AA00 |{確定} |&nbsp;|
|SMB2 |為 true |VMSTEST |00155D30AA01 |{確定} |&nbsp;|

### <a name="assign-an-ip-address-to-the-smb-host-vnics"></a>指定 SMB 主機 vNICs IP 位址

您可以使用這個區段 IP addressrd 給 SMB 主機 vNICs vEthernet \(SMB1\) 和 vEthernet \(SMB2\)。

測試 40 G 2 實體卡與測試-40 G-1 仍然可以存取的 VLAN 101 與 102 設定。 因此，介面卡標記的流量-和 ping 成功。

之前，您會設定為零，這兩個 pNIC VLAN Id，然後為 VLAN 101 VMSTEST vSwitch。 在那之後，您仍然可以使用 MGT 但 vNIC ping 遠端 VLAN 101 介面卡，但目前有不 VLAN 102 成員。

您現在可以使用下列命令範例移除實體而防止這兩個自動-標記輸出資料傳輸與不正確的 VLAN ID 和防止篩選輸入流量的 VLAN 存取設定，不符合存取 VLAN id。

您可以使用下列命令範例完成這些目標加入新的 IP 位址介面 vEthernet (SMB1)，並檢視結果。

    
    New-NetIPAddress -InterfaceAlias "vEthernet (SMB1)" -IPAddress 192.168.2.111 -PrefixLength 24
    
    IPAddress : 192.168.2.111
    InterfaceIndex: 40
    InterfaceAlias: vEthernet (SMB1)
    AddressFamily : IPv4
    Type  : Unicast
    PrefixLength  : 24
    PrefixOrigin  : Manual
    SuffixOrigin  : Manual
    AddressState  : Invalid
    ValidLifetime : Infinite ([TimeSpan]::MaxValue)
    PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
    SkipAsSource  : False
    PolicyStore   : PersistentStore
    

您可以使用下列命令範例遠端 VLAN 102 介面卡的測試及檢視結果。
    
    Test-NetConnection 192.168.2.5 
    
    ComputerName   : 192.168.2.5
    RemoteAddress  : 192.168.2.5
    InterfaceAlias : vEthernet (SMB1)
    SourceAddress  : 192.168.2.111
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    
您可以使用下列命令範例新增新的 IP 位址的介面 vEthernet \(SMB2\)，以及檢視結果。
    
    New-NetIPAddress -InterfaceAlias "vEthernet (SMB2)" -IPAddress 192.168.2.222 -PrefixLength 24 
    
    IPAddress : 192.168.2.222
    InterfaceIndex: 44
    InterfaceAlias: vEthernet (SMB2)
    AddressFamily : IPv4
    Type  : Unicast
    PrefixLength  : 24
    PrefixOrigin  : Manual
    SuffixOrigin  : Manual
    AddressState  : Invalid
    ValidLifetime : Infinite ([TimeSpan]::MaxValue)
    PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
    SkipAsSource  : False
    PolicyStore   : PersistentStore
    
您不需要建立通訊因為測試連接再試一次。

### <a name="place-the-rdma-host-vnics-on-vlan-102"></a>RDMA 主機 vNICs 置於 VLAN 102

您可以使用本節 RDMA 主機 vNICs 給 VLAN 102。

因為「MGT「主機但 vNIC 位於 VLAN 101，您可以使用下列命令範例 RDMA 主機 vNICs 置於預先存在 VLAN 102 並檢視您的動作的結果。

    
    Set-VMNetworkAdapterVlan -VMNetworkAdapterName "SMB1" -VlanId "102" -Access -ManagementOS
    Set-VMNetworkAdapterVlan -VMNetworkAdapterName "SMB2" -VlanId "102" -Access -ManagementOS
    
    Get-VMNetworkAdapterVlan -ManagementOS
    
    VMName VMNetworkAdapterName Mode VlanList
    ------ -------------------- ---- --------
       SMB1 Access   102 
       Mgt  Access   101 
       SMB2 Access   102 
       CORP-External-Switch Untagged
    
### <a name="inspect-the-mapping-of-smb1-and-smb2-to-the-underlying-physical-nics"></a>檢查以基礎實體 Nic SMB2 與對應的 SMB1

您可以使用本節檢查 SMB1 和以在 vSwitch 設定小組基礎實體 Nic SMB2 對應。  實體 Nic 至主機但 vNIC 的關聯，而且隨機受重新建立和破壞期間。

在這個情況您可以使用間接機制來查看目前的關聯。

SMB1 及 SMB2 的 MAC 位址的相關聯的 NIC 小組成員測試 40 G 2。 因為測試-40 G-1 不需要相關聯的 SMB 主機但 vNIC，且將不會允許使用的 RDMA 流量連結 SMB 主機但 vNIC 對應，直到不理想。

若要查看這項資訊，您可以使用下列命令範例。

    
    Get-NetAdapterVPort (Preferred)
    
    Get-NetAdapterVmqQueue
    
    Name   QueueID MacAddressVlanID Processor VmFriendlyName
    ----   ------- ---------------- --------- --------------
    TEST-40G-1 1   E4-1D-2D-07-40-71 1010:17
    TEST-40G-2 1   00-15-5D-30-AA-00 1020:17
    TEST-40G-2 2   00-15-5D-30-AA-01 1020:17
    
    Get-VMNetworkAdapter -ManagementOS
    
    Name IsManagementOs VMName SwitchName   MacAddress   Status IPAddresses
    ---- -------------- ------ ----------   ----------   ------ -----------
    CORP-External-Switch True  CORP-External-Switch 001B785768AA {Ok}  
    Mgt  True  VMSTEST  E41D2D074071 {Ok}  
    SMB1 True  VMSTEST  00155D30AA00 {Ok}  
    SMB2 True  VMSTEST  00155D30AA01 {Ok}  
    

因為您未尚未執行對應下列兩個命令應該返回任何資訊。
    
    Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName SMB1
    Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName SMB2
    
您可以使用下列命令範例地圖 SMB1 和 SMB2 分開實體 NIC 小組的成員，並檢視您的動作的結果。

>[!IMPORTANT]
>確定您已完成此步驟進行之前，或將會失敗，您的實作。
    
    Set-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName "SMB1" -PhysicalNetAdapterName "Test-40G-1"
    Set-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName "SMB2" -PhysicalNetAdapterName "Test-40G-2"
    
    Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST
    
    NetAdapterName : Test-40G-1
    NetAdapterDeviceId : {BAA9A00F-A844-4740-AA93-6BD838F8CFBA}
    ParentAdapter  : VMInternalNetworkAdapter, Name = 'SMB1'
    IsTemplate : False
    CimSession : CimSession: .
    ComputerName   : 27-3145G0803
    IsDeleted  : False
    
    NetAdapterName : Test-40G-2
    NetAdapterDeviceId : {B7AB5BB3-8ACB-444B-8B7E-BC882935EBC8}
    ParentAdapter  : VMInternalNetworkAdapter, Name = 'SMB2'
    IsTemplate : False
    CimSession : CimSession: .
    ComputerName   : 27-3145G0803
    IsDeleted  : False
    
### <a name="confirm-the-mac-associations"></a>確認 MAC 關聯

您可以使用下列命令範例，以確認您先前建立的 MAC 關聯。
    
    Get-NetAdapterVmqQueue
    
    Name   QueueID MacAddressVlanID Processor VmFriendlyName
    ----   ------- ---------------- --------- --------------
    TEST-40G-1 1   E4-1D-2D-07-40-71 1010:17
    TEST-40G-1 2   00-15-5D-30-AA-00 1020:17
    TEST-40G-2 1   00-15-5D-30-AA-01 1020:17
    

因為這兩個主機 vNICs 上相同的子網路並具有相同的 VLAN ID \(102\)，您就可以驗證通訊從遠端系統中，並檢視您的動作的結果。

>[!IMPORTANT]
>在遠端電腦上，執行下列命令。

    
    Test-NetConnection 192.168.2.111
    
    
    ComputerName   : 192.168.2.111
    RemoteAddress  : 192.168.2.111
    InterfaceAlias : Test-40G-2
    SourceAddress  : 192.168.2.5
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    
    Test-NetConnection 192.168.2.222
    
    ComputerName   : 192.168.2.222
    RemoteAddress  : 192.168.2.222
    InterfaceAlias : Test-40G-2
    SourceAddress  : 192.168.2.5
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms 
    
        
    Set-VMNetworkAdapter -ManagementOS -Name "SMB1" -IeeePriorityTag on
    Set-VMNetworkAdapter -ManagementOS -Name "SMB2" -IeeePriorityTag on
    Get-VMNetworkAdapter -ManagementOS -Name "SMB*" | fl Name,SwitchName,IeeePriorityTag,Status
    
    Name: SMB1
    SwitchName  : VMSTEST
    IeeePriorityTag : On
    Status  : {Ok}
    
    Name: SMB2
    SwitchName  : VMSTEST
    IeeePriorityTag : On
    Status  : {Ok}
    

    Get-NetAdapterRdma -Name "vEthernet*" | sort Name | ft -AutoSize
    
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  False  
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  False  
    vEthernet (MGT)   Hyper-V Virtual Ethernet Adapter #2  False   
    
    
    Enable-NetAdapterRdma -Name "vEthernet (SMB1)"
    Enable-NetAdapterRdma -Name "vEthernet (SMB2)"
    Get-NetAdapterRdma -Name "vEthernet*" | sort Name | fl *
    
    
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  True   
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  True  
    vEthernet (MGT)   Hyper-V Virtual Ethernet Adapter #2  False
      

### <a name="validate-rdma-functionality-from-the-remote-system"></a>驗證從遠端系統 RDMA 功能

您可以使用本節驗證本機系統都有 vSwitch，請從遠端系統 RDMA 功能，因此測試這兩個介面卡的 vSwitch 設定小組的成員。

因為這兩個主機 vNICs \（SMB1 和 SMB2\）已指派給 VLAN 102，您可以選取 VLAN 102 遠端系統上的介面卡。  

此程序 NIC 測試 40 G-2 會 RDMA SMB1 (192.168.2.111) 和 SMB2 (192.168.2.222)。

選用：您可能需要此系統防火牆停用。  如需詳細資訊，請洽詢您 fabric 的原則。

    
    Set-NetFirewallProfile -All -Enabled False
    
    Get-NetAdapterAdvancedProperty -Name "Test-40G-2"
    
    Name  DisplayNameDisplayValue   RegistryKeyword RegistryValue  
    ----  -----------------------   --------------- -------------  
    .
    .
    Test-40G-2VLAN ID102VlanID  {102} 
    
    Get-NetAdapter
    
    Name  InterfaceDescriptionifIndex Status   MacAddress LinkSpeed
    ----  --------------------------- ------   ---------- ---------
    Test-40G-2Mellanox ConnectX-3 Pro Ethernet A...#3   3 Up   E4-1D-2D-07-43-D140 Gbps
    
    
    Get-NetAdapterRdma
    
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    Test-40G-2Mellanox ConnectX-3 Pro Ethernet Adap... True   
    
    
    C:\TEST\Test-RDMA.PS1 -IfIndex 3 -IsRoCE $true -RemoteIpAddress 192.168.2.111 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
    VERBOSE: The adapter Test-40G-2 is a physical adapter
    VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
    VERBOSE: QoS/DCB/PFC configuration is correct.
    VERBOSE: RDMA configuration is correct.
    VERBOSE: Checking if remote IP address, 192.168.2.111, is reachable.
    VERBOSE: Remote IP 192.168.2.111 is reachable.
    VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
    VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
    VERBOSE: 34251744 RDMA bytes sent per second
    VERBOSE: 967346308 RDMA bytes written per second
    VERBOSE: 35698177 RDMA bytes sent per second
    VERBOSE: 976601842 RDMA bytes written per second
    VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
    VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.2.111
    
    
    C:\TEST\Test-RDMA.PS1 -IfIndex 3 -IsRoCE $true -RemoteIpAddress 192.168.2.222 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
    VERBOSE: The adapter Test-40G-2 is a physical adapter
    VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
    VERBOSE: QoS/DCB/PFC configuration is correct.
    VERBOSE: RDMA configuration is correct.
    VERBOSE: Checking if remote IP address, 192.168.2.222, is reachable.
    VERBOSE: Remote IP 192.168.2.222 is reachable.
    VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
    VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 0 RDMA bytes sent per second
    VERBOSE: 485137693 RDMA bytes written per second
    VERBOSE: 35200268 RDMA bytes sent per second
    VERBOSE: 939044611 RDMA bytes written per second
    VERBOSE: 34880901 RDMA bytes sent per second
    VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
    VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.2.222
    
### <a name="test-for-rdma-traffic-from-the-local-to-the-remote-computer"></a>從本機 RDMA 流量遠端電腦的測試

在本區段中您就可以驗證 RDMA 流量的本機電腦上傳送到遠端電腦。

測試及檢視流量，您可以使用下列命令範例。

    
    Get-NetAdapter | ft –AutoSize
    
    Name  InterfaceDescriptionifIndex Status   MacAddress LinkSpeed
    ----  --------------------------- ------   ---------- ---------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  45 Up   00-15-5D-30-AA-0380 Gbps
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  41 Up   00-15-5D-30-AA-0280 Gbps
    
    
    C:\TEST\Test-RDMA.PS1 -IfIndex 41 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
    VERBOSE: The adapter vEthernet (SMB1) is a virtual adapter
    VERBOSE: Retrieving vSwitch bound to the virtual adapter
    VERBOSE: Found vSwitch: VMSTEST
    VERBOSE: Found the following physical adapter(s) bound to vSwitch: TEST-40G-1, TEST-40G-2
    VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
    VERBOSE: QoS/DCB/PFC configuration is correct.
    VERBOSE: RDMA configuration is correct.
    VERBOSE: Checking if remote IP address, 192.168.2.5, is reachable.
    VERBOSE: Remote IP 192.168.2.5 is reachable.
    VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
    VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 0 RDMA bytes sent per second
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 15250197 RDMA bytes sent per second
    VERBOSE: 896320913 RDMA bytes written per second
    VERBOSE: 33947559 RDMA bytes sent per second
    VERBOSE: 912160540 RDMA bytes written per second
    VERBOSE: 34091930 RDMA bytes sent per second
    VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
    VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.2.5
    
    
    C:\TEST\Test-RDMA.PS1 -IfIndex 45 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
    VERBOSE: The adapter vEthernet (SMB2) is a virtual adapter
    VERBOSE: Retrieving vSwitch bound to the virtual adapter
    VERBOSE: Found vSwitch: VMSTEST
    VERBOSE: Found the following physical adapter(s) bound to vSwitch: TEST-40G-1, TEST-40G-2
    VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
    VERBOSE: QoS/DCB/PFC configuration is correct.
    VERBOSE: RDMA configuration is correct.
    VERBOSE: Checking if remote IP address, 192.168.2.5, is reachable.
    VERBOSE: Remote IP 192.168.2.5 is reachable.
    VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
    VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 0 RDMA bytes sent per second
    VERBOSE: 385169487 RDMA bytes written per second
    VERBOSE: 33902277 RDMA bytes sent per second
    VERBOSE: 907354685 RDMA bytes written per second
    VERBOSE: 33923662 RDMA bytes sent per second
    VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
    VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.2.5
    
最後一列在此輸出中，「RDMA 流量測試成功：RDMA 流量已傳送至 192.168.2.5，「示範，您已成功在您的介面卡設定匯集 NIC。

## <a name="all-topics-in-this-guide"></a>本指南所有主題

本指南包含下列主題。

- [使用單一網路介面卡的聚合型的 NIC 設定](cnic-single.md)
- [聚合型的 NIC 小組的 NIC 設定](cnic-datacenter.md)
- [聚合型 NIC 實體切換設定](cnic-app-switch-config.md)
- [疑難排解匯集 NIC 設定](cnic-app-troubleshoot.md)
