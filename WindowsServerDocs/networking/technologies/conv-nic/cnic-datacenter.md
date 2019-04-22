---
title: 組合 NIC 組態 （資料中心） 中的聚合式的 NIC
description: 本主題中，我們提供您的部署交集的 NIC 組合 NIC 設定與 Switch Embedded Teaming (SET) 中的指示。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: f01546f8-c495-4055-8492-8806eee99862
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/17/2018
ms.openlocfilehash: 5f99600e24c62da9bdf674897dbadde9246b7bb7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821029"
---
# <a name="converged-nic-in-a-teamed-nic-configuration-datacenter"></a>組合 NIC 組態 （資料中心） 中的聚合式的 NIC

>適用於：Windows Server （半年通道），Windows Server 2016

本主題中，我們提供您的指示來部署組合 NIC 組態與交換器內嵌小組中的交集的 NIC\(設定\)。 

本主題中的範例組態會描述兩個 HYPER-V 主機**HYPER-V 主機 1**並**HYPER-V 主機 2**。 這兩個主機有兩張網路介面卡。 在每個主機上，一張介面卡會連接到 192.168.1.x/24 子網路，而一張介面卡連線至 192.168.2.x/24 子網路。

![Hyper-V 主機](../../media/Converged-NIC/1-datacenter-test-conn.jpg)

## <a name="step-1-test-the-connectivity-between-source-and-destination"></a>步驟 1. 測試來源與目的地之間的連線

確定實體 NIC 可以連線至目的地主機。  這項測試示範使用第 3 層連線能力\(L3\) -或 IP 層-，以及第 2 層\(L2\)虛擬區域網路\(VLAN\)。

