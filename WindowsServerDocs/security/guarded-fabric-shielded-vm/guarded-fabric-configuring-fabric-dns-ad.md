---
description: 深入瞭解：設定受防護主機 (AD) 的網狀架構 DNS
title: 設定受防護主機 (AD) 的網狀架構 DNS
ms.topic: article
ms.assetid: 074b6d09-f16e-49bf-b88a-377139d35067
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 29ec5a824ca16b45bc4445a7d838c71985ca6457
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049866"
---
# <a name="configure-the-fabric-dns-for-guarded-hosts-ad"></a>設定受防護主機 (AD) 的網狀架構 DNS

>適用於：Windows Server 2016


>[!IMPORTANT]
>從 Windows Server 2019 開始，AD 模式已被取代。 針對不可能進行 TPM 證明的環境，請設定 [主機金鑰證明](guarded-fabric-initialize-hgs-key-mode.md)。 主機金鑰證明提供類似于 AD 模式的保證，而且更容易設定。

網狀架構系統管理員必須設定網狀架構 DNS 來允許受防護主機解析 HGS 叢集。
Hgs 叢集必須已 [由 hgs 系統管理員設定](/WindowsServerDocs/virtualization/guarded-fabric-shielded-vm/guarded-fabric-setting-up-the-host-guardian-service-hgs.md)。



[!INCLUDE [Configure fabric DNS](../../../includes/guarded-fabric-configure-fabric-dns.md)]


## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [設定 HGS DNS 和單向信任](guarded-fabric-configure-dns-forwarding-and-trust.md)
