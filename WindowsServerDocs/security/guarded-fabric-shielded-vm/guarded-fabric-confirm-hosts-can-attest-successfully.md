---
title: 確認受防護主機可以證明
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 7485796b-b840-4678-9b33-89e9710fbbc7
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 09/25/2019
ms.openlocfilehash: 2bab2b653127ae13d27dea76225ada91b3ee8ecc
ms.sourcegitcommit: de71970be7d81b95610a0977c12d456c3917c331
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2019
ms.locfileid: "71940694"
---
# <a name="confirm-guarded-hosts-can-attest"></a>確認受防護主機可以證明

>適用於：Windows Server 2019、Windows Server （半年通道）、Windows Server 2016

網狀架構系統管理員必須確認 Hyper-v 主機可以做為受防護主機來執行。 在至少一部受防護主機上，完成下列步驟：

1. 如果您尚未安裝 Hyper-v 角色和主機守護者 Hyper-v 支援功能，請使用下列命令來安裝：

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ```

2. 請確定 Hyper-v 主機可以解析 HGS DNS 名稱，並具有網路連線能力，可連線到 HGS 伺服器上的埠80（或443，如果您設定 HTTPS）。

3. 設定主機的金鑰保護和證明 Url：

    - **透過 Windows PowerShell**：您可以在提升許可權的 Windows PowerShell 主控台中執行下列命令，以設定金鑰保護和證明 Url。 針對 &lt;FQDN @ no__t-1，請使用 HGS 叢集的完整功能變數名稱（FQDN）（例如，hgs），或要求 HGS 系統管理員在 HGS 伺服器上執行**HgsServer 指令程式**，以取得 url）。

        ```PowerShell
        Set-HgsClientConfiguration -AttestationServerUrl 'http://<FQDN>/Attestation' -KeyProtectionServerUrl 'http://<FQDN>/KeyProtection'
         ```

        若要設定 fallback HGS 伺服器，請重複此命令，並指定金鑰保護和證明服務的 fallback Url。 如需詳細資訊，請參閱[Fallback configuration](guarded-fabric-manage-branch-office.md#fallback-configuration)。

    - **透過 VMM**：如果您使用的是 System Center 2016-Virtual Machine Manager （VMM），您可以在 VMM 中設定證明和金鑰保護 Url。 如需詳細資訊，請參閱在**VMM 中**布建受防護主機中的[設定全域 HGS 設定](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-hosts#configure-global-hgs-settings)。

    >**注意事項**
    > - 如果 hgs 系統管理員在[hgs 伺服器上啟用 HTTPS](guarded-fabric-configure-hgs-https.md)，請使用 `https://` 開始 url。
    > - 如果 HGS 系統管理員在 HGS 伺服器上啟用 HTTPS，並使用自我簽署憑證，您必須將憑證匯入每部主機上的「受信任的根憑證授權」存放區中。 若要這麼做，請在每部主機上執行下列命令：
       ```PowerShell
       Import-Certificate -FilePath "C:\temp\HttpsCertificate.cer" -CertStoreLocation Cert:\LocalMachine\Root
       ```
    > - 如果您已設定 HGS 用戶端使用 HTTPS，並已停用 TLS 1.0，請參閱我們的[新式 TLS 指引](guarded-fabric-troubleshoot-hosts.md#modern-tls)

4. 若要在主機上起始證明嘗試，並查看證明狀態，請執行下列命令：

    ```powershell
    Get-HgsClientConfiguration
    ```

    命令的輸出會指出主機是否通過證明，而且現在已受到保護。 如果 `IsHostGuarded` 未傳回**True**，您可以執行 HGS 診斷工具[HgsTrace](https://technet.microsoft.com/library/mt718831.aspx)來進行調查。 若要執行診斷，請在主機上提高許可權的 Windows PowerShell 提示字元中輸入下列命令：

    ```powershell
    Get-HgsTrace -RunDiagnostics -Detailed
    ```

    > [!IMPORTANT]
    > 如果您使用的是 Windows Server 2019 或 Windows 10 版本1809，且使用程式碼完整性原則，`Get-HgsTrace` 會傳回程序**代碼完整性原則**作用中診斷的失敗。
    > 當這是唯一失敗的診斷時，您可以放心地忽略此結果。

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [部署受防護的 VM](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)

## <a name="see-also"></a>另請參閱

- [部署主機守護者服務（HGS）](guarded-fabric-deploying-hgs-overview.md)
- [部署受防護的 VM](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
