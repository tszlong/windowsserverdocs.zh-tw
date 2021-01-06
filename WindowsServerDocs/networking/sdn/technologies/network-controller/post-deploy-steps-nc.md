---
title: 網路控制卡的部署後步驟
description: 本主題提供 Windows Server 2016 Datacenter 中網路控制站的非 Kerberos 部署的憑證設定指示。
manager: grcusanz
ms.topic: article
ms.assetid: eea0aca9-8d89-48fb-8068-fca40c90d34b
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/07/2020
ms.openlocfilehash: 7be8ccaf791155d1c0a8f07e4549a858bf4b82d8
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949384"
---
# <a name="post-deployment-steps-for-network-controller"></a>網路控制卡的部署後步驟

當您安裝網路控制站時，可以選擇 Kerberos 或非 Kerberos 部署。

針對非 \- Kerberos 部署，您必須設定憑證。

## <a name="configure-certificates-for-non-kerberos-deployments"></a>設定非 Kerberos 部署的憑證

如果 \( 網路控制站和管理用戶端的電腦或虛擬機器 vm \) 未加入網域 \- ，您必須 \- 完成下列步驟來設定憑證型驗證。

- 在網路控制站上建立憑證以進行電腦驗證。 憑證主體名稱必須與網路控制站電腦或 VM 的 DNS 名稱相同。

- 在管理用戶端上建立憑證。 此憑證必須受網路控制卡信任。

- 在網路控制站電腦或 VM 上註冊憑證。 憑證必須符合下列需求。

    -  伺服器驗證目的和用戶端驗證目的都必須在增強金鑰使用方法 \( EKU \) 或應用程式原則延伸中進行設定。 伺服器驗證的物件識別碼是1.3.6.1.5.5.7.3.1。 用戶端驗證的物件識別碼是1.3.6.1.5.5.7.3.2。

    - 憑證主體名稱應解析為：

        - 網路控制站電腦或 VM 的 IP 位址（如果網路控制站部署在單一電腦或 VM 上）。

        - 如果網路控制站部署在多部電腦上、多個 Vm，或兩者都部署，則為 REST IP 位址。

    - 所有 REST 用戶端都必須信任此憑證。 憑證也必須受到軟體負載平衡 (SLB) 多工器 (MUX) 以及由網路控制站管理的 southbound 主機電腦所信任。

    - 憑證可以由憑證授權單位單位)  (CA 註冊，也可以是自我簽署的憑證。 不建議在生產環境部署中使用自我簽署憑證，但可接受用於測試實驗室環境。

    - 您必須在所有網路控制卡節點上布建相同的憑證。 在一個節點上建立憑證之後，您可以將憑證 (匯出) 私密金鑰，然後將它匯入其他節點。

如需詳細資訊，請參閱[網路控制卡](Network-Controller.md)。
