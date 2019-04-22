---
title: NIC 小組 MAC 位址使用和管理
description: 當您設定 NIC 小組與交換器獨立模式和位址雜湊或動態配置資源負載分配時，小組所使用的媒體存取控制 (MAC) 位址的輸出流量的主要 NIC 小組成員。 主要的 NIC 小組成員會將作業系統從一組初始的小組成員所選取的網路介面卡。
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 26d105e0-afc3-44b5-bb5e-0c884a4c5d62
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 074a55d2f0a5423af5892ed73dc8a2b8dbda9d5b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824249"
---
# <a name="nic-teaming-mac-address-use-and-management"></a>NIC 小組 MAC 位址使用和管理

>適用於：Windows Server 2016

當您設定 NIC 小組與交換器獨立模式和位址雜湊或動態配置資源負載分配時，小組所使用的媒體存取控制 (MAC) 位址的輸出流量的主要 NIC 小組成員。 主要的 NIC 小組成員會將作業系統從一組初始的小組成員所選取的網路介面卡。  它是您在建立之後，或在主機電腦重新啟動之後，繫結至小組的第一個小組成員。 因為主要小組成員可能會變更以不具決定性的方式，在每個開機，NIC 停用/啟用動作或其他重新設定活動，而主要小組成員可能變更，以及小組的 MAC 位址可能會有所不同。  
  
在大部分情況下這並不會造成問題，但有少數的情況下可能會引發問題。  
  
如果主要小組成員是從小組中移除，並將其放到作業有可能是 MAC 位址衝突。 若要解決此衝突，請停用，然後再啟用 小組的介面。 停用，然後再啟用小組介面的程序會導致從剩餘的小組成員，因此消除了 MAC 位址衝突中選取新的 MAC 位址的介面。  
  
您可以設定 MAC 位址的 NIC 小組與特定 MAC 位址設定的主要小組介面中，就像設定任何實體 NIC 的 MAC 位址時，您可以執行  
  
## <a name="mac-address-use-on-transmitted-packets"></a>使用上傳送封包的 MAC 位址  
當您在 交換器獨立模式，以及位址雜湊或動態配置資源負載分配設定 NIC 小組時，從單一來源 （例如單一 VM) 的封包會同時會分散到多個小組成員。 防止弄混了參數，並避免出現 flapping 的現象警報的 MAC，來源 MAC 位址會取代主要小組成員以外的小組成員上傳輸的框架上不同的 MAC 位址。 因為這個緣故，每位小組成員使用不同的 MAC 位址，並後，才發生失敗，系統會防止 MAC 位址衝突。  
  
主要 NIC 上偵測到失敗時，NIC 小組的軟體開始上選擇要做為暫時的主要小組成員 （也就是其中一個，現在會顯示切換為主要小組我的小組成員使用主要的小組成員的 MAC 位址mber)。  這項變更僅適用於即將要傳送的主要小組成員的 MAC 位址主要小組成員做為其來源 MAC 位址的流量。 其他流量會繼續與任何來源 MAC 位址，它會使用故障前一併傳送。  
  
以下是說明在 NIC 小組 MAC 位址取代行為，根據小組的設定方式的清單：  
  
1.  **交換器獨立模式位址雜湊散發**  
  
    -   主要小組成員傳送所有的 ARP 和 NS 封包  
  
    -   主要小組成員以外傳送 Nic 上的所有流量會都傳送與修改成符合的 NIC 都傳送的來源 MAC 位址  
  
    -   主要小組成員傳送的所有流量會都傳送與原始的來源 MAC 位址 （這可能是小組的來源 MAC 位址）  
  
2.  **在 HYPER-V 通訊埠散發的交換器獨立模式**  
  
    -   每個 vmSwitch 連接埠會與小組成員  
  
    -   每個封包會傳送連接埠相似化的小組成員  
  
    -   在不完成任何來源 MAC 取代  
  
3.  **在 交換器獨立模式使用動態通訊群組**  
  
    -   每個 vmSwitch 連接埠會與小組成員  
  
    -   ARP/NS 的所有封包傳送連接埠相似化的小組成員上  
  
    -   上是相似的小組成員的小組成員傳送的封包會有任何來源 MAC 位址取代完成  
  
    -   必須完成的來源 MAC 位址取代以外的親和的小組成員的小組成員傳送的封包。  
  
4.  **交換器相依模式 （所有發行版本）**  
  
    -   來源 MAC 位址取代不會執行  
  
## <a name="related-topics"></a>相關主題
- [NIC 小組](NIC-Teaming.md):在本主題中，我們提供您的網路介面卡 (NIC) 小組概觀 Windows Server 2016 中。 NIC 小組可讓您將介於 1 到 32 到一或多個以軟體為基礎的虛擬網路介面卡的實體 Ethernet 網路介面卡。 這些虛擬網路介面卡可在網路介面卡故障時，提供快速的效能與容錯。  

- [NIC 小組設定](nic-teaming-settings.md):本主題中，我們會提供您的 NIC 小組 」 屬性，例如小組的概觀，以及負載平衡模式。 我們也提供您關於待命配接器設定和主要小組介面屬性的詳細資料。 如果您有至少兩個網路介面卡在 NIC 小組時，您不需要指定容錯移轉的待命介面卡。
  
- [在主機電腦或 VM 上建立新的 NIC 小組](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md):本主題中，您會建立新的 NIC 小組，在主機電腦上或在執行 Windows Server 2016 的 HYPER-V 虛擬機器 (VM)。

- [疑難排解 NIC 小組](Troubleshooting-NIC-Teaming.md):本主題中，我們會討論如何疑難排解 「 NIC 小組，例如硬體、 實體交換器 securities 和停用或啟用使用 Windows PowerShell 的網路介面卡。 
  


