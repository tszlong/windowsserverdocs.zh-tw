---
title: 交集的 NIC 設定，具有單一網路介面卡
description: 本主題中，我們提供您的設定與 HYPER-V 主機中的單一 NIC 的交集的 NIC 的指示。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: eed5c184-fa55-43a8-a879-b1610ebc70ca
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/14/2018
ms.openlocfilehash: 7777278f374984f242e44fd8a8fa94388df88a30
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836469"
---
# <a name="converged-nic-configuration-with-a-single-network-adapter"></a>交集的 NIC 設定，具有單一網路介面卡

>適用於：Windows Server （半年通道），Windows Server 2016

本主題中，我們提供您的設定與 HYPER-V 主機中的單一 NIC 的交集的 NIC 的指示。

本主題中的範例組態會描述兩個 HYPER-V 主機， **HYPER-V 主機 A**，並**HYPER-V 主機 B**。這兩個主機都已安裝，單一實體 NIC (pNIC)，並為 Nic 連線到機架頂端\(ToR\)實體交換器。 此外，主機都位於相同的子網路，也就是 192.168.1.x/24。

![Hyper-V 主機](../../media/Converged-NIC/1-single-test-conn.jpg)


## <a name="step-1-test-the-connectivity-between-source-and-destination"></a>步驟 1. 測試來源與目的地之間的連線

確定實體 NIC 可以連線至目的地主機。 這項測試示範使用第 3 層連線能力\(L3\)或者 IP 層以及第 2 層\(L2\)。

1. 檢視網路介面卡內容。

   ```PowerShell
   Get-NetAdapter
   ```
   
   _**結果：**_  

   |名稱|InterfaceDescription|ifIndex|狀態|MacAddress|LinkSpeed|
   |-----|--------------------|-------|-----|----------|---------|
   |M1|Mellanox connectx-3 Pro...| 4| Up|7C-FE-90-93-8F-A1|40 Gbps|
   ---

2. 檢視其他配接器屬性，包括 IP 位址。

   ```PowerShell
   Get-NetAdapter M1 | fl *
   ```

   _**結果：**_

   ```PowerShell   
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
   ``` 

## <a name="step-2-ensure-that-source-and-destination-can-communicate"></a>步驟 2. 確保來源和目的地可以進行通訊

在此步驟中，我們會使用**Test-netconnection** Windows PowerShell 命令，但是如果您可以使用**ping**如果您偏好的命令。 

>[!TIP]
>如果您確定您的主機可以彼此通訊，您可以略過此步驟。

1. 確認雙向通訊。

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```
   
   _**結果：**_

   |參數|值|
   |---------|-----|
   |ComputerName|192.168.1.5|
   |RemoteAddress|192.168.1.5|
   |InterfaceAlias|M1|
   |SourceAddress|192.168.1.3|
   |PingSucceeded|True|
   |PingReplyDetails \(RTT\)|0 毫秒|
   ---
   
   在某些情況下，您可能需要停用具有成功執行這項測試的進階安全性的 Windows 防火牆。 如果您停用防火牆，請記住的安全性，並確定您的設定符合貴組織的安全性需求。

2. 停用所有防火牆設定檔。

   ```PowerShell
   Set-NetFirewallProfile -All -Enabled False
   ```
    
3. 停用防火牆設定檔之後, 再次測試連線。 

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**結果：**_

   |參數|值|
   |---------|-----|
   |ComputerName|192.168.1.5|
   |RemoteAddress|192.168.1.5|
   |InterfaceAlias|測試-40 G-1|
   |SourceAddress|192.168.1.3|
   |PingSucceeded|False|
   |PingReplyDetails \(RTT\)|0 毫秒|
   ---



## <a name="step-3-optional-configure-the-vlan-ids-for-nics-installed-in-your-hyper-v-hosts"></a>步驟 3。 （選擇性）針對安裝在 HYPER-V 主機中的 Nic 設定 VLAN 識別碼

許多網路進行組態使用的 Vlan，以及如果您打算在網路中使用 Vlan，您必須重複執行先前的測試設定的 Vlan。 此外，如果您打算使用 RoCE 的 RDMA 服務，您必須啟用 Vlan。

此步驟中，Nic 位於**存取**模式。 不過，當您建立 HYPER-V 虛擬交換器\(vSwitch\)稍後在本指南中，VLAN 內容會套用到 vSwitch 連接埠層級。 

因為交換器可以裝載多個 Vlan，則需要這麼做最上方的機架\(ToR\)實體交換器的主機已連線到在主幹模式中設定連接埠。

>[!NOTE]
>如需如何設定交換器的主幹模式中的指示，請參閱您的 ToR 交換器文件。

下圖顯示兩個 HYPER-V 主機，每個都有一張實體網路介面卡，以及每個設定成在 VLAN 101 上進行通訊。

![設定虛擬區域網路](../../media/Converged-NIC/2-single-configure-vlans.jpg)


>[!IMPORTANT]
>在本機和目的地伺服器上執行此項。 如果目的地伺服器未設定為本機伺服器相同的 VLAN ID，這兩個無法進行通訊。


1. 針對安裝在 HYPER-V 主機中的 Nic 中設定 VLAN 識別碼。

   >[!IMPORTANT]
   >無法執行此命令如果您已連線到主應用程式從遠端透過此介面，因為這會導致失去存取權給主機。
    
   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name M1 -RegistryKeyword VlanID -RegistryValue "101"
   Get-NetAdapterAdvancedProperty -Name M1 | Where-Object {$_.RegistryKeyword -eq "VlanID"} 
   ```

   _**結果：**_

   |名稱 |DisplayName| DisplayValue| RegistryKeyword |RegistryValue|
   |----|-----------|------------|---------------|-------------|
   |M1|VLAN 識別碼|101|VlanID|{101}|
   ---

