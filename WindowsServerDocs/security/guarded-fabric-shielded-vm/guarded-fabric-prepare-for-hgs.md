---
title: 審查 HGS 必要條件
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: f4b4d1a8-bf6d-4881-9150-ddeca8b48038
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 9024557dd42ede27144bf10aa5873b6bb12d585c
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/13/2020
ms.locfileid: "79322480"
---
# <a name="review-prerequisites-for-the-host-guardian-service"></a>審查主機守護者服務的必要條件

>適用于： Windows Server 2019、Windows Server （半年通道）、Windows Server 2016


本主題涵蓋 HGS 必要條件和準備 HGS 部署的初始步驟。

## <a name="prerequisites"></a>必要條件 

-   **硬體**： HGS 可以在實體或虛擬機器上執行，但建議使用實體機器。

    如果您想要以三個節點的實體叢集執行 HGS （以提供可用性），您必須有三部實體伺服器。 （作為叢集的最佳作法，這三部伺服器應該有非常類似的硬體）。
  
-   **作業系統**：主機金鑰證明需要搭配[v2 證明](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#versioned-attestation-policies)運作的 Windows Server 2019 Standard 或 Datacenter edition。 對於以 TPM 為基礎的證明，HGS 可以執行 Windows Server 2019 或 Windows Server 2016 Standard 或 Datacenter edition。

-   **伺服器角色**：主機守護者服務和支援伺服器角色。

-   網狀**架構（主機）網域的設定許可權/許可權**：您將需要設定網狀架構（主機）網域與 HGS 網域之間的 DNS 轉送。 
    
## <a name="upgrading-hgs"></a>升級 HGS

如果您已經部署 HGS 並想要升級其作業系統，請遵循[升級指引](guarded-fabric-upgrade-to-2019.md)，將您的 HGS 和 hyper-v 伺服器升級至最新的作業系統。

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [取得 HGS 的憑證](guarded-fabric-obtain-certs.md)