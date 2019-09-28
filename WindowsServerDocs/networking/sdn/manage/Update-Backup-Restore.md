---
title: 升級、備份及還原 SDN 基礎結構
description: 在本主題中，您將瞭解如何更新、備份及還原 SDN 基礎結構。
manager: dougkim
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: e9a8f2fd-48fe-4a90-9250-f6b32488b7a4
ms.author: grcusanz
author: shortpatti
ms.date: 08/27/2018
ms.openlocfilehash: 7f385e094ca70027d1b036bf53af23c1fc4a1bd1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406054"
---
# <a name="upgrade-backup-and-restore-sdn-infrastructure"></a>升級、備份及還原 SDN 基礎結構

>適用於：Windows Server (半年度管道)、Windows Server 2016

在本主題中，您將瞭解如何更新、備份及還原 SDN 基礎結構。 

## <a name="upgrade-the-sdn-infrastructure"></a>升級 SDN 基礎結構
SDN 基礎結構可以從 Windows Server 2016 升級至 Windows Server 2019。 針對升級順序，遵循「更新 SDN 基礎結構」一節中所述的相同步驟順序。 升級之前，建議您先備份網路控制卡資料庫。

針對網路控制站機器，請在完成升級之後，使用 NetworkControllerNode Cmdlet 來檢查節點的狀態。 在升級其他節點之前，請先確定節點回到「上線」狀態。 當您升級所有網路控制站節點之後，網路控制站會在一小時內更新網路控制卡叢集中執行的微服務。 您可以使用 networkcontroller Cmdlet 來觸發立即更新。 

在軟體定義網路（SDN）系統的所有作業系統元件上安裝相同的 Windows 更新，其中包括：

- 已啟用 SDN 的 Hyper-v 主機
- 網路控制站 Vm
- 軟體 Load Balancer Mux Vm
- RAS 閘道 Vm 

>[!IMPORTANT]
>如果您使用 System Center Virtual Manager，您必須使用最新的更新彙總套件來更新它。

當您更新每個元件時，您可以使用任何安裝 Windows 更新的標準方法。 不過，若要確保工作負載的停機時間最短，以及網路控制站資料庫的完整性，請遵循下列步驟：

1. 更新管理主控台。<p>在您使用網路控制站 Powershell 模組的每部電腦上安裝更新。  包括您本身已安裝 RSAT-NetworkController 角色的任何位置。 排除網路控制站 Vm 本身;您會在下一個步驟中更新它們。

2. 在第一個網路控制站 VM 上，安裝所有更新並重新啟動。

3. 繼續進行下一個網路控制站 VM 之前，請`get-networkcontrollernode`使用 Cmdlet 來檢查您已更新並重新啟動之節點的狀態。

4. 在重新開機迴圈期間，等候網路控制卡節點關閉，然後再次返回。<p>重新開機 VM 之後，可能需要幾分鐘的時間才能回到 [ **_啟動_** ] 狀態。 如需輸出的範例，請參閱 

5. 在每個 SLB Mux VM 上一次安裝一個更新，以確保負載平衡器基礎結構的持續可用性。

6. 從包含處於**待命**模式之 RAS 閘道的主機開始，更新 hyper-v 主機和 RAS 閘道。<p>無法在不中斷租使用者連線的情況下，即時移轉 RAS 閘道 Vm。 在更新週期期間，您必須小心將租使用者連線容錯移轉到新的 RAS 閘道的次數降到最低。 藉由協調主機和 RAS 閘道的更新，每個租使用者最多隻會故障一次。

    a. 撤除能夠進行即時移轉的 Vm 主機。<p>RAS 閘道 Vm 應保留在主機上。

    b. 在此主機上的每個閘道 VM 上安裝更新。

    c. 如果更新需要閘道 VM 重新開機，請重新開機 VM。  

    d. 在包含剛更新之閘道 VM 的主機上安裝更新。

    e. 如果更新需要，請重新開機主機。

    f. 針對包含待命閘道的每個額外主機重複此動作。<p>如果沒有待命閘道，則請針對所有剩餘的主機遵循這些相同的步驟。


### <a name="example-use-the-get-networkcontrollernode-cmdlet"></a>範例：使用 networkcontrollernode Cmdlet 

在此範例中，您會在其中一個`get-networkcontrollernode`網路控制站 vm 中看到 Cmdlet 的輸出。  