2. 重新啟動的網路介面卡，以套用 VLAN id。

   ```PowerShell
   Restart-NetAdapter -Name "M1"
   ```

3. 確認狀態為**向上**。

   ```PowerShell
   Get-NetAdapter -Name "M1"
   ```
   
   _**結果：**_

   |名稱|InterfaceDescription|ifIndex| 狀態|MacAddress|LinkSpeed|
   |----|--------------------|-------|------|----------| ---------|
   |M1|Mellanox connectx-3 Pro 乙太網路 Ada...|4|Up|7C-FE-90-93-8F-A1|40 Gbps|
   ---

   >[!IMPORTANT]
   >可能需要幾秒鐘重新啟動，並成為在網路上可用的裝置。 

4. 驗證連線。<p>如果連線失敗，請檢查在相同 vlan 的 VLAN 設定或目的地參與的交換器。 

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```
    
## <a name="step-4-configure-quality-of-service-qos"></a>步驟 4. 設定服務品質\(QoS\)

>[!NOTE]
>您必須執行所有下列 DCB 與 QoS 設定步驟要與彼此進行通訊的所有主機上。

1. 安裝資料中心橋接\(DCB\)每部 HYPER-V 主機上。

   - **選擇性**iWarp 服務使用的 RDMA 的網路組態。
   - **所需**的網路設定，使用 RoCE\(任何版本\)RDMA 服務。

   ```PowerShell
   Install-WindowsFeature Data-Center-Bridging
   ```

2. SMB 直接傳輸設定 QoS 原則：

   - **選擇性**使用 iWarp 的網路組態。
   - **所需**使用 RoCE 的網路組態。
   
   在下列範例命令中，"3"的值是任意的。 只要一致地使用相同的值在整個設定 QoS 原則，您可以使用介於 1 到 7 之間的任何值。

   ```PowerShell
   New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3
   ```

   _**結果：**_

   |參數|值|
   |---------|-----|
   |名稱 |SMB|
   |擁有者|群組原則\(機器\)|
   |NetworkProfile|全部|
   |優先順序|127|
   |JobObject|&nbsp;| 
   |NetDirectPort|445 |
   |PriorityValue|3 |
 ---

3. RoCE 部署中，開啟**優先順序流量控制**SMB 流量，這並不需要 iWarp。

   ```PowerShell
   Enable-NetQosFlowControl -priority 3
   Get-NetQosFlowControl
   ```

   _**結果：**_

   |Priority|Enabled|PolicySet|ifIndex|IfAlias|
   |---------|-----|--------- |-------| -------|
   |0 |False |全域|&nbsp;|&nbsp;|
   |1 |False |全域|&nbsp;|&nbsp;|
   |2 |False |全域|&nbsp;|&nbsp;|
   |3 |True  |全域|&nbsp;|&nbsp;|
   |4 |False |全域|&nbsp;|&nbsp;|
   |5 |False |全域|&nbsp;|&nbsp;|
   |6 |False |全域|&nbsp;|&nbsp;|
   |7 |False |全域|&nbsp;|&nbsp;|
   ---

4. 本機和目的地網路介面卡啟用 QoS。

   - **不需要**使用 iWarp 的網路組態。
   - **所需**使用 RoCE 的網路組態。

   ```PowerShell
   Enable-NetAdapterQos -InterfaceAlias "M1"
   Get-NetAdapterQos -Name "M1"
   ```

   _**結果：**_

   **名稱**：M1  
   **Enabled**：True  

   _**功能：**_   

   |參數|硬體|目前|
   |---------|--------|-------|
   |MacSecBypass|NotSupported|NotSupported|
   |DcbxSupport|None|None|
   |NumTCs(Max/ETS/PFC)|8/8/8|8/8/8|
   ---
 
   _**OperationalTrafficClasses:**_ 

   |TC|TSA|頻寬|優先順序|
   |----|-----|--------|-------|
   |0| ETS|70%|0-2,4-7|
   |1|ETS|30%|3 |
   ---

   _**OperationalFlowControl:**_  
   
   優先順序 3 啟用  

   _**OperationalClassifications:**_  

   |通訊協定|連接埠/類型|Priority|
   |--------|---------|--------|
   |預設|&nbsp;|0|
   |NetDirect| 445|3|
   ---

5. 保留的頻寬百分比的 SMB 直接傳輸\(RDMA\)。

    在此範例中，會使用 30%的頻寬保留項目。 您應該選取代表您預期您的儲存體流量需要的值。 

   ```PowerShell
   New-NetQosTrafficClass "SMB" -Priority 3 -BandwidthPercentage 30 -Algorithm ETS
   ```

   _**結果：**_

   |名稱|演算法 |Bandwidth(%)| Priority |PolicySet |ifIndex |IfAlias |
   |----|---------| ------------ |--------| ---------|------- |------- |
   |SMB | ETS     | 30 |3 |全域 |&nbsp;|&nbsp;|
   ---                                      

6. 檢視的頻寬保留設定。  

   ```PowerShell
   Get-NetQosTrafficClass
   ```

   _**結果：**_
 
   |名稱|演算法 |Bandwidth(%)| Priority |PolicySet |ifIndex |IfAlias |
   |----|---------| ------------ |--------| ---------|------- |------- |
   |[Default]|ETS|70 |0-2,4-7| 全域|&nbsp;|&nbsp;| 
   |SMB      |ETS|30 |3 |全域|&nbsp;|&nbsp;| 
   ---

## <a name="step-5-optional-resolve-the-mellanox-adapter-debugger-conflict"></a>步驟 5. （選擇性）解決 Mellanox 介面卡偵錯工具衝突 

根據預設，使用的 Mellanox 介面卡時，附加偵錯工具會封鎖 NetQos，這是已知的問題。 因此，如果您使用從 Mellanox 介面卡，並想要附加偵錯工具，使用下列命令解決此問題。 如果您不想要附加偵錯工具，或如果您未使用的 Mellanox 介面卡，則不需要此步驟。

   ```PowerShell    
   Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 –Force
   ``` 

## <a name="step-6-verify-the-rdma-configuration-native-host"></a>步驟 6. 確認 RDMA 設定 （原生主機）

您想要確定網狀架構已正確設定之前建立的 vSwitch 和轉換至 RDMA (交集的 NIC)。 

下圖顯示 HYPER-V 主機的目前狀態。

![測試 RDMA](../../media/Converged-NIC/4-single-test-rdma.jpg)

1. 確認 RDMA 設定。

   ```PowerShell
   Get-NetAdapterRdma
   ```
   _**結果：**_

   |名稱 |InterfaceDescription |Enabled|
   |----|--------------------|-------|
   |M1| Mellanox connectx-3 Pro 乙太網路介面卡 |True|
   ---

2. 判斷**ifIndex**目標介面卡的值。<p>當您執行您所下載的指令碼時，您可以使用在後續步驟中的此值。

   ```PowerShell
   Get-NetIPConfiguration -InterfaceAlias "M*" | ft InterfaceAlias,InterfaceIndex,IPv4Address
   ```

   _**結果：**_ 

   |InterfaceAlias |InterfaceIndex |IPv4Address|
   |-------------- |-------------- |-----------|
   |M2 |14 |{192.168.1.5}|
   ---

3. 下載[DiskSpd.exe 公用程式](https://aka.ms/diskspd)並將它解壓縮到 C:\TEST\.

4. 下載[測試 RDMA powershell 指令碼](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)到本機磁碟機，比方說，C:\TEST 測試資料夾\.

5. 執行**測試 Rdma.ps1** ifIndex 值傳遞至指令碼，以及在相同的 VLAN 上的遠端介面卡的 IP 位址的 PowerShell 指令碼。<p>在此範例中，指令碼會傳遞**ifIndex**值 14 上的遠端網路介面卡 IP 位址 192.168.1.5。
   
   ```PowerShell
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
   ```

   >[!NOTE]
   >如果 RDMA 流量失敗，RoCE 案例的具體來說，如應符合主機設定的適當 PFC/ETS 設定您的 ToR 交換器設定。 請參閱本文件的參考值中的 QoS 一節。

## <a name="step-7-remove-the-access-vlan-setting"></a>步驟 7. 移除存取 VLAN 設定

在準備建立 HYPER-V 交換器，您必須先移除您先前安裝的 VLAN 設定。  

1. 從實體 NIC，以防止自動標記與不正確的 VLAN 識別碼的輸出流量的 NIC 移除存取 VLAN 設定<p>移除這項設定也會讓它篩選輸入資料傳輸不符合存取 VLAN id。
    
   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name M1 -RegistryKeyword VlanID -RegistryValue "0"
   ```    

