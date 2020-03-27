---
title: 具有單一網路介面卡的聚合式 NIC 設定
description: 在本主題中，我們會提供指示，說明如何使用 Hyper-v 主機中的單一 NIC 來設定交集 NIC。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: eed5c184-fa55-43a8-a879-b1610ebc70ca
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/14/2018
ms.openlocfilehash: 5a088df043190de9e7f1df4dccdc2fc832751093
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309626"
---
# <a name="converged-nic-configuration-with-a-single-network-adapter"></a>具有單一網路介面卡的聚合式 NIC 設定

>適用於：Windows Server (半年通道)、Windows Server 2016

在本主題中，我們會提供指示，說明如何使用 Hyper-v 主機中的單一 NIC 來設定交集 NIC。

本主題中的範例設定說明兩個 Hyper-v 主機、 **Hyper-v 主機 A**和**hyper-v 主機 B**。兩部主機都已安裝單一實體 NIC （pNIC），而 Nic 會連線到機架 \(ToR\) 實體交換器的頂端。 此外，主機位於相同的子網，也就是 192.168.1. x/24。

![Hyper-V 主機](../../media/Converged-NIC/1-single-test-conn.jpg)


## <a name="step-1-test-the-connectivity-between-source-and-destination"></a>步驟 1. 測試來源與目的地之間的連線能力

確定實體 NIC 可以連接到目的地主機。 這項測試會使用第3層 \(L3\) 或 IP 層，以及第2層 \(L2\)來示範連線能力。

1. 查看網路介面卡屬性。

   ```PowerShell
   Get-NetAdapter
   ```

   _**更**_  


   | 名稱 |    InterfaceDescription     | IfIndex | 狀態 |    MacAddress     | LinkSpeed |
   |------|-----------------------------|---------|--------|-------------------|-----------|
   |  M1  | Mellanox ConnectX-3 Pro 。 |    4    |   上移   | 7C-FE-90-93-8F-A1 |  40 Gbps  |

   ---

2. 查看其他介面卡屬性，包括 IP 位址。

   ```PowerShell
   Get-NetAdapter M1 | fl *
   ```

   _**更**_

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

## <a name="step-2-ensure-that-source-and-destination-can-communicate"></a>步驟 2. 確定來源和目的地可以通訊

在此步驟中，我們會使用**Test-netconnection** Windows PowerShell 命令，但如果您想要的話，也可以使用**ping**命令。 

>[!TIP]
>如果您確定主機可以彼此通訊，可以略過此步驟。

1. 確認雙向通訊。

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**更**_


   |        參數         |    值    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.1.5 |
   |      RemoteAddress       | 192.168.1.5 |
   |      InterfaceAlias      |     M1      |
   |      SourceAddress       | 192.168.1.3 |
   |      PingSucceeded       |    True     |
   | PingReplyDetails \(RTT\) |    0毫秒     |

   ---

   在某些情況下，您可能需要停用 [具有 Advanced Security 的 Windows 防火牆]，才能順利執行這項測試。 如果您停用防火牆，請記住安全性，並確定您的設定符合您組織的安全性需求。

2. 停用所有防火牆設定檔。

   ```PowerShell
   Set-NetFirewallProfile -All -Enabled False
   ```

3. 停用防火牆設定檔之後，請再次測試連線。 

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**更**_


   |        參數         |    值    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.1.5 |
   |      RemoteAddress       | 192.168.1.5 |
   |      InterfaceAlias      | 測試-40G-1  |
   |      SourceAddress       | 192.168.1.3 |
   |      PingSucceeded       |    False    |
   | PingReplyDetails \(RTT\) |    0毫秒     |

   ---



## <a name="step-3-optional-configure-the-vlan-ids-for-nics-installed-in-your-hyper-v-hosts"></a>步驟 3： 選擇性設定安裝在 Hyper-v 主機中的 Nic 的 VLAN 識別碼

許多網路設定都會使用 Vlan，如果您打算在網路中使用 Vlan，就必須重複先前的測試，並設定 Vlan。 此外，如果您打算將 RoCE 用於 RDMA 服務，則必須啟用 Vlan。

在此步驟中，Nic 處於**存取**模式。 不過，當您在本指南稍後的 \(vSwitch 建立 Hyper-v 虛擬交換器\) 時，會在 vSwitch 埠層級套用 VLAN 屬性。 