您在範例輸出中看到的節點狀態為：

- NCNode1.contoso.com = Down
- NCNode2.contoso.com = Up
- NCNode3.contoso.com = Up

>[!IMPORTANT]
>您必須等候幾分鐘，直到節點的狀態變更為 [已 _**啟動**_ ]，然後再更新任何其他節點（一次一個）。

更新所有網路控制站節點之後，網路控制站會在一小時內更新網路控制卡叢集中執行的微服務。 

>[!TIP]
>您可以使用`update-networkcontroller` Cmdlet 來觸發立即更新。


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

### <a name="example-use-the-update-networkcontroller-cmdlet"></a>範例：使用 networkcontroller Cmdlet
在此範例中，您會看到`update-networkcontroller` Cmdlet 的輸出，以強制網路控制站更新。 

>[!IMPORTANT]
>當您沒有其他要安裝的更新時，請執行此 Cmdlet。


```Powershell
PS C:\> update-networkcontroller
NetworkControllerClusterVersion NetworkControllerVersion
------------------------------- ------------------------
10.1.1                          10.1.15
```

## <a name="backup-the-sdn-infrastructure"></a>備份 SDN 基礎結構

網路控制站資料庫的定期備份可確保在發生損毀或資料遺失時的業務持續性。  備份網路控制站 Vm 並不足夠，因為它不會確保會話會跨多個網路控制站節點繼續進行。

**滿足**
* SMB 共用和認證，具有共用和檔案系統的讀取/寫入權限。
* 如果也使用 GMSA 來安裝網路控制站，您可以選擇使用群組受管理的服務帳戶（GMSA）。

**步**

1. 使用您選擇的 VM 備份方法，或使用 Hyper-v 匯出每個網路控制站 VM 的複本。<p>備份網路控制站 VM 可確保存在解密資料庫所需的憑證。  

2. 如果使用 System Center Virtual Machine Manager （SCVMM），請停止 SCVMM 服務，並透過 SQL Server 將其備份。<p>此處的目標是要確保在這段期間不會對 SCVMM 進行任何更新，這可能會造成網路控制站備份和 SCVMM 之間的不一致。  

   >[!IMPORTANT]
   >在網路控制卡備份完成之前，請勿重新開機 SCVMM 服務。

3. 使用`new-networkcontrollerbackup` Cmdlet 備份網路控制站資料庫。

4. 使用`get-networkcontrollerbackup` Cmdlet 檢查備份的完成和成功。

5. 如果使用 SCVMM，請啟動 SCVMM 服務。



### <a name="example-backing-up-the-network-controller-database"></a>範例：備份網路控制站資料庫

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

### <a name="example-checking-the-status-of-a-network-controller-backup-operation"></a>範例：檢查網路控制卡備份操作的狀態

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

當您從備份還原所有必要的元件時，SDN 環境會回到操作狀態。  

>[!IMPORTANT]
>這些步驟會根據所還原的元件數目而有所不同。


1. 如有必要，請重新部署 Hyper-v 主機和所需的存放裝置。

2. 如有需要，請從備份還原網路控制站 Vm、RAS 閘道 Vm 和 Mux Vm。 

3. 停止所有 Hyper-v 主機上的 NC 主機代理程式和 SLB 主機代理程式：

    ```
    stop-service slbhostagent

    stop-service nchostagent
    ```

4. 停止 RAS 閘道 Vm。

5. 停止 SLB Mux Vm。

6. 使用`new-networkcontrollerrestore` Cmdlet 還原網路控制卡。

7. 檢查 [還原] **ProvisioningState** ，以瞭解還原順利完成的時間。

8. 如果使用 SCVMM，請使用與網路控制站備份同時建立的備份來還原 SCVMM 資料庫。

9. 如果您想要從備份還原工作負載 Vm，請立即這麼做。

10. 使用 networkcontrollerconfigurationstate Cmdlet 檢查系統的健康情況。

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

### <a name="example-checking-the-status-of-a-network-controller-database-restore"></a>範例：檢查網路控制站資料庫還原的狀態

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


如需可能出現之設定狀態訊息的相關資訊，請參閱針對[Windows Server 2016 軟體定義網路堆疊進行疑難排解](https://docs.microsoft.com/windows-server/networking/sdn/troubleshoot/troubleshoot-windows-server-software-defined-networking-stack)。