2. 確認**VlanID 設定**會顯示為零的 VLAN ID 值。

   ```PowerShell    
   Get-NetAdapterAdvancedProperty -name m1 | Where-Object {$_.RegistryKeyword -eq 'VlanID'} 
   ```  


## <a name="step-8-create-a-hyper-v-vswitch-on-your-hyper-v-hosts"></a>步驟 8。 在 HYPER-V 主機上建立 HYPER-V vSwitch

下圖說明 HYPER-V 主機 1 使用了 vSwitch。

![建立 HYPER-V 虛擬交換器](../../media/Converged-NIC/5-single-create-vswitch.jpg)


1. 在 HYPER-V 主機 a 上的 HYPER-V 中建立的外部 HYPER-V vSwitch <p>在此範例中，參數稱為 VMSTEST。 此外，參數**AllowManagementOS**會建立一個繼承實體 NIC 的 MAC 和 IP 位址的主機 vNIC

   ```PowerShell
   New-VMSwitch -Name VMSTEST -NetAdapterName "M1" -AllowManagementOS $true
   ```
   _**結果：**_

   |名稱 |SwitchType |NetAdapterInterfaceDescription|
   |---- |---------- |------------------------------|
   |VMSTEST |外部 |Mellanox connectx-3 Pro 乙太網路介面卡|
   ---

