---
title: 建立 VM 並連線至租用戶虛擬網路或 VLAN
description: 在本主題中，我們將示範如何建立租使用者 VM，並將它連線到您使用 Hyper-v 網路虛擬化或虛擬區域網路（VLAN）所建立的虛擬網路。
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 3c62f533-1815-4f08-96b1-dc271f5a2b36
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/24/2018
ms.openlocfilehash: 3949c4f10015a7fdfe4955b950b109a702553c45
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854511"
---
# <a name="create-a-vm-and-connect-to-a-tenant-virtual-network-or-vlan"></a>建立 VM 並連線至租用戶虛擬網路或 VLAN

>適用於：Windows Server (半年通道)、Windows Server 2016

在本主題中，您會建立租使用者 VM，並將它連線至使用 Hyper-v 網路虛擬化或虛擬區域網路（VLAN）所建立的虛擬網路。 您可以使用 Windows PowerShell 網路控制卡 Cmdlet 來連線到虛擬網路或 NetworkControllerRESTWrappers，以連線到 VLAN。

使用本主題中所述的處理常式來部署虛擬裝置。 有幾個額外的步驟，您就可以設定設備來處理或檢查虛擬網路上的其他 Vm 之間流動或流出的資料封包。

本主題中的各節包含範例 Windows PowerShell 命令，其中包含許多參數的範例值。 執行這些命令之前，請務必將這些命令中的範例值取代為適用于您的部署的值。 


## <a name="prerequisites"></a>必要條件

1. 在 VM 的存留期內，使用靜態 MAC 位址建立的 VM 網路介面卡。<p>如果在 VM 存留期間，MAC 位址變更，網路控制卡將無法設定網路介面卡所需的原則。 未設定網路的原則可防止網路介面卡處理網路流量，而且所有與網路的通訊都會失敗。  

2. 如果 VM 在啟動時需要網路存取，請勿啟動 VM，直到在 VM 網路介面卡埠上設定介面識別碼為止。 如果您在設定介面識別碼之前啟動 VM，且網路介面不存在，VM 就無法在網路控制站的網路上通訊，並套用所有原則。

3. 如果您需要此網路介面的自訂 Acl，請使用[使用存取控制清單（acl）管理資料中心網路流量流程](../../sdn/manage/Use-Access-Control-Lists--ACLs--to-Manage-Datacenter-Network-Traffic-Flow.md)主題中的指示，立即建立 acl

請確定您已建立虛擬網路，然後再使用此範例命令。 如需詳細資訊，請參閱[建立、刪除或更新租使用者虛擬網路](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create%2c-delete%2c-or-update-tenant-virtual-networks)。

## <a name="create-a-vm-and-connect-to-a-virtual-network-by-using-the-windows-powershell-network-controller-cmdlets"></a>使用 Windows PowerShell 網路控制卡 Cmdlet 建立 VM 並聯機至虛擬網路


1. 使用具有靜態 MAC 位址的 VM 網路介面卡來建立 VM。 

   ```PowerShell    
   New-VM -Generation 2 -Name "MyVM" -Path "C:\VMs\MyVM" -MemoryStartupBytes 4GB -VHDPath "c:\VMs\MyVM\Virtual Hard Disks\WindowsServer2016.vhdx" -SwitchName "SDNvSwitch" 
    
   Set-VM -Name "MyVM" -ProcessorCount 4
    
   Set-VMNetworkAdapter -VMName "MyVM" -StaticMacAddress "00-11-22-33-44-55" 
   ```

2. 取得虛擬網路，其中包含您要連接網路介面卡的子網。

   ```Powershell 
   $vnet = get-networkcontrollervirtualnetwork -connectionuri $uri -ResourceId "Contoso_WebTier"
   ```

