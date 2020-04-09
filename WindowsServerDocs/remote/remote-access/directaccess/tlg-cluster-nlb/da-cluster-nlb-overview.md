---
title: DirectAccess Cluster-NLB 測試實驗室案例概觀
description: 本主題是測試實驗室指南的一部分-示範 windows Server 2016 的 Windows NLB 叢集中的 DirectAccess
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: cd1e9efd-19e9-49e7-8432-881f661c9792
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 7394562ce7a5c08a81fb3c243fb8671d281d8370
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80819041"
---
# <a name="overview-of-the-directaccess-cluster-nlb-test-lab-scenario"></a>DirectAccess Cluster-NLB 測試實驗室案例概觀

>適用於：Windows Server (半年通道)、Windows Server 2016

在此測試實驗室案例中，會使用下列內容來部署 DirectAccess：  
  
-   **DC1**-設定為網域控制站、網域名稱系統（DNS）伺服器和動態主機設定通訊協定（DHCP）伺服器的伺服器。  
  
-   **EDGE1**-內部網路上的伺服器，設定為遠端存取服務器叢集中的第一部遠端存取服務器。 這部伺服器有兩張網路介面卡;一個連線到內部網路，另一個連線到外部網路。  
  
-   **EDGE2**-內部網路上的伺服器，設定為遠端存取服務器叢集中的第二部遠端存取服務器。 這部伺服器有兩張網路介面卡;一個連線到內部網路，另一個連線到外部網路。  
  
-   **APP1**-內部網路上的伺服器，設定為 web 和檔案伺服器，並做為企業根憑證授權單位（CA）  
  
-   **APP2**-內部網路上的電腦，設定為僅限 IPv4 的 web 和檔案伺服器。 這部電腦是用來反白顯示 NAT64/DNS64 功能。  
  
-   **INET1**-設定為網際網路 DNS 和 DHCP 伺服器的伺服器。  
  
-   **NAT1**-設定為使用網際網路連線共用的網路位址轉譯器（NAT）裝置的用戶端電腦。  
  
-   **CLIENT1**-設定為 directaccess 用戶端的用戶端電腦，在內部網路、模擬的網際網路和家用網路之間移動時，將會用來測試 directaccess 連線能力。  
  
測試實驗室包含三個子網，可模擬下列各項：  
  
-   一個由 NAT 連線到網際網路的家用網路，名為 Homenet （192.168.137.0/24）。  
  
-   網際網路子網（131.107.0.0/24）所代表的外部網路。  
  
-   遠端存取服務器與網際網路分隔的內部網路，名為公司網路（10.0.0.0/24; 2001： db8：1：/64）。  
  
每個子網上的電腦都會使用實體或虛擬中樞或交換器來連線，如下圖所示。  
  
![測試實驗室概觀](../../../media/Overview-of-the-Test-Lab-Scenario_5/TLG_DA_Cluster.png)  
  


