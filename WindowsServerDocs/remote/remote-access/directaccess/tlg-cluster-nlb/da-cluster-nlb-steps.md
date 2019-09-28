---
title: DirectAccess Cluster-NLB 測試實驗室案例設定步驟
description: 本主題是測試實驗室指南的一部分-示範 windows Server 2016 的 Windows NLB 叢集中的 DirectAccess
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e508d3ee-ffa6-463f-a3dd-9e35e745c005
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 108c63298ad3382f5ece790258f2d278bb03b78b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388399"
---
# <a name="steps-for-configuring-the-directaccess-cluster-nlb-test-lab"></a>DirectAccess Cluster-NLB 測試實驗室案例設定步驟

>適用於：Windows Server (半年度管道)、Windows Server 2016

下列步驟說明如何設定遠端存取基礎結構、設定遠端存取服務器和用戶端，以及從網際網路和 Homenet 子網測試 DirectAccess 連線能力。  
  
在此測試實驗室指南中，您將執行下列步驟，建立已啟用網路負載平衡（NLB）的遠端存取叢集：  
  
-   [步驟1完成 DirectAccess](STEP-1-Complete-the-DirectAccess-Configuration.md)設定。 完成 @no__t 0Test 實驗室指南中的所有步驟：示範使用混合 IPv4 和 IPv6 @ no__t-0 的 DirectAccess 單一伺服器設定。  
  
-   [步驟 2：設定 EDGE1 @ no__t-0。 在 EDGE1 上設定遠端存取角色，以進行負載平衡。  
  
-   [步驟 3：安裝和設定 EDGE2 @ no__t-0。 EDGE2 會作為遠端存取叢集中的第二部遠端存取服務器。  
  
-   [步驟 4：建立網路負載平衡遠端存取叢集 @ no__t-0-EDGE1 已設定為遠端存取叢集中的第一部伺服器。 EDGE2 已聯結至叢集，並已針對叢集設定 NLB。  
  
-   [步驟 5：測試來自網際網路和透過叢集 @ no__t-0 的 DirectAccess 連線能力。 在 NLB 和叢集設定完成之後，您可以透過負載平衡叢集測試 DirectAccess 用戶端連線能力。  
  
-   [步驟 6：測試來自 NAT 裝置 @ no__t-0 後方的 DirectAccess 用戶端連線能力。 將用戶端電腦移到 NAT 裝置後方，以模擬從家用路由器後方進行 DirectAccess 用戶端連線的測試。  
  
-   [步驟 7：返回公司網路 @ no__t-0 時，測試連線能力。 請確定用戶端電腦在返回公司網路時仍然可以存取公司資源。  
  
-   [步驟 8：將設定快照集 @ no__t-0。 完成測試實驗室之後，請建立工作遠端存取 NLB 叢集的快照，讓您可以稍後再返回以測試其他案例。  
  


