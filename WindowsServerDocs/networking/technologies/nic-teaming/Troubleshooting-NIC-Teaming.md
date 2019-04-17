---
title: 疑難排解 NIC 小組
description: 本主題提供疑難排解 NIC 小組中的 Windows Server 2016 的相關資訊。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fdee02ec-3a7e-473e-9784-2889dc1b6dbb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 634ec1a5eee0cd661ca89c2673e5b74abd458cd6
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="troubleshooting-nic-teaming"></a>疑難排解 NIC 小組

>適用於：Windows Server（以每年次管道）、Windows Server 2016

本主題提供資訊疑難排解 NIC 小組，並包含下列區段，描述問題 NIC 小組的可能原因。  
  
-   [硬體不符合規格](#bkmk_hardware)  
  
-   [實體切換安全性功能](#bkmk_switch)  
  
-   [停用，並讓使用 Windows PowerShell](#bkmk_ps)  
  
## <a name="bkmk_hardware"></a>硬體不符合規格  
當規格標準通訊協定實作硬體不符合時，可能會影響 NIC 小組效能。  
  
在正常運作，NIC 小組可能會傳送封包從相同的 IP 位址，但有多個不同的來源媒體存取控制 (MAC) 位址。 通訊協定標準，根據這些封包接收器必須解析的主機或 VM 的 IP 位址特定的 MAC 位址，而非回應的 MAC 地址，收到一封包。  戶端正確實作地址解析度通訊協定的 IPv4 位址解析度通訊協定 (ARP) 或 IPv6 的鄰居探索通訊協定 (NDP)，將會傳送具有正確的目的地的 MAC 位址（VM 或主機的擁有該 IP 位址的 MAC 位址）封包。  
  
不過，有些 embedded 硬體不正確實作的地址解析度通訊協定，並也可能會不明確 IP 位址解析使用 ARP 或 NDP 的 MAC 位址。  存放裝置區域網路（舊）控制器，是裝置的這種方式可以執行的範例。 非符合裝置中收到的封包複製來源所包含的 MAC 位址，並使用它做目的地的相對應的輸出封包 MAC 位址。  
  
這會傳送到錯誤目的地的 MAC 位址封包。 因此，封包所中斷 HYPER-V Virtual 開關切換至因為它們不符合所有已知的目的地。  
  
如果您有無法連接到舊控制器或其他 embedded 硬體，您應該需要封包擷取判斷是否硬體正確實作 ARP 或 NDP，並連絡您的硬體製造商以尋求支援。  
  
## <a name="bkmk_switch"></a>實體切換安全性功能  
根據設定，NIC 小組可能會傳送封包從相同的 IP 位址的數個不同的來源 MAC 地址。  這可以成為了安全性使動態 ARP 檢查或 IP 來源例如實體開關切換至上的功能，尤其是實體切換並不知道的連接埠是小組的成員。 這種情形您設定切換獨立模式中的 [NIC 小組。  您應該會檢查切換登判斷是否切換安全性功能會造成連接 NIC 小組的問題。  
  
## <a name="bkmk_ps"></a>停用，並讓網路介面卡，使用 Windows PowerShell  
失敗 NIC 小組的一個常見原因是在小組介面停用。 很多時候，介面會意外停用，執行下列命令 Windows PowerShell 順序時：  
  
```  
Disable-NetAdapter *  
Enable-NetAdapter *  
```  
  
這一系列命令不讓所有停用它 NetAdapters。  
  
這是因為停用所有的基礎實體成員 Nic 會導致介面移除，且不會再出現在 Get-NetAdapter NIC 小組。 因此，**讓-NetAdapter \ ***命令無法讓 NIC 團隊，因為的介面卡，會被移除。  
  
**讓-NetAdapter \ ***命令，但可以讓成員 Nic，然後（片刻）會重新建立小組介面。 在這個情況，小組介面仍會在」已停用「狀態因為它未重新讓。 之後，它會重新建立讓小組介面，可讓開始重新排列網路流量。  
  
## <a name="see-also"></a>也了  
[NIC 小組](NIC-Teaming.md)  
  


