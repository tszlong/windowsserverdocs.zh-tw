---
title: 升級、 備份和還原 SDN 基礎結構
description: 本主題中，您將了解如何更新、 備份和還原 SDN 基礎結構。
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: e9a8f2fd-48fe-4a90-9250-f6b32488b7a4
ms.author: grcusanz
author: shortpatti
ms.date: 08/27/2018
ms.openlocfilehash: 3374d1b79b84edd78dca3b61c73ea2db1dff9561
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854459"
---
# <a name="upgrade-backup-and-restore-sdn-infrastructure"></a>升級、 備份和還原 SDN 基礎結構

>適用於：Windows Server （半年通道），Windows Server 2016

本主題中，您將了解如何更新、 備份和還原 SDN 基礎結構。 

## <a name="upgrade-the-sdn-infrastructure"></a>升級的 SDN 基礎結構
SDN 基礎結構可以從 Windows Server 2016 升級至 Windows Server 2019。 升級順序，請遵循相同的步驟順序，如 「 更新 SDN 基礎結構 」 一節中所述。 之前升級，建議要備份的網路控制卡資料庫。

適用於網路控制站機器中，使用 Get NetworkControllerNode cmdlet 來升級完成之後，請檢查節點的狀態。 請確定節點恢復為升級的其他節點之前 「 啟動 」 狀態。 一旦您已升級的所有網路控制卡節點，網路控制站更新網路控制卡叢集中執行的一小時內的微服務。 您可以使用更新 networkcontroller cmdlet 立即更新觸發程序。 

在所有的軟體定義網路 (SDN) 系統，其中包含作業系統元件上安裝相同的 Windows 更新：

- SDN 啟用 HYPER-V 主機
- 網路控制卡 Vm
- 軟體負載平衡器 Mux Vm
- RAS 閘道的 Vm 

>[!IMPORTANT]
>如果您使用 System Center Virtual Manager，您必須使用最新的更新彙總套件來進行更新。

當您更新每個元件時，您可以使用任何標準方法，安裝 Windows 更新。 不過，為了確保最少的停機時間的工作負載和網路控制卡資料庫的完整性，請遵循下列步驟：

1. 更新管理主控台。<p>在每個您用來使用網路控制站的 Powershell 模組的電腦上安裝更新。  任何位置，包括您已自行安裝的 RSAT NetworkController 角色。 不包括網路控制卡 Vm 本身，您可以更新它們的下一個步驟。

2. 在第一個網路控制站 VM 上安裝所有更新，然後重新啟動。

3. 在繼續之前的下一步 的網路控制站 vm，使用`get-networkcontrollernode`cmdlet 來檢查之節點的更新，並重新啟動的狀態。

4. 在重新開機循環期間等候網路控制卡節點關閉，再啟動一次。<p>之後重新啟動 VM，可能需要幾分鐘，它就會歸還給才能**_向上_** 狀態。 如需輸出的範例，請參閱 

5. 其中每個 SLB Mux VM 上安裝更新，以確保持續可用的負載平衡器基礎結構的一次。

6. 更新 HYPER-V 主機和 RAS 閘道，包含位於 RAS 閘道的主機開始**待命**模式。<p>RAS 閘道的 Vm 無法即時移轉，而不會遺失租用戶的連線。 在更新週期中，您必須非常小心的次數降到最低租用戶連接到新的 RAS 閘道的容錯移轉。 協調更新的主機和 RAS 閘道，每個租用戶會容錯移轉一次，最多。

    a. 撤除能夠即時移轉的 Vm 的主機。<p>RAS 閘道 Vm 應保留在主機上。

    b. 此主機上的每個閘道 VM 上安裝更新。

    c.  如果更新需要 VM 重新啟動閘道再重新啟動 VM。  

    d. 包含閘道剛更新的 VM 主機上安裝更新。

    e. 如果所需的更新，請重新啟動主機。

    f. 重複的每個額外的主機，其中包含待命的閘道。<p>如果仍然沒有待命的閘道器，然後遵循相同的步驟適用於所有剩餘的主機。


