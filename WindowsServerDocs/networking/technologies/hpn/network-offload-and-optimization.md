---
title: 網路卸載和最佳化技術
description: 本主題概要說明 Windows Server 2016 中的卸載和優化技術，並包含這些技術的其他指引連結。
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/20/2018
ms.openlocfilehash: e6f53fa8462a51e23fa9fc00aad89d8a8a3ee445
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87947002"
---
# <a name="network-offload-and-optimization-technologies"></a>網路卸載和最佳化技術

在本主題中，我們將概述 Windows Server 2016 中可用的各種網路卸載和優化功能，並討論它們如何協助讓網路更有效率。 這些技術僅包含軟體 (因此) 功能與技術、軟體和硬體 (SH) 整合的功能和技術，以及僅 (具備的) 功能與技術的硬體。

Windows Server 2016 中提供的三種網路功能類別如下：

1.  [僅限軟體 (因此) 功能與技術](hpn-software-only-features.md)：這些功能會實作為作業系統的一部分，而且與基礎 NIC (的) 無關。 有時候，這些功能需要調整 NIC 以進行最佳操作。 這些範例包括 Hyper-v 功能，例如 vmQoS、Acl 和非 Hyper-v 功能（如 NIC 小組）。

2.  [軟體和硬體 (SH) 整合功能與技術](hpn-software-hardware-features.md)：這些功能同時具有軟體和硬體元件。 軟體的熟知系結至功能所需的硬體功能。 其中的範例包括 VMMQ、VMQ、傳送端 IPv4 總和檢查碼卸載和 RSS。

3.  [僅硬體 (可) 的功能與技術](hpn-hardware-only-features.md)：這些硬體加速可將網路效能與軟體搭配使用，但不會熟知任何軟體功能的一部分。 這些範例包括中斷仲裁、流量控制，以及接收端 IPv4 總和檢查碼卸載。

4. [NIC advanced properties](hpn-nic-advanced-properties.md)：您可以使用 get-netadapter Cmdlet，透過 Windows PowerShell 管理 nic 和所有功能。  您也可以使用 [網路控制台] ( # A0) 來管理 Nic 和所有功能。

>[!TIP]
>- 因此，不論 NIC 速度或 NIC 功能為何，所有硬體架構都會提供功能和技術。
>
>- 只有當您的網路介面卡支援功能或技術時，才可以使用 SH 和 HO 功能。

---