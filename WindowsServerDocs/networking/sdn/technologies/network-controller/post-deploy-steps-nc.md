---
title: 網路控制卡的部署後步驟
description: 本主題提供非 Kerberos 部署 Windows Server 2016 Datacenter 中的網路控制站的憑證組態指示。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: eea0aca9-8d89-48fb-8068-fca40c90d34b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f7d6bbd50537e24f392eabde7d103c91a4f07c90
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871079"
---
# <a name="post-deployment-steps-for-network-controller"></a>網路控制卡的部署後步驟

當您安裝網路控制站時，您可以選擇 Kerberos 或非 Kerberos 的部署。

針對非\-Kerberos 的部署，您必須設定憑證。

## <a name="configure-certificates-for-non-kerberos-deployments"></a>設定憑證以非 Kerberos 部署

如果電腦或虛擬機器\(Vm\)網路控制站和管理用戶端不是網域\-聯結，您必須設定憑證\-藉由完成下列架構驗證步驟。

- 電腦驗證網路控制站上建立憑證。 憑證主體名稱必須是相同的網路控制站的電腦或 VM 的 DNS 名稱。

- 建立管理用戶端上的憑證。 此憑證必須受網路控制站。
  
- 註冊網路控制站的電腦或 VM 上的憑證。 憑證必須符合下列需求。
  
    -  在 增強金鑰使用方法，則必須設定伺服器驗證目的和用戶端驗證目的\(EKU\)或應用程式原則延伸模組。 伺服器驗證的物件識別元為 1.3.6.1.5.5.7.3.1。 用戶端驗證的物件識別元是 1.3.6.1.5.5.7.3.2。
  
    - 憑證主體名稱應該解析為：
  
        - 網路控制站的電腦或 VM，如果在單一電腦或 VM 上部署網路控制站的 IP 位址。

        - 如果網路控制卡部署於多部電腦、 多個 Vm，或兩者，REST IP 位址。
  
    - 所有 REST 用戶端必須都信任此憑證。 憑證必須也被受信任軟體負載平衡 (SLB) 多工器 (MUX) 和 southbound 主機電腦受網路控制站。
  
    - 憑證註冊的憑證授權單位 (CA)，也可以是自我簽署的憑證。 自我簽署的憑證不建議用於生產環境部署，但可接受用於測試實驗室環境。
  
    - 網路控制站的所有節點都必須都提供相同的憑證。 在一個節點上建立憑證之後, 您可以 （含私密金鑰） 匯出的憑證，並匯入其他節點。

如需詳細資訊，請參閱[網路控制卡](Network-Controller.md)。
