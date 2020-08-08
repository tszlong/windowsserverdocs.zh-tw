---
title: 網路控制卡的部署後步驟
description: 本主題提供 Windows Server 2016 Datacenter 中網路控制站非 Kerberos 部署的憑證設定指示。
manager: grcusanz
ms.topic: article
ms.assetid: eea0aca9-8d89-48fb-8068-fca40c90d34b
ms.author: anpaul
author: AnirbanPaul
ms.openlocfilehash: adb488e97693b42435cc0c23e48f22de7966b517
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87952451"
---
# <a name="post-deployment-steps-for-network-controller"></a>網路控制卡的部署後步驟

當您安裝網路控制卡時，可以選擇 [Kerberos] 或 [非 Kerberos] 部署。

針對非 \- Kerberos 部署，您必須設定憑證。

## <a name="configure-certificates-for-non-kerberos-deployments"></a>設定非 Kerberos 部署的憑證

如果網路控制站的電腦或虛擬機器 \( vm \) 與管理用戶端未加入網域 \- ，您必須 \- 完成下列步驟來設定以憑證為基礎的驗證。

- 在網路控制卡上建立用於電腦驗證的憑證。 憑證主體名稱必須與網路控制站電腦或 VM 的 DNS 名稱相同。

- 在管理用戶端上建立憑證。 此憑證必須受到網路控制卡的信任。

- 在網路控制站電腦或 VM 上註冊憑證。 憑證必須符合下列需求。

    -  [伺服器驗證] 目的和 [用戶端驗證] 目的都必須在 [增強金鑰使用方法 \( EKU] \) 或 [應用程式原則延伸] 中設定。 伺服器驗證的物件識別碼是1.3.6.1.5.5.7.3.1。 用戶端驗證的物件識別碼是1.3.6.1.5.5.7.3.2。

    - 憑證主體名稱應解析為：

        - 網路控制站電腦或 VM 的 IP 位址（如果網路控制站部署在單一電腦或 VM 上）。

        - 如果網路控制站部署在多部電腦、多個 Vm 或兩者上，則會有 REST IP 位址。

    - 此憑證必須受到所有 REST 用戶端的信任。 憑證也必須受到軟體負載平衡的信任 (SLB) 多工器 (MUX) 和網路控制站所管理的 southbound 主機電腦。

    - 憑證可以由憑證授權單位單位 (CA) 註冊，也可以是自我簽署憑證。 不建議在生產環境部署中使用自我簽署憑證，但可接受測試實驗室環境。

    - 您必須在所有網路控制站節點上布建相同的憑證。 在一個節點上建立憑證之後，您可以匯出具有私密金鑰的憑證 () 並將它匯入其他節點。

如需詳細資訊，請參閱[網路控制卡](Network-Controller.md)。
