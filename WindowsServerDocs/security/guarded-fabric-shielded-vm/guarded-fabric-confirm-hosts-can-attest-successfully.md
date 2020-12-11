---
title: 確認受防護主機可以證明
description: 深入瞭解：確認受防護的主機可以證明
ms.topic: article
ms.assetid: 7485796b-b840-4678-9b33-89e9710fbbc7
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 09/25/2019
ms.openlocfilehash: 88741fcb0f10018cacb358db8a43eb6292f48811
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049826"
---
# <a name="confirm-guarded-hosts-can-attest"></a>確認受防護主機可以證明

>適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

網狀架構系統管理員必須確認 Hyper-v 主機可以執行為受防護主機。 在至少一個受防護主機上完成下列步驟：

1. 如果您尚未安裝 Hyper-v 角色與主機守護者 Hyper-v 支援功能，請使用下列命令進行安裝：

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ```

2. 如果您在 HGS 伺服器上設定 HTTPS) ，請確定 Hyper-v 主機可以解析 HGS DNS 名稱，且具有連線到埠 80 (或443的網路連接。

3. 設定主機的金鑰保護和證明 Url：

    - **透過 Windows PowerShell**：您可以在提高許可權的 Windows PowerShell 主控台中執行下列命令，以設定金鑰保護和證明 url。 針對 &lt; FQDN &gt; ，請使用 hgs 叢集 (FQDN) 的完整功能變數名稱 (例如，hgs，或要求 hgs 系統管理員在 hgs 伺服器上執行 **HgsServer 指令程式** ，以取得) 的 url。

        ```PowerShell
        Set-HgsClientConfiguration -AttestationServerUrl 'http://<FQDN>/Attestation' -KeyProtectionServerUrl 'http://<FQDN>/KeyProtection'
         ```

        若要設定 fallback HGS 伺服器，請重複此命令，並指定金鑰保護和證明服務的回溯 Url。 如需詳細資訊，請參閱 [Fallback](guarded-fabric-manage-branch-office.md#fallback-configuration)設定。

    - **透過 vmm**：如果您使用 SYSTEM CENTER VIRTUAL MACHINE MANAGER (vmm) ，您可以在 vmm 中設定證明和金鑰保護 url。 如需詳細資訊，請參閱在 **VMM 中** 布建受防護的主機中設定 [全域 HGS 設定](/system-center/vmm/guarded-deploy-host#configure-global-hgs-settings)。

    >**注意事項**
    > - 如果 HGS 系統管理員在 [hgs 伺服器上啟用 HTTPS](guarded-fabric-configure-hgs-https.md)，請使用來開始 url `https://` 。
    > - 如果 HGS 系統管理員在 HGS 伺服器上啟用 HTTPS，並使用自我簽署憑證，您必須將憑證匯入每部主機上的「受信任的根憑證授權單位」存放區。 若要這樣做，請在每部主機上執行下列命令：
       ```PowerShell
       Import-Certificate -FilePath "C:\temp\HttpsCertificate.cer" -CertStoreLocation Cert:\LocalMachine\Root
       ```
    > - 如果您已將 HGS 用戶端設定為使用 HTTPS，並已在整個系統中停用 TLS 1.0，請參閱我們的 [新式 tls 指引](guarded-fabric-troubleshoot-hosts.md#modern-tls)

4. 若要在主機上起始證明嘗試，並查看證明狀態，請執行下列命令：

    ```powershell
    Get-HgsClientConfiguration
    ```

    命令的輸出會指出主機是否通過證明，現在是否受到保護。 如果 `IsHostGuarded` 未傳回 **True**，您可以執行 HGS 診斷工具 [HgsTrace](https://technet.microsoft.com/library/mt718831.aspx)來進行調查。 若要執行診斷，請在主機上提高許可權的 Windows PowerShell 提示字元中輸入下列命令：

    ```powershell
    Get-HgsTrace -RunDiagnostics -Detailed
    ```

    > [!IMPORTANT]
    > 如果您使用的是 Windows Server 2019 或 Windows 10 版本1809或更新版本，且使用程式碼完整性原則，則會傳回 `Get-HgsTrace` 程式 **代碼完整性原則** 使用中診斷的失敗。
    > 當這是唯一失敗的診斷時，您可以放心地忽略此結果。

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [部署受防護的 VM](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)

## <a name="additional-references"></a>其他參考資料

- [部署主機守護者服務 (HGS)](guarded-fabric-deploying-hgs-overview.md) /(英文/)
- [部署受防護的 VM](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