3. 在網路控制卡中建立網路介面物件。

   >[!TIP]
   >在此步驟中，您會使用自訂 ACL。

   ```PowerShell
   $vmnicproperties = new-object Microsoft.Windows.NetworkController.NetworkInterfaceProperties
   $vmnicproperties.PrivateMacAddress = "001122334455" 
   $vmnicproperties.PrivateMacAllocationMethod = "Static" 
   $vmnicproperties.IsPrimary = $true 

   $vmnicproperties.DnsSettings = new-object Microsoft.Windows.NetworkController.NetworkInterfaceDnsSettings
   $vmnicproperties.DnsSettings.DnsServers = @("24.30.1.11", "24.30.1.12")
    
   $ipconfiguration = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfiguration
   $ipconfiguration.resourceid = "MyVM_IP1"
   $ipconfiguration.properties = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfigurationProperties
   $ipconfiguration.properties.PrivateIPAddress = "24.30.1.101"
   $ipconfiguration.properties.PrivateIPAllocationMethod = "Static"
    
   $ipconfiguration.properties.Subnet = new-object Microsoft.Windows.NetworkController.Subnet
   $ipconfiguration.properties.subnet.ResourceRef = $vnet.Properties.Subnets[0].ResourceRef
    
   $vmnicproperties.IpConfigurations = @($ipconfiguration)
   New-NetworkControllerNetworkInterface –ResourceID "MyVM_Ethernet1" –Properties $vmnicproperties –ConnectionUri $uri
   ```

4. 從網路控制卡取得網路介面的 InstanceId。

   ```PowerShell 
    $nic = Get-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId "MyVM-Ethernet1"
   ```

5. 設定 Hyper-v VM 網路介面卡埠上的介面識別碼。

   >[!NOTE]
   >您必須在已安裝 VM 的 Hyper-v 主機上執行這些命令。

   ```PowerShell 
   #Do not change the hardcoded IDs in this section, because they are fixed values and must not change.
    
   $FeatureId = "9940cd46-8b06-43bb-b9d5-93d50381fd56"
    
   $vmNics = Get-VMNetworkAdapter -VMName "MyVM"
    
   $CurrentFeature = Get-VMSwitchExtensionPortFeature -FeatureId $FeatureId -VMNetworkAdapter $vmNics
    
   if ($CurrentFeature -eq $null)
   {
   $Feature = Get-VMSystemSwitchExtensionPortFeature -FeatureId $FeatureId
    
   $Feature.SettingData.ProfileId = "{$($nic.InstanceId)}"
   $Feature.SettingData.NetCfgInstanceId = "{56785678-a0e5-4a26-bc9b-c0cba27311a3}"
   $Feature.SettingData.CdnLabelString = "TestCdn"
   $Feature.SettingData.CdnLabelId = 1111
   $Feature.SettingData.ProfileName = "Testprofile"
   $Feature.SettingData.VendorId = "{1FA41B39-B444-4E43-B35A-E1F7985FD548}"
   $Feature.SettingData.VendorName = "NetworkController"
   $Feature.SettingData.ProfileData = 1
    
   Add-VMSwitchExtensionPortFeature -VMSwitchExtensionFeature  $Feature -VMNetworkAdapter $vmNics
   }
   else
   {
   $CurrentFeature.SettingData.ProfileId = "{$($nic.InstanceId)}"
   $CurrentFeature.SettingData.ProfileData = 1
    
   Set-VMSwitchExtensionPortFeature -VMSwitchExtensionFeature $CurrentFeature  -VMNetworkAdapter $vmNic
   }
   ```

6. 啟動 VM。

   ```PowerShell
    Get-VM -Name "MyVM" | Start-VM 
   ```

您已成功建立 VM、將 VM 連線到租使用者虛擬網路，並已啟動 VM，使其可以處理租使用者工作負載。

## <a name="create-a-vm-and-connect-to-a-vlan-by-using-networkcontrollerrestwrappers"></a>使用 NetworkControllerRESTWrappers 建立 VM 並聯機到 VLAN


1. 建立 VM，並將靜態 MAC 位址指派給 VM。

   ```PowerShell
   New-VM -Generation 2 -Name "MyVM" -Path "C:\VMs\MyVM" -MemoryStartupBytes 4GB -VHDPath "c:\VMs\MyVM\Virtual Hard Disks\WindowsServer2016.vhdx" -SwitchName "SDNvSwitch" 

   Set-VM -Name "MyVM" -ProcessorCount 4

   Set-VMNetworkAdapter -VMName "MyVM" -StaticMacAddress "00-11-22-33-44-55" 
   ```

