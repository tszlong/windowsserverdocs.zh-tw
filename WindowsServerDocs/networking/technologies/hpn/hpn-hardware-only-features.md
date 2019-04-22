---
title: 高效能的網路功能
description: 本主題提供的卸載和 Windows Server 2016 中的最佳化技術的概觀，並包含這些技術的其他指導連結。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/12/2018
ms.openlocfilehash: 09e18a41452baa0add8e055ceb22d2f5c2ad886e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815829"
---
## <a name="hardware-only-ho-features-and-technologies"></a>僅限硬體 (HO) 功能和技術

這些硬體加速提升網路效能與軟體搭配使用，但不會舉手任何軟體功能的一部分。 插斷仲裁 」、 「 流程控制和 「 接收端 IPv4 總和檢查碼卸載，都是其範例。

>[!TIP]
>如果已安裝的 NIC 支援此提供 SH 和主控功能。 下列功能描述將討論如何判斷您的 NIC 是否支援此功能。

### <a name="address-checksum-offload"></a>位址總和檢查碼卸載

位址總和檢查碼卸載是卸載計算的地址的總和檢查碼 （IP、 TCP、 UDP） NIC 硬體，同時傳送和接收的 NIC 功能。

為接收路徑上總和檢查碼卸載計算 （視情況） 的 IP、 TCP 和 UDP 標頭中的總和檢查碼，並指出 os，總和檢查碼通過、 失敗或未核取。 如果 NIC 判斷提示的總和檢查碼有效，OS 會接受傳將封包。 如果 NIC 判斷提示的總和檢查碼不正確或未核取，IP/TCP/UDP 堆疊在內部會計算總和檢查碼一次。 如果計算的總和檢查碼失敗時，取得捨棄封包。

為傳送路徑上的總和檢查碼卸載計算，並插入適當的 IP、 TCP 或 UDP 標頭中的總和檢查碼。

停用總和檢查碼卸載，傳送路徑上的不會停用總和檢查碼計算及插入封包傳送至使用大型傳送卸載 (LSO) 功能的 miniport 驅動程式。  若要停用所有的總和檢查碼卸載計算，使用者必須也會停用讓 LSO。

_**管理位址總和檢查碼卸載**_

在 進階內容中有數個不同的屬性：

-   IPv4 總和檢查碼卸載

-   TCP 總和檢查碼卸載 (IPv4)

-   TCP 總和檢查碼卸載 (IPv6)

-   UDP 總和檢查碼卸載 (IPv4)

-   UDP 總和檢查碼卸載 (IPv6)

根據預設，這些都一定會啟用。 我們建議一律啟用所有這些卸載。

總和檢查碼卸載可以管理使用啟用 NetAdapterChecksumOffload 和停用 NetAdapterChecksumOffload cmdlet。 例如，下列 cmdlet 會啟用 TCP (IPv4) 和 UDP (IPv4) 總和檢查碼計算：

```PowerShell
Enable-NetAdapterChecksumOffload –Name * -TcpIPv4 -UdpIPv4
```

_**使用 地址的總和檢查碼卸載的秘訣**_

位址總和檢查碼卸載應該要一直啟用無論何種工作負載或情況。 這所有的大多數的 basic 卸載技術不見得能改善您的網路效能。 總和檢查碼卸載也會需要其他的無狀態卸載工作包括接收端調整 (RSS)、 接收區段聯合 (RSC)，以及大型傳送卸載 (LSO)。

### <a name="interrupt-moderation-im"></a>插斷仲裁 (IM)

IM 訊息緩衝處理多個接收的封包之前中斷作業系統。 當 NIC 接收封包時，它會啟動計時器。 當緩衝區已滿，或計時器到期，視何者先，NIC 中斷作業系統。 

以上只是開啟/關閉插斷仲裁的支援的 Nic 數目。 大部分的 Nic 支援 IM 低、 中和高比率的概念。 不同的比率表示短一點或長計時器和適當的緩衝區大小調整，以減少延遲 （低插斷仲裁），或降低插斷 （高的插斷仲裁）。

沒有要刪除降低插斷與過度延遲封包傳遞之間取得平衡。 一般而言，封包處理與啟用的插斷仲裁會更有效率。 高效能或低延遲的應用程式可能需要評估停用或縮減插斷仲裁的影響。

### <a name="jumbo-frames"></a>大型訊框

Jumbo 框架是 NIC 和網路的功能，可讓應用程式以傳送遠大於預設值是 1500 個位元組的框架。 通常需大型訊框限制為大約 9000 個位元組，但可能會變得更小。

不沒有 jumbo 框架支援 Windows Server 2012 R2 中的任何變更。

Windows Server 2016 中沒有新的卸載：MTU_for_HNV. 這個新的卸載搭配巨大框架設定，以確保封裝的流量不需要在主機與相鄰的交換器之間的區隔。 這項新功能的 SDN 堆疊具有自動計算哪些通告的 MTU 和在網路上使用何種 MTU 的 NIC。 MTU 這些值會不同，如果正在使用中的任何 HNV 卸載。 （功能相容性的資料表，表 1 中 MTU_for_HNV 會有相同的互動，卸載有，因為它直接相關 HNVv2 HNVv2 卸載）。

### <a name="large-send-offload-lso"></a>大型傳送卸載 (LSO)

讓 LSO 可讓您將大型資料區塊交給 NIC 和 NIC 中斷資料符合內最大傳輸單位 (MTU) 的網路封包中的應用程式。

### <a name="receive-segment-coalescing-rsc"></a>Receive Segment Coalescing (RSC)

接收區段聯合，也就是大型接收卸載，是會屬於相同的資料流到達之間網路中斷，交給作業系統之前將它們聯合成單一的封包的封包的 NIC 功能。 RSC 並不適用於繫結至 HYPER-V 虛擬交換器的 Nic。 如需詳細資訊，請參閱 <<c0> [ 接收區段聯合 (RSC)](https://docs.microsoft.com/windows-server/networking/technologies/hpn/rsc-in-the-vswitch)。