---
title: DirectAccess Cluster-NLB 測試實驗室案例概觀
description: 深入瞭解 DirectAccess 叢集-NLB 測試實驗室案例，以及它所包含的三個子網。
manager: brianlic
ms.topic: article
ms.assetid: cd1e9efd-19e9-49e7-8432-881f661c9792
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 7c4b0aac8c5d16826a5e173fb28424333887f6be
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98040268"
---
# <a name="overview-of-the-directaccess-cluster-nlb-test-lab-scenario"></a>DirectAccess Cluster-NLB 測試實驗室案例概觀

>適用於：Windows Server (半年度管道)、Windows Server 2016

在此測試實驗室案例中，DirectAccess 的部署方式如下：

-   **DC1**-設定為網域控制站、網域名稱系統 (DNS) server 的伺服器，以及 (DHCP) Server 的動態主機設定通訊協定。

-   **EDGE1**-內部網路上設定為遠端存取服務器叢集中第一部遠端存取服務器的伺服器。 這部伺服器有兩張網路介面卡;一個連接到內部網路，另一個連接到外部網路。

-   **EDGE2**-內部網路上設定為遠端存取服務器叢集中第二部遠端存取服務器的伺服器。 這部伺服器有兩張網路介面卡;一個連接到內部網路，另一個連接到外部網路。

-   **APP1**-內部網路上設定為 web 和檔案伺服器的伺服器，以及做為企業根憑證授權單位 (CA) 

-   **APP2**-內部網路上設定為僅限 IPv4 web 和檔案伺服器的電腦。 此電腦是用來反白顯示 NAT64/DNS64 功能。

-   **INET1**-設定為網際網路 DNS 和 DHCP 伺服器的伺服器。

-   **NAT1**-使用網際網路連線共用 (NAT) 裝置設定為網路位址轉譯器的用戶端電腦。

-   **CLIENT1**-設定為 directaccess 用戶端的用戶端電腦，在內部網路、模擬的網際網路和家用網路之間移動時，將會用來測試 directaccess 連線能力。

測試實驗室包含三個子網，可模擬下列各項：

-   名為 Homenet (192.168.137.0/24 的家用網路) 由 NAT 連接到網際網路。

-   網際網路子網代表的外部網路 (131.107.0.0/24) 。

-   名為公司網路的內部網路 (10.0.0.0/24;2001： db8：1：：/64) 由遠端存取服務器與網際網路隔開。

每個子網上的電腦都會使用實體或虛擬中樞或交換器進行連線，如下圖所示。

![測試實驗室概觀](../../../media/Overview-of-the-Test-Lab-Scenario_5/TLG_DA_Cluster.png)



