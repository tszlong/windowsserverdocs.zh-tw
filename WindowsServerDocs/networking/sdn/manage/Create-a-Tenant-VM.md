---
title: 建立 VM 與網路或 VLAN 連接至 Virtual 房客
description: 本主題是輔助的軟體定義網路上如何管理承租人工作負載和 Windows Server 2016 Virtual 網路的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3c62f533-1815-4f08-96b1-dc271f5a2b36
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 001eb3efa073e4ffbdfad41f88949303159a7274
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="create-a-vm-and-connect-to-a-tenant-virtual-network-or-vlan"></a>建立 VM 與網路或 VLAN 連接至 Virtual 房客

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用此主題建立房客 virtual 電腦 \(VM\) 並連接到任一 Virtual 的網路，您的 HYPER-V 網路模擬建立或 virtual 本機的區域網路 \(VLAN\) VM。 

本主題包含下列各節。

- [建立 VM 及使用 Windows PowerShell Network Controller cmdlet 連接 Virtual 網路](#bkmk_vn)
- [建立 VM，以及連接至 VLAN 使用 NetworkControllerRESTWrappers](#bkmk_vlan)

## <a name="requirements"></a>需求
之前，請先執行下列區段中的程序，請注意下列需求。

1. 您必須建立 VM 網路介面卡的靜態媒體存取控制 \(MAC\) 位址，讓 VM 生命不會變更 VM 的 MAC 地址。 
>[!NOTE]
>如果 VM MAC 位址變更 VM 生命，Network Controller 無法設定所需的網路介面卡的原則。 若並未設定為網路介面卡的原則，將無法處理網路流量的網路介面卡和所有通訊網路的問題將會都失敗。 

2. 如果 VM 需要在開機網路存取權，請務必不開始在最後的設定步驟-上 VM 網路介面卡連接埠設定介面 ID 後 VM。 如果您開始 VM，才能完成此步驟，VM 無法通訊網路上 Network Controller 中建立網路介面控制器已套用所有適用的原則-Virtual 的網路原則，直到存取控制清單 \(ACLs\) 和服務 \(QoS\) 品質。

您也可以使用的程序部署 virtual 設備本主題中所述。 有幾個額外的步驟，您可以設定設備處理，或查看 flow 或其他 Vm Virtual 網路上的資料封包。

>[!IMPORTANT]
>以下的各節包含包含許多參數值範例範例 Windows PowerShell 命令。 請確認值是適用於您的部署，執行下列命令之前，先取代範例值這些命令列中。  

## <a name="bkmk_vn"></a>建立 VM 及使用 Windows PowerShell Network Controller cmdlet 連接 Virtual 網路

這一節包含下列主題。

1.  [建立 VM VM 網路介面卡的靜態的 MAC 位址](#bkmk_create)
2.  [取得，其中包含您要的網路介面卡連接子的網路 Virtual 網路](#bkmk_getvn)
3.  [Network Controller 中建立網路介面物件](#bkmk_object)
4.  [執行個體識別碼取得 Network Controller 的網路介面](#bkmk_getinstance)
5.  [設定介面 ID HYPER-V VM 網路介面卡連接埠](#bkmk_setinstance)
6.  [[開始] VM](#bkmk_start)

### <a name="bkmk_create"></a>建立 VM VM 網路介面卡的靜態的 MAC 位址

建立 VM 網路介面卡的靜態的 MAC 位址，請使用下列命令範例。

    
    New-VM -Generation 2 -Name "MyVM" -Path "C:\VMs\MyVM" -MemoryStartupBytes 4GB -VHDPath "c:\VMs\MyVM\Virtual Hard Disks\WindowsServer2016.vhdx" -SwitchName "SDNvSwitch" 
    
    Set-VM -Name "MyVM" -ProcessorCount 4
    
    Set-VMNetworkAdapter -VMName "MyVM" -StaticMacAddress "00-11-22-33-44-55" 
    

### <a name="bkmk_getvn"></a>取得，其中包含您要的網路介面卡連接子的網路 Virtual 網路
確定您已經有建立 Virtual 網路才能使用此範例中的命令。 如需詳細資訊，請查看[建立、Delete 或更新承租人 Virtual 網路](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create%2c-delete%2c-or-update-tenant-virtual-networks)。

若要取得 Virtual 網路，使用下列命令範例。

    
    $vnet = get-networkcontrollervirtualnetwork -connectionuri $uri -ResourceId “Contoso_WebTier”
    

>[!NOTE]
>如果您需要自訂 Acl 此網路介面，然後建立 ACL 現在主題中使用的指示來[使用存取控制清單 (Acl) 來管理 Datacenter 網路流量流程](../../sdn/manage/Use-Access-Control-Lists--ACLs--to-Manage-Datacenter-Network-Traffic-Flow.md)

### <a name="bkmk_object"></a>Network Controller 中建立網路介面物件

Network Controller 中建立網路介面物件，請使用下列命令範例。

>[!NOTE]
>如果您建立自訂 ACL 上述步驟之後，您現在使用。

    
    $vmnicproperties = new-object Microsoft.Windows.NetworkController.NetworkInterfaceProperties
    $vmnicproperties.PrivateMacAddress = "001122334455" 
    $vmnicproperties.PrivateMacAllocationMethod = "Static" 
    $vmnicproperties.IsPrimary = $true 

    $vmnicproperties.DnsSettings = new-object Microsoft.Windows.NetworkController.NetworkInterfaceDnsSettings
    $vmnicproperties.DnsSettings.DnsServers = @("24.30.1.11", "24.30.1.12")
    
    $ipconfiguration = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfiguration
    $ipconfiguration.resourceid = "MyVM_IP1"
    $ipconfiguration.properties = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfigurationProperties
    $ipconfiguration.properties.PrivateIPAddress = “24.30.1.101”
    $ipconfiguration.properties.PrivateIPAllocationMethod = "Static"
    
    $ipconfiguration.properties.Subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $ipconfiguration.properties.subnet.ResourceRef = $vnet.Properties.Subnets[0].ResourceRef
    
    $vmnicproperties.IpConfigurations = @($ipconfiguration)
    New-NetworkControllerNetworkInterface –ResourceID “MyVM_Ethernet1” –Properties $vmnicproperties –ConnectionUri $uri
    

### <a name="bkmk_getinstance"></a>執行個體識別碼取得 Network Controller 的網路介面
若要執行個體識別碼網路介面 Network Controller，使用下列命令範例。

    
    $nic = Get-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId "MyVM-Ethernet1"
    

### <a name="bkmk_setinstance"></a>設定介面 ID HYPER-V VM 網路介面卡連接埠
設定介面 ID 上 HYPER-V VM 網路介面卡連接埠，使用下列命令範例。

>[!NOTE]
>您必須執行下列命令，HYPER-V 主機上安裝 VM 的位置。

    
    #Do not change the hardcoded IDs in this section, because they are fixed values and must not change.
    
    $FeatureId = "9940cd46-8b06-43bb-b9d5-93d50381fd56"
    
    $vmNics = Get-VMNetworkAdapter -VMName “MyVM”
    
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
    

### <a name="bkmk_start"></a>[開始] VM
若要開始 VM 中，使用下列命令範例。

    
    Get-VM -Name “MyVM” | Start-VM 
    
您現在順利建立 VM、連接 VM 房客 Virtual 網路，並開始 VM，使其可以處理承租人工作負載。

## <a name="bkmk_vlan"></a>建立 VM，以及連接至 VLAN 使用 NetworkControllerRESTWrappers

這一節包含下列主題。

1. [建立 VM 和指派靜態 MAC 位址](#bkmk_mac)
2. [設定 VLAN ID VM 網路介面卡](#bkmk_vid)
3. [取得的邏輯網路子網路，並建立網路介面](#bkmk_subnet)
4. [執行個體識別碼設定 HYPER-V 連接埠](#bkmk_instance)
5. [[開始] VM](#bkmk_startvlan)


###<a name="bkmk_mac"></a>建立 VM 和指派靜態 MAC 位址
建立 VM，並將靜態媒體存取控制 \(MAC\) 位址指派給 VM 中，您可以使用下列命令範例。

    New-VM -Generation 2 -Name "MyVM" -Path "C:\VMs\MyVM" -MemoryStartupBytes 4GB -VHDPath "c:\VMs\MyVM\Virtual Hard Disks\WindowsServer2016.vhdx" -SwitchName "SDNvSwitch" 

    Set-VM -Name "MyVM" -ProcessorCount 4

    Set-VMNetworkAdapter -VMName "MyVM" -StaticMacAddress "00-11-22-33-44-55" 

###<a name="bkmk_vid"></a>設定 VLAN ID VM 網路介面卡
若要設定 VLAN ID 的網路介面卡，您可以使用下列範例命令。


    Set-VMNetworkAdapterIsolation –VMName “MyVM” -AllowUntaggedTraffic $true -IsolationMode VLAN -DefaultIsolationId 123


###<a name="bkmk_subnet"></a>取得的邏輯網路子網路，並建立網路介面

取得的邏輯網路子網路，並建立網路介面使用的邏輯網路子網路，您可以使用下列命令範例。


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
    $ipconfiguration.properties.PrivateIPAddress = “10.127.132.177”
    $ipconfiguration.properties.PrivateIPAllocationMethod = "Static"

    $ipconfiguration.properties.Subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $ipconfiguration.properties.subnet.ResourceRef = $logicalnet.Properties.Subnets[0].ResourceRef

    $vmnicproperties.IpConfigurations = @($ipconfiguration)
    $vnic = New-NetworkControllerNetworkInterface –ResourceID “MyVM_Ethernet1” –Properties $vmnicproperties –ConnectionUri $uri

    $vnic.InstanceId
    

###<a name="bkmk_instance"></a>執行個體識別碼設定 HYPER-V 連接埠
若要設定的執行個體識別碼 HYPER-V 連接埠，您可以使用下列命令範例 HYPER-V 主機上 VM 所在的位置。

  
    #The hardcoded Ids in this section are fixed values and must not change.
    $FeatureId = "9940cd46-8b06-43bb-b9d5-93d50381fd56"

    $vmNics = Get-VMNetworkAdapter -VMName “MyVM”

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


###<a name="bkmk_startvlan"></a>[開始] VM
若要開始 VM 中，您可以使用下列範例命令。


    Get-VM -Name “MyVM” | Start-VM 

您現在順利建立 VM、連接 VM VLAN，並開始 VM，使其可以處理承租人工作負載。

  

