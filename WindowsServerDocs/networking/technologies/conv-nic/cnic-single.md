---
title: 使用單一網路介面卡的聚合型的 NIC 設定
description: 本主題提供部署匯集 NIC 與 Windows Server 2016 的單一網路介面卡上的指示。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: eed5c184-fa55-43a8-a879-b1610ebc70ca
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d6663a966026afb301a4bad90a9573d16fc82875
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="converged-nic-configuration-with-a-single-network-adapter"></a>使用單一網路介面卡的聚合型的 NIC 設定

>適用於：Windows Server（以每年次管道）、Windows Server 2016

下列章節提供匯集 NIC 設定在您的 HYPER-V 主機單一 NIC 的指示操作。

本指南範例設定描述兩個 HYPER-V 主機，**HYPER-V 主機 A**，以及**HYPER-V 主機 B**。

## <a name="test-connectivity-between-source-and-destination"></a>測試連接之間來源和目的地資訊

本節測試連接之間來源和目的地 HYPER-V 主機時所需的步驟。

下圖描述兩個 HYPER-V 主機，**HYPER-V 主機 A**和**HYPER-V 主機 B**。 

這兩部伺服器已安裝的單一實體 NIC (pNIC) 和 Nic 連接到頂端架 \(ToR\) 實體切換。 此外，伺服器位於上相同的子網路，也就是 192.168.1.x/24。

![HYPER-V 主機](../../media/Converged-NIC/1-single-test-conn.jpg)


### <a name="test-nic-connectivity-to-the-hyper-v-virtual-switch"></a>測試 NIC 連接 Hyper\ HYPER-V Virtual 開關切換至

您可以使用此步驟，來確保實體而的您稍後將會建立 Hyper\ HYPER-V Virtual 參數，可連接目的主機。 

這項測試使用層級 3 \(L3\)-或 IP 層級-以及層級 2 \(L2\) 示範連接。

您可以使用下列 Windows PowerShell 命令，以取得的網路介面卡的屬性。

    Get-NetAdapter
     
以下是此命令的範例結果。

|名稱|InterfaceDescription|ifIndex|狀態|MacAddress|LinkSpeed|
|-----|--------------------|-------|-----|----------|---------|
|
|M1|Mellanox ConnectX-3 Pro...| 4| 向上|7C-FE-90-93-8F-A1|40 Gbps|

您可以使用其中一項下列命令，以取得額外的介面卡屬性，包括的 IP 位址。

    Get-NetAdapter M1 | fl *

以下是此命令的編輯的範例結果。
    
    MacAddress   : 7C-FE-90-93-8F-A1
    Status   : Up
    LinkSpeed: 40 Gbps
    MediaType: 802.3
    PhysicalMediaType: 802.3
    AdminStatus  : Up
    MediaConnectionState : Connected
    DriverInformation: Driver Date 2016-08-28 Version 5.25.12665.0 NDIS 6.60
    DriverFileName   : mlx4eth63.sys
    NdisVersion  : 6.60
    ifOperStatus : Up
    ifAlias  : M1
    InterfaceAlias   : M1
    ifIndex  : 4
    ifDesc   : Mellanox ConnectX-3 Pro Ethernet Adapter
    ifName   : ethernet_32773
    DriverVersion: 5.25.12665.0
    LinkLayerAddress : 7C-FE-90-93-8F-A1
    Caption  :
    Description  :
    ElementName  :
    InstanceID   : {39B58B4C-8833-4ED2-A2FD-E105E7146D43}
    CommunicationStatus  :
    DetailedStatus   :
    HealthState  :
    InstallDate  :
    Name : M1
    OperatingStatus  :
    OperationalStatus:
    PrimaryStatus:
    StatusDescriptions   :
    AvailableRequestedStates :
    EnabledDefault   : 2
    EnabledState : 5
    OtherEnabledState:
    RequestedState   : 12
    TimeOfLastStateChange:
    TransitioningToState : 12
    AdditionalAvailability   :
    Availability :
    CreationClassName: MSFT_NetAdapter
    

### <a name="ensure-that-source-and-destination-can-communicate"></a>確保能通訊來源和目的地資訊