2. 設定 VM 網路介面卡上的 VLAN ID。

   ```PowerShell
   Set-VMNetworkAdapterIsolation –VMName "MyVM" -AllowUntaggedTraffic $true -IsolationMode VLAN -DefaultIsolationId 123
   ```

3. 取得邏輯網路子網並建立網路介面。 

   ```PowerShell
    $logicalnet = get-networkcontrollerLogicalNetwork -connectionuri $uri -ResourceId "00000000-2222-1111-9999-000000000002"

    $vmnicproperties = new-object Microsoft.Windows.NetworkController.NetworkInterfaceProperties
    $vmnicproperties.PrivateMacAddress = "00-1D-C8-B7-01-02"
    $vmnicproperties.PrivateMacAllocationMethod = "Static"
    $vmnicproperties.IsPrimary = $true 
    
    $vmnicproperties.DnsSettings = new-object Microsoft.Windows.NetworkController.NetworkInterfaceDnsSettings
    $vmnicproperties.DnsSettings.DnsServers = $logicalnet.Properties.Subnets[0].DNSServers

    $ipconfiguration = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfiguration
    $ipconfiguration.resourceid = "MyVM_Ip1"
    $ipconfiguration.properties = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfigurationProperties
    $ipconfiguration.properties.PrivateIPAddress = "10.127.132.177"
    $ipconfiguration.properties.PrivateIPAllocationMethod = "Static"

    $ipconfiguration.properties.Subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $ipconfiguration.properties.subnet.ResourceRef = $logicalnet.Properties.Subnets[0].ResourceRef

    $vmnicproperties.IpConfigurations = @($ipconfiguration)
    $vnic = New-NetworkControllerNetworkInterface –ResourceID "MyVM_Ethernet1" –Properties $vmnicproperties –ConnectionUri $uri

    $vnic.InstanceId
   ```

4. 設定 Hyper-v 埠上的 InstanceId。

   ```PowerShell  
   #The hardcoded Ids in this section are fixed values and must not change.
   $FeatureId = "9940cd46-8b06-43bb-b9d5-93d50381fd56"

   $vmNics = Get-VMNetworkAdapter -VMName "MyVM"

   $CurrentFeature = Get-VMSwitchExtensionPortFeature -FeatureId $FeatureId -VMNetworkAdapter $vmNic
        
   if ($CurrentFeature -eq $null)
   {
       $Feature = Get-VMSystemSwitchExtensionFeature -FeatureId $FeatureId
        
       $Feature.SettingData.ProfileId = "{$InstanceId}"
       $Feature.SettingData.NetCfgInstanceId = "{56785678-a0e5-4a26-bc9b-c0cba27311a3}"
       $Feature.SettingData.CdnLabelString = "TestCdn"
       $Feature.SettingData.CdnLabelId = 1111
       $Feature.SettingData.ProfileName = "Testprofile"
       $Feature.SettingData.VendorId = "{1FA41B39-B444-4E43-B35A-E1F7985FD548}"
       $Feature.SettingData.VendorName = "NetworkController"
       $Feature.SettingData.ProfileData = 1
                
       Add-VMSwitchExtensionFeature -VMSwitchExtensionFeature  $Feature -VMNetworkAdapter $vmNic
   }        
   else
   {
       $CurrentFeature.SettingData.ProfileId = "{$InstanceId}"
       $CurrentFeature.SettingData.ProfileData = 1

       Set-VMSwitchExtensionPortFeature -VMSwitchExtensionFeature $CurrentFeature  -VMNetworkAdapter $vmNic
   }
   ```

5. 啟動 VM。

   ```PowerShell
   Get-VM -Name "MyVM" | Start-VM 
   ```

您已成功建立 VM、將 VM 連線至 VLAN，並已啟動 VM，使其可以處理租使用者工作負載。

  