1. 檢視第一個網路介面卡內容。

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
   ```
  
   _**結果：**_

   |名稱|InterfaceDescription|ifIndex|狀態|MacAddress|LinkSpeed|
   |-----|--------------------|-------|-----|----------|---------|
   |測試-40 G-1|Mellanox connectx-3 Pro 乙太網路介面卡|11|Up|E4-1D-2D-07-43-D0|40 Gbps|
   ---
   
2. 檢視第一次的配接器，包括 IP 位址的其他屬性。 

   ```PowerShell
   Get-NetIPAddress -InterfaceAlias "Test-40G-1"
   Get-NetIPAddress -InterfaceAlias "TEST-40G-1" | Where-Object {$_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress
   ```
   
   _**結果：**_

   |參數|值|
   |---------|-----|
   |IPAddress| 192.168.1.3|
   |InterfaceIndex|11|
   |InterfaceAlias|測試-40 G-1|
   |AddressFamily|IPv4|
   |類型| 單點傳播|
   |PrefixLength|24|
   ---

3. 檢視第二個網路介面卡內容。

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-2" | ft -AutoSize
   ```
   
   _**結果：**_

   |名稱 |InterfaceDescription |ifIndex |狀態 |MacAddress |LinkSpeed|
   |----|--------------------|-------|------|----------|---------|
   |TEST-40G-2 |Mellanox connectx-3 Pro 乙太網路...#2 |13 |Up |E4-1D-2D-07-40-70 |40 Gbps|
   ---
   
4. 檢視針對第二個介面卡，包括 IP 位址的其他屬性。

   ```PowerShell
   Get-NetIPAddress -InterfaceAlias "Test-40G-2"
   Get-NetIPAddress -InterfaceAlias "Test-40G-2" | Where-Object {$_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress
   ```
   
   _**結果：**_

   |參數|值|
   |---------|-----|
   |IPAddress|192.168.2.3|
   |InterfaceIndex|13|
   |InterfaceAlias|TEST-40G-2|
   |AddressFamily|IPv4|
   |類型|單點傳播|
   |PrefixLength|24|
   ---

5. 請確認該其他 NIC 小組或設定成員 pNICs 具有有效的 IP 位址。<p>使用個別的子網路\(xxx.xxx。**2**.xxx vs xxx.xxx。**1**.xxx\)，以便從這個配接器傳送至目的地。 否則，如果您在相同的子網路上找到這兩個 pNICs，Windows TCP/IP 堆疊負載平衡之間的介面，而且簡單的驗證會變得更複雜。


## <a name="step-2-ensure-that-source-and-destination-can-communicate"></a>步驟 2. 確保來源和目的地可以進行通訊

在此步驟中，我們會使用**Test-netconnection** Windows PowerShell 命令，但是如果您可以使用**ping**如果您偏好的命令。 

1. 確認雙向通訊。

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
   
   
4. 額外的 Nic，請確認連線能力。 針對包含在 NIC 或一組小組中的所有後續 pNICs 重複先前的步驟。

   ```PowerShell    
   Test-NetConnection 192.168.2.5
   ```
   
   _**結果：**_

   |參數|值|
   |---------|-----|
   |ComputerName|192.168.2.5|
   |RemoteAddress|192.168.2.5|
   |InterfaceAlias|測試-40 G-2|
   |SourceAddress|192.168.2.3|
   |PingSucceeded|False|
   |PingReplyDetails \(RTT\)|0 毫秒|
   ---

## <a name="step-3-configure-the-vlan-ids-for-nics-installed-in-your-hyper-v-hosts"></a>步驟 3。 針對安裝在 HYPER-V 主機中的 Nic 設定 VLAN 識別碼

許多網路進行組態使用的 Vlan，以及如果您打算在網路中使用 Vlan，您必須重複執行先前的測試設定的 Vlan。

此步驟中，Nic 位於**存取**模式。 不過，當您建立 HYPER-V 虛擬交換器\(vSwitch\)稍後在本指南中，VLAN 內容會套用到 vSwitch 連接埠層級。 

因為交換器可以裝載多個 Vlan，則需要這麼做最上方的機架\(ToR\)實體交換器的主機已連線到在主幹模式中設定連接埠。

>[!NOTE]
>如需如何設定交換器的主幹模式中的指示，請參閱您的 ToR 交換器文件。

下圖顯示兩個網路介面卡每個 VLAN 101 和網路介面卡內容中設定的 VLAN 102 的兩個 HYPER-V 主機。

![設定虛擬區域網路](../../media/Converged-NIC/2-datacenter-configure-vlans.jpg)


>[!TIP]
>根據美國電機暨電子工程師\(IEEE\)網路標準的服務品質\(QoS\)內嵌的 802.1p 標頭處理中的實體 NIC 屬性內 802.1Q \(VLAN\)標頭，當您設定 VLAN 識別碼。

1. 在第一個 NIC，也就是測試 40 G 1 上設定 VLAN 識別碼。

   ```PowerShell    
   Set-NetAdapterAdvancedProperty -Name "Test-40G-1" -RegistryKeyword VlanID -RegistryValue "101"
   Get-NetAdapterAdvancedProperty -Name "Test-40G-1" | Where-Object {$_.RegistryKeyword -eq "VlanID"} | ft -AutoSize
   ```

   _**結果：**_   

   |名稱 |DisplayName| DisplayValue| RegistryKeyword |RegistryValue|
   |----|-----------|------------|---------------|-------------|
   |TEST-40G-1|VLAN 識別碼|101|VlanID|{101}|
   ---

2. 重新啟動的網路介面卡，以套用 VLAN id。

   ```PowerShell
   Restart-NetAdapter -Name "Test-40G-1"
   ```
   
3. 確認狀態為**向上**。

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
   ```
   
   _**結果：**_

   |名稱|InterfaceDescription|ifIndex| 狀態|MacAddress|LinkSpeed|
   |----|--------------------|-------|------|----------| ---------|
   |測試-40 G-1|Mellanox connectx-3 Pro 乙太網路 Ada...|11|Up|E4-1D-2D-07-43-D0|40 Gbps|
   ---
    
4. 在第二個 NIC，也就是測試 40 G 2 上設定 VLAN 識別碼。

   ```PowerShell    
   Set-NetAdapterAdvancedProperty -Name "Test-40G-2" -RegistryKeyword VlanID -RegistryValue "102"
   Get-NetAdapterAdvancedProperty -Name "Test-40G-2" | Where-Object {$_.RegistryKeyword -eq "VlanID"} | ft -AutoSize
   ``` 

   _**結果：**_

   |名稱 |DisplayName| DisplayValue| RegistryKeyword |RegistryValue|
   |----|-----------|------------|---------------|-------------|
   |TEST-40G-2|VLAN 識別碼|102|VlanID|{102}|
   ---
   
5. 重新啟動的網路介面卡，以套用 VLAN id。

   ```PowerShell
   Restart-NetAdapter -Name "Test-40G-2" 
   ```
   
6. 確認狀態為**向上**。

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
   ```
   
   _**結果：**_

   |名稱|InterfaceDescription|ifIndex| 狀態|MacAddress|LinkSpeed|
   |----|--------------------|-------|------|----------| ---------|
   |測試-40 G-2 |Mellanox connectx-3 Pro 乙太網路 Ada... |11 |Up |E4-1D-2D-07-43-D1 |40 Gbps|
   ---

   >[!IMPORTANT]
   >可能需要幾秒鐘重新啟動，並成為在網路上可用的裝置。 
   
7. 確認第一個 NIC，也就是測試-40 G-1 的連線。<p>如果連線失敗，請檢查在相同 vlan 的 VLAN 設定或目的地參與的交換器。 

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**結果：**_   

   |參數|值|
   |---------|-----|
   |ComputerName|192.168.1.5|
   |RemoteAddress|192.168.1.5|
   |InterfaceAlias|測試-40 G-1|
   |SourceAddress|192.168.1.5|
   |PingSucceeded|True|
   |PingReplyDetails \(RTT\)|0 毫秒|
   ---
   
8. 第一個 nic，也就是測試 40 G 2 驗證的連線。<p>如果連線失敗，請檢查在相同 vlan 的 VLAN 設定或目的地參與的交換器。

   ```PowerShell    
   Test-NetConnection 192.168.2.5
   ```

   _**結果：**_    

   |參數|值|
   |---------|-----|
   |ComputerName|192.168.2.5|
   |RemoteAddress|192.168.2.5|
   |InterfaceAlias|測試-40 G-2|
   |SourceAddress|192.168.2.3|
   |PingSucceeded|True|
   |PingReplyDetails \(RTT\)|0 毫秒|
   ---
   
   >[!IMPORTANT]
   >不是罕見狀況**Test-netconnection**或 ping 失敗，發生在執行之後，立即**重新啟動 NetAdapter**。  請等候它的網路介面卡，若要完全初始化，並再試一次。
   >
   >如果 VLAN 101 連線運作，但 VLAN 102 連線，問題可能是參數必須設定為允許連接埠流量所需的 VLAN 上。 您可以檢查這個暫時失敗配接器設為 VLAN 101，並重複連線測試。

   
   下圖顯示您的 HYPER-V 主機之後已成功設定 Vlan。

   ![設定服務品質](../../media/Converged-NIC/3-datacenter-configure-qos.jpg)
   
   
## <a name="step-4-configure-quality-of-service-qos"></a>步驟 4. 設定服務品質\(QoS\)

>[!NOTE]
>您必須執行所有下列 DCB 與 QoS 設定步驟要與彼此進行通訊的所有主機上。

1. 安裝資料中心橋接\(DCB\)每部 HYPER-V 主機上。

   - **選擇性**使用 iWarp 的網路組態。
   - **所需**的網路設定，使用 RoCE\(任何版本\)RDMA 服務。

   ```PowerShell
   Install-WindowsFeature Data-Center-Bridging
   ```

   _**結果：**_

   |成功 |需要重新啟動 |結束碼|功能結果|
   |------- |-------------- |--------- |-------------- |
   |True |否 |成功| {資料中心橋接}|
   ---
   
2. SMB 直接傳輸設定 QoS 原則：

   - **選擇性**使用 iWarp 的網路組態。
   - **所需**的網路設定，使用 RoCE\(任何版本\)RDMA 服務。
   
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
   |NetDirectPort|445
   |PriorityValue|3
   ---

3. 介面上設定其他其他流量的 QoS 原則。   

   ```PowerShell
   New-NetQosPolicy "DEFAULT" -Default -PriorityValue8021Action 0
   ```

   _**結果：**_   

   |參數|值|
   |---------|-----|
   |名稱 | DEFAULT|
   |擁有者|群組原則\(機器\)|
   |NetworkProfile|全部|
   |優先順序|127|
   |範本| 預設|
   |JobObject| &nbsp;|
   |PriorityValue|0|
   ---
   
4. 開啟**優先順序流量控制**SMB 流量，這並不需要 iWarp。

   ```PowerShell
   Enable-NetQosFlowControl -priority 3
   Get-NetQosFlowControl
   ```
   
   _**結果：**_
   
   |Priority|Enabled|PolicySet|IfIndex|IfAlias|
   |---------|-----|--------- |-------| -------|
   |0 |False |全域|&nbsp;|&nbsp;|
   |1 |False |全域|&nbsp;|&nbsp;|
   |2 |False |全域|&nbsp;|&nbsp;|
   |3 |True |全域|&nbsp;|&nbsp;|
   |4 |False |全域|&nbsp;|&nbsp;|
   |5 |False |全域|&nbsp;|&nbsp;|
   |6 |False |全域|&nbsp;|&nbsp;|
   |7 |False |全域|&nbsp;|&nbsp;|
   ---
   
   >**重要**如果您的結果不符這些結果，因為 3 以外的項目已啟用值為 True，您必須停用**FlowControl**這些類別。
   >
   >```PowerShell
   >Disable-NetQosFlowControl -priority 0,1,2,4,5,6,7
   >Get-NetQosFlowControl
   >```
   >在更複雜的設定，不過這些案例會在本指南的範圍之外的其他流量類別時，可能需要流量控制。


5. 啟用 QoS 的第一個 NIC，也就是測試-40 G-1。

   ```PowerShell
   Enable-NetAdapterQos -InterfaceAlias "Test-40G-1"
   Get-NetAdapterQos -Name "Test-40G-1"

   Name: TEST-40G-1 
   Enabled: True
   ```
   _**功能**:_   

   |參數|硬體|目前|
   |---------|--------|-------|
   |MacSecBypass|NotSupported|NotSupported|
   |DcbxSupport|None|None|
   |NumTCs(Max/ETS/PFC)|8/8/8|8/8/8|
   ---
 
   _**OperationalTrafficClasses**:_    

   |TC|TSA|頻寬|優先順序|
   |----|-----|--------|-------|
   |0| [Strict]|&nbsp;|0-7|
   ---

   _**OperationalFlowControl**:_  

   優先順序 3 啟用  

   _**OperationalClassifications**:_  

   |通訊協定|連接埠/類型|Priority|
   |--------|---------|--------|
   |預設|&nbsp;|0|
   |NetDirect| 445|3|
   ---
   
6. 第二個 NIC，也就是測試 40 G 2 啟用 QoS。

   ```PowerShell
   Enable-NetAdapterQos -InterfaceAlias "Test-40G-2"
   Get-NetAdapterQos -Name "Test-40G-2"

   Name: TEST-40G-2 
   Enabled: True 
   ```

   _**功能**:_ 

   |參數|硬體|目前|
   |---------|--------|-------|
   |MacSecBypass|NotSupported|NotSupported|
   |DcbxSupport|None|None|
   |NumTCs(Max/ETS/PFC)|8/8/8|8/8/8|
   ---

   _**OperationalTrafficClasses**:_  

   |TC|TSA|頻寬|優先順序|
   |----|-----|--------|-------|
   |0| [Strict]|&nbsp;|0-7|
   ---
   
    _**OperationalFlowControl**:_  

    優先順序 3 啟用  
   
   _**OperationalClassifications**:_  

   |通訊協定|連接埠/類型|Priority|
   |--------|---------|--------|
   |預設|&nbsp;|0|
   |NetDirect| 445|3|
   ---

   
7. 保留 SMB 直接傳輸的一半的頻寬\(RDMA\)

   ```PowerShell
   New-NetQosTrafficClass "SMB" -priority 3 -bandwidthpercentage 50 -algorithm ETS
   ```
   
   _**結果：**_  
   
   |名稱|演算法 |Bandwidth(%)| Priority |PolicySet |IfIndex |IfAlias |
   |----|---------| ------------ |--------| ---------|------- |------- |
   |SMB | ETS     | 50 |3 |全域 |&nbsp;|&nbsp;|   
   ---

8. 檢視的頻寬保留設定：   

   ```PowerShell
   Get-NetQosTrafficClass | ft -AutoSize
   ```
   
   _**結果：**_  

   |名稱|演算法 |Bandwidth(%)| Priority |PolicySet |IfIndex |IfAlias |
   |----|---------| ------------ |--------| ---------|------- |------- |
   |[Default]| ETS|50 |0-2,4-7|  全域|&nbsp;|&nbsp;| 
   |SMB |ETS|50 |3 |全域|&nbsp;|&nbsp;| 
   ---
   
9. （選擇性）建立租用戶 IP 流量的兩個額外的流量類別。 

   >[!TIP]
   >您可以省略的"IP1"和"IP2"值。

   ```PowerShell
   New-NetQosTrafficClass "IP1" -Priority 1 -bandwidthpercentage 10 -algorithm ETS
   ```
   
   _**結果：**_
   
   |名稱|演算法 |Bandwidth(%)| Priority |PolicySet |IfIndex |IfAlias |
   |----|---------| ------------ |--------| ---------|------- |------- |
   |IP1 |ETS |10 |1 |全域|&nbsp;|&nbsp;|
   ---
   
   ```PowerShell
   New-NetQosTrafficClass "IP2" -Priority 2 -bandwidthpercentage 10 -algorithm ETS
   ```
   
   _**結果：**_

   |名稱|演算法 |Bandwidth(%)| Priority |PolicySet |IfIndex |IfAlias |
   |----|---------| ------------ |--------| ---------|------- |------- |
   |IP2 |ETS |10 |2 |全域|&nbsp;|&nbsp;|
   ---
   
10. 檢視 QoS 流量類別。

    ```PowerShell
    Get-NetQosTrafficClass | ft -AutoSize
    ```
    
    _**結果：**_

    |名稱|演算法 |Bandwidth(%)| Priority |PolicySet |IfIndex |IfAlias |
    |----|---------| ------------ |--------| ---------|------- |------- |
    |[Default] |ETS |30 |0,4-7 |全域|&nbsp;|&nbsp;|
    |SMB |ETS |50 |3 |全域|&nbsp;|&nbsp;|
    |IP1 |ETS |10 |1 |全域|&nbsp;|&nbsp;|
    |IP2 |ETS |10 |2 |全域|&nbsp;|&nbsp;|
    ---
   
11. （選擇性）偵錯工具會覆寫。<p>根據預設，附加偵錯工具會封鎖 NetQos。 

    ```PowerShell
    Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 –Force
    Get-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" | ft AllowFlowControlUnderDebugger
    ```

    _**結果：**_  

    ```
    AllowFlowControlUnderDebugger
    -----------------------------
    1
    ```

## <a name="step-5-verify-the-rdma-configuration-mode-1"></a>步驟 5. 確認 RDMA 設定\(模式 1\) 

您想要確定網狀架構已正確設定之前建立的 vSwitch 和轉換至 RDMA,\(模式 2\)。

下圖顯示 HYPER-V 主機的目前狀態。

![測試 RDMA](../../media/Converged-NIC/4-datacenter-test-rdma.jpg)


1. 確認 RDMA 設定。

   ```PowerShell
   Get-NetAdapterRdma | ft -AutoSize
   ```
   
   _**結果：**_

   |名稱 |InterfaceDescription |Enabled|
   |----|--------------------|-------|
   |TEST-40G-1| Mellanox ConnectX 4 VPI 配接器 2 |True|
   |TEST-40G-2| Mellanox ConnectX 4 VPI 配接器 |True|
   ---

2. 判斷**ifIndex**您目標介面卡的值。<p>當您執行您所下載的指令碼時，您可以使用在後續步驟中的此值。   

   ```PowerShell
   Get-NetIPConfiguration -InterfaceAlias "TEST*" | ft InterfaceAlias,InterfaceIndex,IPv4Address
   ```
   
   _**結果：**_

   |InterfaceAlias |InterfaceIndex |IPv4Address|
   |-------------- |-------------- |-----------|
   |TEST-40G-1 |14 |{192.168.1.3}|
   |TEST-40G-2 | 13 |{192.168.2.3}|
   ---
   
3. 下載[DiskSpd.exe 公用程式](https://aka.ms/diskspd)並將它解壓縮到 C:\TEST\.

4. 下載[測試 RDMA PowerShell 指令碼](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)到本機磁碟機，比方說，C:\TEST 測試資料夾\.

5. 執行**測試 Rdma.ps1** ifIndex 值傳遞至指令碼，以及在相同 VLAN 上的第一個遠端介面卡的 IP 位址的 PowerShell 指令碼。<p>在此範例中，指令碼會傳遞**ifIndex**值 14 上的遠端網路介面卡 IP 位址 192.168.1.5。
   
   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 14 -IsRoCE $true -RemoteIpAddress 192.168.1.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**結果：**_ 
   
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
   >如果 RDMA 流量失敗，RoCE 案例的具體來說，如應符合主機設定的適當 PFC/ETS 設定您的 ToR 交換器設定。 請參閱本文件的參考值中的 QoS 一節。

6. 執行**測試 Rdma.ps1** ifIndex 值傳遞至指令碼，以及在相同 VLAN 上的第二個遠端介面卡的 IP 位址的 PowerShell 指令碼。<p>在此範例中，指令碼會傳遞**ifIndex**上遠端網路介面卡的 IP 位址 192.168.2.5 13 的值。

   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 13 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**結果：**_ 
   
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

## <a name="step-6-create-a-hyper-v-vswitch-on-your-hyper-v-hosts"></a>步驟 6. 在 HYPER-V 主機上建立 HYPER-V vSwitch


下圖顯示 HYPER-V 主機 1 使用了 vSwitch。

![建立 HYPER-V 虛擬交換器](../../media/Converged-NIC/5-datacenter-create-vswitch.jpg)

1. 在 HYPER-V 主機 1 上的設定模式中建立的 vSwitch。

   ```PowerShell
   New-VMSwitch –Name "VMSTEST" –NetAdapterName "TEST-40G-1","TEST-40G-2" -EnableEmbeddedTeaming $true -AllowManagementOS $true
   ```
   
   _**結果：**_

   |名稱 |SwitchType |NetAdapterInterfaceDescription|
   |---- |---------- |------------------------------|
   |VMSTEST |外部 |組合介面|
   ---
   
2. 集中檢視的實體介面卡小組。

   ```PowerShell
   Get-VMSwitchTeam -Name "VMSTEST" | fl
   ```
   
   _**結果：**_  
   
   ```
   Name: VMSTEST  
   Id: ad9bb542-dda2-4450-a00e-f96d44bdfbec  
   NetAdapterInterfaceDescription: {Mellanox ConnectX-3 Pro Ethernet Adapter, Mellanox ConnectX-3 Pro Ethernet Adapter #2}  
   TeamingMode: SwitchIndependent  
   LoadBalancingAlgorithm: Dynamic   
   ```
   
3. 顯示兩個主機 vNIC 檢視

   ```PowerShell
    Get-NetAdapter
   ```
   
   _**結果：**_

   |名稱 |InterfaceDescription |ifIndex |狀態 |MacAddress |LinkSpeed|
   |---- |--------------------|-------|------|----------|---------|
   |vEthernet (VMSTEST)|HYPER-V 虛擬乙太網路介面卡 #2 |28 |Up|E4-1D-2D-07-40-71|80 Gbps|
   ---
   
4. 檢視主機 vNIC 的其他屬性。 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS
   ```
   
   _**結果：**_

   |名稱 |IsManagementOs |VMName |SwitchName |MacAddress |狀態 |IPAddresses|
   |----|--------------|------|----------|----------|------|-----------|
   |VMSTEST|True |VMSTEST |E41D2D074071| {[確定]}|&nbsp;|
   ---
   

5. 測試遠端 VLAN 101 介面卡的網路連線。

   ```PowerShell
   Test-NetConnection 192.168.1.5 
   ```
   
   _**結果：**_  
   
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

在此步驟中，您會移除存取 VLAN 設定，從實體 NIC，並設定好 VLANID 使用 vSwitch。

您必須移除存取 VLAN 設定，以防止這兩種自動標記為不正確的 VLAN 識別碼的輸出流量，並從篩選輸入流量，不符合存取 VLAN id。

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
   
   _**結果：**_  
   
   ```
   VMName VMNetworkAdapterName Mode   VlanList
   ------ -------------------- ----   --------
          VMSTEST              Access 101     
   ```
   
   
3. 測試網路連線。

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

   >**重要**如果您的結果不是類似的範例結果，並 ping 失敗且顯示訊息 「 警告：192.168.1.5 的 Ping 失敗--狀態：DestinationHostUnreachable，「 確認 」 vEthernet (VMSTEST) 」 有適當的 IP 位址。
   >
   >```PowerShell
   >Get-NetIPAddress -InterfaceAlias "vEthernet (VMSTEST)"
   >```
   >
   >如果未設定的 IP 位址，這會修正此問題。
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
   

4. 重新命名管理 nic。

   ```PowerShell
   Rename-VMNetworkAdapter -ManagementOS -Name “VMSTEST” -NewName “MGT”
   Get-VMNetworkAdapter -ManagementOS
   ```
   
   _**結果：**_ 
   
   |名稱 |IsManagementOs |VMName |SwitchName |MacAddress |狀態 |IPAddresses
   |----|--------------|------|----------|----------|------|-----------|
   |CORP-External-Switch |True |&nbsp;|CORP-External-Switch |001B785768AA |{[確定]}|&nbsp;|
   |MGT |True |&nbsp;|VMSTEST |E41D2D074071 |{[確定]}|&nbsp;|
   ---
   
5. 檢視 NIC 的其他屬性。

   ```PowerShell
   Get-NetAdapter
   ```
   
   _**結果：**_

   |名稱 |InterfaceDescription |ifIndex |狀態 |MacAddress |LinkSpeed|
   |----|--------------------|------|----------|---------|------|
   |vEthernet (MGT) |HYPER-V 虛擬乙太網路介面卡 #2 |28 |Up | E4-1D-2D-07-40-71 |80 Gbps|
   ---
   
## <a name="step-8-test-hyper-v-vswitch-rdma"></a>步驟 8。 測試 HYPER-V vSwitch RDMA

下圖顯示您的 HYPER-V 主機，包括 HYPER-V 主機 1 上的 vSwitch 的目前狀態。

![測試 HYPER-V 虛擬交換器](../../media/Converged-NIC/6-datacenter-test-vswitch-rdma.jpg)

1. 設定來補充先前的 VLAN 設定主機 vNIC 上標記的優先順序。

   ```PowerShell    
   Set-VMNetworkAdapter -ManagementOS -Name "MGT" -IeeePriorityTag on
   Get-VMNetworkAdapter -ManagementOS -Name "MGT" | fl Name,IeeePriorityTag
   ```
   
   _**結果：**_  
      
   名稱：MGT  
   IeeePriorityTag:開啟  
    
2. 建立兩個主機 Vnic 的 RDMA，並將它們連接至 vSwitch VMSTEST。

   ```PowerShell    
   Add-VMNetworkAdapter –SwitchName "VMSTEST" –Name SMB1 –ManagementOS
   Add-VMNetworkAdapter –SwitchName "VMSTEST" –Name SMB2 –ManagementOS
   ```
   
3. 檢視管理 NIC 屬性。

   ```PowerShell    
   Get-VMNetworkAdapter -ManagementOS
   ```
   
   _**結果：**_ 

   |名稱 |IsManagementOs |VMName |SwitchName |MacAddress |狀態 |IPAddresses|
   |----|--------------|------|----------|----------|------|-----------|
   |CORP-External-Switch |True |CORP-External-Switch |001B785768AA|{[確定]} |&nbsp;| 
   |Mgt |True |VMSTEST |E41D2D074071 |{[確定]} |&nbsp;|
   |SMB1 |True |VMSTEST |00155D30AA00 |{[確定]} |&nbsp;|
   |SMB2 |True |VMSTEST |00155D30AA01 |{[確定]} |&nbsp;|
   ---
   
## <a name="step-9-assign-an-ip-address-to-the-smb-host-vnics-vethernet-smb1-and-vethernet-smb2"></a>步驟 9： 將 IP 位址指派給 SMB 主機 Vnic vEthernet \(SMB1\) vEthernet 和\(SMB2\)

測試-40 G-1 和 2-測試-40 G 的實體介面卡仍具有存取 VLAN 101 和 102 設定。 因為這個緣故，配接器標記流量-並 ping 成功。 先前，您會設定為零，這兩個 pNIC VLAN Id，然後 VMSTEST vSwitch 設 VLAN 101。 在那之後，您都仍然能夠使用 MGT vNIC，ping 遠端 VLAN 101 配接器，但目前沒有 VLAN 102 的成員。



1. 存取 VLAN 設定移除的實體 NIC，以防止這兩種自動標記為不正確的 VLAN 識別碼的輸出流量，以及防止篩選輸入資料傳輸不符合存取 VLAN id。

   ```PowerShell    
   New-NetIPAddress -InterfaceAlias "vEthernet (SMB1)" -IPAddress 192.168.2.111 -PrefixLength 24
   ```

   _**結果：**_  

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

2. 測試遠端 VLAN 102 配接器。
    
   ```PowerShell
   Test-NetConnection 192.168.2.5 
   ```
   
   _**結果：**_  
   
   ```
   ComputerName   : 192.168.2.5
   RemoteAddress  : 192.168.2.5
   InterfaceAlias : vEthernet (SMB1)
   SourceAddress  : 192.168.2.111
   PingSucceeded  : True
   PingReplyDetails (RTT) : 0 ms
   ```
    
3. 加入新的 IP 位址的介面 vEthernet \(SMB2\)。

   ```PowerShell
   New-NetIPAddress -InterfaceAlias "vEthernet (SMB2)" -IPAddress 192.168.2.222 -PrefixLength 24 
   ```
   
   _**結果：**_ 
   
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
   
4. 再次測試連線。    


5. 對預先存在的 VLAN 102 RDMA 主機 Vnic。

   ```PowerShell
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "SMB1" -VlanId "102" -Access -ManagementOS
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "SMB2" -VlanId "102" -Access -ManagementOS
    
   Get-VMNetworkAdapterVlan -ManagementOS
   ```

   _**結果：**_ 

   ```   
   VMName VMNetworkAdapterName Mode VlanList
   ------ -------------------- ---- --------
      SMB1 Access   102 
      Mgt  Access   101 
      SMB2 Access   102 
      CORP-External-Switch Untagged
   ```
   
6. 檢查 SMB1 和 SMB2 至基礎的實體 Nic vSwitch 設定小組下的對應。<p>主機 vNIC 至實體 Nic 的關聯是隨機的而且可能隨時重新平衡期間建立和解構。 在此情況下，您可以使用間接的機制，來檢查目前的關聯。 SMB1 和 SMB2 的 MAC 位址會與 NIC 小組成員測試 40 G 2 相關聯。 這並不理想，因為測試-40 G-1 並沒有相關聯的 SMB 主機 vNIC，而且不允許使用量的 RDMA 流量透過連結直到 SMB 主機 vNIC 會對應到它。

   ```PowerShell    
   Get-NetAdapterVPort (Preferred)
    
   Get-NetAdapterVmqQueue
   ```
   
   _**結果：**_ 
   
   ```
   Name   QueueID MacAddressVlanID Processor VmFriendlyName
   ----   ------- ---------------- --------- --------------
   TEST-40G-1 1   E4-1D-2D-07-40-71 1010:17
   TEST-40G-2 1   00-15-5D-30-AA-00 1020:17
   TEST-40G-2 2   00-15-5D-30-AA-01 1020:17
   ```

7. 檢視 VM 的網路介面卡內容。

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS
   ```
   
   _**結果：**_ 
   
   ```
   Name IsManagementOs VMName SwitchName   MacAddress   Status IPAddresses
   ---- -------------- ------ ----------   ----------   ------ -----------
   CORP-External-Switch True  CORP-External-Switch 001B785768AA {Ok}  
   Mgt  True  VMSTEST  E41D2D074071 {Ok}  
   SMB1 True  VMSTEST  00155D30AA00 {Ok}  
   SMB2 True  VMSTEST  00155D30AA01 {Ok}  
   ```

8. 檢視網路介面卡小組對應。<p>因為尚未執行的對應，結果應該不會傳回資訊。
    
   ```PowerShell
   Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName SMB1
   Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName SMB2
   ```
   
   
9. 對應 SMB1 和 SMB2 不同的實體 NIC 小組的成員，並檢視您的動作的結果。

   >[!IMPORTANT]
   >請確定您完成此步驟，在繼續之前，或您的實作就會失敗。
    
   ```PowerShell
   Set-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName "SMB1" -PhysicalNetAdapterName "Test-40G-1"
   Set-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName "SMB2" -PhysicalNetAdapterName "Test-40G-2"
    
   Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST
   ```

   _**結果：**_ 
   
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
   
10. 請確認先前建立的 MAC 關聯。

    ```PowerShell    
    Get-NetAdapterVmqQueue
    ```

    _**結果：**_ 
   
    ```   
    Name   QueueID MacAddressVlanID Processor VmFriendlyName
    ----   ------- ---------------- --------- --------------
    TEST-40G-1 1   E4-1D-2D-07-40-71 1010:17
    TEST-40G-1 2   00-15-5D-30-AA-00 1020:17
    TEST-40G-2 1   00-15-5D-30-AA-01 1020:17
    ```


11. 測試從遠端系統的連線，因為這兩個主機 Vnic 位於相同的子網路，且有相同的 VLAN ID \(102\)。

    ```PowerShell 
    Test-NetConnection 192.168.2.111
    ```

    _**結果：**_   

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

    _**結果：**_   

    ```
    ComputerName   : 192.168.2.222
    RemoteAddress  : 192.168.2.222
    InterfaceAlias : Test-40G-2
    SourceAddress  : 192.168.2.5
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms 
    ```
12. 設定名稱、 參數名稱和優先順序標記。   

    ```PowerShell
    Set-VMNetworkAdapter -ManagementOS -Name "SMB1" -IeeePriorityTag on
    Set-VMNetworkAdapter -ManagementOS -Name "SMB2" -IeeePriorityTag on
    Get-VMNetworkAdapter -ManagementOS -Name "SMB*" | fl Name,SwitchName,IeeePriorityTag,Status
    ```

    _**結果：**_   
    
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

13. 檢視虛擬網路介面卡內容。
    
    ```PowerShell
    Get-NetAdapterRdma -Name "vEthernet*" | sort Name | ft -AutoSize
    ```

    _**結果：**_   

    ```
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  False  
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  False  
    vEthernet (MGT)   Hyper-V Virtual Ethernet Adapter #2  False   
    ```

14. 啟用虛擬網路介面卡。  
    
    ```PowerShell
    Enable-NetAdapterRdma -Name "vEthernet (SMB1)"
    Enable-NetAdapterRdma -Name "vEthernet (SMB2)"
    Get-NetAdapterRdma -Name "vEthernet*" | sort Name | fl *
    ```

    _**結果：**_   
    
    ```
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  True   
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  True  
    vEthernet (MGT)   Hyper-V Virtual Ethernet Adapter #2  False
    ```

## <a name="step-10-validate-the-rdma-functionality"></a>步驟 10。 確認 RDMA 功能。

您想要驗證遠端系統的 RDMA 功能，對本機系統，有了 vSwitch，這兩個的 vSwitch 將小組的成員。<p>因為這兩個主機 Vnic \(SMB1 和 SMB2\)指派至 VLAN 102 中，您可以選取遠端系統上的 VLAN 102 介面卡。 <p>在此範例中，NIC 為測試-40 G-2 會 RDMA SMB1 (192.168.2.111) 和 SMB2 (192.168.2.222)。

>[!TIP]
>您可能需要停用此系統上的防火牆。  如需詳細資訊，請參閱您的網狀架構原則。
>
>```PowerShell
>Set-NetFirewallProfile -All -Enabled False
>    
>Get-NetAdapterAdvancedProperty -Name "Test-40G-2"
>```
>
>_**結果：**_ 
>   
>```
>Name  DisplayNameDisplayValue   RegistryKeyword RegistryValue  
>----  -----------------------   --------------- -------------  
> .
> .
>Test-40G-2VLAN ID102VlanID  {102} 
>```
    
1. 檢視網路介面卡內容。

   ```PowerShell
   Get-NetAdapter
   ```
    
   _**結果：**_ 
    
   ```
   Name  InterfaceDescriptionifIndex Status   MacAddress LinkSpeed
   ----  --------------------------- ------   ---------- ---------
   Test-40G-2Mellanox ConnectX-3 Pro Ethernet A...#3   3 Up   E4-1D-2D-07-43-D140 Gbps
   ```
   
2. 檢視網路介面卡 RDMA 資訊。

   ```PowerShell
   Get-NetAdapterRdma
   ```
    
   _**結果：**_  
    
   ```
   Name  InterfaceDescription Enabled
   ----  -------------------- -------
   Test-40G-2Mellanox ConnectX-3 Pro Ethernet Adap... True   
   ```

3. 對第一個的實體介面卡的 RDMA 流量測試。

   ```PowerShell 
   C:\TEST\Test-RDMA.PS1 -IfIndex 3 -IsRoCE $true -RemoteIpAddress 192.168.2.111 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```
    
   _**結果：**_ 
    
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

4. 執行第二個實體介面卡上的 RDMA 流量測試。

   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 3 -IsRoCE $true -RemoteIpAddress 192.168.2.222 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```
    
   _**結果：**_ 
    
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
    
5. 從本機到遠端電腦的 RDMA 流量測試。

    ```PowerShell
    Get-NetAdapter | ft –AutoSize
    ```
    
    _**結果：**_ 
    
    ```
    Name  InterfaceDescriptionifIndex Status   MacAddress LinkSpeed
    ----  --------------------------- ------   ---------- ---------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  45 Up   00-15-5D-30-AA-0380 Gbps
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  41 Up   00-15-5D-30-AA-0280 Gbps
    ```

6. 對第一個虛擬介面卡的 RDMA 流量測試。    
    
   ```
   C:\TEST\Test-RDMA.PS1 -IfIndex 41 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```
    
   _**結果：**_ 
    
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

7. 對第二個虛擬介面卡的 RDMA 流量測試。

   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 45 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```
    
   _**結果：**_ 
    
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
    
在此輸出中，最後一行"RDMA 流量測試成功：RDMA 流量傳送至 192.168.2.5，「 顯示，您已成功在您的配接器上設定交集的 NIC。

## <a name="related-topics"></a>相關主題 

- [具有單一網路介面卡的交集的 NIC 設定](cnic-single.md)
- [交集的 NIC 的實體交換器組態](cnic-app-switch-config.md)
- [疑難排解交集 NIC 組態](cnic-app-troubleshoot.md)