您可以使用此步驟以確認雙向通訊 \ (ping 來源目的地和這兩個 systems\ 上反之亦然)。  下列範例中，**測試-NetConnection**使用 Windows PowerShell 命令時，但如果您想要您可以使用**ping**命令。 

>[!NOTE]
>如果您不確定您的主機可以彼此，您可以略過此步驟。

    Test-NetConnection 192.168.1.5

以下是此命令的範例結果。

|參數|值。|
|---------|-----|
|
|電腦名稱|192.168.1.5|
|RemoteAddress|192.168.1.5|
|InterfaceAlias|M1|
|SourceAddress|192.168.1.3|
|PingSucceeded|為 true|
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

## <a name="configure-vlans-optional"></a>設定 Vlan \(Optional\)

許多網路設定，請使用 Vlan。 如果您打算使用 Vlan 您網路中，您必須使用 Vlan 設定重複之前測試。 \ (如果您打算 RoCE RDMA 服務使用您必須讓 Vlan。\)

針對此步驟，Nic 可在**存取**模式。 但是當您在本文稍後建立 HYPER-V Virtual 切換 \(vSwitch\)，VLAN 屬性會套用到 vSwitch 連接埠層級。 

切換可以主機多個 Vlan，因為它是所需的最上方的架 \(ToR\) 實體切換主機已連接到設定主幹模式中的連接埠。

>[!NOTE]
>如需如何設定主幹模式開關切換至上的指示，請洽詢您 ToR 切換文件。

下圖描述兩個 HYPER-V 主機，每一個實體網路介面卡，與每個設定的 VLAN 101 使用。

![設定的區域網路](../../media/Converged-NIC/2-single-configure-vlans.jpg)

### <a name="configure-the-vlan-id"></a>設定 VLAN ID

您可以使用此步驟來設定 VLAN Id Nic 安裝在您的 HYPER-V 主機。

#### <a name="configure-nic-m1"></a>設定 NIC M1

