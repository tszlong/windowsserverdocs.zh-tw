---
title: 確認可以證明的受防護的主機
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 7485796b-b840-4678-9b33-89e9710fbbc7
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 02/05/2019
ms.openlocfilehash: 87878eba785c0e1cc50454a74b2af4a159e88e12
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443658"
---
# <a name="confirm-guarded-hosts-can-attest"></a>確認可以證明的受防護的主機 

>適用於：Windows Server 2019，Windows Server （半年通道），Windows Server 2016


網狀架構系統管理員必須確認 HYPER-V 主機可執行做為受防護主機。 完成在至少一部受防護主機上的下列步驟：

1.  如果您有尚未安裝 HYPER-V 角色和主機守護者 HYPER-V 支援功能，請使用下列命令安裝它們：

        Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart

2.  請確定 HYPER-V 主機可以解析 HGS DNS 名稱，並具備網路連線到連接埠 80 （或如果您設定 HTTPS 443） HGS 伺服器。

2.  設定主機的金鑰保護與證明 Url:

    - **透過 Windows PowerShell**:您可以藉由在提升權限的 Windows PowerShell 主控台執行下列命令來設定金鑰保護與證明 Url。 針對&lt;FQDN&gt;，使用完整格式網域名稱 (FQDN) HGS 叢集的 (例如 hgs.bastion.local，或要求執行的 HGS 系統管理員**Get HgsServer** HGS 伺服器，以擷取 cmdletUrl)。

        `Set-HgsClientConfiguration -AttestationServerUrl 'http://<FQDN>/Attestation' -KeyProtectionServerUrl 'http://<FQDN>/KeyProtection'`

        若要設定後援 HGS 伺服器，重複此命令並指定後援金鑰保護與證明服務 Url。 如需詳細資訊，請參閱 <<c0> [ 後援設定](guarded-fabric-manage-branch-office.md#fallback-configuration)。 

    - **透過 VMM**:如果您使用 System Center 2016-Virtual Machine Manager (VMM) 中，您可以在 VMM 中設定證明與金鑰保護 Url。 如需詳細資訊，請參閱 <<c0> [ 設定全域 HGS 設定](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-hosts#configure-global-hgs-settings)中**佈建受防護的主機在 VMM 中的**。
    
    >**附註**
    > - 如果 HGS 系統管理員[HGS 伺服器上啟用 HTTPS](guarded-fabric-configure-hgs-https.md)，開始使用的 Url `https://`。
    > - 如果 HGS 系統管理員 HGS 伺服器上啟用 HTTPS，並使用自我簽署的憑證，您必須將憑證匯入每個主機上的受信任的根憑證授權單位存放區。 若要這樣做，請在每部主機上執行下列命令：<br>
        `Import-Certificate -FilePath "C:\temp\HttpsCertificate.cer" -CertStoreLocation Cert:\LocalMachine\Root`
    
3.  若要起始證明嘗試在主機上的，並檢視證明 」 狀態，請執行下列命令：

        Get-HgsClientConfiguration

    命令的輸出會指出是否在主機通過證明，而且現在受防護。 如果`IsHostGuarded`不會傳回 **，則為 True**，您可以執行 HGS 診斷工具[Get HgsTrace](https://technet.microsoft.com/library/mt718831.aspx)，以調查。 在主機上執行診斷，請在提升權限的 Windows PowerShell 提示字元中輸入下列命令：

        Get-HgsTrace -RunDiagnostics -Detailed

    > [!IMPORTANT]
    > 如果您使用 Windows Server 2019 或 Windows 10 版本 1809年，並使用程式碼完整性原則，`Get-HgsTrace`傳回的失敗**程式碼完整性原則 Active**診斷。
    > 僅失敗診斷時，您可以放心忽略此結果。

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [部署受防護的 VM](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)

## <a name="see-also"></a>另請參閱

- [部署主機守護者服務 (HGS)](guarded-fabric-deploying-hgs-overview.md)
- [部署受防護的 VM](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)