2. 檢視網路介面卡內容。

   ```PowerShell
   Get-NetAdapter | ft -AutoSize
   ```

   _**結果：**_

   |名稱 |InterfaceDescription | ifIndex |狀態 |MacAddress |LinkSpeed|
   |---- |-------------------- |-------| ------|----------|---------|
   |vEthernet \(VMSTEST\) |HYPER-V 虛擬乙太網路介面卡 #2|27 |Up |E4-1D-2D-07-40-71 |40 Gbps|
   ---

3. 管理主機 vNIC，在兩種方式之一。 

   - **NetAdapter**檢視的運作方式取決於 「 vEthernet \(VMSTEST\)」 名稱。 在此檢視中，顯示並非所有的網路介面卡內容。
   - **VMNetworkAdapter**檢視卸除 「 vEthernet"前置詞，並會直接使用 vmswitch 名稱。 (建議使用) 

   ```PowerShell
   Get-VMNetworkAdapter –ManagementOS | ft -AutoSize
   ```

   _**結果：**_

   |名稱 |IsManagementOs |VMName |SwitchName |MacAddress |狀態 |IPAddresses|
   |----|-------------- |------ |----------|----------|------ |-----------|
   |CORP-External-Switch |True |CORP-External-Switch| 001B785768AA |{[確定]} |&nbsp;|
   |VMSTEST |True |VMSTEST | E41D2D074071| {[確定]} | &nbsp;| 
   ---