由於交換器可以裝載多個 Vlan，因此在機架的頂端 \(ToR\) 實體交換器必須擁有主機連線的埠，以在主幹模式中設定。

>[!NOTE]
>如需有關如何在交換器上設定主幹模式的指示，請參閱 ToR 交換器檔。

下圖顯示兩部 Hyper-v 主機，每個都有一個實體網路介面卡，且每個都設定為在 VLAN 101 上進行通訊。

![設定虛擬區域網路絡](../../media/Converged-NIC/2-single-configure-vlans.jpg)


>[!IMPORTANT]
>在本機和目的地伺服器上執行此操作。 如果目的地伺服器未設定為使用與本機伺服器相同的 VLAN ID，則兩者無法通訊。


1. 設定安裝在 Hyper-v 主機中的 Nic 的 VLAN ID。

   >[!IMPORTANT]
   >如果您是透過此介面從遠端連線到主機，請不要執行此命令，因為這會導致無法存取主機。

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name M1 -RegistryKeyword VlanID -RegistryValue "101"
   Get-NetAdapterAdvancedProperty -Name M1 | Where-Object {$_.RegistryKeyword -eq "VlanID"} 
   ```

   _**更**_


   | 名稱 | DisplayName | DisplayValue | RegistryKeyword | RegistryValue |
   |------|-------------|--------------|-----------------|---------------|
   |  M1  |   VLAN 識別碼   |     101      |     VlanID      |     {101}     |

   ---

2. 重新開機網路介面卡以套用 VLAN ID。

   ```PowerShell
   Restart-NetAdapter -Name "M1"
   ```

3. 確定狀態為 [已**啟動**]。

   ```PowerShell
   Get-NetAdapter -Name "M1"
   ```

   _**更**_


   | 名稱 |          InterfaceDescription           | IfIndex | 狀態 |    MacAddress     | LinkSpeed |
   |------|-----------------------------------------|---------|--------|-------------------|-----------|
   |  M1  | Mellanox ConnectX-3 Pro Ethernet 的 Ada 。 |    4    |   上移   | 7C-FE-90-93-8F-A1 |  40 Gbps  |

   ---

   >[!IMPORTANT]
   >可能需要幾秒鐘的時間，裝置才會重新開機並在網路上變成可用。 

4. 確認連線能力。<p>如果連線失敗，請檢查相同 VLAN 中的交換器 VLAN 設定或目的地參與。 

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

## <a name="step-4-configure-quality-of-service-qos"></a>步驟 4. 設定服務品質 \(QoS\)

>[!NOTE]
>您必須在所有想要彼此通訊的主機上執行下列所有 DCB 和 QoS 設定步驟。

1. 在每部 Hyper-v 主機上安裝資料中心橋接 \(DCB\)。

   - **選擇性**適用于使用 IWARP 進行 RDMA 服務的網路設定。
   - 使用 RoCE \(任何 RDMA 服務版本\) 的網路設定**所需**。

   ```PowerShell
   Install-WindowsFeature Data-Center-Bridging
   ```

2. 設定 SMB 直接傳輸的 QoS 原則：

   - **選用**，適用于使用 iWarp 的網路設定。
   - 使用 RoCE 的網路設定**所需**。

   在下面的範例命令中，值 "3" 是任意的。 只要您在 QoS 原則的設定中一致使用相同的值，您就可以使用介於1到7之間的任何值。

   ```PowerShell
   New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3
   ```

   _**更**_


   |   參數    |          值           |
   |----------------|--------------------------|
   |      名稱      |           SMB            |
   |     擁有者      | 群組原則 \(機\) |
   | NetworkProfile |           全部            |
   |   優先順序   |           127            |
   |   JobObject    |          &nbsp;          |
   | NetDirectPort  |           445            |
   | PriorityValue  |            3             |

   ---

3. 針對 RoCE 部署，開啟 SMB 流量的**優先順序流程式控制制**，這不是 iWarp 所需。

   ```PowerShell
   Enable-NetQosFlowControl -priority 3
   Get-NetQosFlowControl
   ```

   _**更**_


   | 優先順序 | 已啟用 | PolicySet | IfIndex | IfAlias |
   |----------|---------|-----------|---------|---------|
   |    0     |  False  |  全域   | &nbsp;  | &nbsp;  |
   |    1     |  False  |  全域   | &nbsp;  | &nbsp;  |
   |    2     |  False  |  全域   | &nbsp;  | &nbsp;  |
   |    3     |  True   |  全域   | &nbsp;  | &nbsp;  |
   |    4     |  False  |  全域   | &nbsp;  | &nbsp;  |
   |    5     |  False  |  全域   | &nbsp;  | &nbsp;  |
   |    6     |  False  |  全域   | &nbsp;  | &nbsp;  |
   |    7     |  False  |  全域   | &nbsp;  | &nbsp;  |

   ---

4. 啟用本機和目的地網路介面卡的 QoS。

   - **不需要**使用 iWarp 的網路設定。
   - 使用 RoCE 的網路設定**所需**。

   ```PowerShell
   Enable-NetAdapterQos -InterfaceAlias "M1"
   Get-NetAdapterQos -Name "M1"
   ```

   _**更**_

   **名稱**： M1  
   **已啟用**： True  

   _**功能**_   


   |      參數      |   硬體   |   目前    |
   |---------------------|--------------|--------------|
   |    MacSecBypass     | NotSupported | NotSupported |
   |     DcbxSupport     |     無     |     無     |
   | NumTCs （Max/ETS/PFC） |    8/8/8     |    8/8/8     |

   ---

   _**OperationalTrafficClasses:**_ 


   | TC | TSA | 頻寬 | 重點 |
   |----|-----|-----------|------------|
   | 0  | ETS |    70%    |  0-2、4-7   |
   | 1  | ETS |    大約    |     3      |

   ---

   _**OperationalFlowControl:**_  

   已啟用優先順序3  

   _**OperationalClassifications:**_  


   | 通訊協定  | 埠/類型 | 優先順序 |
   |-----------|-----------|----------|
   |  預設  |  &nbsp;   |    0     |
   | NetDirect |    445    |    3     |

   ---

5. 保留 SMB Direct \(RDMA\)的頻寬百分比。

    在此範例中，會使用30% 的頻寬保留。 您應該選取一個值，代表您預期儲存體流量所需的內容。 

   ```PowerShell
   New-NetQosTrafficClass "SMB" -Priority 3 -BandwidthPercentage 30 -Algorithm ETS
   ```

   _**更**_


   | 名稱 | 演算法 | 頻寬（%） | 優先順序 | PolicySet | IfIndex | IfAlias |
   |------|-----------|--------------|----------|-----------|---------|---------|
   | SMB  |    ETS    |      30      |    3     |  全域   | &nbsp;  | &nbsp;  |

   ---                                      

6. 查看頻寬保留設定。  

   ```PowerShell
   Get-NetQosTrafficClass
   ```

   _**更**_


   |   名稱    | 演算法 | 頻寬（%） | 優先順序 | PolicySet | IfIndex | IfAlias |
   |-----------|-----------|--------------|----------|-----------|---------|---------|
   | [預設] |    ETS    |      70      | 0-2、4-7  |  全域   | &nbsp;  | &nbsp;  |
   |    SMB    |    ETS    |      30      |    3     |  全域   | &nbsp;  | &nbsp;  |

   ---

## <a name="step-5-optional-resolve-the-mellanox-adapter-debugger-conflict"></a>步驟 5. 選擇性解決 Mellanox 介面卡偵錯工具衝突 

根據預設，當使用 Mellanox 介面卡時，附加的偵錯工具會封鎖 NetQos，這是已知的問題。 因此，如果您是從 Mellanox 使用介面卡，並想要附加偵錯工具，請使用下列命令來解決此問題。 如果您不想要附加偵錯工具，或如果您不是使用 Mellanox 介面卡，則不需要執行此步驟。

   ```PowerShell    
   Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 –Force
   ``` 

## <a name="step-6-verify-the-rdma-configuration-native-host"></a>步驟 6. 確認 RDMA 設定（原生主機）

您想要確保在建立 vSwitch 並轉換至 RDMA （交集 NIC）之前，已正確設定網狀架構。 

下圖顯示 Hyper-v 主機的目前狀態。

![測試 RDMA](../../media/Converged-NIC/4-single-test-rdma.jpg)

1. 確認 RDMA 設定。

   ```PowerShell
   Get-NetAdapterRdma
   ```
   _**更**_


   | 名稱 |           InterfaceDescription           | 已啟用 |
   |------|------------------------------------------|---------|
   |  M1  | Mellanox ConnectX-3 Pro 乙太網路介面卡 |  True   |

   ---

2. 判斷目標介面卡的**ifIndex**值。<p>當您執行下載的腳本時，會在後續步驟中使用此值。

   ```PowerShell
   Get-NetIPConfiguration -InterfaceAlias "M*" | ft InterfaceAlias,InterfaceIndex,IPv4Address
   ```

   _**更**_ 


   | InterfaceAlias | InterfaceIndex |  IPv4Address  |
   |----------------|----------------|---------------|
   |       M2       |       14       | {192.168.1.5} |

   ---

3. 下載[DiskSpd 公用程式](https://aka.ms/diskspd)，並將其解壓縮至 C:\TEST\.

4. 將[測試-RDMA powershell 腳本](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)下載到本機磁片磁碟機上的測試檔案夾，例如，C:\TEST\.

5. 執行**Test-Rdma** PowerShell 腳本，將 ifIndex 值連同相同 VLAN 上遠端介面卡的 IP 位址傳遞給腳本。<p>在此範例中，腳本會在遠端網路介面卡 IP 位址192.168.1.5 上傳遞14的**ifIndex**值。

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
   >如果 RDMA 流量失敗，特別針對 RoCE 案例，請參閱 ToR 交換器設定，以取得應符合主機設定的適當 PFC/ETS 設定。 如需參考值，請參閱本檔中的 QoS 一節。

## <a name="step-7-remove-the-access-vlan-setting"></a>步驟 7： 移除存取 VLAN 設定

在準備建立 Hyper-v 交換器時，您必須移除先前安裝的 VLAN 設定。  

1. 從實體 NIC 移除存取 VLAN 設定，以防止 NIC 以不正確的 VLAN ID 自動標記輸出流量。<p>移除此設定也會讓它無法篩選不符合存取 VLAN 識別碼的輸入流量。

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name M1 -RegistryKeyword VlanID -RegistryValue "0"
   ```    

