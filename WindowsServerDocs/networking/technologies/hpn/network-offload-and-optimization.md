---
title: 網路卸載和最佳化技術
description: 本主題提供的卸載和 Windows Server 2016 中的最佳化技術的概觀，並包含這些技術的其他指導連結。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/20/2018
ms.openlocfilehash: 9cd63ae557eab6e8e4f69fedd20619d4e7af9184
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847849"
---
# <a name="network-offload-and-optimization-technologies"></a>網路卸載和最佳化技術

本主題中，我們會提供不同的網路卸載和最佳化可用的功能在 Windows Server 2016 概觀，並討論它們如何協助讓網路更有效率。 這些技術包括軟體只 (SO) 功能與技術，軟體和硬體 (SH) 整合功能和技術，以及硬體只 （主機） 功能與技術。

Windows Server 2016 中可用的網路功能的三個分類如下： 

1.  [只有軟體 (SO) 功能與技術](hpn-software-only-features.md):這些功能會實作為作業系統的一部分，而且是獨立的基礎 NIC。 有時這些功能會需要一些調整的 nic，最佳的作業。 這些範例包含 hyper-v 功能，例如 vmQoS、 Acl 及非 HYPER-V 功能，例如 NIC 小組。   

2.  [軟體和硬體 (SH) 整合功能與技術](hpn-software-hardware-features.md):這些功能有軟體和硬體元件。 軟體會緊密繫結所需的功能運作的硬體功能。 其中的範例包括 VMMQ、 VMQ，傳送端 IPv4 總和檢查碼卸載、 和 RSS。   

3.  [硬體只 （主機） 功能與技術](hpn-hardware-only-features.md):這些硬體加速提升網路效能與軟體搭配使用，但不會舉手任何軟體功能的一部分。 插斷仲裁 」、 「 流程控制和 「 接收端 IPv4 總和檢查碼卸載，都是其範例。 

4. [進階屬性的 NIC](hpn-nic-advanced-properties.md):您可以管理 Nic 和透過 Windows PowerShell 使用 NetAdapter cmdlet 的所有功能。  您也可以管理 Nic 和使用 「 網路控制台 (ncpa.cpl) 的所有功能。 

>[!TIP]
>- 因此功能及技術，可在所有硬體架構中，不論 NIC 速度或 NIC 功能。
>
>- 只有在您的網路介面卡支援的功能或技術時，才使用 SH 和主控功能。

---