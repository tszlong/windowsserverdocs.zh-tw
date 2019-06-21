---
title: 測試實驗室案例概觀
description: 本主題是一部分的 「 測試實驗室指南： 示範 DirectAccess 多站台部署的 Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9afeced4-1a9b-4cb3-9fc4-d7e44c675755
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b067d5f247f5dc13ea294d83a76f267c5a57ffe7
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283292"
---
# <a name="overview-of-the-test-lab-scenario"></a>測試實驗室案例概觀

>適用於：Windows Server （半年通道），Windows Server 2016

在這個測試實驗室案例中，DirectAccess 的部署具有：  
  
-   **DC1**-伺服器設定為網域控制站、 DNS 伺服器和 corp.contoso.com 網域的 DHCP 伺服器。  
  
-   **2-DC1**-伺服器設定為網域控制站和 corp2.corp.contoso.com 網域的 DNS 伺服器。  
  
-   **EDGE1 和 2 EDGE1**-內部網路上設定為遠端存取伺服器的兩部伺服器。 每一部伺服器有兩個網路介面卡;一個連線到內部網路，和其他連線到外部網路。  
  
-   **APP1 和 2-APP1**-內部網路上設定為 web 和檔案伺服器的兩部伺服器。  
  
-   **APP2**-設定為 IPv4 唯一 web 和檔案伺服器的內部網路上電腦。 此電腦用來反白顯示的 nat64/dns64 功能。  
  
-   **「 路由器 1 」** -伺服器設定為提供兩個公司內部網路之間的路由。  
  
-   **INET1**-設定為網際網路 DNS 和 DHCP 伺服器的伺服器。  
  
-   **NAT1**-用戶端電腦設定為網路位址轉譯器 (NAT) 裝置使用網際網路連線共用。  
  
-   **CLIENT1 和 CLIENT2**-會設定為將用來測試內部網路，模擬的網際網路、 家用網路之間移動時的 DirectAccess 連線的 DirectAccess 用戶端的兩個用戶端電腦。 **CLIENT2**是 Windows 7&reg;用戶端。  
  
測試實驗室包含模擬下列 4 個子網路：  
  
-   家用網路，名為家用網路 (192.168.137.0/24) 連線到網際網路的 NAT  
  
-   網際網路子網路 (131.107.0.0/24) 所代表的外部網路。  
  
-   內部網路 corpnet (10.0.0.0/24; 2001:db8:1:: / 64) 與網際網路相分隔 EDGE1 遠端存取伺服器。  
  
-   內部網路，名為 「 2 Corpnet1 (10.2.0.0/24; 2001:db8:2:: / 64) 與網際網路相分隔 2 EDGE1 遠端存取伺服器。  
  
每個子網路上的電腦連線使用實體或虛擬的集線器或交換器，如下圖所示。  
  
![測試實驗室概觀](../../../media/Overview-of-the-Test-Lab-Scenario_4/TLG_DA_Multisite.png)  
  