2. 確認**VlanID 設定**將 [VLAN ID] 值顯示為零。

   ```PowerShell    
   Get-NetAdapterAdvancedProperty -name m1 | Where-Object {$_.RegistryKeyword -eq 'VlanID'} 
   ```  


## <a name="step-8-create-a-hyper-v-vswitch-on-your-hyper-v-hosts"></a>步驟 8： 在您的 Hyper-v 主機上建立 Hyper-v vSwitch

下圖描述 Hyper-v 主機1與 vSwitch。

![建立 Hyper-v 虛擬交換器](../../media/Converged-NIC/5-single-create-vswitch.jpg)


1. 在 hyper-v 主機 A 上的 Hyper-v 中建立外部 Hyper-v vSwitch。 <p>在此範例中，參數的名稱為 VMSTEST。 此外，參數**AllowManagementOS**會建立一個主機 vNIC，以繼承實體 NIC 的 MAC 和 IP 位址。

   ```PowerShell
   New-VMSwitch -Name VMSTEST -NetAdapterName "M1" -AllowManagementOS $true
   ```
   _**更**_


   |  名稱   | Switchtype |      NetAdapterInterfaceDescription      |
   |---------|------------|------------------------------------------|
   | VMSTEST |  外部  | Mellanox ConnectX-3 Pro 乙太網路介面卡 |

   ---

