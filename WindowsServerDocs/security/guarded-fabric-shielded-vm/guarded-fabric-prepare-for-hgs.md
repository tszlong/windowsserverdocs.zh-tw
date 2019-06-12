---
title: 檢閱 HGS 必要條件
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: f4b4d1a8-bf6d-4881-9150-ddeca8b48038
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 9a668a39990b79862b99c2c7d9aeaf6540fa376d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447369"
---
# <a name="review-prerequisites-for-the-host-guardian-service"></a>檢閱 「 主機守護者服務的必要條件

>適用於：Windows Server 2019，Windows Server （半年通道），Windows Server 2016


本主題涵蓋 HGS 必要條件和準備 HGS 部署的初始步驟。

## <a name="prerequisites"></a>先決條件 

-   **硬體**:HGS 可以實體或虛擬機器上執行，但實體機器建議。

    如果您想要執行 HGS 作為三個節點實體叢集 （可用性），您必須有三個實體伺服器。 （叢集的最佳做法是，三部伺服器應該有非常類似的硬體）。
  
-   **作業系統**:主機金鑰證明可讓您需要 Windows Server 2019 Standard 或 Datacenter edition 作業系統與[v2 證明](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#versioned-attestation-policies)。 針對 TPM 型證明 HGS 可以執行 Windows Server 2019 或 Windows Server 2016、 Standard 或 Datacenter edition。

-   **伺服器角色**:主機守護者服務及支援伺服器角色。

-   **設定權限/權限的網狀架構 （主機） 網域**:您必須設定 DNS 轉送，網狀架構 （主機） 網域與 HGS 網域之間。 
    
## <a name="upgrading-hgs"></a>升級 HGS

如果您已經部署 HGS，而且想要升級它的作業系統，請遵循[升級指南](guarded-fabric-upgrade-to-2019.md)到 HGS 和 HYPER-V 的伺服器升級至最新的作業系統。

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [取得憑證以用於 HGS](guarded-fabric-obtain-certs.md)