4. 測試連接。

   ```Powershell    
   Test-NetConnection 192.168.1.5
   ```

   _**結果：**_ 

   ```
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (CORP-External-Switch)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
   ```

5. 指派，並檢視網路介面卡的 VLAN 設定。
    
   ```PowerShell
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "VMSTEST" -VlanId "101" -Access -ManagementOS
   Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName "VMSTEST"
   ```    

   _**結果：**_

   |VMName |VMNetworkAdapterName |模式 |VlanList|
   |------ |-------------------- |---- |--------|
   |&nbsp;|VMSTEST |存取權 |101   |
   ---  
 
6. 測試連接。<p>可能需要幾秒鐘的時間完成，才可成功 ping 到另一個介面卡。  

   ```PowerShell    
   Test-NetConnection 192.168.1.5
   ```

   _**結果：**_
     
   ```
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (VMSTEST)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
   ```

## <a name="step-9-test-hyper-v-virtual-switch-rdma-mode-2"></a>步驟 9： 測試 HYPER-V 虛擬交換器 RDMA （模式 2）

下圖說明您的 HYPER-V 主機，包括 HYPER-V 主機 1 上的 vSwitch 的目前狀態。

![測試 HYPER-V 虛擬交換器](../../media/Converged-NIC/6-single-test-vswitch-rdma.jpg)


1. 設定主機 vNIC 上標記的優先順序。

   ```PowerShell    
   Set-VMNetworkAdapter -ManagementOS -Name "VMSTEST" -IeeePriorityTag on
   Get-VMNetworkAdapter -ManagementOS -Name "VMSTEST" | fl Name,IeeePriorityTag
   ```  
   
   _**結果：**_

    名稱：VMSTEST IeeePriorityTag:開啟


2. 檢視網路介面卡 RDMA 資訊。 

   ```PowerShell
   Get-NetAdapterRdma
   ```   

   _**結果：**_

   |名稱 |InterfaceDescription |Enabled |
   |---- |-------------------- |-------|
   |vEthernet \(VMSTEST\)| HYPER-V 虛擬乙太網路介面卡 #2|False|
   ---

   >[!NOTE]
   >如果參數**Enabled**具有值**False**，這表示，不會啟用 RDMA。
    

3. 檢視網路介面卡資訊。

   ```PowerShell
   Get-NetAdapter
   ```

   _**結果：**_   
 
   |名稱 |InterfaceDescription |ifIndex |狀態 |MacAddress |LinkSpeed|
   |----|--------------------|-------|------|----------|---------|
   |vEthernet (VMSTEST)|HYPER-V 虛擬乙太網路介面卡 #2|27|Up|E4-1D-2D-07-40-71|40 Gbps|
   ---


4. 在主機 vNIC 上啟用 RDMA。

   ```PowerShell
   Enable-NetAdapterRdma -Name "vEthernet (VMSTEST)"
   Get-NetAdapterRdma -Name "vEthernet (VMSTEST)"
   ```

   _**結果：**_

   |名稱 |InterfaceDescription |Enabled |
   |---- |-------------------- |-------|
   |vEthernet \(VMSTEST\)| HYPER-V 虛擬乙太網路介面卡 #2|True|
   ---

   >[!NOTE]
   >如果參數**Enabled**具有值 **，則為 True**，這表示，會啟用 RDMA。

5. 執行 RDMA 流量測試。

   ```PowerShell    
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
   ```

在此輸出中，最後一行"RDMA 流量測試成功：RDMA 流量傳送至 192.168.1.5，「 顯示，您已成功在您的配接器上設定交集的 NIC。

## <a name="related-topics"></a>相關主題
- [交集的 NIC 組合的 NIC 設定](cnic-datacenter.md)
- [交集的 NIC 的實體交換器組態](cnic-app-switch-config.md)
- [疑難排解交集 NIC 組態](cnic-app-troubleshoot.md)