### <a name="example-use-the-get-networkcontrollernode-cmdlet"></a>範例：使用 get networkcontrollernode cmdlet 

在此範例中，您會看到的輸出`get-networkcontrollernode`指令程式從網路控制卡 Vm 的其中一個內執行。  

您在此範例輸出中看到節點的狀態是：

- NCNode1.contoso.com = Down
- NCNode2.contoso.com = Up
- NCNode3.contoso.com = Up

>[!IMPORTANT]
>您必須等待數分鐘，直到狀態的節點會變更為_**向上**_ 更新一次一個任何其他節點之前。

一旦您已更新的所有網路控制卡節點，網路控制站更新網路控制卡叢集中執行的一小時內的微服務。 

>[!TIP]
>您可以觸發立即更新使用`update-networkcontroller`cmdlet。


```Powershell
PS C:\> get-networkcontrollernode
Name            : NCNode1.contoso.com
Server          : NCNode1.Contoso.com
FaultDomain     : fd:/NCNode1.Contoso.com
RestInterface   : Ethernet
NodeCertificate :
Status          : Down

Name            : NCNode2.Contoso.com
Server          : NCNode2.contoso.com
FaultDomain     : fd:/ NCNode2.Contoso.com
RestInterface   : Ethernet
NodeCertificate :
Status          : Up

Name            : NCNode3.Contoso.com
Server          : NCNode3.Contoso.com
FaultDomain     : fd:/ NCNode3.Contoso.com
RestInterface   : Ethernet
NodeCertificate :
Status          : Up
```

### <a name="example-use-the-update-networkcontroller-cmdlet"></a>範例：使用更新 networkcontroller cmdlet
在此範例中，您會看到的輸出`update-networkcontroller`cmdlet 來強制更新網路控制站。 

>[!IMPORTANT]
>當您有沒有更多的更新安裝時，請執行這個指令程式。


```Powershell
PS C:\> update-networkcontroller
NetworkControllerClusterVersion NetworkControllerVersion
------------------------------- ------------------------
10.1.1                          10.1.15
```

## <a name="backup-the-sdn-infrastructure"></a>備份 SDN 基礎結構

網路控制卡資料庫的定期備份可確保發生災害或資料遺失的商務持續性。  備份網路控制卡 Vm 不足，無法因為它無法確保工作階段會繼續在多個網路控制卡節點。

**需求：**
* SMB 共用，並具有共用和檔案系統之讀取/寫入權限的認證。
* 如果使用 GMSA 一併安裝網路控制站，您可以選擇使用群組受控服務帳戶 (GMSA)。

**程序：**

1.  使用您選擇的 VM 備份的方法，或使用 HYPER-V 匯出每個網路控制站 VM 的複本。<p>備份網路控制站 VM，可確保必要的憑證來解密資料庫都存在。  

2.  若使用 System Center Virtual Machine Manager (SCVMM)，請停止 SCVMM 服務，並透過 SQL Server 備份。<p>這裡的目標是確保在此期間，這可能造成不一致的備份網路控制站和 SCVMM 更新取得 scvmm 中進行。  

   >[!IMPORTANT]
   >重新之前不會啟動 SCVMM 服務網路控制卡備份已完成。

3.  備份與網路控制卡資料庫`new-networkcontrollerbackup`cmdlet。

4.  檢查完成和成功的備份，與`get-networkcontrollerbackup`cmdlet。

5.  如果使用 SCVMM，啟動 SCVMM 服務。



### <a name="example-backing-up-the-network-controller-database"></a>範例：將網路控制卡資料庫備份

