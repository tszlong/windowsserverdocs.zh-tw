---
title: 對 NIC 小組進行疑難排解
description: 本主題提供針對 Windows Server 2016 中的 NIC 小組進行疑難排解的相關資訊。
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fdee02ec-3a7e-473e-9784-2889dc1b6dbb
ms.author: lizross
author: eross-msft
ms.date: 09/13/2018
ms.openlocfilehash: 75b6ae2f2c7d6b4ab28aaedcc7309ccba3dcbd02
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316375"
---
# <a name="troubleshooting-nic-teaming"></a>對 NIC 小組進行疑難排解

>適用於：Windows Server (半年通道)、Windows Server 2016

在本主題中，我們將討論針對 NIC 小組進行疑難排解的方法，例如硬體和實體交換器的安全性。  當標準通訊協定的硬體實現不符合規格時，NIC 小組的效能可能會受到影響。 此外，視設定而定，NIC 小組可能會從相同的 IP 位址傳送封包，並將多個 MAC 位址與實體交換器上的安全性功能結合在一起。

  
## <a name="hardware-that-doesnt-conform-to-specification"></a>不符合規格的硬體  
  
在正常作業期間，NIC 小組可能會從相同的 IP 位址傳送封包，但有多個 MAC 位址。 根據通訊協定標準，這些封包的接收者必須將主機或 VM 的 IP 位址解析為特定的 MAC 位址，而不是回應接收封包中的 MAC 位址。  正確執行位址解析通訊協定（ARP 和 NDP）的用戶端會傳送具有正確目的地 MAC 位址的封包，也就是擁有該 IP 位址的 VM 或主機的 MAC 位址。 
  
有些內嵌式硬體並不會正確地執行位址解析通訊協定，而且也可能無法使用 ARP 或 NDP 明確地將 IP 位址解析成 MAC 位址。  例如，存放區域網路（SAN）控制器可能會以這種方式執行。 不符合規範的裝置會從接收的封包複製來源 MAC 位址，然後在對應的傳出封包中使用它做為目的地 MAC 位址，因而導致封包傳送至錯誤的目的地 MAC 位址。 因此，Hyper-v 虛擬交換器會捨棄封包，因為它們不符合任何已知的目的地。  
  
如果您無法連線到 SAN 控制器或其他內嵌硬體，則應該採取封包捕獲來判斷您的硬體是否正確地執行 ARP 或 NDP，並洽詢硬體廠商尋求支援。  

  
## <a name="physical-switch-security-features"></a>實體交換器安全性功能  
視設定而定，NIC 小組可能會從相同的 IP 位址傳送封包，並將多個來源 MAC 位址與實體交換器上的安全性功能結合在一起。 例如，動態 ARP 檢查或 IP 來源防護，特別是當實體交換器不知道埠是小組的一部分時，在您以交換器獨立模式設定 NIC 小組時，就會發生這種情況。 檢查交換器記錄檔，以判斷交換器安全性功能是否會造成連線問題。 
  
## <a name="disabling-and-enabling-network-adapters-by-using-windows-powershell"></a>使用 Windows PowerShell 停用及啟用網路介面卡  

NIC 小組失敗的常見原因是，小組介面已停用，而且在許多情況下，執行一連串的命令時，不小心。  這個特定的命令順序並不會啟用所有的 NetAdapters，因為停用所有 Nic 的基礎實體成員會移除 NIC 小組介面。 

在此情況下，NIC 小組介面不會再顯示在 Get-netadapter 中，因此， **get-netadapter \\** * 不會啟用 NIC 小組。 不過， **get-netadapter \\** * 命令會啟用成員 nic，然後（在短時間之後）重新建立小組介面。 小組介面會維持「停用」狀態，直到重新啟用為止，讓網路流量開始流動。 

下列 Windows PowerShell 命令順序可能會意外停用小組介面：  
  
```PowerShell 
Disable-NetAdapter *  
Enable-NetAdapter *  
```  
  

  
## <a name="related-topics"></a>相關主題  
- [NIC](NIC-Teaming.md)小組：在本主題中，我們將概述 Windows Server 2016 中的網路介面卡（NIC）小組。 NIC 小組可讓您將一和32實體 Ethernet 網路介面卡分成一或多個以軟體為基礎的虛擬網路介面卡。 這些虛擬網路介面卡可在網路介面卡故障時，提供快速的效能與容錯。   

- [NIC 小組 MAC 位址使用和管理](NIC-Teaming-MAC-Address-Use-and-Management.md)：當您使用「交換器獨立模式」和「位址雜湊」或「動態」負載分佈來設定 NIC 小組時，小組會使用連出流量的主要 NIC 小組成員的媒體存取控制（MAC）位址。 主要 NIC 小組成員是由一組初始小組成員的作業系統選取的網路介面卡。

- [Nic 小組設定](nic-teaming-settings.md)：在本主題中，我們將概述 nic 團隊的屬性，例如團隊和負載平衡模式。 我們也會提供有關待命介面卡設定和主要小組介面屬性的詳細資料。 如果您的 NIC 小組中至少有兩張網路介面卡，則不需要指定待命介面卡來容錯。
  


