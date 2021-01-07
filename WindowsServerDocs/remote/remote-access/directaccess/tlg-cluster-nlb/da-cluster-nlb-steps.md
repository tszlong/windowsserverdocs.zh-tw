---
title: DirectAccess Cluster-NLB 測試實驗室案例設定步驟
description: 本主題是測試實驗室指南的一部分-示範 Windows Server 2016 的 Windows NLB 叢集中的 DirectAccess
manager: brianlic
ms.topic: article
ms.assetid: e508d3ee-ffa6-463f-a3dd-9e35e745c005
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: d8502b50d54292adbd74209e5dff2d676d105701
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97950364"
---
# <a name="steps-for-configuring-the-directaccess-cluster-nlb-test-lab"></a>DirectAccess Cluster-NLB 測試實驗室案例設定步驟

>適用於：Windows Server (半年度管道)、Windows Server 2016

下列步驟說明如何設定遠端存取基礎結構、設定遠端存取服務器和用戶端，以及從網際網路和 Homenet 子網測試 DirectAccess 連線能力。

在此測試實驗室指南中，您將會執行下列步驟，以建立 (NLB) 啟用遠端存取叢集的網路負載平衡：

-   [步驟1：完成 DirectAccess](STEP-1-Complete-the-DirectAccess-Configuration.md)設定。 完成 [測試實驗室指南：使用混合的 IPv4 與 IPv6 示範 DirectAccess 單一伺服器安裝程式](https://go.microsoft.com/fwlink/p/?LinkId=237004)中的所有步驟。

-   [步驟2：設定 EDGE1](STEP-2-Configure-EDGE1.md)。 在 EDGE1 上設定遠端存取角色以進行負載平衡。

-   [步驟3：安裝和設定 EDGE2](STEP-3-Install-and-Configure-EDGE2.md)。 EDGE2 會作為遠端存取叢集中的第二部遠端存取服務器。

-   [步驟4：建立網路負載平衡的遠端存取](STEP-4-Create-the-Network-Load-Balanced-Remote-Access-Cluster.md)叢集-EDGE1 已設定為遠端存取叢集中的第一部伺服器。 EDGE2 已加入叢集，且已針對叢集設定 NLB。

-   [步驟5：從網際網路和透過叢集測試 DirectAccess 連線能力](STEP-5-Test-DirectAccess-Connectivity-from-the-Internet-and-Through-the-Cluster.md)。 NLB 和叢集設定完成之後，您可以透過負載平衡叢集來測試 DirectAccess 用戶端連線能力。

-   [步驟6：從 NAT 裝置後方測試 DirectAccess 用戶端連線能力](STEP-6-Test-DirectAccess-Client-Connectivity-from-Behind-a-NAT-Device.md)。 將用戶端電腦移至 NAT 裝置後方，以模擬從家用路由器後方測試 DirectAccess 用戶端連線能力。

-   [步驟7：返回公司網路時測試連線能力](STEP-7-Test-Connectivity-When-Returning-to-the-Corpnet.md)。 返回公司網路時，請確定用戶端電腦仍然可以存取公司資源。

-   [步驟8：設定設定的快照](da-cluster-nlb-s8-snapshot.md)。 完成測試實驗室之後，請建立工作遠端存取 NLB 叢集的快照，讓您稍後可以返回以測試其他案例。



