---
title: NIC 小組 MAC 位址使用和管理
description: 當您使用「交換器獨立模式」設定 NIC 小組，並使用「位址雜湊」或「動態」負載散發時，小組會在輸出流量上使用主要 NIC 小組成員的媒體存取控制（MAC）位址。 主要 NIC 小組成員是由一組初始小組成員的作業系統選取的網路介面卡。
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 26d105e0-afc3-44b5-bb5e-0c884a4c5d62
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d8e7130d5774c19cc3d51045786bfef319cf7d16
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316438"
---
# <a name="nic-teaming-mac-address-use-and-management"></a>NIC 小組 MAC 位址使用和管理

>適用於︰Windows Server 2016

當您使用「交換器獨立模式」設定 NIC 小組，並使用「位址雜湊」或「動態」負載散發時，小組會在輸出流量上使用主要 NIC 小組成員的媒體存取控制（MAC）位址。 主要 NIC 小組成員是由一組初始小組成員的作業系統選取的網路介面卡。  這是您在建立小組之後，或在重新開機主機電腦之後，要系結的第一個小組成員。 由於主要小組成員可能會在每次開機時以不具決定性的方式變更，因此，NIC 停用/啟用動作或其他重新設定活動，主要小組成員可能會變更，而小組的 MAC 位址可能會有所不同。  
  
在大多數情況下，這不會造成問題，但在某些情況下可能會發生問題。  
  
如果主要小組成員已從小組移除，然後放入作業中，可能會發生 MAC 位址衝突。 若要解決此衝突，請停用再啟用小組介面。 停用再啟用小組介面的程式會導致介面從其餘小組成員中選取新的 MAC 位址，藉此排除 MAC 位址衝突。  
  
您可以將 NIC 小組的 MAC 位址設定為特定 MAC 位址，方法是在主要小組介面中設定它，就像您在設定任何實體 NIC 的 MAC 位址時一樣。  
  
## <a name="mac-address-use-on-transmitted-packets"></a>MAC 位址在傳輸的封包上使用  
當您以交換器獨立模式設定 NIC 小組，並使用位址雜湊或動態負載散發時，來自單一來源（例如單一 VM）的封包會同時分散到多個小組成員。 為了避免交換器混淆並防止 MAC flapping 警示，在主要小組成員以外的小組成員上傳送的框架上，會以不同的 MAC 位址取代來源 MAC 位址。 因此，每個小組成員都使用不同的 MAC 位址，而且除非發生失敗，否則會阻止 MAC 位址衝突。  
  
在主要 NIC 上偵測到失敗時，NIC 小組軟體會開始在您選擇做為暫時主要小組成員（也就是將會顯示為主要小組的切換身分的人員）上，使用主要小組成員的 MAC 位址。成員）。  這種變更僅適用于要在主要小組成員上傳送的流量，其主要小組成員的 MAC 位址會作為其來源 MAC 位址。 其他流量會繼續與失敗前所使用的任何來源 MAC 位址一起傳送。  
  
以下列出根據小組的設定方式來描述 NIC 小組 MAC 位址取代行為的清單：  
  
1.  **以位址雜湊散發的交換器獨立模式**  
  
    -   所有 ARP 和 NS 封包都會傳送給主要小組成員  
  
    -   在主要小組成員以外的 Nic 上傳送的所有流量，會隨著來源 MAC 位址的修改而傳送，以符合傳送的 NIC  
  
    -   主要小組成員傳送的所有流量都會與原始來源 MAC 位址（可能是小組的來源 MAC 位址）一起傳送  
  
2.  **使用 Hyper-v 通訊埠發佈的交換器獨立模式**  
  
    -   每個 vmSwitch 埠會相似化為至小組成員  
  
    -   每個封包都會在相似化為埠的小組成員上傳送  
  
    -   未完成來源 MAC 取代  
  
3.  **使用動態散發的交換器獨立模式**  
  
    -   每個 vmSwitch 埠會相似化為至小組成員  
  
    -   所有 ARP/NS 封包都會傳送至相似化為埠的小組成員  
  
    -   在相似化為小組成員的小組成員上傳送的封包未完成來源 MAC 位址取代  
  
    -   在相似化為小組成員以外的小組成員上傳送的封包將會完成來源 MAC 位址取代  
  
4.  **在切換相依模式中（所有發行）**  
  
    -   未執行任何來源 MAC 位址取代  
  
## <a name="related-topics"></a>相關主題
- [NIC](NIC-Teaming.md)小組：在本主題中，我們將概述 Windows Server 2016 中的網路介面卡（NIC）小組。 NIC 小組可讓您將一和32實體 Ethernet 網路介面卡分成一或多個以軟體為基礎的虛擬網路介面卡。 這些虛擬網路介面卡可在網路介面卡故障時，提供快速的效能與容錯。  

- [Nic 小組設定](nic-teaming-settings.md)：在本主題中，我們將概述 nic 團隊的屬性，例如團隊和負載平衡模式。 我們也會提供有關待命介面卡設定和主要小組介面屬性的詳細資料。 如果您的 NIC 小組中至少有兩張網路介面卡，則不需要指定待命介面卡來容錯。
  
- 在[主機電腦或 VM 上建立新的 Nic 小組](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md)：在本主題中，您會在主機電腦或執行 Windows Server 2016 的 hyper-v 虛擬機器（VM）中建立新的 nic 小組。

- 針對[nic 小組進行疑難排解](Troubleshooting-NIC-Teaming.md)：在本主題中，我們會討論針對 nic 小組進行疑難排解的方法，例如硬體、實體交換器的安全性，以及使用 Windows PowerShell 來停用或啟用網路介面卡。 
  