2. 查看網路介面卡屬性。

   ```PowerShell
   Get-NetAdapter | ft -AutoSize
   ```

   _**更**_


   |         名稱          |        InterfaceDescription         | IfIndex | 狀態 |    MacAddress     | LinkSpeed |
   |-----------------------|-------------------------------------|---------|--------|-------------------|-----------|
   | vEthernet \(VMSTEST\) | Hyper-v 虛擬乙太網路介面卡 #2 |   27    |   上移   | E4-1D-2D-07-40-71 |  40 Gbps  |

   ---

3. 以兩種方式的其中一種來管理主機 vNIC。 

   - **Get-netadapter** view 會根據 "VETHERNET \(VMSTEST\)" 名稱來運作。 並非所有的網路介面卡屬性都會顯示在此視圖中。
   - **VMNetworkAdapter** view 會卸載 "vEthernet" 前置詞，而只會使用 vmswitch 名稱。 (建議使用) 

   ```PowerShell
   Get-VMNetworkAdapter –ManagementOS | ft -AutoSize
   ```

   _**更**_


   |         名稱         | IsManagementOs |        VMName        |  SwitchName  | MacAddress | 狀態 | IPAddresses |
   |----------------------|----------------|----------------------|--------------|------------|--------|-------------|
   | CORP-External-交換器 |      True      | CORP-External-交換器 | 001B785768AA |    @    | &nbsp; |             |
   |       VMSTEST        |      True      |       VMSTEST        | E41D2D074071 |    @    | &nbsp; |             |

   ---