使用下列命令，設定 VLAN ID 的第一個而 M1，然後檢視 [顯示設定。

>[!IMPORTANT]
>無法執行此命令如果您連接至主機遠端透過這個介面，因為有這樣做，會導致的存取權限主機。
    
    Set-NetAdapterAdvancedProperty -Name M1 -RegistryKeyword VlanID -RegistryValue "101"
    Get-NetAdapterAdvancedProperty -Name M1 | Where-Object {$_.RegistryKeyword -eq "VlanID"} 
    
以下是此命令的範例結果。

|名稱 |顯示名稱| DisplayValue| RegistryKeyword |RegistryValue|
|----|-----------|------------|---------------|-------------|
|
|M1|VLAN ID|101|VlanID|{101}|


請確定該 VLAN ID 生效的網路介面卡實作獨立使用下列命令重新開機網路介面卡。

    Restart-NetAdapter -Name "M1"

您可以使用下列命令，以確保您的網路介面卡狀態的是**上**再繼續。

    Get-NetAdapter -Name "M1"

以下是此命令的範例結果。

|名稱|InterfaceDescription|ifIndex| 狀態|MacAddress|LinkSpeed|
|----|--------------------|-------|------|----------| ---------|
|
|M1|Mellanox ConnectX-3 Pro 乙太網路草原...|4|向上|7C-FE-90-93-8F-A1|40 Gbps|

請確定您在本機和目的地的伺服器上執行此步驟。 如果目的伺服器並未設定為 [本機伺服器相同的 VLAN ID，這兩個無法通訊。

### <a name="verify-connectivity"></a>請確認連接

您可以使用本節之後重新啟動網路介面卡，請確認連接。 您可以確認連接之後申請這兩個介面卡的 VLAN 標記。 如果連接失敗，您可以檢查 VLAN 設定或目的參與相同 VLAN 切換。 

>[!IMPORTANT]
>您上一節中執行步驟後，可能需要幾秒鐘的裝置重新開機，並在網路上推出。

#### <a name="verify-connectivity-for-nic-test-40g-1"></a>請確認連接的 NIC 測試-40 G-1

請確認連接的第一個而，您可以執行下列命令。

    Test-NetConnection 192.168.1.5
    
## <a name="configure-data-center-bridging-dcb"></a>設定資料中心橋接 \(DCB\)

下一個步驟是設定 DCB 和服務品質 \(QoS\)，需要先安裝 Windows Server 2016 功能 DCB。

>[!NOTE]
>您必須是為了讓彼此所有伺服器上執行的所有下列 DCB 和 QoS 設定步驟。

### <a name="install-data-center-bridging-dcb"></a>安裝資料中心橋接 \(DCB\)

您可以使用此步驟，安裝以及 DCB。 

>[!IMPORTANT]
>- 安裝和設定 DCB 是**選擇性**的 [網路設定中，使用 iWarp RDMA 服務。
>- 安裝和設定 DCB 是**需要**的 [網路設定中，使用 RoCE \(any version\) RDMA 服務。


您可以在每個您的 HYPER-V 主機上安裝 DCB 使用下列命令。 

    Install-WindowsFeature Data-Center-Bridging

### <a name="set-the-qos-policies-for-smb-direct"></a>設定 SMB 直接 QoS 原則 

您可以使用下列命令來設定 SMB 直接 QoS 原則。

>[!IMPORTANT]
>- 這個步驟可使用 iWarp 網路設定為選擇性。
>- 需要的網路設定中，使用 RoCE 此步驟。
>- 以下的範例指令，在 [值] 3 [為任意。 只要您持續使用相同的整個 QoS 原則的設定值，您可以使用任何介於 1 與 7。

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

### <a name="for-roce-deployments-turn-on-priority-flow-control-for-smb-traffic"></a>適用於 RoCE 部署打開 SMB 流量高優先順序傳送控制項 

如果您使用 RoCE RDMA 服務，您可以使用下列命令，可讓 SMB 流量控制，並檢視結果。 高優先順序傳送控制需要 RoCE，但您使用 iWarp 時的非必要。

    Enable-NetQosFlowControl -priority 3
    Get-NetQosFlowControl

以下是範例結果的**取得-NetQosFlowControl**命令。

|高優先順序|支援|PolicySet|IfIndex|IfAlias|
|---------|-----|--------- |-------| -------|
|
|0 |False |全球|&nbsp;|&nbsp;|
|1 |False |全球|&nbsp;|&nbsp;|
|2 |False |全球|&nbsp;|&nbsp;|
|3 |為 true  |全球|&nbsp;|&nbsp;|
|4 |False |全球|&nbsp;|&nbsp;|
|5 |False |全球|&nbsp;|&nbsp;|
|6 |False |全球|&nbsp;|&nbsp;|
|7 |False |全球|&nbsp;|&nbsp;|

### <a name="enable-qos-for-the-local-and-destination-network-adapters"></a>讓 QoS 本機和目的地的網路介面卡
這個步驟中，您可以讓 DCB 特定網路介面卡上。

>[!IMPORTANT]
>-  這個步驟就不需要的網路設定中，使用 iWarp。
>-  需要的網路設定中，使用 RoCE 此步驟。




#### <a name="enable-qos-for-nic-m1"></a>適用於 NIC M1 讓 QoS

您可以使用下列命令，讓 QoS 並檢視您的設定的結果。

    Enable-NetAdapterQos -InterfaceAlias "M1"
    Get-NetAdapterQos -Name "M1"

以下是此命令的範例結果。

**名稱**: M1**支援**: True**功能**:   

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
|0| ETS|70%|0-2,4-7|
|1|ETS|30%|3

**OperationalFlowControl**：優先順序 3 功能的**OperationalClassifications**:

|通訊協定|連接埠日類型|高優先順序|
|--------|---------|--------|
|
|預設值|&nbsp;|0|
|NetDirect| 445|3|

### <a name="reserve-a-percentage-of-the-bandwidth-for-smb-direct-rdma"></a>適用於 SMB 直接 \(RDMA\) 保留百分比的頻寬

您可以使用下列命令來為 SMB 直接保留百分比的頻寬。  

在此範例中，使用 30%頻寬保留。 您應該選取，表示您預期會要求您儲存的流量的值。 值**-bandwidthpercentage**參數必須多 10%。

    New-NetQosTrafficClass "SMB" -Priority 3 -BandwidthPercentage 30 -Algorithm ETS

以下是此命令的範例結果。

|名稱|演算法 |Bandwidth(%)| 高優先順序 |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|SMB | ETS     | 30 |3 |全球 |&nbsp;|&nbsp;|                                      
 
您可以使用下列命令，以檢視頻寬保留的資訊。

    Get-NetQosTrafficClass

以下是此命令的範例結果。
 
|名稱|演算法 |Bandwidth(%)| 高優先順序 |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|[預設值]|ETS|70 |0-2,4-7| 全球|&nbsp;|&nbsp;| 
|SMB      |ETS|30 |3 |全球|&nbsp;|&nbsp;| 

## <a name="remove-debugger-conflict-mellanox-adapter-only"></a>移除偵錯工具衝突 \(Mellanox adapter only\)

如果您使用介面卡 Mellanox 從您要執行此步驟進行偵錯工具。 根據預設，使用 Mellanox 介面卡時，附加偵錯工具會封鎖 NetQos。 您可以使用下列命令，若要覆寫偵錯工具。

    
    Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 –Force
    

## <a name="test-rdma-native-host"></a>測試 RDMA（原生主機）

您可以使用此步驟，以確保建立 vSwitch 和轉換為 RDMA \(Converged NIC\) 之前 fabric 已正確設定。

下圖描述目前 HYPER-V 主機的狀態。

![測試 RDMA](../../media/Converged-NIC/4-single-test-rdma.jpg)

若要確認 RDMA 設定，您可以執行下列命令。

    Get-NetAdapterRdma

以下是此命令的範例結果。

|名稱 |InterfaceDescription |支援|
|----|--------------------|-------|
|
|M1| Mellanox ConnectX-3 Pro 乙太網路卡 |為 true|

### <a name="download-diskspdexe-and-a-powershell-script"></a>下載 DiskSpd.exe 和 PowerShell 指令碼

若要繼續時，您必須先下載下列項目。

- 下載 DiskSpd.exe 公用程式，並將公用程式 C:\TEST\ 到[Diskspd 公用：A 穩定儲存測試工具（取代 SQLIO）](https://gallery.technet.microsoft.com/DiskSpd-a-robust-storage-6cd2f223)

- 若要 C:\TEST\ 下載 Test-RDMA powershell 指令碼 [https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)

### <a name="determine-the-ifindex-value-of-your-target-adapter"></a>判斷您的目標介面卡的 ifIndex 值

您可以使用下列命令來探索 ifIndex 目標介面卡的值。 您已下載的指令碼執行時，您可以使用此值後續步驟。

    Get-NetIPConfiguration -InterfaceAlias "M*" | ft InterfaceAlias,InterfaceIndex,IPv4Address

以下是此命令的範例結果。

|InterfaceAlias |InterfaceIndex |IPv4Address|
|-------------- |-------------- |-----------|
|
|M2 |14 |{192.168.1.5}|

### <a name="run-the-powershell-script"></a>執行 PowerShell 指令碼

當您執行的 Windows PowerShell 指令碼 Test-Rdma.ps1 時，您可以傳送 ifIndex 值指令碼，以及遠端相同的 VLAN 上介面卡的 IP 位址。

您可以使用下列命令範例為 14 ifIndex 指令碼執行的網路介面卡 192.168.1.5。
    
    C:\TEST\Test-RDMA.PS1 -IfIndex 14 -IsRoCE $true -RemoteIpAddress 192.168.1.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
    VERBOSE: The adapter M2 is a physical adapter
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

## <a name="remove-the-access-vlan-setting"></a>移除存取 VLAN 設定

建立 HYPER-V 開關切換至準備您必須移除您安裝上方的 VLAN 設定。  您可以使用下列命令來移除實體 NIC 存取 VLAN 設定 這個動作會防止自動標記輸出資料傳輸與不正確的 VLAN ID，而，並也會防止它篩選輸入流量不符合存取 VLAN id。

    
    Set-NetAdapterAdvancedProperty -Name M1 -RegistryKeyword VlanID -RegistryValue "0"
    

您可以使用下列命令的範例，確認 VlanID 設定，並檢視結果，顯示的 VLAN ID 值為零。

    
    Get-NetAdapterAdvancedProperty -name m1 | Where-Object {$_.RegistryKeyword -eq 'VlanID'} 
    


## <a name="create-a-hyper-v-virtual-switch"></a>建立 HYPER-V Virtual 開關切換至

建立 HYPER-V Virtual 切換 \(vSwitch\) 在您的 HYPER-V 主機上，您可以使用此一節。

下圖與 vSwitch 描述 HYPER-V 主機 1。

![建立 HYPER-V Virtual 開關切換至](../../media/Converged-NIC/5-single-create-vswitch.jpg)


### <a name="create-an-external-hyper-v-virtual-switch"></a>建立 HYPER-V Virtual 切換外部

您可以使用本節外部 vSwitch 建立 HYPER-V HYPER-V 主機消失。

您可以使用下列範例命令，以建立名稱為 VMSTEST 切換。

>[!NOTE]
>參數**AllowManagementOS**下列命令中建立主機但 vNIC 繼承的 MAC 地址和實體 NIC IP 位址

    New-VMSwitch -Name VMSTEST -NetAdapterName "M1" -AllowManagementOS $true

以下是此命令的範例結果。

|名稱 |SwitchType |NetAdapterInterfaceDescription|
|---- |---------- |------------------------------|
|VMSTEST |外部 |Mellanox ConnectX-3 Pro 乙太網路卡|

您可以使用下列命令，以檢視網路介面卡的屬性。

    Get-NetAdapter | ft -AutoSize

以下是此命令的範例結果。

|名稱 |InterfaceDescription | ifIndex |狀態 |MacAddress |LinkSpeed|
|---- |-------------------- |-------| ------|----------|---------|
|
|vEthernet \(VMSTEST\) |HYPER-V Virtual 乙太網路卡 #2|27 |向上 |E4-1D-2D-07-40-71 |40 Gbps|


您可以管理主機但 vNIC 兩種方式。 其中一個方法是**NetAdapter**檢視中，它的運作方式根據」vEthernet \(VMSTEST\)」的名稱。

其他方法是**VMNetworkAdapter**檢視中，會捨棄」vEthernet「前置詞，只要使用 vmswitch 名稱。

**VMNetworkAdapter**檢視中，會顯示不會顯示的一些網路介面卡屬性**NetAdapter**命令。

您可以使用下列命令，以檢視的**VMNetworkAdapter**方法。

    Get-VMNetworkAdapter –ManagementOS | ft -AutoSize

以下是此命令的範例結果。

|名稱 |IsManagementOs |VMName |SwitchName |MacAddress |狀態 |IPAddresses|
|----|-------------- |------ |----------|----------|------ |-----------|
|
|CORP-外部開關切換至 |為 true |CORP-外部開關切換至| 001B785768AA |{確定} |&nbsp;|
|VMSTEST |為 true |VMSTEST | E41D2D074071| {確定} | &nbsp;| 

### <a name="test-the-connection"></a>測試連接

您可以使用下列命令範例測試連接和檢視結果。
    
    Test-NetConnection 192.168.1.5

    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (CORP-External-Switch)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    
您可以使用下列命令範例指派，以及檢視網路介面卡 VLAN 設定。
    
    Set-VMNetworkAdapterVlan -VMNetworkAdapterName "VMSTEST" -VlanId "101" -Access -ManagementOS
    Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName "VMSTEST"
    

|VMName |VMNetworkAdapterName |模式 |VlanList|
|------ |-------------------- |---- |--------|
|
|&nbsp;|VMSTEST |存取 |101     
 
### <a name="test-the-connection"></a>測試連接

重新叫用，可能需要變更完成之前您成功可以 ping 到另一個介面卡幾秒鐘。

您可以使用下列命令範例測試連接和檢視結果。
    
    Test-NetConnection 192.168.1.5
     
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (VMSTEST)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    

## <a name="test-hyper-v-virtual-switch-rdma-mode-2"></a>測試 HYPER-V Virtual 切換 RDMA（2 模式）

下圖描述您的 HYPER-V 主機，包括 HYPER-V 主機 1 vSwitch 目前狀態。

![測試 HYPER-V Virtual 開關切換至](../../media/Converged-NIC/6-single-test-vswitch-rdma.jpg)


### <a name="set-priority-tagging-on-the-host-vnic"></a>在主機但 vNIC 標記設定優先順序

您可以使用下列命令範例設定優先順序主機但 vNIC 上標記，並檢視您的動作的結果。
    
    Set-VMNetworkAdapter -ManagementOS -Name "VMSTEST" -IeeePriorityTag on
    Get-VMNetworkAdapter -ManagementOS -Name "VMSTEST" | fl Name,IeeePriorityTag
    
    Name: VMSTEST
    IeeePriorityTag : On
    
若要檢視 RDMA 資訊的網路介面卡，您可以使用下列命令範例。 在結果中，當參數**啟用**的值**\ [false\]**，表示該 RDMA 不支援。
    
    Get-NetAdapterRdma
    
|名稱 |InterfaceDescription |支援 |
|---- |-------------------- |-------|
|
|vEthernet \(VMSTEST\)| HYPER-V Virtual 乙太網路卡 #2|False|

### <a name="enable-rdma-on-the-host-vnic"></a>在主機但 vNIC 讓 RDMA

若要檢視網路介面卡屬性，讓 RDMA 介面卡，然後檢視 [網路介面卡 RDMA 資訊，您可以使用下列命令範例。
    
    Get-NetAdapter
    
|名稱 |InterfaceDescription |ifIndex |狀態 |MacAddress |LinkSpeed|
|----|--------------------|-------|------|----------|---------|
|
|vEthernet (VMSTEST)|HYPER-V Virtual 乙太網路卡 #2|27|向上|E4-1D-2D-07-40-71|40 Gbps|

    Enable-NetAdapterRdma -Name "vEthernet (VMSTEST)"
    Get-NetAdapterRdma -Name "vEthernet (VMSTEST)"

在下列結果中，當參數**啟用**的值**為 True**，表示可以 RDMA。

|名稱 |InterfaceDescription |支援 |
|---- |-------------------- |-------|
|
|vEthernet \(VMSTEST\)| HYPER-V Virtual 乙太網路卡 #2|為 true|


    
    Get-NetAdapter 
    

|名稱 |InterfaceDescription |ifIndex |狀態 |MacAddress |LinkSpeed|
|----|--------------------|-------|------|----------|---------|
|
|vEthernet (VMSTEST)|HYPER-V Virtual 乙太網路卡 #2|27|向上|E4-1D-2D-07-40-71|40 Gbps|

### <a name="perform-rdma-traffic-test-by-using-the-script"></a>使用指令碼執行 RDMA 流量測試

您可以使用下列命令來執行您下載的指令碼，並檢視結果。
    
    C:\TEST\Test-RDMA.PS1 -IfIndex 27 -IsRoCE $true -RemoteIpAddress 192.168.1.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
    VERBOSE: The adapter vEthernet (VMSTEST) is a virtual adapter
    VERBOSE: Retrieving vSwitch bound to the virtual adapter
    VERBOSE: Found vSwitch: VMSTEST
    VERBOSE: Found the following physical adapter(s) bound to vSwitch: TEST-40G-1
    VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
    VERBOSE: QoS/DCB/PFC configuration is correct.
    VERBOSE: RDMA configuration is correct.
    VERBOSE: Checking if remote IP address, 192.168.1.5, is reachable.
    VERBOSE: Remote IP 192.168.1.5 is reachable.
    VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
    VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 0 RDMA bytes sent per second
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 0 RDMA bytes sent per second
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 0 RDMA bytes sent per second
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 9162492 RDMA bytes sent per second
    VERBOSE: 938797258 RDMA bytes written per second
    VERBOSE: 34621865 RDMA bytes sent per second
    VERBOSE: 933572610 RDMA bytes written per second
    VERBOSE: 35035861 RDMA bytes sent per second
    VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
    VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.1.5
    
最後一列在此輸出中，「RDMA 流量測試成功：RDMA 流量已傳送至 192.168.1.5，「示範，您已成功在您的介面卡設定匯集 NIC。

## <a name="all-topics-in-this-guide"></a>本指南所有主題

本指南包含下列主題。

- [使用單一網路介面卡的聚合型的 NIC 設定](cnic-single.md)
- [聚合型的 NIC 小組的 NIC 設定](cnic-datacenter.md)
- [聚合型 NIC 實體切換設定](cnic-app-switch-config.md)
- [疑難排解匯集 NIC 設定](cnic-app-troubleshoot.md)
