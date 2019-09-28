---
title: 設定受防護主機的網狀架構 DNS （AD）
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 074b6d09-f16e-49bf-b88a-377139d35067
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 411b845d57c36916dcbc73d51675f5d9f92bfa0e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386762"
---
# <a name="configure-the-fabric-dns-for-guarded-hosts"></a>為受防護主機設定網狀架構 DNS

>適用於：Windows Server 2016


>[!IMPORTANT]
>從 Windows Server 2019 開始，AD 模式已淘汰。 針對不可能進行 TPM 證明的環境，請設定[主機金鑰證明](guarded-fabric-initialize-hgs-key-mode.md)。 主機金鑰證明提供與 AD 模式類似的保證，而且設定起來較簡單。 

網狀架構系統管理員必須設定網狀架構 DNS，以允許受防護主機解析 HGS 叢集。 Hgs 叢集必須已[由 hgs 系統管理員設定](/WindowsServerDocs/virtualization/guarded-fabric-shielded-vm/guarded-fabric-setting-up-the-host-guardian-service-hgs.md)。



[!INCLUDE [Configure fabric DNS](../../../includes/guarded-fabric-configure-fabric-dns.md)] 


## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [設定 HGS DNS 和單向信任](guarded-fabric-configure-dns-forwarding-and-trust.md)
