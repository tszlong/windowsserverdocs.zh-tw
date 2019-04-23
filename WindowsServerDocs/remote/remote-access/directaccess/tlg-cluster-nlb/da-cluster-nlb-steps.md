---
title: DirectAccess Cluster-NLB 測試實驗室案例設定步驟
description: 本主題是測試實驗室指南-示範在具備 Windows NLB 的 Windows Server 2016 的叢集中的 DirectAccess 的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e508d3ee-ffa6-463f-a3dd-9e35e745c005
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3a243debf79d2c9d12de511153b74f23c5a44cce
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880739"
---
# <a name="steps-for-configuring-the-directaccess-cluster-nlb-test-lab"></a>DirectAccess Cluster-NLB 測試實驗室案例設定步驟

>適用於：Windows Server （半年通道），Windows Server 2016

下列步驟說明如何設定遠端存取基礎結構、 設定遠端存取伺服器和用戶端，以及測試來自網際網路及家用網路的子網路的 DirectAccess 連線。  
  
在這個測試實驗室指南，您將會建立網路負載平衡 (NLB) 會啟用遠端存取叢集，藉由執行下列步驟：  
  
-   [步驟 1 完成 DirectAccess 設定](STEP-1-Complete-the-DirectAccess-Configuration.md)。 完成中的所有步驟[測試實驗室指南：示範 DirectAccess 單一伺服器安裝在混合的 IPv4 和 IPv6](https://go.microsoft.com/fwlink/p/?LinkId=237004)。  
  
-   [步驟 2:設定 EDGE1](STEP-2-Configure-EDGE1.md)。 在 EDGE1 上設定遠端存取角色，進行負載平衡。  
  
-   [步驟 3:安裝和設定 EDGE2](STEP-3-Install-and-Configure-EDGE2.md)。 EDGE2 做為第二部遠端存取伺服器中遠端存取叢集。  
  
-   [步驟 4:建立網路負載平衡的遠端存取叢集](STEP-4-Create-the-Network-Load-Balanced-Remote-Access-Cluster.md)-EDGE1 已做為遠端存取叢集的第一個伺服器。 EDGE2 已加入至叢集，NLB 設定叢集。  
  
-   [步驟 5:測試網際網路與透過叢集的 DirectAccess 連線](STEP-5-Test-DirectAccess-Connectivity-from-the-Internet-and-Through-the-Cluster.md)。 NLB 和叢集設定完成之後，您可以測試 DirectAccess 用戶端連線透過負載平衡叢集。  
  
-   [步驟 6:測試從 NAT 裝置後方的 DirectAccess 用戶端連線](STEP-6-Test-DirectAccess-Client-Connectivity-from-Behind-a-NAT-Device.md)。 移動來模擬測試的 DirectAccess 用戶端連線，從主要的路由器後方的 NAT 裝置後方的用戶端電腦。  
  
-   [步驟 7:測試連線時傳回至 Corpnet](STEP-7-Test-Connectivity-When-Returning-to-the-Corpnet.md)。 請確定，用戶端電腦仍然可以存取公司資源時回到公司網路。  
  
-   [步驟 8:建立設定的快照](da-cluster-nlb-s8-snapshot.md)。 完成測試實驗室之後，需要正常運作的遠端存取 NLB 叢集的快照集，以便您可以返回至該更新版本，以測試其他狀況。  
  