```Powershell
$URI = "https://NC.contoso.com"
$Credential = Get-Credential

# Get or Create Credential object for File share user

$ShareUserResourceId = "BackupUser"

$ShareCredential = Get-NetworkControllerCredential -ConnectionURI $URI -Credential $Credential | Where {$_.ResourceId -eq $ShareUserResourceId }
If ($ShareCredential -eq $null) {
    $CredentialProperties = New-Object Microsoft.Windows.NetworkController.CredentialProperties
    $CredentialProperties.Type = "usernamePassword"
    $CredentialProperties.UserName = "contoso\alyoung"
    $CredentialProperties.Value = "<Password>"

    $ShareCredential = New-NetworkControllerCredential -ConnectionURI $URI -Credential $Credential -Properties $CredentialProperties -ResourceId $ShareUserResourceId -Force
}

# Create backup

$BackupTime = (get-date).ToString("s").Replace(":", "_")

$BackupProperties = New-Object Microsoft.Windows.NetworkController.NetworkControllerBackupProperties
$BackupProperties.BackupPath = "\\fileshare\backups\NetworkController\$BackupTime"
$BackupProperties.Credential = $ShareCredential

$Backup = New-NetworkControllerBackup -ConnectionURI $URI -Credential $Credential -Properties $BackupProperties -ResourceId $BackupTime -Force
```

### <a name="example-checking-the-status-of-a-network-controller-backup-operation"></a>範例：檢查網路控制站的備份作業的狀態

