---
description: 深入瞭解：使用系統管理員信任的證明初始化 HGS
title: 使用系統管理員信任的證明初始化 HGS
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 65ba0bd90d42ac038eeb8eb304def80ce7093ff1
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049756"
---
# <a name="initialize-hgs-using-admin-trusted-attestation"></a>使用系統管理員信任的證明初始化 HGS

>適用於：Windows Server (半年度管道)、Windows Server 2016

>[!IMPORTANT]
>從 Windows Server 2019 開始，已淘汰 (AD 模式) 的管理員信任證明。 針對不可能進行 TPM 證明的環境，請設定 [主機金鑰證明](guarded-fabric-initialize-hgs-key-mode.md)。 主機金鑰證明提供類似于 AD 模式的保證，而且更容易設定。


這些步驟會根據您是在新樹系或現有防禦樹系中初始化 HGS 而有所不同：

1. [初始化新樹系中的 HGS 叢集 (預設) ](guarded-fabric-initialize-hgs-ad-mode-default.md)

   -或者-

   [初始化現有防禦樹系中的 HGS 叢集](guarded-fabric-initialize-hgs-ad-mode-bastion.md)

2. [設定網狀架構網域中的 DNS 轉送](guarded-fabric-configuring-fabric-dns.md)

3. [設定 DNS 轉送以及 HGS 網域中的單向信任](guarded-fabric-configure-dns-forwarding-and-trust.md)



