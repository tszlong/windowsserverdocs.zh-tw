---
title: 網路卸載和最佳化技術
description: 本主題概要說明 Windows Server 2016 中的卸載和優化技術，並包含這些技術的其他指引連結。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/20/2018
ms.openlocfilehash: 57e8a61bc54be05a32daa7eabc55a09553cee28a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405688"
---
# <a name="network-offload-and-optimization-technologies"></a>網路卸載和最佳化技術

在本主題中，我們將概述 Windows Server 2016 中可用的各種網路卸載和優化功能，並討論它們如何協助讓網路更有效率。 這些技術包括僅限軟體（因此）的功能與技術、軟體和硬體（SH）整合功能和技術，以及僅限硬體（HO）功能與技術。

Windows Server 2016 中提供的三種網路功能類別如下： 

1.  [僅限軟體（SO）功能與技術](hpn-software-only-features.md)：這些功能會實作為作業系統的一部分，而且與基礎 NIC 無關。 有時候，這些功能需要調整 NIC 以進行最佳操作。 這些範例包括 Hyper-v 功能，例如 vmQoS、Acl 和非 Hyper-v 功能（如 NIC 小組）。   

2.  [軟體和硬體（SH）整合功能與技術](hpn-software-hardware-features.md)：這些功能都具有軟體和硬體元件。 軟體的熟知系結至功能所需的硬體功能。 其中的範例包括 VMMQ、VMQ、傳送端 IPv4 總和檢查碼卸載和 RSS。   

3.  [僅限硬體（HO）功能與技術](hpn-hardware-only-features.md)：這些硬體加速可改善網路效能與軟體，但不會熟知任何軟體功能的一部分。 這些範例包括中斷仲裁、流量控制，以及接收端 IPv4 總和檢查碼卸載。 

4. [NIC advanced 屬性](hpn-nic-advanced-properties.md)：您可以透過 Windows PowerShell 使用 Get-netadapter Cmdlet 來管理 Nic 和所有功能。  您也可以使用網路控制台（ncpa）來管理 Nic 和所有功能。 

>[!TIP]
>- 因此，不論 NIC 速度或 NIC 功能為何，所有硬體架構都會提供功能和技術。
>
>- 只有當您的網路介面卡支援功能或技術時，才可以使用 SH 和 HO 功能。

---