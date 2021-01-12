---
title: 高效能網路
description: 深入瞭解高效能的網路功能，以及硬體加速如何提升網路效能與軟體，但不了如指掌任何軟體功能。
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/12/2018
ms.openlocfilehash: 13ab5cec3ef8ec035351f0a2a30c2152b1f48499
ms.sourcegitcommit: d42b80f947dbfa8660d982be67d77745a28081e5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/12/2021
ms.locfileid: "98113080"
---
# <a name="hardware-only-ho-features-and-technologies"></a>僅限硬體 (HO) 功能和技術

這些硬體加速可改善網路效能與軟體，但不了如指掌任何軟體功能的一部分。 這些範例包括中斷仲裁、流量控制，以及接收端 IPv4 總和檢查碼卸載。

>[!TIP]
>如果已安裝的 NIC 支援 SH 和 HO 功能，則可使用。 以下的功能描述將說明如何分辨您的 NIC 是否支援此功能。

## <a name="address-checksum-offload"></a>位址總和檢查碼卸載

位址總和檢查碼卸載是一項 NIC 功能，可將位址總和檢查碼的計算卸載 (IP、TCP、UDP) 傳送和接收的 NIC 硬體。

在接收路徑上，總和檢查碼卸載會將 IP、TCP 和 UDP 標頭中的總和檢查碼 (適當的) 來計算，並向作業系統指出總和檢查碼是否已通過、失敗或未核取。 如果 NIC 判斷總和檢查碼是否有效，則 OS 會接受封包 unchallenged。 如果 NIC 判斷提示的總和檢查碼無效或未核取，則 IP/TCP/UDP 堆疊會在內部再次計算總和檢查碼。 如果計算的總和檢查碼失敗，則會捨棄封包。

在傳送路徑上，總和檢查碼卸載會在適當的情況下，計算並將總和檢查碼插入至 IP、TCP 或 UDP 標頭。

停用傳送路徑上的總和檢查碼卸載時，不會針對使用大型傳送卸載 (LSO) 功能傳送至微型埠驅動程式的封包，停用總和檢查碼計算和插入。  若要停用所有總和檢查碼卸載計算，使用者也必須停用 LSO。

_**管理位址總和檢查碼卸載**_

在 Advanced 屬性中有幾個不同的屬性：

-   IPv4 總和檢查碼卸載

-   TCP 總和檢查碼卸載 (IPv4) 

-    (IPv6) 的 TCP 總和檢查碼卸載

-   UDP 總和檢查碼卸載 (IPv4) 

-    (IPv6) 的 UDP 總和檢查碼卸載

依預設，這些都是永遠啟用的。 建議您一律啟用這些卸載。

您可以使用 Enable-NetAdapterChecksumOffload 和 Disable-NetAdapterChecksumOffload Cmdlet 來管理總和檢查碼卸載。 例如，下列 Cmdlet 會啟用 TCP (IPv4) 和 UDP (IPv4) 總和檢查碼計算：

```PowerShell
Enable-NetAdapterChecksumOffload –Name * -TcpIPv4 -UdpIPv4
```

_**使用位址總和檢查碼卸載的秘訣**_

無論工作負載或情況為何，都應該一律啟用位址總和檢查碼卸載。 這最基本的所有卸載技術都能改善您的網路效能。 若要進行其他無狀態卸載，也需要總和檢查碼卸載，包括接收端調整 (RSS) 、接收區段聯合 (RSC) 和大型傳送卸載 (LSO) 。

## <a name="interrupt-moderation-im"></a>中斷仲裁 (IM) 

IM 在中斷作業系統之前，會緩衝多個收到的封包。 當 NIC 收到封包時，它會啟動計時器。 當緩衝區已滿或計時器過期（以先發生者為准）時，NIC 就會中斷作業系統。

許多 Nic 都支援不只是開啟/關閉以進行中斷仲裁。 大部分的 Nic 都支援 IM 的低、中和高比率的概念。 不同的速率代表較短或較長的計時器和適當的緩衝區大小調整，以降低延遲 (低中斷仲裁) 或減少中斷 (高中斷仲裁) 。

減少中斷和過度延遲封包傳遞之間有一個平衡。 一般來說，在啟用中斷仲裁的情況下，封包處理更有效率。 高效能或低延遲的應用程式可能需要評估停用或減少中斷仲裁的影響。

## <a name="jumbo-frames"></a>大型訊框

巨型幀是 NIC 和網路功能，可讓應用程式傳送比預設1500個位元組更大的框架。 大型框架的限制通常大約是9000個位元組，但可能較小。

Windows Server 2012 R2 中的大型框架支援沒有任何變更。

在 Windows Server 2016 中，有一個新的卸載： MTU_for_HNV。 這個新的卸載適用于大型框架設定，以確保封裝的流量不需要在主機和連續的切換之間進行分割。 SDN 堆疊的這項新功能會自動計算要公告的 MTU，以及要在網路上使用的 MTU。 如果有任何 HNV 卸載正在使用中，則這些 MTU 值會不同。  (在功能相容性資料表中，[表 1] MTU_for_HNV 與 HNVv2 卸載的互動相同，因為它與 HNVv2 卸載是直接相關的。 ) 

## <a name="large-send-offload-lso"></a>大型傳送卸載 (LSO)

LSO 可讓應用程式將大型資料區塊傳遞至 NIC，而 NIC 會將資料分割成符合網路最大傳輸單位 (MTU) 的封包。

## <a name="receive-segment-coalescing-rsc"></a>Receive Segment Coalescing (RSC)

接收區段聯合（也稱為大型接收卸載）是一種 NIC 功能，它會將封包傳送到網路中斷之間的相同資料流程中，並將它們合併成單一封包，然後再傳遞至作業系統。 在系結至 Hyper-v 虛擬交換器的 Nic 上無法使用 RSC。 如需詳細資訊，請參閱 [接收區段聯合 (RSC) ](./rsc-in-the-vswitch.md)。