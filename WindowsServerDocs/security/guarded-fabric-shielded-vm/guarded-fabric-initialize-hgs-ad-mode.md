---
title: 使用系統管理員信任的證明來初始化 HGS
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 491754e8bcaad4524084604b78c7c6ed0fdee295
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402356"
---
# <a name="initialize-hgs-using-admin-trusted-attestation"></a>使用系統管理員信任的證明來初始化 HGS

>適用於：Windows Server (半年度管道)、Windows Server 2016

>[!IMPORTANT]
>從 Windows Server 2019 開始，系統管理員信任的證明（AD 模式）已淘汰。 針對不可能進行 TPM 證明的環境，請設定[主機金鑰證明](guarded-fabric-initialize-hgs-key-mode.md)。 主機金鑰證明提供與 AD 模式類似的保證，而且設定起來較簡單。 


這些步驟會根據您是在新樹系或現有防禦樹系中初始化 HGS 而有所不同：

1. [在新樹系中初始化 HGS 叢集（預設值）](guarded-fabric-initialize-hgs-ad-mode-default.md)

   \- 或者 -

   [初始化現有防禦樹系中的 HGS 叢集](guarded-fabric-initialize-hgs-ad-mode-bastion.md)

2. [設定網狀架構網域中的 DNS 轉送](guarded-fabric-configuring-fabric-dns.md)

3. [設定 DNS 轉送和 HGS 網域中的單向信任](guarded-fabric-configure-dns-forwarding-and-trust.md)



