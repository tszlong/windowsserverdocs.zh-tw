---
title: 高效能網路功能
description: 本主題概要說明 Windows Server 2016 中的卸載和優化技術，並包含這些技術的其他指引連結。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/12/2018
ms.openlocfilehash: d5a4d5f06cd433fa92c617a3cb36e95d09be3b27
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950455"
---
# <a name="hardware-only-ho-features-and-technologies"></a>僅限硬體 (HO) 功能和技術

這些硬體加速可改善網路效能與軟體，但不會熟知任何軟體功能的一部分。 這些範例包括中斷仲裁、流量控制，以及接收端 IPv4 總和檢查碼卸載。

>[!TIP]
>如果已安裝的 NIC 支援 SH 和 HO 功能，則可以使用。 下面的功能描述將涵蓋如何判斷您的 NIC 是否支援此功能。

## <a name="address-checksum-offload"></a>位址總和檢查碼卸載

位址總和檢查碼卸載是一項 NIC 功能，可將位址總和檢查碼（IP、TCP、UDP）的計算卸載至 NIC 硬體，以進行傳送和接收。

在接收路徑上，總和檢查碼卸載會計算 IP、TCP 和 UDP 標頭中的總和檢查碼（適當的話），並向作業系統指出總和檢查碼是通過、失敗或未核取。 如果 NIC 判斷總和檢查碼是有效的，則 OS 會接受封包 unchallenged。 如果 NIC 判斷提示的總和檢查碼無效或未核取，則 IP/TCP/UDP 堆疊會在內部重新計算總和檢查碼。 如果計算的總和檢查碼失敗，則會捨棄封包。

在傳送路徑上，總和檢查碼卸載會在適當的情況下，計算並插入到 IP、TCP 或 UDP 標頭中。

停用傳送路徑上的總和檢查碼卸載，並不會停用使用大型傳送卸載（LSO）功能傳送至迷你埠驅動程式之封包的總和檢查碼計算和插入。  若要停用所有總和檢查碼卸載計算，使用者也必須停用 LSO。

_**管理位址總和檢查碼卸載**_

在 Advanced 屬性中，有數個不同的屬性：

-   IPv4 總和檢查碼卸載

-   TCP 總和檢查碼卸載（IPv4）

-   TCP 總和檢查碼卸載（IPv6）

-   UDP 總和檢查碼卸載（IPv4）

-   UDP 總和檢查碼卸載（IPv6）

根據預設，這些一律會啟用。 我們建議您一律啟用所有這些卸載。

您可以使用 NetAdapterChecksumOffload 和 Disable-NetAdapterChecksumOffload Cmdlet 來管理總和檢查碼卸載。 例如，下列 Cmdlet 會啟用 TCP （IPv4）和 UDP （IPv4）總和檢查碼計算：

```PowerShell
Enable-NetAdapterChecksumOffload –Name * -TcpIPv4 -UdpIPv4
```

_**使用位址總和檢查碼卸載的秘訣**_

無論工作負載或情況為何，都應一律啟用位址總和檢查碼卸載。 這種最基本的卸載技術，一律會改善您的網路效能。 其他無狀態卸載也需要進行總和檢查碼卸載，包括接收端調整（RSS）、接收區段聯合（RSC），以及大型傳送卸載（LSO）。

## <a name="interrupt-moderation-im"></a>中斷仲裁（IM）

在中斷作業系統之前，IM 會緩衝多個接收的封包。 當 NIC 收到封包時，它會啟動計時器。 當緩衝區已滿，或計時器過期（以先發生者為准）時，NIC 會中斷作業系統。 

許多 Nic 的插斷仲裁不只支援開啟/關閉。 大部分的 Nic 都支援 IM 的低、中和高比率的概念。 不同的費率代表較短或較長的計時器和適當的緩衝區大小調整，以減少延遲（低中斷仲裁）或減少中斷（高中斷仲裁）。

減少插斷和過度延遲封包傳遞之間有一個平衡。 一般來說，在啟用中斷仲裁的情況下，封包處理會更有效率。 高效能或低延遲的應用程式可能需要評估停用或減少中斷仲裁的影響。

## <a name="jumbo-frames"></a>大型訊框

巨型框架是一種 NIC 和網路功能，可讓應用程式傳送比預設1500位元組更大的畫面。 通常，巨型框架的限制大約是9000個位元組，但可能較小。

Windows Server 2012 R2 中的超長框架支援沒有任何變更。

在 Windows Server 2016 中有新的卸載： MTU_for_HNV。 這個新的卸載功能適用于超長的框架設定，以確保封裝的流量不需要在主機與連續的交換器之間分割。 SDN 堆疊的這項新功能，NIC 會自動計算要通告的 MTU，以及要在網路上使用的 MTU。 如果正在使用任何 HNV 卸載，則 MTU 的這些值會不同。 （在 [功能相容性] 資料表中，[表 1] MTU_for_HNV 會與 HNVv2 卸載的互動相同，因為它與 HNVv2 卸載直接相關）。

## <a name="large-send-offload-lso"></a>大型傳送卸載 (LSO)

LSO 允許應用程式將大型資料區塊傳遞至 NIC，而 NIC 會將資料分割成封包，以符合網路的最大傳輸單位（MTU）。

## <a name="receive-segment-coalescing-rsc"></a>接收區段聯合 (RSC)

接收區段聯合（也稱為大型接收卸載）是一項 NIC 功能，它會將封包視為位於網路中斷之間的相同資料流程，並將它們合併成單一封包，然後再將它們傳遞給作業系統。 已系結至 Hyper-v 虛擬交換器的 Nic 上無法使用 RSC。 如需詳細資訊，請參閱[接收區段聯合（RSC）](https://docs.microsoft.com/windows-server/networking/technologies/hpn/rsc-in-the-vswitch)。