```Powershell
PS C:\ > Get-NetworkControllerBackup -ConnectionUri $URI -Credential $Credential -ResourceId $Backup.ResourceId
| ConvertTo-JSON -Depth 10
{
    "Tags":  null,
    "ResourceRef":  "/networkControllerBackup/2017-04-25T16_53_13",
    "InstanceId":  "c3ea75ae-2892-4e10-b26c-a2243b755dc8",
    "Etag":  "W/\"0dafea6c-39db-401b-bda5-d2885ded470e\"",
    "ResourceMetadata":  null,
    "ResourceId":  "2017-04-25T16_53_13",
    "Properties":  {
                    "BackupPath":  "\\\\fileshare\backups\NetworkController\\2017-04-25T16_53_13",
                    "ErrorMessage":  "",
                    "FailedResourcesList":  [

                                            ],
                    "SuccessfulResourcesList":  [
                                                    "/networking/v1/credentials/11ebfc10-438c-4a96-a1ee-8a048ce675be",
                                                    "/networking/v1/credentials/41229069-85d4-4352-be85-034d0c5f4658",
                                                    "/networking/v1/credentials/b2a82c93-2583-4a1f-91f8-232b801e11bb",
                                                    "/networking/v1/credentials/BackupUser",
                                                    "/networking/v1/credentials/fd5b1b96-b302-4395-b6cd-ed9703435dd1",
                                                    "/networking/v1/virtualNetworkManager/configuration",
                                                    "/networking/v1/virtualSwitchManager/configuration",
                                                    "/networking/v1/accessControlLists/f8b97a4c-4419-481d-b757-a58483512640",
                                                    "/networking/v1/logicalnetworks/24fa1af9-88d6-4cdc-aba0-66e38c1a7bb8",
                                                    "/networking/v1/logicalnetworks/48610528-f40b-4718-938e-99c2be76f1e0",
                                                    "/networking/v1/logicalnetworks/89035b49-1ee3-438a-8d7a-f93cbae40619",
                                                    "/networking/v1/logicalnetworks/a9c8eaa0-519c-4988-acd6-11723e9efae5",
                                                    "/networking/v1/logicalnetworks/d4ea002c-c926-4c57-a178-461d5768c31f",
                                                    "/networking/v1/macPools/11111111-1111-1111-1111-111111111111",
                                                    "/networking/v1/loadBalancerManager/config",
                                                    "/networking/v1/publicIPAddresses/2c502b2d-b39a-4be1-a85a-55ef6a3a9a1d",
                                                    "/networking/v1/GatewayPools/Default",
                                                    "/networking/v1/servers/4c4c4544-0058-5810-8056-b4c04f395931",
                                                    "/networking/v1/servers/4c4c4544-0058-5810-8057-b4c04f395931",
                                                    "/networking/v1/servers/4c4c4544-0058-5910-8056-b4c04f395931",
                                                    "/networking/v1/networkInterfaces/058430d3-af43-4328-a440-56540f41da50",
                                                    "/networking/v1/networkInterfaces/08756090-6d55-4dec-98d5-80c4c5a47db8",
                                                    "/networking/v1/networkInterfaces/2175d74a-aacd-44e2-80d3-03f39ea3bc5d",
                                                    "/networking/v1/networkInterfaces/2400c2c3-2291-4b0b-929c-9bb8da55851a",
                                                    "/networking/v1/networkInterfaces/4c695570-6faa-4e4d-a552-0b36ed3e0962",
                                                    "/networking/v1/networkInterfaces/7e317638-2914-42a8-a2dd-3a6d966028d6",
                                                    "/networking/v1/networkInterfaces/834e3937-f43b-4d3c-88be-d79b04e63bce",
                                                    "/networking/v1/networkInterfaces/9d668fe6-b1c6-48fc-b8b1-b3f98f47d508",
                                                    "/networking/v1/networkInterfaces/ac4650ac-c3ef-4366-96e7-d9488fb661ba",
                                                    "/networking/v1/networkInterfaces/b9f23e35-d79e-495f-a1c9-fa626b85ae13",
                                                    "/networking/v1/networkInterfaces/fdd929f1-f64f-4463-949a-77b67fe6d048",
                                                    "/networking/v1/virtualServers/15a891ee-7509-4e1d-878d-de0cb4fa35fd",
                                                    "/networking/v1/virtualServers/57416993-b410-44fd-9675-727cd4e98930",
                                                    "/networking/v1/virtualServers/5f8aebdc-ee5b-488f-ac44-dd6b57bd316a",
                                                    "/networking/v1/virtualServers/6c812217-5931-43dc-92a8-1da3238da893",
                                                    "/networking/v1/virtualServers/d78b7fa3-812d-4011-9997-aeb5ded2b431",
                                                    "/networking/v1/virtualServers/d90820a5-635b-4016-9d6f-bf3f1e18971d",
                                                    "/networking/v1/loadBalancerMuxes/5f8aebdc-ee5b-488f-ac44-dd6b57bd316a_suffix",
                                                    "/networking/v1/loadBalancerMuxes/d78b7fa3-812d-4011-9997-aeb5ded2b431_suffix",
                                                    "/networking/v1/loadBalancerMuxes/d90820a5-635b-4016-9d6f-bf3f1e18971d_suffix",
                                                    "/networking/v1/Gateways/15a891ee-7509-4e1d-878d-de0cb4fa35fd_suffix",
                                                    "/networking/v1/Gateways/57416993-b410-44fd-9675-727cd4e98930_suffix",
                                                    "/networking/v1/Gateways/6c812217-5931-43dc-92a8-1da3238da893_suffix",
                                                    "/networking/v1/virtualNetworks/b3dbafb9-2655-433d-b47d-a0e0bbac867a",
                                                    "/networking/v1/virtualNetworks/d705968e-2dc2-48f2-a263-76c7892fb143",
                                                    "/networking/v1/loadBalancers/24fa1af9-88d6-4cdc-aba0-66e38c1a7bb8_10.127.132.2",
                                                    "/networking/v1/loadBalancers/24fa1af9-88d6-4cdc-aba0-66e38c1a7bb8_10.127.132.3",
                                                    "/networking/v1/loadBalancers/24fa1af9-88d6-4cdc-aba0-66e38c1a7bb8_10.127.132.4"
                                                ],
                    "InProgressResourcesList":  [

                                                ],
                    "ProvisioningState":  "Succeeded",
                    "Credential":  {
                                        "Tags":  null,
                                        "ResourceRef":  "/credentials/BackupUser",
                                        "InstanceId":  "00000000-0000-0000-0000-000000000000",
                                        "Etag":  null,
                                        "ResourceMetadata":  null,
                                        "ResourceId":  null,
                                        "Properties":  null
                                    }
                }
}
```

