---
title: 網路控制卡的部署後步驟
description: 本主題提供 Windows Server 2016 Datacenter 中網路控制站非 Kerberos 部署的憑證設定指示。
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: eea0aca9-8d89-48fb-8068-fca40c90d34b
ms.author: anpaul
author: AnirbanPaul
ms.openlocfilehash: bb43de904be7a811083e71edce9421a236f20a27
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859631"
---
# <a name="post-deployment-steps-for-network-controller"></a>網路控制卡的部署後步驟

當您安裝網路控制卡時，可以選擇 [Kerberos] 或 [非 Kerberos] 部署。

若為非\-Kerberos 部署，您必須設定憑證。

## <a name="configure-certificates-for-non-kerberos-deployments"></a>設定非 Kerberos 部署的憑證

如果電腦或虛擬機器 \(網路控制站\) 的 Vm，而且管理用戶端不是加入網域\-，您必須完成下列步驟，以設定憑證\-型驗證。

- 在網路控制卡上建立用於電腦驗證的憑證。 憑證主體名稱必須與網路控制站電腦或 VM 的 DNS 名稱相同。

- 在管理用戶端上建立憑證。 此憑證必須受到網路控制卡的信任。
  
- 在網路控制站電腦或 VM 上註冊憑證。 憑證必須符合下列需求。
  
    -  [伺服器驗證] 目的和 [用戶端驗證] 目的都必須在 [增強金鑰使用方法] 中設定 \(EKU\) 或應用程式原則延伸。 伺服器驗證的物件識別碼是1.3.6.1.5.5.7.3.1。 用戶端驗證的物件識別碼是1.3.6.1.5.5.7.3.2。
  
    - 憑證主體名稱應解析為：
  
        - 網路控制站電腦或 VM 的 IP 位址（如果網路控制站部署在單一電腦或 VM 上）。

        - 如果網路控制站部署在多部電腦、多個 Vm 或兩者上，則會有 REST IP 位址。
  
    - 此憑證必須受到所有 REST 用戶端的信任。 憑證也必須受到軟體負載平衡（SLB）多工器（MUX）和網路控制卡所管理之 southbound 主機電腦的信任。
  
    - 憑證可以由憑證授權單位單位（CA）註冊，也可以是自我簽署憑證。 不建議在生產環境部署中使用自我簽署憑證，但可接受測試實驗室環境。
  
    - 您必須在所有網路控制站節點上布建相同的憑證。 在一個節點上建立憑證之後，您可以匯出憑證（使用私密金鑰），並將它匯入其他節點。

如需詳細資訊，請參閱[網路控制卡](Network-Controller.md)。
