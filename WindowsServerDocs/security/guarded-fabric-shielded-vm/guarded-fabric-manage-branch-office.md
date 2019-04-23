---
title: 分公司辦公室的考量
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.openlocfilehash: d93c37227af1eb62368fbcd4ec5d6a48374b45ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877059"
---
# <a name="branch-office-considerations"></a>分公司考量

> 適用於：Windows Server 2019，Windows Server （半年通道） 

本文說明執行受防護的虛擬機器，在分公司和其他遠端的情況下，HYPER-V 主機可能會有一段時間發生 HGS 的連線能力有限的最佳做法。

## <a name="fallback-configuration"></a>後援設定

從 Windows Server 1709 版開始，您可以設定一組額外的主機守護者服務 Url 使用的 HYPER-V 主機時您主要 HGS 沒有回應。
這可讓您執行本機 HGS 叢集用於做為主要伺服器更佳的效能能夠改為使用您公司的資料中心的 HGS 如果本機伺服器已關機。

若要使用的後援選項，您必須設定兩部 HGS 伺服器。 他們可以執行 Windows Server 2019 或 Windows Server 2016，然後是相同或不同叢集的一部分。 如果它們是不同的叢集，您會想要建立操作的作法，以確保證明原則會在兩部伺服器之間的同步處理。 它們都必須要能夠正確授權執行受防護的 Vm，並已啟動受防護的 Vm 所需的金鑰材料的 HYPER-V 主機。 您可以選擇有一組共用的加密和簽章憑證的兩個叢集之間，或使用不同的憑證，以及設定 HGS 受防護的 VM 來授權虛擬機器防護資料在這兩個 guardians （加密/簽署的憑證組）檔案。

然後升級至 Windows Server 1709 版或 Windows Server 2019 您 HYPER-V 主機，並執行下列命令：
```powershell
# Replace https://hgs.primary.com and https://hgs.backup.com with your own domain names and protocols
Set-HgsClientConfiguration -KeyProtectionServerUrl 'https://hgs.primary.com/KeyProtection' -AttestationServerUrl 'https://hgs.primary.com/Attestation' -FallbackKeyProtectionServerUrl 'https://hgs.backup.com/KeyProtection' -FallbackAttestationServerUrl 'https://hgs.backup.com/Attestation'
```

若要取消設定後援伺服器，只要省略這兩個後援的參數：
```powershell
Set-HgsClientConfiguration -KeyProtectionServerUrl 'https://hgs.primary.com/KeyProtection' -AttestationServerUrl 'https://hgs.primary.com/Attestation'
```

為了讓 HYPER-V 主機通過證明的主要和後援伺服器，您必須確保您證明的資訊是最新的這兩個 HGS 叢集。
此外，用來將虛擬機器的 TPM 解密的憑證必須可在這兩個 HGS 叢集。
您可以使用不同的憑證設定每一個 HGS 和設定 VM 來信任兩者，或將一組共用的憑證新增至這兩個 HGS 叢集。

如需如何設定 HGS 後援 url 的分公司中的詳細資訊，請參閱部落格文章[改善分公司支援受防護的 vm，在 Windows Server 1709 版](https://blogs.technet.microsoft.com/datacentersecurity/2017/11/15/improved-branch-office-support-for-shielded-vms-in-windows-server-version-1709/)。


## <a name="offline-mode"></a>離線模式

離線模式可讓您受防護的 VM 開啟時無法達到 HGS，只要您的 HYPER-V 主機的安全性組態尚未變更。
離線模式的運作方式是快取的特殊版本的 HYPER-V 主機上 VM TPM 金鑰保護裝置。
金鑰保護裝置加密 （使用虛擬化型安全性識別索引鍵） 的主控件目前的安全性組態。
如果您的主機就無法與 HGS 通訊，且未變更其安全性設定，它會使用快取的金鑰保護裝置啟動受防護的 VM。
當系統上，變更安全性設定時，例如新的程式碼完整性原則，套用或停用安全開機時，將會失效的快取的金鑰保護裝置，主應用程式會有任何之前的 HGS 證明受防護的 Vm 可以離線再次啟動。

離線模式需要 17609 或更新版本的 Windows Server Insider Preview 組建主機守護者服務的叢集和 HYPER-V 主機。
它是由 HGS，預設會停用的原則進行控制。
若要啟用支援離線模式，請在 HGS 節點上執行下列命令：

```powershell
Set-HgsKeyProtectionConfiguration -AllowKeyMaterialCaching:$true
```

因為快取的金鑰保護裝置是唯一的每個受防護的 vm，您必須完全關閉 （而不是重新啟動），以及啟動您之後在 HGS 上啟用此設定時，取得快取的金鑰保護裝置的受防護的 Vm。
如果受防護的 VM 移轉到執行較舊版本的 Windows Server 的 HYPER-V 主機，或從較舊版本的 HGS 取得新的金鑰保護裝置，不能在離線模式中，啟動本身，但可以繼續存取 HGS 時 avail 在線上模式中執行可以。
