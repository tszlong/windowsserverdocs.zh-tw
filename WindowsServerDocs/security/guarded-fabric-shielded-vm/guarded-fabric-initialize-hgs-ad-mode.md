---
title: 初始化 HGS 使用系統管理信任證明
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 664404cb72981e162bca016df14847e684d987c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821829"
---
# <a name="initialize-hgs-using-admin-trusted-attestation"></a>初始化 HGS 使用系統管理信任證明

>適用於：Windows Server （半年通道），Windows Server 2016

>[!IMPORTANT]
>已被取代開頭為 Windows Server 2019 的系統管理信任證明 （AD 模式）。 TPM 證明不可能的環境下，設定[裝載金鑰證明](guarded-fabric-initialize-hgs-key-mode.md)。 主機金鑰證明提供類似的保證，能 AD 模式，並已設定的工作變得更容易。 


這些步驟，視您是否初始化新的樹系或現有的防禦樹系中的 HGS 而有所不同：

1. [初始化新的樹系 （預設值） 的 HGS 叢集](guarded-fabric-initialize-hgs-ad-mode-default.md)

   - 或者 -

   [初始化 HGS 叢集中現有的防禦樹系](guarded-fabric-initialize-hgs-ad-mode-bastion.md)

2. [在 網狀架構網域中設定 DNS 轉送](guarded-fabric-configuring-fabric-dns.md)

3. [設定 DNS 轉送和 HGS 網域中的單向信任](guarded-fabric-configure-dns-forwarding-and-trust.md)



