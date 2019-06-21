---
title: DirectAccess Cluster-NLB 測試實驗室案例概觀
description: 本主題是測試實驗室指南-示範在具備 Windows NLB 的 Windows Server 2016 的叢集中的 DirectAccess 的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cd1e9efd-19e9-49e7-8432-881f661c9792
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a6d82713dfb12e6775402d29bfcebaa0ec8b066c
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281548"
---
# <a name="overview-of-the-directaccess-cluster-nlb-test-lab-scenario"></a>DirectAccess Cluster-NLB 測試實驗室案例概觀

>適用於：Windows Server （半年通道），Windows Server 2016

在這個測試實驗室案例中，DirectAccess 的部署具有：  
  
-   **DC1**-伺服器設定為網域控制站、 網域名稱系統 (DNS) 伺服器和動態主機設定通訊協定 (DHCP) 伺服器。  
  
-   **EDGE1**-內部網路設定為第一部遠端存取伺服器的遠端存取伺服器叢集中的伺服器。 此伺服器有兩個網路介面卡;一個連線到內部網路，和其他連線到外部網路。  
  
-   **EDGE2**-內部網路設定為第二部遠端存取伺服器的遠端存取伺服器叢集中的伺服器。 此伺服器有兩個網路介面卡;一個連線到內部網路，和其他連線到外部網路。  
  
-   **APP1**-會設定為 web 和檔案伺服器，並為企業根憑證授權單位 (CA) 的內部網路伺服器  
  
-   **APP2**-設定為 IPv4 唯一 web 和檔案伺服器的內部網路上電腦。 此電腦用來反白顯示的 nat64/dns64 功能。  
  
-   **INET1**-設定為網際網路 DNS 和 DHCP 伺服器的伺服器。  
  
-   **NAT1**-用戶端電腦設定為網路位址轉譯器 (NAT) 裝置使用網際網路連線共用。  
  
-   **CLIENT1**-設定為 DirectAccess 用戶端將用來測試內部網路，模擬的網際網路、 家用網路之間移動時的 DirectAccess 連線的用戶端電腦。  
  
測試實驗室包含模擬下列的三個子網路：  
  
-   家用網路，名為家用網路 (192.168.137.0/24) 連線到網際網路的 NAT  
  
-   網際網路子網路 (131.107.0.0/24) 所代表的外部網路。  
  
-   內部網路 corpnet (10.0.0.0/24; 2001:db8:1:: / 64) 與網際網路相分隔遠端存取伺服器。  
  
每個子網路上的電腦連線使用實體或虛擬的集線器或交換器，如下圖所示。  
  
![測試實驗室概觀](../../../media/Overview-of-the-Test-Lab-Scenario_5/TLG_DA_Cluster.png)  
  


