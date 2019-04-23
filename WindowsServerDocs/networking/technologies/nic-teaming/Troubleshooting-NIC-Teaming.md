---
title: 對 NIC 小組進行疑難排解
description: 本主題提供疑難排解 Windows Server 2016 中的 NIC 小組的資訊。
manager: dougkim
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
ms.date: 09/13/2018
ms.openlocfilehash: d39dc6a4dcf5dca8186b0599fb479ed5ae684e0f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856249"
---
# <a name="troubleshooting-nic-teaming"></a>對 NIC 小組進行疑難排解

>適用於：Windows Server （半年通道），Windows Server 2016

本主題中，我們會討論如何疑難排解 「 NIC 小組，例如硬體與實體交換器交割，並。  當硬體實作的標準通訊協定不符合規格時，NIC 小組的效能可能會受到影響。 此外，根據組態，NIC 小組可能會傳送封包從相同的 IP 位址，以操控實體交換器上的安全性功能的多個 MAC 位址。

  
## <a name="hardware-that-doesnt-conform-to-specification"></a>不符合規格的硬體  
  
一般作業期間，NIC 小組可能會傳送封包從相同的 IP 位址，還具有多個 MAC 位址。 根據通訊協定標準，這些封包接收者必須解析成特定的 MAC 位址，而不是從接收的封包回應的 MAC 位址的主機或 VM 的 IP 位址。  正確實作的位址解析通訊協定 （ARP 和 ndp 底下） 的用戶端會傳送具有正確的目的地 MAC 位址的封包，也就是 VM 或擁有該 IP 位址的主機的 MAC 位址。 
  
某些內嵌的硬體不會正確地實作位址解析通訊協定，並也可能會不明確 IP 位址解析成使用 ARP 或 ndp 底下的 MAC 位址。  例如，存放區域網路 (SAN) 控制器可能會以這種方式執行。 不合格的裝置接收之封包從複製的來源 MAC 位址，並將它作為對應的傳出封包，導致傳送至錯誤的目的地 MAC 位址的封包中的目的地 MAC 位址。 因為這個緣故，封包會與 HYPER-V 虛擬交換器所卸除，因為它們不符合任何已知的目的地。  
  
如果您需要疑難排解連線到 SAN 的控制站或其他內嵌硬體，您應該採取封包擷取，以判斷 ARP 或 ndp 底下，，如果正確地實作您的硬體，並連絡您的硬體廠商以取得支援。  

  
## <a name="physical-switch-security-features"></a>實體交換器的安全性功能  
根據組態，NIC 小組可能會傳送封包從相同的 IP 位址，以操控實體交換器上的安全性功能的多個來源 MAC 位址。 比方說，動態 ARP 檢查或 IP 來源防護時，特別是當實體切換並不知道的連接埠是小組，其發生於當您在 交換器獨立模式中設定 NIC 小組的一部分。 檢查切換記錄檔來判斷是否交換器的安全性功能會造成連線問題。 
  
## <a name="disabling-and-enabling-network-adapters-by-using-windows-powershell"></a>停用，並透過使用 Windows PowerShell 中啟用網路介面卡  

NIC 小組失敗的常見原因是，小組介面已停用，在許多情況下，不小心執行一連串的命令時。  這一系列特定的命令不會啟用所有停用，因為停用所有基礎的實體成員的 Nic 移除 NIC 小組介面 NetAdapters。 

在此情況下，NIC 小組介面不會再顯示在 Get-netadapter，，因為這個緣故，**啟用 NetAdapter \*** 不會啟用 NIC 小組。 **啟用 NetAdapter \*** 命令，不過，啟用的成員 Nic，然後 （短時間） 之後會重新建立小組介面。 小組介面會保留在 「 停用 」 狀態，直到重新啟用，允許網路流量，以開始流動。 

下列 Windows PowerShell 的命令順序可能會不小心停用 team 介面：  
  
```PowerShell 
Disable-NetAdapter *  
Enable-NetAdapter *  
```  
  

  
## <a name="related-topics"></a>相關主題  
- [NIC 小組](NIC-Teaming.md):在本主題中，我們提供您的網路介面卡 (NIC) 小組概觀 Windows Server 2016 中。 NIC 小組可讓您將介於 1 到 32 到一或多個以軟體為基礎的虛擬網路介面卡的實體 Ethernet 網路介面卡。 這些虛擬網路介面卡可在網路介面卡故障時，提供快速的效能與容錯。   

- [NIC 小組 MAC 位址使用和管理](NIC-Teaming-MAC-Address-Use-and-Management.md):當您設定 NIC 小組與交換器獨立模式和位址雜湊或動態配置資源負載分配時，小組所使用的媒體存取控制 (MAC) 位址的輸出流量的主要 NIC 小組成員。 主要的 NIC 小組成員會將作業系統從一組初始的小組成員所選取的網路介面卡。

- [NIC 小組設定](nic-teaming-settings.md):本主題中，我們會提供您的 NIC 小組 」 屬性，例如小組的概觀，以及負載平衡模式。 我們也提供您關於待命配接器設定和主要小組介面屬性的詳細資料。 如果您有至少兩個網路介面卡在 NIC 小組時，您不需要指定容錯移轉的待命介面卡。
  