## <a name="restore-the-sdn-infrastructure-from-a-backup"></a>從備份還原 SDN 基礎結構

當您從備份還原所有必要的元件時，SDN 環境就會傳回作業的狀態。  

>[!IMPORTANT]
>步驟視還原的元件數目而有所不同。


1. 如果有必要，請重新部署為 HYPER-V 主機和必要的儲存體。

2. 如有必要，請從備份還原網路控制卡 Vm、 RAS 閘道 Vm 和 Mux Vm。 

3. 停止 NC 主機代理程式和所有的 HYPER-V 主機上的 SLB 主機代理程式：

    ```
    stop-service slbhostagent

    stop-service nchostagent
    ```

4. 停止 RAS 閘道 Vm。

5. 停止 SLB Mux Vm。

6. 還原網路控制站與`new-networkcontrollerrestore`cmdlet。

7. 請檢查還原**ProvisioningState**知道當還原已順利完成。

8. 如果使用 SCVMM，請還原 SCVMM 資料庫使用已在此同時做為網路控制卡備份建立的備份。

9. 如果您想要從備份還原工作負載的 Vm，請立即。

10. 檢查您的系統偵錯 networkcontrollerconfigurationstate 指令程式的健全狀況。

```Powershell
$cred = Get-Credential
Debug-NetworkControllerConfigurationState -NetworkController "https://NC.contoso.com" -Credential $cred

Fetching ResourceType:     accessControlLists
Fetching ResourceType:     servers
Fetching ResourceType:     virtualNetworks
Fetching ResourceType:     networkInterfaces
Fetching ResourceType:     virtualGateways
Fetching ResourceType:     loadbalancerMuxes
Fetching ResourceType:     Gateways
```

### <a name="example-restoring-a-network-controller-database"></a>範例：還原網路控制卡資料庫
 
```Powershell
$URI = "https://NC.contoso.com"
$Credential = Get-Credential

$ShareUserResourceId = "BackupUser"
$ShareCredential = Get-NetworkControllerCredential -ConnectionURI $URI -Credential $Credential | Where {$_.ResourceId -eq $ShareUserResourceId }

$RestoreProperties = New-Object Microsoft.Windows.NetworkController.NetworkControllerRestoreProperties
$RestoreProperties.RestorePath = "\\fileshare\backups\NetworkController\2017-04-25T16_53_13"
$RestoreProperties.Credential = $ShareCredential

$RestoreTime = (Get-Date).ToString("s").Replace(":", "_")
New-NetworkControllerRestore -ConnectionURI $URI -Credential $Credential -Properties $RestoreProperties -ResourceId $RestoreTime -Force
```

### <a name="example-checking-the-status-of-a-network-controller-database-restore"></a>範例：檢查網路控制卡資料庫還原狀態

```PowerShell
PS C:\ > get-networkcontrollerrestore -connectionuri $uri -credential $cred -ResourceId $restoreTime | convertto-json -depth 10
{
    "Tags":  null,
    "ResourceRef":  "/networkControllerRestore/2017-04-26T15_04_44",
    "InstanceId":  "22edecc8-a613-48ce-a74f-0418789f04f6",
    "Etag":  "W/\"f14f6b84-80a7-4b73-93b5-59a9c4b5d98e\"",
    "ResourceMetadata":  null,
    "ResourceId":  "2017-04-26T15_04_44",
    "Properties":  {
                    "RestorePath":  "\\\\sa18fs\\sa18n22\\NetworkController\\2017-04-25T16_53_13",
                    "ErrorMessage":  null,
                    "FailedResourcesList":  null,
                    "SuccessfulResourcesList":  null,
                    "ProvisioningState":  "Succeeded",
                    "Credential":  null
                }
}
```


如需設定狀態訊息，可能會出現資訊，請參閱[疑難排解 Windows Server 2016 軟體定義網路堆疊](https://docs.microsoft.com/windows-server/networking/sdn/troubleshoot/troubleshoot-windows-server-software-defined-networking-stack)。