---
title: 混合式 NIC 設定（datacenter）中的交集式網路介面卡
description: 在本主題中，我們將提供指示，說明如何使用交換器內嵌小組（SET），在組合的 NIC 設定中部署聚合式 NIC。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: f01546f8-c495-4055-8492-8806eee99862
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/17/2018
ms.openlocfilehash: e4c305a7c8c4c4618b0df1e1b2a646356d8f821f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356116"
---
# <a name="converged-nic-in-a-teamed-nic-configuration-datacenter"></a>混合式 NIC 設定（datacenter）中的交集式網路介面卡

>適用於：Windows Server (半年度管道)、Windows Server 2016

在本主題中，我們將提供指示，說明如何使用交換器內嵌\(小組集合，在組合的 nic 設定\)中部署聚合式 nic。 

本主題中的範例設定會說明兩個 Hyper-v 主機、 **Hyper-v 主機 1**和**hyper-v 主機 2**。 兩部主機都有兩張網路介面卡。 在每部主機上，一張介面卡會連線到 192.168.1. x/24 子網，而其中一個介面卡會連線到 192.168.2/24 子網。

![Hyper-V 主機](../../media/Converged-NIC/1-datacenter-test-conn.jpg)

## <a name="step-1-test-the-connectivity-between-source-and-destination"></a>步驟 1. 測試來源與目的地之間的連線能力

確定實體 NIC 可以連接到目的地主機。  這項測試會使用第 3 \(層 L3\)或 IP 層，以及第 2 \(層 L2\)虛擬區域網路絡\(VLAN\)來示範連線能力。

