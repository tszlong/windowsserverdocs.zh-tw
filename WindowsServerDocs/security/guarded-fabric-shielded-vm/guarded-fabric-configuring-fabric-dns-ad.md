---
title: 針對受防護主機 (AD) 中設定網狀架構 DNS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 074b6d09-f16e-49bf-b88a-377139d35067
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 9d302dcd06b7a3a40afbb6f613c39caaabbeba91
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443722"
---
# <a name="configure-the-fabric-dns-for-guarded-hosts"></a>設定受防護主機網狀架構 DNS

>適用於：Windows Server 2016


>[!IMPORTANT]
>AD 模式開頭為 Windows Server 2019 已被取代。 TPM 證明不可能的環境下，設定[裝載金鑰證明](guarded-fabric-initialize-hgs-key-mode.md)。 主機金鑰證明提供類似的保證，能 AD 模式，並已設定的工作變得更容易。 

網狀架構系統管理員必須設定 DNS 會允許解析 HGS 叢集的受防護的主機網狀架構。 HGS 叢集必須已經是[由 HGS 系統管理員設定](/WindowsServerDocs/virtualization/guarded-fabric-shielded-vm/guarded-fabric-setting-up-the-host-guardian-service-hgs.md)。



[!INCLUDE [Configure fabric DNS](../../../includes/guarded-fabric-configure-fabric-dns.md)] 


## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [設定 HGS DNS 和單向信任](guarded-fabric-configure-dns-forwarding-and-trust.md)
