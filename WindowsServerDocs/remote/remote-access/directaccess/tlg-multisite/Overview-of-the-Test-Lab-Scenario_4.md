---
title: 測試實驗室案例概觀
description: 本主題是測試實驗室指南的一部分-示範適用于 Windows Server 2016 的 DirectAccess 多網站部署
manager: brianlic
ms.topic: article
ms.assetid: 9afeced4-1a9b-4cb3-9fc4-d7e44c675755
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 833221ee118c758ffc2ade7419ee75cf82bf9f35
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97950344"
---
# <a name="overview-of-the-test-lab-scenario"></a>測試實驗室案例概觀

>適用於：Windows Server (半年度管道)、Windows Server 2016

在此測試實驗室案例中，DirectAccess 的部署方式如下：

-   **DC1**-設定為 corp.contoso.com 網域的網域控制站、DNS 伺服器和 DHCP 伺服器的伺服器。

-   **2-DC1**-設定為 corp2.corp.contoso.com 網域的網域控制站和 DNS 伺服器的伺服器。

-   **EDGE1 和 2-EDGE1**-內部網路上設定為遠端存取服務器的兩部伺服器。 每部伺服器都有兩張網路介面卡;一個連接到內部網路，另一個連接到外部網路。

-   **APP1 和 2-APP1**-內部網路上設定為 web 和檔案伺服器的兩部伺服器。

-   **APP2**-內部網路上設定為僅限 IPv4 web 和檔案伺服器的電腦。 此電腦是用來反白顯示 NAT64/DNS64 功能。

-   **ROUTER1**-設定為在兩個公司內部網路之間提供路由的伺服器。

-   **INET1**-設定為網際網路 DNS 和 DHCP 伺服器的伺服器。

-   **NAT1**-使用網際網路連線共用 (NAT) 裝置設定為網路位址轉譯器的用戶端電腦。

-   **CLIENT1 和 CLIENT2**-兩部設定為 directaccess 用戶端的用戶端電腦，在內部網路、模擬的網際網路和家用網路之間移動時，將用來測試 directaccess 連線能力。 **CLIENT2** 是 Windows 7 &reg;  用戶端。

測試實驗室包含四個子網，可模擬下列各項：

-   名為 Homenet (192.168.137.0/24 的家用網路) 由 NAT 連接到網際網路。

-   網際網路子網代表的外部網路 (131.107.0.0/24) 。

-   名為公司網路的內部網路 (10.0.0.0/24;2001： db8：1：：/64) 由 EDGE1 遠端存取服務器與網際網路隔開。

-   名為 2-Corpnet1 (10.2.0.0/24 的內部網路;2001： db8：2：：/64) 由 2 EDGE1 遠端存取服務器與網際網路隔開。

每個子網上的電腦都會使用實體或虛擬中樞或交換器進行連線，如下圖所示。

![測試實驗室概觀](../../../media/Overview-of-the-Test-Lab-Scenario_4/TLG_DA_Multisite.png)



