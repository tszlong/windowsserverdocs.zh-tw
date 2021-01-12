---
title: 網路卸載和最佳化技術
description: 瞭解 Windows Server 2016 中可用的不同網路卸載和優化功能，並討論這些功能如何讓網路更有效率。
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/20/2018
ms.openlocfilehash: 8d0728dda0bca8188668c1bbb3570390f8608432
ms.sourcegitcommit: d42b80f947dbfa8660d982be67d77745a28081e5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/12/2021
ms.locfileid: "98112914"
---
# <a name="network-offload-and-optimization-technologies"></a>網路卸載和最佳化技術

在本主題中，我們將概述 Windows Server 2016 中可用的不同網路卸載和優化功能，並討論這些功能如何讓網路更有效率。 這些技術僅包含軟體 (因此) 的功能和技術、軟體和硬體 (SH) 整合的功能和技術，以及僅 (的) 功能和技術。

Windows Server 2016 提供的三種網路功能類別如下：

1.  [軟體 (因此) 的功能和技術](hpn-software-only-features.md)：這些功能會實作為作業系統的一部分，而且與基礎 NIC (的) 無關。 有時候，這些功能會需要對 NIC 進行一些調整，才能達到最佳的操作。 其中的範例包括 Hyper-v 功能，例如 vmQoS、Acl 和非 Hyper-v 功能（例如 NIC 小組）。

2.  [軟體和硬體 (SH) 整合的功能和技術](hpn-software-hardware-features.md)：這些功能都有軟體和硬體元件。 軟體瞭若指掌系結至功能運作所需的硬體功能。 其中的範例包括 VMMQ、VMQ、傳送端 IPv4 總和檢查碼卸載和 RSS。

3.  [硬體僅 () 的功能和技術](hpn-hardware-only-features.md)：這些硬體加速可提升與軟體搭配使用的網路效能，但不了如指掌任何軟體功能的一部分。 這些範例包括中斷仲裁、流量控制，以及接收端 IPv4 總和檢查碼卸載。

4. [NIC advanced 屬性](hpn-nic-advanced-properties.md)：您可以使用 get-netadapter Cmdlet，透過 Windows PowerShell 管理 nic 和所有功能。  您也可以使用網路主控台 ( # A0) 來管理 Nic 和所有功能。

>[!TIP]
>- 如此一來，無論 NIC 速度或 NIC 的功能為何，都可以在所有硬體架構中使用功能和技術。
>
>- 只有當您的網路介面卡支援功能或技術時，才可使用 SH 和 HO 功能。

---