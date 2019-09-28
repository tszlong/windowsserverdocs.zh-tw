---
title: 測試實驗室案例概觀
description: 本主題屬於測試實驗室指南-示範適用于 Windows Server 2016 的 DirectAccess 多網站部署
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9afeced4-1a9b-4cb3-9fc4-d7e44c675755
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bcee25c4a13afc2b41d6b1a9a43a1489054ef43d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388415"
---
# <a name="overview-of-the-test-lab-scenario"></a>測試實驗室案例概觀

>適用於：Windows Server (半年度管道)、Windows Server 2016

在此測試實驗室案例中，會使用下列內容來部署 DirectAccess：  
  
-   **DC1**-設定為 corp.contoso.com 網域的網域控制站、DNS 伺服器和 DHCP 伺服器的伺服器。  
  
-   **2-DC1**-設定為 corp2.corp.contoso.com 網域的網域控制站和 DNS 伺服器的伺服器。  
  
-   **EDGE1 和 2-EDGE1**-內部網路上設定為遠端存取服務器的兩部伺服器。 每部伺服器都有兩張網路介面卡;一個連線到內部網路，另一個連線到外部網路。  
  
-   **APP1 和 2-APP1**-內部網路上設定為 web 和檔案伺服器的兩部伺服器。  
  
-   **APP2**-內部網路上的電腦，設定為僅限 IPv4 的 web 和檔案伺服器。 這部電腦是用來反白顯示 NAT64/DNS64 功能。  
  
-   **ROUTER1**-設定為提供兩個公司內部網路之間路由的伺服器。  
  
-   **INET1**-設定為網際網路 DNS 和 DHCP 伺服器的伺服器。  
  
-   **NAT1**-設定為使用網際網路連線共用的網路位址轉譯器（NAT）裝置的用戶端電腦。  
  
-   **CLIENT1 和 CLIENT2**-設定為 directaccess 用戶端的兩部用戶端電腦，在內部網路、模擬的網際網路和家用網路之間移動時，將會用來測試 directaccess 連線能力。 **CLIENT2**是 Windows 7 @ no__t-1 用戶端。  
  
測試實驗室包含四個子網，可模擬下列各項：  
  
-   一個由 NAT 連線到網際網路的家用網路，名為 Homenet （192.168.137.0/24）。  
  
-   網際網路子網（131.107.0.0/24）所代表的外部網路。  
  
-   EDGE1 遠端存取服務器與網際網路分隔的內部網路，名為公司網路（10.0.0.0/24; 2001： db8：1：/64）。  
  
-   2-EDGE1 遠端存取服務器，名為 2-Corpnet1 （10.2.0.0/24; 2001： db8：2：：/64）的內部網路與網際網路隔開。  
  
每個子網上的電腦都會使用實體或虛擬中樞或交換器來連線，如下圖所示。  
  
![測試實驗室概觀](../../../media/Overview-of-the-Test-Lab-Scenario_4/TLG_DA_Multisite.png)  
  


