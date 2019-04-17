---
title: 後 Post-Deployment 步驟 Network Controller
description: 本主題提供適用於在 Windows Server 2016 Datacenter Network Controller 的非 Kerberos 部署憑證設定指示操作。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: eea0aca9-8d89-48fb-8068-fca40c90d34b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f7d6bbd50537e24f392eabde7d103c91a4f07c90
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="post-deployment-steps-for-network-controller"></a>後 Post-Deployment 步驟 Network Controller

當您安裝網路控制器時，因此您可以選擇 Kerberos 或非 Kerberos 部署。

針對 non\-Kerberos 部署，您必須設定的憑證。

## <a name="configure-certificates-for-non-kerberos-deployments"></a>設定適用於非 Kerberos 部署憑證

如果 Network Controller and 管理 client 的電腦或虛擬機器 \(VMs\) 不 domain\ 加入，您必須完成下列步驟來設定 certificate\ 為基礎的驗證。

- 建立驗證電腦網路控制器上的憑證。 憑證主體名稱必須是相同的網路控制器電腦或 VM 的 DNS 名稱。

- 在 [管理 client 建立憑證。 此憑證的必須信任網路控制器。
  
- 註冊 Network Controller 的電腦上 VM 的憑證。 憑證必須符合下列需求。
  
    -  必須設定伺服器的驗證目的和 Client 驗證目的增強金鑰使用方法 \(EKU\) 或應用程式原則擴充功能。 伺服器驗證的物件識別碼是 1.3.6.1.5.5.7.3.1。 Client 驗證的物件識別碼是 1.3.6.1.5.5.7.3.2。
  
    - 憑證主體名稱解析應：
  
        - Network Controller 的電腦或 VM 部電腦上 VM 部署 Network Controller 的 IP 位址。

        - 如果在多部電腦，多個 Vm 中，或兩者部署 Network Controller 的其餘 IP 位址。
  
    - 這個憑證必須信任用所有其他部分。 也必須軟體負載平衡 (SLB) 多工器 (MUX) 由 Network Controller southbound 主機電腦的受信任的憑證。
  
    - 憑證可以將退出的憑證授權單位或可能會自動簽署的憑證。 自動簽署的憑證會建議您不要 production 部署，但接受實驗室測試環境中。
  
    - 必須所有網路控制器節點上提供相同的憑證。 一個節點上建立的憑證之後, 您可以憑證匯出（的私密金鑰）並將它匯入其他節點。

如需詳細資訊，請查看[Network Controller](Network-Controller.md)。
