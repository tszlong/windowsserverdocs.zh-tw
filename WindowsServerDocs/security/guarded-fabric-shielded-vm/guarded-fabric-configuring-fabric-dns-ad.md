---
title: 設定受防護主機的網狀架構 DNS （AD）
ms.prod: windows-server
ms.topic: article
ms.assetid: 074b6d09-f16e-49bf-b88a-377139d35067
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 5b7deafb49083ad55d5a49d1ad3c5e063e91483f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856821"
---
# <a name="configure-the-fabric-dns-for-guarded-hosts"></a>為受防護主機設定網狀架構 DNS

>適用於︰Windows Server 2016


>[!IMPORTANT]
>從 Windows Server 2019 開始，AD 模式已淘汰。 針對不可能進行 TPM 證明的環境，請設定[主機金鑰證明](guarded-fabric-initialize-hgs-key-mode.md)。 主機金鑰證明提供與 AD 模式類似的保證，而且設定起來較簡單。 

網狀架構系統管理員必須設定網狀架構 DNS，以允許受防護主機解析 HGS 叢集。 Hgs 叢集必須已[由 hgs 系統管理員設定](/WindowsServerDocs/virtualization/guarded-fabric-shielded-vm/guarded-fabric-setting-up-the-host-guardian-service-hgs.md)。



[!INCLUDE [Configure fabric DNS](../../../includes/guarded-fabric-configure-fabric-dns.md)] 


## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [設定 HGS DNS 和單向信任](guarded-fabric-configure-dns-forwarding-and-trust.md)