1. 查看第一個網路介面卡屬性。

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
   ```

   _**更**_


   |    Name    |           InterfaceDescription           | ifIndex | 狀態 |    macAddress     | LinkSpeed |
   |------------|------------------------------------------|---------|--------|-------------------|-----------|
   | 測試-40G-1 | Mellanox ConnectX-3 Pro 乙太網路介面卡 |   11    |   Up   | E4-1D-2D-07-43-D0 |  40 Gbps  |

   ---

2. 查看第一個介面卡的其他屬性，包括 IP 位址。 

   ```PowerShell
   Get-NetIPAddress -InterfaceAlias "Test-40G-1"
   Get-NetIPAddress -InterfaceAlias "TEST-40G-1" | Where-Object {$_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress
   ```

   _**更**_


   |   參數    |    值    |
   |----------------|-------------|
   |   IPAddress    | 192.168.1.3 |
   | InterfaceIndex |     11      |
   | InterfaceAlias | 測試-40G-1  |
   | AddressFamily  |    IPv4     |
   |      Type      |   單點傳播   |
   |  PrefixLength  |     24      |

   ---

3. 查看第二張網路介面卡屬性。

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-2" | ft -AutoSize
   ```

   _**更**_


   |    Name    |          InterfaceDescription           | ifIndex | 狀態 |    macAddress     | LinkSpeed |
   |------------|-----------------------------------------|---------|--------|-------------------|-----------|
   | 測試-40G-2 | Mellanox ConnectX-3 Pro Ethernet A ... #2 |   13    |   Up   | E4-1D-2D-07-40-70 |  40 Gbps  |

   ---

4. 查看第二張介面卡的其他屬性，包括 IP 位址。

   ```PowerShell
   Get-NetIPAddress -InterfaceAlias "Test-40G-2"
   Get-NetIPAddress -InterfaceAlias "Test-40G-2" | Where-Object {$_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress
   ```

   _**更**_


   |   參數    |    值    |
   |----------------|-------------|
   |   IPAddress    | 192.168.2.3 |
   | InterfaceIndex |     13      |
   | InterfaceAlias | 測試-40G-2  |
   | AddressFamily  |    IPv4     |
   |      Type      |   單點傳播   |
   |  PrefixLength  |     24      |

   ---

5. 確認其他 NIC 小組或設定成員 pNICs 具有有效的 IP 位址。<p>使用個別的子網\(xxx.xxx。**2**. xxx 與 xxx.xxx。**1**. xxx\)，協助從這個介面卡傳送到目的地。 否則，如果您在相同的子網上找到這兩個 pNICs，則 Windows TCP/IP 堆疊會在介面之間進行負載平衡，而簡單的驗證會變得更複雜。


## <a name="step-2-ensure-that-source-and-destination-can-communicate"></a>步驟 2. 確定來源和目的地可以通訊

在此步驟中，我們會使用**Test-netconnection** Windows PowerShell 命令，但如果您想要的話，也可以使用**ping**命令。 

1. 確認雙向通訊。

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
   |      PingSucceeded       |    偽    |
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
   |      PingSucceeded       |    偽    |
   | PingReplyDetails \(RTT\) |    0毫秒     |

   ---


4. 確認其他 Nic 的連線能力。 針對 NIC 或設定小組中包含的所有後續 pNICs 重複上述步驟。

   ```PowerShell    
   Test-NetConnection 192.168.2.5
   ```

   _**更**_


   |        參數         |    值    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.2.5 |
   |      RemoteAddress       | 192.168.2.5 |
   |      InterfaceAlias      | 測試-40G-2  |
   |      SourceAddress       | 192.168.2.3 |
   |      PingSucceeded       |    偽    |
   | PingReplyDetails \(RTT\) |    0毫秒     |

   ---

## <a name="step-3-configure-the-vlan-ids-for-nics-installed-in-your-hyper-v-hosts"></a>步驟 3。 設定安裝在 Hyper-v 主機中的 Nic 的 VLAN 識別碼

許多網路設定都會使用 Vlan，如果您打算在網路中使用 Vlan，就必須重複先前的測試，並設定 Vlan。

在此步驟中，Nic 處於**存取**模式。 不過，當您稍後在本指南中建立 hyper-v \(虛擬\)交換器 vSwitch 時，會在 vSwitch 埠層級套用 VLAN 屬性。 

由於交換器可以裝載多個 vlan，因此，機架\(ToR\)實體交換器的頂端必須擁有主機連線的埠，以在主幹模式中設定。

>[!NOTE]
>如需有關如何在交換器上設定主幹模式的指示，請參閱 ToR 交換器檔。

下圖顯示兩個具有兩個網路介面卡的 Hyper-v 主機，分別在網路介面卡內容中設定 VLAN 101 和 VLAN 102。

![設定虛擬區域網路絡](../../media/Converged-NIC/2-datacenter-configure-vlans.jpg)


>[!TIP]
>根據美國電氣和\(電子網工程師的 IEEE\)網路標準，實體 NIC 中的服務\(QoS\)屬性品質會在內嵌的 802.1 p 標頭上採取行動當您設定 VLAN \(識別碼\)時，請在 802.1 q VLAN 標頭中。

1. 設定第一個 NIC 上的 VLAN ID，測試-40G-1。

   ```PowerShell    
   Set-NetAdapterAdvancedProperty -Name "Test-40G-1" -RegistryKeyword VlanID -RegistryValue "101"
   Get-NetAdapterAdvancedProperty -Name "Test-40G-1" | Where-Object {$_.RegistryKeyword -eq "VlanID"} | ft -AutoSize
   ```

   _**更**_   


   |    Name    | DisplayName | DisplayValue | RegistryKeyword | RegistryValue |
   |------------|-------------|--------------|-----------------|---------------|
   | 測試-40G-1 |   VLAN 識別碼   |     101      |     VlanID      |     {101}     |

   ---

2. 重新開機網路介面卡以套用 VLAN ID。

   ```PowerShell
   Restart-NetAdapter -Name "Test-40G-1"
   ```

3. 確定狀態為 [已**啟動**]。

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
   ```

   _**更**_


   |    Name    |          InterfaceDescription           | ifIndex | 狀態 |    macAddress     | LinkSpeed |
   |------------|-----------------------------------------|---------|--------|-------------------|-----------|
   | 測試-40G-1 | Mellanox ConnectX-3 Pro Ethernet 的 Ada 。 |   11    |   Up   | E4-1D-2D-07-43-D0 |  40 Gbps  |

   ---

4. 設定第二個 NIC 上的 VLAN ID，測試-40G-2。

   ```PowerShell    
   Set-NetAdapterAdvancedProperty -Name "Test-40G-2" -RegistryKeyword VlanID -RegistryValue "102"
   Get-NetAdapterAdvancedProperty -Name "Test-40G-2" | Where-Object {$_.RegistryKeyword -eq "VlanID"} | ft -AutoSize
   ``` 

   _**更**_


   |    Name    | DisplayName | DisplayValue | RegistryKeyword | RegistryValue |
   |------------|-------------|--------------|-----------------|---------------|
   | 測試-40G-2 |   VLAN 識別碼   |     102      |     VlanID      |     {102}     |

   ---

5. 重新開機網路介面卡以套用 VLAN ID。

   ```PowerShell
   Restart-NetAdapter -Name "Test-40G-2" 
   ```

6. 確定狀態為 [已**啟動**]。

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
   ```

   _**更**_


   |    Name    |          InterfaceDescription           | ifIndex | 狀態 |    macAddress     | LinkSpeed |
   |------------|-----------------------------------------|---------|--------|-------------------|-----------|
   | 測試-40G-2 | Mellanox ConnectX-3 Pro Ethernet 的 Ada 。 |   11    |   Up   | E4-1D-2D-07-43-D1 |  40 Gbps  |

   ---

   >[!IMPORTANT]
   >可能需要幾秒鐘的時間，裝置才會重新開機並在網路上變成可用。 

7. 確認第一個 NIC 的連線能力，測試-40G-1。<p>如果連線失敗，請檢查相同 VLAN 中的交換器 VLAN 設定或目的地參與。 

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**更**_   


   |        參數         |    值    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.1.5 |
   |      RemoteAddress       | 192.168.1.5 |
   |      InterfaceAlias      | 測試-40G-1  |
   |      SourceAddress       | 192.168.1.5 |
   |      PingSucceeded       |    True     |
   | PingReplyDetails \(RTT\) |    0毫秒     |

   ---

8. 確認第一個 NIC 的連線能力，測試-40G-2。<p>如果連線失敗，請檢查相同 VLAN 中的交換器 VLAN 設定或目的地參與。

   ```PowerShell    
   Test-NetConnection 192.168.2.5
   ```

   _**更**_    


   |        參數         |    值    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.2.5 |
   |      RemoteAddress       | 192.168.2.5 |
   |      InterfaceAlias      | 測試-40G-2  |
   |      SourceAddress       | 192.168.2.3 |
   |      PingSucceeded       |    True     |
   | PingReplyDetails \(RTT\) |    0毫秒     |

   ---

   >[!IMPORTANT]
   >在您執行**重新開機 get-netadapter**之後，**測試 test-netconnection**或 ping 失敗不會立即發生。  因此，請等候網路介面卡完全初始化，然後再試一次。
   >
   >如果 VLAN 101 連線正常，但 VLAN 102 連線不存在，則問題可能是交換器必須設定為允許在所需 VLAN 上的埠流量。 您可以藉由暫時將失敗的介面卡設定為 VLAN 101，並重複連線測試，來檢查這項功能。


   下圖顯示成功設定 Vlan 之後的 Hyper-v 主機。

   ![設定服務品質](../../media/Converged-NIC/3-datacenter-configure-qos.jpg)


## <a name="step-4-configure-quality-of-service-qos"></a>步驟 4. 設定服務\(品質 QoS\)

>[!NOTE]
>您必須在所有想要彼此通訊的主機上執行下列所有 DCB 和 QoS 設定步驟。

1. 在每個 hyper-v \(主機\)上安裝資料中心橋接 DCB。

   - **選用**，適用于使用 iWarp 的網路設定。
   - 對於使用 RoCE \(任何 RDMA 服務版本\)的網路設定而言是必要的。

   ```PowerShell
   Install-WindowsFeature Data-Center-Bridging
   ```

   _**更**_


   | 成功 | 需要重新開機 | 結束碼 |     功能結果     |
   |---------|----------------|-----------|------------------------|
   |  True   |       否       |  成功  | {資料中心橋接} |

   ---

2. 設定 SMB 直接傳輸的 QoS 原則：

   - **選用**，適用于使用 iWarp 的網路設定。
   - 對於使用 RoCE \(任何 RDMA 服務版本\)的網路設定而言是必要的。

   在下面的範例命令中，值 "3" 是任意的。 只要您在 QoS 原則的設定中一致使用相同的值，您就可以使用介於1到7之間的任何值。

   ```PowerShell
   New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3
   ```

   _**更**_


   |   參數    |          值           |
   |----------------|--------------------------|
   |      Name      |           SMB            |
   |     擁有者      | 群組原則\(機\) |
   | NetworkProfile |           全部            |
   |   優先順序   |           127            |
   |   JobObject    |          &nbsp;          |
   | NetDirectPort  |           445            |
   | PriorityValue  |            3             |

   ---

3. 為介面上的其他流量設定其他 QoS 原則。   

   ```PowerShell
   New-NetQosPolicy "DEFAULT" -Default -PriorityValue8021Action 0
   ```

   _**更**_   


   |   參數    |          值           |
   |----------------|--------------------------|
   |      Name      |         DEFAULT          |
   |     擁有者      | 群組原則\(機\) |
   | NetworkProfile |           全部            |
   |   優先順序   |           127            |
   |    範本    |         預設          |
   |   JobObject    |          &nbsp;          |
   | PriorityValue  |            0             |

   ---

4. 開啟 SMB 流量的**優先順序流程式控制制**，這不是 iWarp 所需。

   ```PowerShell
   Enable-NetQosFlowControl -priority 3
   Get-NetQosFlowControl
   ```

   _**更**_


   | Priority | Enabled | PolicySet | ifIndex | IfAlias |
   |----------|---------|-----------|---------|---------|
   |    0     |  偽  |  全域   | &nbsp;  | &nbsp;  |
   |    1     |  偽  |  全域   | &nbsp;  | &nbsp;  |
   |    2     |  偽  |  全域   | &nbsp;  | &nbsp;  |
   |    3     |  True   |  全域   | &nbsp;  | &nbsp;  |
   |    4     |  偽  |  全域   | &nbsp;  | &nbsp;  |
   |    5     |  偽  |  全域   | &nbsp;  | &nbsp;  |
   |    6     |  偽  |  全域   | &nbsp;  | &nbsp;  |
   |    7     |  偽  |  全域   | &nbsp;  | &nbsp;  |

   ---

   >**重要事項**如果您的結果不符合這些結果，因為3以外的專案具有已啟用的值 True，所以您必須停用這些類別的**FlowControl** 。
   >
   >```PowerShell
   >Disable-NetQosFlowControl -priority 0,1,2,4,5,6,7
   >Get-NetQosFlowControl
   >```
   >在更複雜的設定下，其他流量類別可能需要流量控制，不過這些案例不在本指南的範圍內。


5. 啟用第一個 NIC 的 QoS，測試-40G-1。

   ```PowerShell
   Enable-NetAdapterQos -InterfaceAlias "Test-40G-1"
   Get-NetAdapterQos -Name "Test-40G-1"

   Name: TEST-40G-1 
   Enabled: True
   ```
   _**功能**：_   


   |      參數      |   硬體   |   目前    |
   |---------------------|--------------|--------------|
   |    MacSecBypass     | NotSupported | NotSupported |
   |     DcbxSupport     |     None     |     None     |
   | NumTCs （Max/ETS/PFC） |    8/8/8     |    8/8/8     |

   ---

   _**OperationalTrafficClasses**：_    


   | TC |  TSA   | 頻寬 | 重點 |
   |----|--------|-----------|------------|
   | 0  | [Strict] |  &nbsp;   |    0-7     |

   ---

   _**OperationalFlowControl**：_  

   已啟用優先順序3  

   _**OperationalClassifications**：_  


   | Protocol  | 埠/類型 | Priority |
   |-----------|-----------|----------|
   |  預設  |  &nbsp;   |    0     |
   | NetDirect |    445    |    3     |

   ---

6. 啟用第二個 NIC 的 QoS （測試-40G-2）。

   ```PowerShell
   Enable-NetAdapterQos -InterfaceAlias "Test-40G-2"
   Get-NetAdapterQos -Name "Test-40G-2"

   Name: TEST-40G-2 
   Enabled: True 
   ```

   _**功能**：_ 


   |      參數      |   硬體   |   目前    |
   |---------------------|--------------|--------------|
   |    MacSecBypass     | NotSupported | NotSupported |
   |     DcbxSupport     |     None     |     None     |
   | NumTCs （Max/ETS/PFC） |    8/8/8     |    8/8/8     |

   ---

   _**OperationalTrafficClasses**：_  


   | TC |  TSA   | 頻寬 | 重點 |
   |----|--------|-----------|------------|
   | 0  | [Strict] |  &nbsp;   |    0-7     |

   ---

    _**OperationalFlowControl**：_  

    已啟用優先順序3  

   _**OperationalClassifications**：_  


   | Protocol  | 埠/類型 | Priority |
   |-----------|-----------|----------|
   |  預設  |  &nbsp;   |    0     |
   | NetDirect |    445    |    3     |

   ---


7. 保留 SMB Direct \(RDMA 的一半頻寬\)

   ```PowerShell
   New-NetQosTrafficClass "SMB" -priority 3 -bandwidthpercentage 50 -algorithm ETS
   ```

   _**更**_  


   | Name | 演算法 | 頻寬（%） | Priority | PolicySet | ifIndex | IfAlias |
   |------|-----------|--------------|----------|-----------|---------|---------|
   | SMB  |    ETS    |      50      |    3     |  全域   | &nbsp;  | &nbsp;  |

   ---

8. 查看頻寬保留設定：   

   ```PowerShell
   Get-NetQosTrafficClass | ft -AutoSize
   ```

   _**更**_  


   |   Name    | 演算法 | 頻寬（%） | Priority | PolicySet | ifIndex | IfAlias |
   |-----------|-----------|--------------|----------|-----------|---------|---------|
   | 預設 |    ETS    |      50      | 0-2、4-7  |  全域   | &nbsp;  | &nbsp;  |
   |    SMB    |    ETS    |      50      |    3     |  全域   | &nbsp;  | &nbsp;  |

   ---

9. 選擇性為租使用者 IP 流量建立兩個額外的流量類別。 

   >[!TIP]
   >您可以省略 "IP1" 和 "IP2" 值。

   ```PowerShell
   New-NetQosTrafficClass "IP1" -Priority 1 -bandwidthpercentage 10 -algorithm ETS
   ```

   _**更**_


   | Name | 演算法 | 頻寬（%） | Priority | PolicySet | ifIndex | IfAlias |
   |------|-----------|--------------|----------|-----------|---------|---------|
   | IP1  |    ETS    |      10      |    1     |  全域   | &nbsp;  | &nbsp;  |

   ---

   ```PowerShell
   New-NetQosTrafficClass "IP2" -Priority 2 -bandwidthpercentage 10 -algorithm ETS
   ```

   _**更**_


   | Name | 演算法 | 頻寬（%） | Priority | PolicySet | ifIndex | IfAlias |
   |------|-----------|--------------|----------|-----------|---------|---------|
   | IP2  |    ETS    |      10      |    2     |  全域   | &nbsp;  | &nbsp;  |

   ---

10. 查看 QoS 流量類別。

    ```PowerShell
    Get-NetQosTrafficClass | ft -AutoSize
    ```

    _**更**_


    |   Name    | 演算法 | 頻寬（%） | Priority | PolicySet | ifIndex | IfAlias |
    |-----------|-----------|--------------|----------|-----------|---------|---------|
    | 預設 |    ETS    |      30      |  0、4-7   |  全域   | &nbsp;  | &nbsp;  |
    |    SMB    |    ETS    |      50      |    3     |  全域   | &nbsp;  | &nbsp;  |
    |    IP1    |    ETS    |      10      |    1     |  全域   | &nbsp;  | &nbsp;  |
    |    IP2    |    ETS    |      10      |    2     |  全域   | &nbsp;  | &nbsp;  |

    ---

11. 選擇性覆寫偵錯工具。<p>根據預設，附加的偵錯工具會封鎖 NetQos。 

    ```PowerShell
    Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 –Force
    Get-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" | ft AllowFlowControlUnderDebugger
    ```

    _**更**_  

    ```
    AllowFlowControlUnderDebugger
    -----------------------------
    1
    ```

## <a name="step-5-verify-the-rdma-configuration-mode-1"></a>步驟 5. 確認 RDMA \(設定模式1\) 

您想要確保在建立 vSwitch 並轉換成 RDMA \(模式 2\)之前，已正確設定網狀架構。

下圖顯示 Hyper-v 主機的目前狀態。

![測試 RDMA](../../media/Converged-NIC/4-datacenter-test-rdma.jpg)


1. 確認 RDMA 設定。

   ```PowerShell
   Get-NetAdapterRdma | ft -AutoSize
   ```

   _**更**_


   |    Name    |        InterfaceDescription        | Enabled |
   |------------|------------------------------------|---------|
   | 測試-40G-1 | Mellanox ConnectX-4 VPI 介面卡 #2 |  True   |
   | 測試-40G-2 |  Mellanox ConnectX-4 VPI 介面卡   |  True   |

   ---

2. 判斷目標介面卡的**ifIndex**值。<p>當您執行下載的腳本時，會在後續步驟中使用此值。   

   ```PowerShell
   Get-NetIPConfiguration -InterfaceAlias "TEST*" | ft InterfaceAlias,InterfaceIndex,IPv4Address
   ```

   _**更**_


   | InterfaceAlias | InterfaceIndex |  IPv4Address  |
   |----------------|----------------|---------------|
   |   測試-40G-1   |       14       | {192.168.1.3} |
   |   測試-40G-2   |       13       | {192.168.2.3} |

   ---

3. 下載[DiskSpd 公用程式](https://aka.ms/diskspd)，並將其解壓縮至 C:\TEST\.

4. 將[測試 RDMA PowerShell 腳本](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)下載到本機磁片磁碟機上的測試檔案夾，例如 C:\TEST\.

5. 執行**Test-Rdma** PowerShell 腳本，將 ifIndex 值連同相同 VLAN 上第一個遠端介面卡的 IP 位址傳遞給腳本。<p>在此範例中，腳本會在遠端網路介面卡 IP 位址192.168.1.5 上傳遞14的**ifIndex**值。

   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 14 -IsRoCE $true -RemoteIpAddress 192.168.1.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**更**_ 

   ```   
   VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\diskspd.exe
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

6. 執行**Test-Rdma** PowerShell 腳本，將 ifIndex 值連同相同 VLAN 上第二個遠端介面卡的 IP 位址傳遞給腳本。<p>在此範例中，腳本會在遠端網路介面卡 IP 位址192.168.2.5 上傳遞**ifIndex**值13。

   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 13 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**更**_ 

   ```   
   VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\diskspd.exe
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
   ``` 

## <a name="step-6-create-a-hyper-v-vswitch-on-your-hyper-v-hosts"></a>步驟 6. 在您的 Hyper-v 主機上建立 Hyper-v vSwitch


下圖顯示具有 vSwitch 的 Hyper-v 主機1。

![建立 Hyper-v 虛擬交換器](../../media/Converged-NIC/5-datacenter-create-vswitch.jpg)

1. 在 Hyper-v 主機1上以設定模式建立 vSwitch。

   ```PowerShell
   New-VMSwitch –Name "VMSTEST" –NetAdapterName "TEST-40G-1","TEST-40G-2" -EnableEmbeddedTeaming $true -AllowManagementOS $true
   ```

   _**Result**_


   |  Name   | Switchtype | NetAdapterInterfaceDescription |
   |---------|------------|--------------------------------|
   | VMSTEST |  外部  |        組合-介面        |

   ---

2. 查看集合中的實體介面卡小組。

   ```PowerShell
   Get-VMSwitchTeam -Name "VMSTEST" | fl
   ```

   _**更**_  

   ```
   Name: VMSTEST  
   Id: ad9bb542-dda2-4450-a00e-f96d44bdfbec  
   NetAdapterInterfaceDescription: {Mellanox ConnectX-3 Pro Ethernet Adapter, Mellanox ConnectX-3 Pro Ethernet Adapter #2}  
   TeamingMode: SwitchIndependent  
   LoadBalancingAlgorithm: Dynamic   
   ```

3. 顯示主機 vNIC 的兩個視圖

   ```PowerShell
    Get-NetAdapter
   ```

   _**更**_


   |        Name         |        InterfaceDescription         | ifIndex | 狀態 |    macAddress     | LinkSpeed |
   |---------------------|-------------------------------------|---------|--------|-------------------|-----------|
   | vEthernet （VMSTEST） | Hyper-v 虛擬乙太網路介面卡 #2 |   28    |   Up   | E4-1D-2D-07-40-71 |  80 Gbps  |

   ---

4. 查看主機 vNIC 的其他屬性。 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS
   ```

   _**更**_


   |  Name   | IsManagementOs | VMName  |  SwitchName  | macAddress | 狀態 | IPAddresses |
   |---------|----------------|---------|--------------|------------|--------|-------------|
   | VMSTEST |      True      | VMSTEST | E41D2D074071 |    @    | &nbsp; |             |

   ---


5. 測試連到遠端 VLAN 101 介面卡的網路連線。

   ```PowerShell
   Test-NetConnection 192.168.1.5 
   ```

   _**更**_  

   ```
   WARNING: Ping to 192.168.1.5 failed -- Status: DestinationHostUnreachable

   ComputerName   : 192.168.1.5
   RemoteAddress  : 192.168.1.5
   InterfaceAlias : vEthernet (CORP-External-Switch)
   SourceAddress  : 10.199.48.170
   PingSucceeded  : False
   PingReplyDetails (RTT) : 0 ms
   ```

## <a name="step-7-remove-the-access-vlan-setting"></a>步驟 7. 移除存取 VLAN 設定

在此步驟中，您會從實體 NIC 移除存取 VLAN 設定，並使用 vSwitch 來設定 VLANID。

您必須移除存取 VLAN 設定，以防止使用不正確的 VLAN ID 自動標記輸出流量，以及篩選不符合存取 VLAN 識別碼的輸入流量。

1. 移除設定。

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name "Test-40G-1" -RegistryKeyword VlanID -RegistryValue "0"
   Set-NetAdapterAdvancedProperty -Name "Test-40G-2" -RegistryKeyword VlanID -RegistryValue "0"
   ```

2. 設定 VLAN 識別碼。

   ```PowerShell
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "VMSTEST" -VlanId "101" -Access -ManagementOS
   Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName "VMSTEST"
   ```

   _**更**_  

   ```
   VMName VMNetworkAdapterName Mode   VlanList
   ------ -------------------- ----   --------
          VMSTEST              Access 101     
   ```


3. 測試網路連接。

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

   >**重要事項**如果您的結果與範例結果不類似，ping 就會失敗並顯示訊息「警告：Ping 至192.168.1.5 失敗--狀態：DestinationHostUnreachable，"確認" vEthernet （VMSTEST） "具有適當的 IP 位址。
   >
   >```PowerShell
   >Get-NetIPAddress -InterfaceAlias "vEthernet (VMSTEST)"
   >```
   >
   >如果未設定 IP 位址，請更正問題。
   >
   >```PowerShell
   >New-NetIPAddress -InterfaceAlias "vEthernet (VMSTEST)" -IPAddress 192.168.1.3 -PrefixLength 24
   >
   >IPAddress : 192.168.1.3
   >InterfaceIndex: 37
   >InterfaceAlias: vEthernet (VMSTEST)
   >AddressFamily : IPv4
   >Type  : Unicast
   >PrefixLength  : 24
   >PrefixOrigin  : Manual
   >SuffixOrigin  : Manual
   >AddressState  : Tentative
   >ValidLifetime : Infinite ([TimeSpan]::MaxValue)
   >PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
   >SkipAsSource  : False
   >PolicyStore   : ActiveStore
   >```  


4. 重新命名管理 NIC。

   ```PowerShell
   Rename-VMNetworkAdapter -ManagementOS -Name “VMSTEST” -NewName “MGT”
   Get-VMNetworkAdapter -ManagementOS
   ```

   _**更**_ 


   |         Name         | IsManagementOs | VMName |      SwitchName      |  macAddress  | 狀態 | IPAddresses |
   |----------------------|----------------|--------|----------------------|--------------|--------|-------------|
   | CORP-External-交換器 |      True      | &nbsp; | CORP-External-交換器 | 001B785768AA |  @  |   &nbsp;    |
   |         進行中的          |      True      | &nbsp; |       VMSTEST        | E41D2D074071 |  @  |   &nbsp;    |

   ---

5. 查看其他 NIC 屬性。

   ```PowerShell
   Get-NetAdapter
   ```

   _**更**_


   |      Name       |        InterfaceDescription         | ifIndex | 狀態 |    macAddress     | LinkSpeed |
   |-----------------|-------------------------------------|---------|--------|-------------------|-----------|
   | vEthernet （進行中） | Hyper-v 虛擬乙太網路介面卡 #2 |   28    |   Up   | E4-1D-2D-07-40-71 |  80 Gbps  |

   ---

## <a name="step-8-test-hyper-v-vswitch-rdma"></a>步驟8。 測試 Hyper-v vSwitch RDMA

下圖顯示您 Hyper-v 主機的目前狀態，包括 Hyper-v 主機1上的 vSwitch。

![測試 Hyper-v 虛擬交換器](../../media/Converged-NIC/6-datacenter-test-vswitch-rdma.jpg)

1. 在主機 vNIC 上設定優先順序標記，以補充先前的 VLAN 設定。

   ```PowerShell    
   Set-VMNetworkAdapter -ManagementOS -Name "MGT" -IeeePriorityTag on
   Get-VMNetworkAdapter -ManagementOS -Name "MGT" | fl Name,IeeePriorityTag
   ```

   _**更**_  

   檔案名進行中的  
   IeeePriorityTag :開啟  

2. 為 RDMA 建立兩個主機 Vnic，並將其連線至 vSwitch VMSTEST。

   ```PowerShell    
   Add-VMNetworkAdapter –SwitchName "VMSTEST" –Name SMB1 –ManagementOS
   Add-VMNetworkAdapter –SwitchName "VMSTEST" –Name SMB2 –ManagementOS
   ```

3. 查看管理 NIC 屬性。

   ```PowerShell    
   Get-VMNetworkAdapter -ManagementOS
   ```

   _**更**_ 


   |         Name         | IsManagementOs |        VMName        |  SwitchName  | macAddress | 狀態 | IPAddresses |
   |----------------------|----------------|----------------------|--------------|------------|--------|-------------|
   | CORP-External-交換器 |      True      | CORP-External-交換器 | 001B785768AA |    @    | &nbsp; |             |
   |         進行中的          |      True      |       VMSTEST        | E41D2D074071 |    @    | &nbsp; |             |
   |         SMB1         |      True      |       VMSTEST        | 00155D30AA00 |    @    | &nbsp; |             |
   |         SMB2         |      True      |       VMSTEST        | 00155D30AA01 |    @    | &nbsp; |             |

   ---

## <a name="step-9-assign-an-ip-address-to-the-smb-host-vnics-vethernet-smb1-and-vethernet-smb2"></a>步驟 9： 將 IP 位址指派給 SMB 主機 vnic vEthernet \(SMB1\)和 vEthernet \(SMB2\)

測試-40G-1 和測試-40G-2 實體介面卡仍有已設定的存取 VLAN 101 和102。 因此，介面卡會標記流量，而 ping 會成功。 先前，您已將兩個 pNIC VLAN Id 設定為零，然後將 VMSTEST vSwitch 設為 VLAN 101。 之後，您仍然可以使用 [vNIC] 來 ping 遠端 VLAN 101 介面卡，但目前沒有 VLAN 102 成員。



1. 從實體 NIC 移除存取 VLAN 設定，以防止它同時自動將輸出流量標記為不正確的 VLAN ID，並防止它篩選不符合存取 VLAN ID 的輸入流量。

   ```PowerShell    
   New-NetIPAddress -InterfaceAlias "vEthernet (SMB1)" -IPAddress 192.168.2.111 -PrefixLength 24
   ```

   _**更**_  

   ```   
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
   ```

2. 測試遠端 VLAN 102 介面卡。

   ```PowerShell
   Test-NetConnection 192.168.2.5 
   ```

   _**更**_  

   ```
   ComputerName   : 192.168.2.5
   RemoteAddress  : 192.168.2.5
   InterfaceAlias : vEthernet (SMB1)
   SourceAddress  : 192.168.2.111
   PingSucceeded  : True
   PingReplyDetails (RTT) : 0 ms
   ```

3. 為介面 vEthernet \(SMB2\)新增新的 IP 位址。

   ```PowerShell
   New-NetIPAddress -InterfaceAlias "vEthernet (SMB2)" -IPAddress 192.168.2.222 -PrefixLength 24 
   ```

   _**更**_ 

   ```
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
   ```

4. 再次測試連接。    


5. 將 RDMA 主機 Vnic 放在既有的 VLAN 102 上。

   ```PowerShell
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "SMB1" -VlanId "102" -Access -ManagementOS
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "SMB2" -VlanId "102" -Access -ManagementOS

   Get-VMNetworkAdapterVlan -ManagementOS
   ```

   _**更**_ 

   ```   
   VMName VMNetworkAdapterName Mode VlanList
   ------ -------------------- ---- --------
      SMB1 Access   102 
      Mgt  Access   101 
      SMB2 Access   102 
      CORP-External-Switch Untagged
   ```

6. 檢查 SMB1 和 SMB2 與 vSwitch SET 小組下的基礎實體 Nic 的對應。<p>主機 vNIC 與實體 Nic 的關聯是隨機的，而且在建立和銷毀期間可能會重新平衡。 在這種情況下，您可以使用間接機制來檢查目前的關聯。 SMB1 和 SMB2 的 MAC 位址與 NIC 小組成員測試-40G-2 相關聯。 這不是理想的做法，因為測試-40G-1 沒有相關聯的 SMB 主機 vNIC，而且在 SMB 主機 vNIC 對應到該連結之前，將不允許透過該連結使用 RDMA 流量。

   ```PowerShell    
   Get-NetAdapterVPort (Preferred)

   Get-NetAdapterVmqQueue
   ```

   _**更**_ 

   ```
   Name   QueueID MacAddressVlanID Processor VmFriendlyName
   ----   ------- ---------------- --------- --------------
   TEST-40G-1 1   E4-1D-2D-07-40-71 1010:17
   TEST-40G-2 1   00-15-5D-30-AA-00 1020:17
   TEST-40G-2 2   00-15-5D-30-AA-01 1020:17
   ```

7. 查看 VM 網路介面卡屬性。

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS
   ```

   _**更**_ 

   ```
   Name IsManagementOs VMName SwitchName   MacAddress   Status IPAddresses
   ---- -------------- ------ ----------   ----------   ------ -----------
   CORP-External-Switch True  CORP-External-Switch 001B785768AA {Ok}  
   Mgt  True  VMSTEST  E41D2D074071 {Ok}  
   SMB1 True  VMSTEST  00155D30AA00 {Ok}  
   SMB2 True  VMSTEST  00155D30AA01 {Ok}  
   ```

8. 查看網路介面卡小組對應。<p>結果不應傳回信息，因為您尚未執行對應。

   ```PowerShell
   Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName SMB1
   Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName SMB2
   ```


9. 將 SMB1 和 SMB2 對應到不同的實體 NIC 小組成員，以及查看您的動作結果。

   >[!IMPORTANT]
   >請確定您在繼續進行之前完成此步驟，否則您的執行會失敗。

   ```PowerShell
   Set-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName "SMB1" -PhysicalNetAdapterName "Test-40G-1"
   Set-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName "SMB2" -PhysicalNetAdapterName "Test-40G-2"

   Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST
   ```

   _**更**_ 

   ```   
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
   ```

10. 確認先前建立的 MAC 關聯。

    ```PowerShell    
    Get-NetAdapterVmqQueue
    ```

    _**更**_ 

    ```   
    Name   QueueID MacAddressVlanID Processor VmFriendlyName
    ----   ------- ---------------- --------- --------------
    TEST-40G-1 1   E4-1D-2D-07-40-71 1010:17
    TEST-40G-1 2   00-15-5D-30-AA-00 1020:17
    TEST-40G-2 1   00-15-5D-30-AA-01 1020:17
    ```


11. 測試遠端系統的連線，因為這兩個主機 vnic 都位於相同的子網上，而且具有相同\(的\)VLAN ID 102。

    ```PowerShell 
    Test-NetConnection 192.168.2.111
    ```

    _**更**_   

    ```
    ComputerName   : 192.168.2.111
    RemoteAddress  : 192.168.2.111
    InterfaceAlias : Test-40G-2
    SourceAddress  : 192.168.2.5
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    ```

    ```PowerShell   
    Test-NetConnection 192.168.2.222
    ```

    _**更**_   

    ```
    ComputerName   : 192.168.2.222
    RemoteAddress  : 192.168.2.222
    InterfaceAlias : Test-40G-2
    SourceAddress  : 192.168.2.5
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms 
    ```
12. 設定 [名稱]、[交換器名稱] 和 [優先順序] 標籤。   

    ```PowerShell
    Set-VMNetworkAdapter -ManagementOS -Name "SMB1" -IeeePriorityTag on
    Set-VMNetworkAdapter -ManagementOS -Name "SMB2" -IeeePriorityTag on
    Get-VMNetworkAdapter -ManagementOS -Name "SMB*" | fl Name,SwitchName,IeeePriorityTag,Status
    ```

    _**更**_   

    ```
    Name: SMB1
    SwitchName  : VMSTEST
    IeeePriorityTag : On 
    Status  : {Ok}

    Name: SMB2
    SwitchName  : VMSTEST
    IeeePriorityTag : On
    Status  : {Ok}
    ```

13. 查看 vEthernet 網路介面卡屬性。

    ```PowerShell
    Get-NetAdapterRdma -Name "vEthernet*" | sort Name | ft -AutoSize
    ```

    _**更**_   

    ```
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  False  
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  False  
    vEthernet (MGT)   Hyper-V Virtual Ethernet Adapter #2  False   
    ```

14. 啟用 vEthernet 網路介面卡。  

    ```PowerShell
    Enable-NetAdapterRdma -Name "vEthernet (SMB1)"
    Enable-NetAdapterRdma -Name "vEthernet (SMB2)"
    Get-NetAdapterRdma -Name "vEthernet*" | sort Name | fl *
    ```

    _**更**_   

    ```
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  True   
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  True  
    vEthernet (MGT)   Hyper-V Virtual Ethernet Adapter #2  False
    ```

## <a name="step-10-validate-the-rdma-functionality"></a>步驟10。 驗證 RDMA 功能。

您想要將來自遠端系統的 RDMA 功能，驗證到 vSwitch 集小組成員的本機系統（具有 vSwitch）。<p>因為主機 vnic \(的 SMB1 和 SMB2\)都已指派給 vlan 102，所以您可以選取遠端系統上的 vlan 102 介面卡。 <p>在此範例中，NIC 測試-40G-2 會執行 RDMA 至 SMB1 （192.168.2.111）和 SMB2 （192.168.2.222）。

>[!TIP]
>您可能需要停用此系統上的防火牆。  如需詳細資訊，請參閱網狀架構原則。
>
>```PowerShell
>Set-NetFirewallProfile -All -Enabled False
>    
>Get-NetAdapterAdvancedProperty -Name "Test-40G-2"
>```
>
>_**更**_ 
>   
>```
>Name  DisplayNameDisplayValue   RegistryKeyword RegistryValue  
>----  -----------------------   --------------- -------------  
> .
> .
>Test-40G-2VLAN ID102VlanID  {102} 
>```

1. 查看網路介面卡屬性。

   ```PowerShell
   Get-NetAdapter
   ```

   _**更**_ 

   ```
   Name  InterfaceDescriptionifIndex Status   MacAddress LinkSpeed
   ----  --------------------------- ------   ---------- ---------
   Test-40G-2Mellanox ConnectX-3 Pro Ethernet A...#3   3 Up   E4-1D-2D-07-43-D140 Gbps
   ```

2. 查看網路介面卡 RDMA 資訊。

   ```PowerShell
   Get-NetAdapterRdma
   ```

   _**更**_  

   ```
   Name  InterfaceDescription Enabled
   ----  -------------------- -------
   Test-40G-2Mellanox ConnectX-3 Pro Ethernet Adap... True   
   ```

3. 在第一個實體介面卡上執行 RDMA 流量測試。

   ```PowerShell 
   C:\TEST\Test-RDMA.PS1 -IfIndex 3 -IsRoCE $true -RemoteIpAddress 192.168.2.111 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**更**_ 

   ```
   VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\diskspd.exe
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
   ```

4. 在第二個實體介面卡上執行 RDMA 流量測試。

   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 3 -IsRoCE $true -RemoteIpAddress 192.168.2.222 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**更**_ 

   ```
   VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\diskspd.exe
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
   ```

5. 測試從本機到遠端電腦的 RDMA 流量。

    ```PowerShell
    Get-NetAdapter | ft –AutoSize
    ```

    _**更**_ 

    ```
    Name  InterfaceDescriptionifIndex Status   MacAddress LinkSpeed
    ----  --------------------------- ------   ---------- ---------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  45 Up   00-15-5D-30-AA-0380 Gbps
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  41 Up   00-15-5D-30-AA-0280 Gbps
    ```

6. 在第一個虛擬配接器上執行 RDMA 流量測試。    

   ```
   C:\TEST\Test-RDMA.PS1 -IfIndex 41 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**更**_ 

   ```
   VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\diskspd.exe
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
   ```

7. 在第二個虛擬網路介面卡上執行 RDMA 流量測試。

   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 45 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**更**_ 

   ```
   VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\diskspd.exe
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
   ```

此輸出中的最後一行「RDMA 流量測試成功：RDMA 流量已傳送至192.168.2.5，表示您已成功在介面卡上設定聚合式 NIC。

## <a name="related-topics"></a>相關主題 

- [具有單一網路介面卡的聚合式 NIC 設定](cnic-single.md)
- [聚合式 NIC 的實體交換器設定](cnic-app-switch-config.md)
- [針對聚合式 NIC 設定進行疑難排解](cnic-app-troubleshoot.md)
