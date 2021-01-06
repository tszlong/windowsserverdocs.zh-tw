---
description: 深入瞭解：分公司考慮
title: 分公司考慮
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 12/10/2020
ms.openlocfilehash: 4786d4ba72b2d5a5e6901fc0ffef67c1213c1602
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949184"
---
# <a name="branch-office-considerations"></a>分公司考量

> 適用于： Windows Server 2019、Windows Server (半年通道) 、

本文說明在分公司和其他遠端案例中執行受防護虛擬機器的最佳作法，其中 Hyper-v 主機可能會有一段時間，且對 HGS 的連線能力有限。

## <a name="fallback-configuration"></a>Fallback 設定

從 Windows Server 1709 版開始，您可以在 Hyper-v 主機上設定一組額外的主機守護者服務 Url，以在您的主要 HGS 沒有回應時使用。
這可讓您執行本機 HGS 叢集（作為主伺服器使用），以在本機伺服器關閉時，能夠切換回您公司資料中心的 HGS，以獲得更佳的效能。

若要使用 fallback 選項，您必須設定兩部 HGS 伺服器。 他們可以執行 Windows Server 2019 或 Windows Server 2016，也可以是相同或不同叢集的一部分。 如果它們是不同的叢集，您會想要建立作業實務，以確保兩個伺服器之間的證明原則會同步。 兩者都必須能夠正確地授權 Hyper-v 主機執行受防護的 Vm，並擁有啟動受防護 Vm 所需的金鑰材料。 您可以選擇在兩個叢集之間有一對共用的加密和簽署憑證，或使用不同的憑證，並設定 HGS 受防護的 VM，在防護資料檔案中授權 (加密/簽署憑證組) 。

然後將您的 Hyper-v 主機升級至 Windows Server 1709 版或 Windows Server 2019，然後執行下列命令：
```powershell
# Replace https://hgs.primary.com and https://hgs.backup.com with your own domain names and protocols
Set-HgsClientConfiguration -KeyProtectionServerUrl 'https://hgs.primary.com/KeyProtection' -AttestationServerUrl 'https://hgs.primary.com/Attestation' -FallbackKeyProtectionServerUrl 'https://hgs.backup.com/KeyProtection' -FallbackAttestationServerUrl 'https://hgs.backup.com/Attestation'
```

若要取消設定 fallback 伺服器，只要省略兩個回溯參數：
```powershell
Set-HgsClientConfiguration -KeyProtectionServerUrl 'https://hgs.primary.com/KeyProtection' -AttestationServerUrl 'https://hgs.primary.com/Attestation'
```

為了讓 Hyper-v 主機通過主要和回溯伺服器的證明，您必須確定您的證明信息與這兩個 HGS 叢集都是最新的。
此外，用來解密虛擬機器 TPM 的憑證必須可在這兩個 HGS 叢集中使用。
您可以使用不同的憑證來設定每個 HGS，並將 VM 設定為信任兩者，或將共用的憑證組新增至這兩個 HGS 叢集。

如需在分公司使用 fallback Url 設定 HGS 的詳細資訊，請參閱 [Windows Server 中受防護 vm 的支援分公司增強的分公司支援1709版](/archive/blogs/datacentersecurity/improved-branch-office-support-for-shielded-vms-in-windows-server-version-1709)。


## <a name="offline-mode"></a>離線模式

當您的 Hyper-v 主機的安全性設定未變更時，離線模式可讓您的受防護 VM 開啟 HGS。
離線模式的運作方式是在 Hyper-v 主機上快取特殊版本的 VM TPM 金鑰保護裝置。
金鑰保護裝置會使用虛擬化型安全性識別金鑰) ，加密為主機 (目前的安全性設定。
如果您的主機無法與 HGS 通訊，而且其安全性設定未變更，它將能夠使用快取的金鑰保護裝置來啟動受防護的 VM。
當系統上的安全性設定變更（例如，套用新的程式碼完整性原則或停用安全開機）時，快取的金鑰保護裝置將會失效，且主機必須先證明 HGS，才能讓任何受防護的 Vm 再次離線啟動。

離線模式需要主機守護者服務叢集和 Hyper-v 主機的 Windows Server Insider Preview 組建17609或更新版本。
它是由 HGS 上的原則所控制，預設為停用。
若要啟用離線模式的支援，請在 HGS 節點上執行下列命令：

```powershell
Set-HgsKeyProtectionConfiguration -AllowKeyMaterialCaching:$true
```

由於可快取的金鑰保護裝置對每個受防護的 VM 而言是唯一的，因此您必須完全關閉 (不會重新開機) 並啟動受防護的 Vm，以在 HGS 上啟用此設定之後取得可快取的金鑰保護裝置。
如果您受防護的 VM 會遷移到執行舊版 Windows Server 的 Hyper-v 主機，或從舊版 HGS 取得新的金鑰保護裝置，它將無法在離線模式下啟動，但可以在可以存取 HGS 時繼續以線上模式執行。
