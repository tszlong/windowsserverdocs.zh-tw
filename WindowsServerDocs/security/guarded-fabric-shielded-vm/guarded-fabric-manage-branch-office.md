---
title: 分公司考慮
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.openlocfilehash: 5a07553e6662fd79230d566ba2049c5e8997f4d6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403580"
---
# <a name="branch-office-considerations"></a>分公司考量

> 適用於：Windows Server 2019、Windows Server （半年通道）、 

本文說明在分公司和其他遠端案例中執行受防護虛擬機器的最佳作法，其中 Hyper-v 主機可能有一段時間與 HGS 的連線能力有限。

## <a name="fallback-configuration"></a>回退設定

從 Windows Server 1709 版開始，您可以在 Hyper-v 主機上設定一組額外的主機守護者服務 Url，以便在主要 HGS 沒有回應時使用。
這可讓您執行本機 HGS 叢集作為主伺服器，以便在本機伺服器關閉時，能夠切換回您公司資料中心的 HGS，以獲得更佳的效能。

若要使用 fallback 選項，您必須設定兩部 HGS 伺服器。 他們可以執行 Windows Server 2019 或 Windows Server 2016，而且可能屬於相同或不同的叢集。 如果它們是不同的叢集，您會想要建立作業實務，以確保兩個伺服器之間的證明原則是同步的。 它們都必須能夠正確授權 Hyper-v 主機執行受防護的 Vm，並擁有啟動受防護 Vm 所需的金鑰內容。 您可以選擇在兩個叢集之間擁有一對共用加密和簽署憑證，或使用不同的憑證，並設定 HGS 受防護的 VM，以授權防護資料中的保護者（加密/簽署憑證組）文字檔.

然後，將您的 Hyper-v 主機升級為 Windows Server 1709 版或 Windows Server 2019，然後執行下列命令：
```powershell
# Replace https://hgs.primary.com and https://hgs.backup.com with your own domain names and protocols
Set-HgsClientConfiguration -KeyProtectionServerUrl 'https://hgs.primary.com/KeyProtection' -AttestationServerUrl 'https://hgs.primary.com/Attestation' -FallbackKeyProtectionServerUrl 'https://hgs.backup.com/KeyProtection' -FallbackAttestationServerUrl 'https://hgs.backup.com/Attestation'
```

若要取消設定 fallback 伺服器，只需要省略兩個回溯參數：
```powershell
Set-HgsClientConfiguration -KeyProtectionServerUrl 'https://hgs.primary.com/KeyProtection' -AttestationServerUrl 'https://hgs.primary.com/Attestation'
```

為了讓 Hyper-v 主機通過具有主要和回溯伺服器的證明，您必須確定您的證明信息與這兩個 HGS 叢集都是最新的。
此外，用來解密虛擬機器 TPM 的憑證必須同時在兩個 HGS 叢集內提供。
您可以使用不同的憑證來設定每個 HGS，並將 VM 設定為信任這兩個，或將共用的一組憑證新增至這兩個 HGS 叢集。

如需有關使用回溯 Url 在分公司中設定 HGS 的詳細資訊，請參閱在[Windows Server 版本1709中，針對受防護的 Vm 改進分公司支援](https://blogs.technet.microsoft.com/datacentersecurity/2017/11/15/improved-branch-office-support-for-shielded-vms-in-windows-server-version-1709/)的文章。


## <a name="offline-mode"></a>離線模式

[離線] 模式可讓您的受防護 VM 在無法連線到 HGS 時開啟，只要您的 Hyper-v 主機的安全性設定未變更即可。
離線模式的運作方式是在 Hyper-v 主機上快取特定版本的 VM TPM 金鑰保護裝置。
金鑰保護裝置會加密為主機目前的安全性設定（使用虛擬化型安全性識別金鑰）。
如果您的主機無法與 HGS 通訊，而且其安全性設定尚未變更，它就可以使用快取的金鑰保護裝置來啟動受防護的 VM。
當系統上的安全性設定變更時（例如，套用新的程式碼完整性原則或停用安全開機），快取的金鑰保護裝置將會失效，而且主機必須先向 HGS 證明，才能再次啟動任何受防護的 Vm。

離線模式需要有 Windows Server Insider Preview 組建17609或更新版本，才能同時用於主機守護者服務叢集和 Hyper-v 主機。
它是由 HGS 上的原則控制，預設為停用。
若要啟用離線模式支援，請在 HGS 節點上執行下列命令：

```powershell
Set-HgsKeyProtectionConfiguration -AllowKeyMaterialCaching:$true
```

由於可快取的金鑰保護裝置對每個受防護的 VM 而言是唯一的，因此您必須完全關閉（而非重新開機），並啟動受防護的 Vm，以在 HGS 上啟用此設定之後，取得可快取的金鑰保護裝置。
如果您的受防護 VM 會遷移至執行舊版 Windows Server 的 Hyper-v 主機，或從舊版 HGS 取得新的金鑰保護裝置，它將無法在離線模式中啟動，但可在存取 HGS 時繼續在線上模式中執行能夠.
