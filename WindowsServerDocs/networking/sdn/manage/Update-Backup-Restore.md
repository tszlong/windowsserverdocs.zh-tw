---
title: "更新、備份與還原軟體定義的網路基礎結構"
description: "本主題是輔助的軟體定義網路上如何在 Windows Server 2016 的更新、 備份及還原 SDN 基礎結構的一部分。"
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: e9a8f2fd-48fe-4a90-9250-f6b32488b7a4
ms.author: grcusanz
author: grcusanz
ms.openlocfilehash: bb7194ec865db980962853b87d68a84a5446269e
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="update-backup-and-restore-software-defined-networking-infrastructure"></a>更新、備份與還原軟體定義的網路基礎結構

>適用於：Windows Server（以每年次管道）、Windows Server 2016

本主題包含下列各節。

- [更新 SDN 基礎結構](#bkmk_Updating)
- [備份 SDN 基礎結構](#bkmk_backup)
- [從備份還原 SDN 基礎結構](#bkmk_restore)

## <a name="bkmk_Updating"></a>更新 SDN 基礎結構

更新是所有的作業系統系統元件的軟體定義網路 (SDN) 上安裝 Windows 更新的處理程序。  這包括 SDN HYPER-V 主機的網路控制器 Vm、 軟體負載平衡器 Mux Vm 和 RAS 閘道 Vm 的支援。  很重要的所有的這些元件有完全相同設定安裝的更新。  如果使用 System Center 一樣 Manager 也建議您，您也更新的最新更新彙總套件，以及。

每個元件更新安裝 windows 更新使用的任何標準方法執行，步驟如下所述的但是請務必依照以確保最低下時間工作負載，並確保 Network Controller 資料庫的完整性。

### <a name="step-1-update-the-management-consoles"></a>步驟 1： 更新管理主機
在每一部電腦，您可以使用的網路控制器 Powershell 模組安裝必要更新。  這任何位置點一下包含您已經安裝本身 RSAT-NetworkController 角色。  這會包括網路控制器 Vm 本身不為他們將會更新中執行 「 步驟 2。

### <a name="step-2-update-the-network-controllers"></a>步驟 2： 更新網路控制器
每個網路控制器 VM 必須更新，並會完全回 online 中的下一個之前 Network Controller 叢集後，這是在更新循環最重要的步驟。

開始一個網路控制器 VM 並安裝所有所需的更新。  如有需要，請重新開機 VM。

之前的下一個網路控制器 VM 使用取得-networkcontrollernode 檢查更新] 節點狀態並重新開機。  等待在重新開機循環後，再回來一次 Network Controller 節點。  VM 重新開機之後，仍可能需要幾分鐘來回復到上狀態。

#### <a name="example-using-get-networkcontrollernode-to-check-the-status-of-network-controller-nodes"></a>範例： 若要查看的網路控制器節點狀態使用取得-networkcontrollernode

此範例中顯示執行取得-networkcontrollernode 的其中一個網路控制器 Vm 中的輸出。  它會顯示舒適地在兩個節點時，NCNode1.contoso.com 已關閉。  您必須等待幾分鐘的時間之前狀態該節點變更為向上之前的更新的任何其他節點。

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

僅限所有網路控制器節點上狀態之後可以您重複這些步驟針對每個其他的 Network Controller 節點。  繼續其中每個節點更新一次。

所有節點 Network Controller 的更新之後，Network Controller 將更新 microservices Network Controller 叢集中執行中一小時。  您可以立即更新使用的更新-networkcontroller cmdlet 觸發程序。

#### <a name="example-using-update-networkcontroller-to-force-network-controller-to-update"></a>範例： 使用更新-networkcontroller 強制 Network Controller 更新

並不會顯示剩餘安裝更新時，此命令顯示更新-networkcontroller 的結果。

```Powershell
PS C:\> update-networkcontroller
NetworkControllerClusterVersion NetworkControllerVersion
------------------------------- ------------------------
10.1.1                          10.1.15
```

### <a name="step-3-update-slb-muxes"></a>步驟 3： 更新 SLB Muxes

在每個 SLB Mux VM 一個安裝更新，來確保連續負載平衡器基礎結構一次。

### <a name="step-4-update-hyper-v-hosts-and-ras-gateways"></a>步驟 4： 更新 HYPER-V 主機和 RAS 閘道

不會中斷連接承租人移轉 RAS 閘道 Vm 不能動態，因為小心以該承租人連接將會移轉到新的 RAS 閘道更新期間減少的次數。  協調更新主機和 RAS 閘道每個承租人將只容錯移轉至少一次。  

為每個主機，包含 RAS 閘道待命模式中的主機開始，請依照下列步驟：

1.  逃離 Vm 即時移轉的支援的主機。  RAS 閘道 Vm 應維持在主機上。
2.  在每個閘道 VM，該主機上安裝的更新。
3.  如果更新需要重新開機，然後重新開機 VM VM 閘道。  
4.  包含閘道 VM 只是更新主機上安裝的更新。
5.  如果所需的更新，請重新開機主機。
6.  重複包含待命閘道每個額外的主機。  如果仍未待命閘道，然後依照其餘的所有主機相同的步驟。

## <a name="bkmk_backup"></a>備份 SDN 基礎結構

定期備份 Network Controller 資料庫非常重要確保業務持續性發生嚴重損壞或資料遺失。  備份網路控制器 Vm 不足，因為不確定該仲裁維護跨多個網路控制器節點。
需求：
 * 在 SMB 共用和認證的權限讀取/寫入共用和檔案系統。
 * 您也可以使用 Network Controller 的安裝方向是使用 GMSA，以及群組管理服務 Account (GMSA)。

請依照下列步驟來執行備份：

1. 備份網路控制器 Vm 使用 VM 備份方法您選擇，或使用 HYPER-V 匯出每個網路控制器 VM 的複本。  這樣可確保，包括還原 Vm 的基礎結構完整重建執行時，如果解密資料庫必要的憑證有。
2. 如果您使用 System Center 一樣管理員 (SCVMM)，停止 SCVMM 服務，透過確保任何更新對這期間，無法建立的備份 Network Controller 和 SCVMM 一致 SCVMM SQL Server 備份。  請不要重新開始 SCVMM 服務 Network Controller 備份之前完成。
3. 備份 Network Controller 資料庫中使用新 networkcontrollerbackup。

 #### <a name="example-backing-up-the-network-controller-database"></a>範例： Network Controller 資料庫備份
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

4. 使用取得-networkcontrollerbackup 檢查完成和成功的備份。

 #### <a name="example-checking-the-status-of-a-network-controller-backup-operation"></a>範例： 檢查備份 Network Controller 的狀態

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

5.  如果使用 SCVMM 就可以開始 SCVMM 服務。

## <a name="bkmk_restore"></a>從備份還原 SDN 基礎結構

還原 」 是從 SDN 環境回到操作狀態的備份還原所有的必要元件程序。  步驟會根據元件正在還原量稍微而有所不同。

1. 如有需要，重新部署 HYPER-V 主機和必要的儲存空間。

2. 如有需要，請從備份還原網路控制器 Vm、 RAS 閘道 Vm 和 Mux Vm。 

3. 所有 HYPER-V 主機上停止 NC 主機代理程式和 SLB 主機代理程式

    ```
    stop-service slbhostagent

    stop-service nchostagent
    ```

4. 停止 RAS 閘道 Vm

5. 停止 SLB Mux Vm

6. 還原使用新 networkcontrollerrestore cmdlet 網路控制器。

 #### <a name="example-restoring-a-network-controller-database"></a>範例： 還原 Network Controller 資料庫
 
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

7. 請了解當還原已成功完成 ProvisioningState 還原。

 #### <a name="example-checking-the-status-of-a-network-controller-database-restore"></a>範例： 檢查 Network Controller 資料庫還原的狀態

    ```
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

8. 如果使用 SCVMM，還原 SCVMM 資料庫中使用與 Network Controller 備份同時所建立的備份。

9. 工作負載 Vm 正在從備份還原，如果您可以立即執行。

10. 用於偵錯-networkcontrollerconfigurationstate cmdlet 檢查您的系統的健康狀態。

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

資訊可能會出現的設定狀態訊息，請查看[進行疑難排解的 Windows Server 2016 軟體定義網路堆疊](https://docs.microsoft.com/windows-server/networking/sdn/troubleshoot/troubleshoot-windows-server-software-defined-networking-stack)。