4. 測試連接。

   ```Powershell    
   Test-NetConnection 192.168.1.5
   ```

   _**更**_ 

   ```
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (CORP-External-Switch)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
   ```

5. 指派和查看網路介面卡 VLAN 設定。

   ```PowerShell
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "VMSTEST" -VlanId "101" -Access -ManagementOS
   Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName "VMSTEST"
   ```    

   _**更**_


   | VMName | VMNetworkAdapterName |  Mode  | Vlanlist 下面 |
   |--------|----------------------|--------|----------|
   | &nbsp; |       VMSTEST        | 存取 |   101    |

   ---  

6. 測試連接。<p>可能需要幾秒鐘的時間才會成功，才能順利 ping 其他介面卡。  

   ```PowerShell    
   Test-NetConnection 192.168.1.5
   ```

   _**更**_

   ```
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (VMSTEST)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
   ```

## <a name="step-9-test-hyper-v-virtual-switch-rdma-mode-2"></a>步驟 9： 測試 Hyper-v 虛擬交換器 RDMA （模式2）

下圖描述 Hyper-v 主機的目前狀態，包括 Hyper-v 主機1上的 vSwitch。

![測試 Hyper-v 虛擬交換器](../../media/Converged-NIC/6-single-test-vswitch-rdma.jpg)


1. 在主機 vNIC 上設定優先順序標記。

   ```PowerShell    
   Set-VMNetworkAdapter -ManagementOS -Name "VMSTEST" -IeeePriorityTag on
   Get-VMNetworkAdapter -ManagementOS -Name "VMSTEST" | fl Name,IeeePriorityTag
   ```  

   _**更**_

    名稱： VMSTEST IeeePriorityTag： On


2. 查看網路介面卡 RDMA 資訊。 

   ```PowerShell
   Get-NetAdapterRdma
   ```   

   _**更**_


   |         名稱          |        InterfaceDescription         | 已啟用 |
   |-----------------------|-------------------------------------|---------|
   | vEthernet \(VMSTEST\) | Hyper-v 虛擬乙太網路介面卡 #2 |  False  |

   ---

   >[!NOTE]
   >如果**已啟用**參數的值為**False**，則表示未啟用 RDMA。


3. 查看網路介面卡資訊。

   ```PowerShell
   Get-NetAdapter
   ```

   _**更**_   


   |        名稱         |        InterfaceDescription         | IfIndex | 狀態 |    MacAddress     | LinkSpeed |
   |---------------------|-------------------------------------|---------|--------|-------------------|-----------|
   | vEthernet （VMSTEST） | Hyper-v 虛擬乙太網路介面卡 #2 |   27    |   上移   | E4-1D-2D-07-40-71 |  40 Gbps  |

   ---


4. 在主機 vNIC 上啟用 RDMA。

   ```PowerShell
   Enable-NetAdapterRdma -Name "vEthernet (VMSTEST)"
   Get-NetAdapterRdma -Name "vEthernet (VMSTEST)"
   ```

   _**更**_


   |         名稱          |        InterfaceDescription         | 已啟用 |
   |-----------------------|-------------------------------------|---------|
   | vEthernet \(VMSTEST\) | Hyper-v 虛擬乙太網路介面卡 #2 |  True   |

   ---

   >[!NOTE]
   >如果**已啟用**參數的值**為 True**，則表示已啟用 RDMA。

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

在此輸出中的最後一行「RDMA 流量測試成功： RDMA 流量已傳送至192.168.1.5」，顯示您已成功在介面卡上設定聚合式 NIC。

## <a name="related-topics"></a>相關主題
- [聚合式 NIC 組合 NIC 設定](cnic-datacenter.md)
- [聚合式 NIC 的實體交換器設定](cnic-app-switch-config.md)
- [針對聚合式 NIC 設定進行疑難排解](cnic-app-troubleshoot.md)
