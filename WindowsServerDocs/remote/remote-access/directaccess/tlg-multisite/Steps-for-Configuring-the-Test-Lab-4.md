---
title: 設定測試實驗室的步驟
description: 本主題屬於測試實驗室指南-示範適用于 Windows Server 2016 的 DirectAccess 多網站部署
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc7205b4-a822-4038-ab67-ec0a870737f2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dd8b8864dff98e51bf55aad9307523df4a0c30bf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404694"
---
# <a name="steps-for-configuring-the-test-lab"></a>設定測試實驗室的步驟

>適用於：Windows Server (半年度管道)、Windows Server 2016

下列步驟說明如何設定遠端存取基礎結構、設定遠端存取服務器和用戶端，以及從網際網路和 Homenet 子網測試 DirectAccess 連線能力。  
  
在此測試實驗室指南中，您將執行下列步驟來建立多網站遠端存取部署：  
  
-   [步驟 1：完成基本設定 @ no__t-0。 完成 @no__t 0Test 實驗室指南中的所有步驟：示範使用混合 IPv4 和 IPv6 @ no__t-0 的 DirectAccess 單一伺服器設定。  
  
-   [步驟 2：安裝和設定 ROUTER1 @ no__t-0。 ROUTER1 提供公司網路與2公司網路子網之間的路由和轉送功能。  
  
-   [步驟 3：安裝和設定 CLIENT2 @ no__t-0。 CLIENT2 是一部 Windows 7 用戶端電腦，用來示範 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 遠端存取部署的回溯相容性。  
  
-   [步驟 4：設定 APP1 @ no__t-0。 以 ROUTER1 做為預設閘道，並將 APP1 設定為替代 DNS 伺服器。  
  
-   [步驟 5：設定 DC1 @ no__t-0。 使用額外的 Active Directory 網站和 Windows 7 用戶端電腦的其他安全性群組來設定 DC1。  
  
-   [步驟 6：安裝和設定 2-DC1 @ no__t-0。 在多網站部署中，您有兩個或多個網域和網站。 2-DC1 提供 corp2.corp.contoso.com 網域的網域控制站和 DNS 服務。  
  
-   [步驟 7：安裝和設定 2-APP1 @ no__t-0。 2-APP1 是 2-公司網路中的 web 和檔案伺服器。  
  
-   [步驟 8：設定 INET1 @ no__t-0。 INET1 會在此測試實驗室指南中模擬網際網路。 您必須設定 DNS 專案，以解析為 EDGE1 的公用 IP 位址。  
  
-   [步驟 9：設定 EDGE1 @ no__t-0。 在 EDGE1 上設定2公司網路的 DNS 伺服器和路由。  
  
-   [步驟 10：安裝和設定 2-EDGE1 @ no__t-0。 多網站部署中需要兩部遠端存取服務器。 2-EDGE1 提供第二個網域的遠端存取服務。  
  
-   [步驟 11：設定多網站部署 @ no__t-0。 設定這兩個遠端存取服務器之後，您可以設定您的多網站部署。  
  
-   [步驟 12：測試 DirectAccess 連線能力 @ no__t-0。 透過 EDGE1 和 2-EDGE1，從網際網路子網的兩部用戶端電腦測試 DirectAccess 連線能力。  
  
-   [步驟 13：測試 NAT 裝置後方的 DirectAccess 連線能力 @ no__t-0。 從 NAT 裝置後方測試 DirectAccess 連線能力。  
  
-   [步驟 14：將設定快照集 @ no__t-0。 完成測試實驗室之後，請建立工作遠端存取多網站部署的快照，讓您稍後可以返回以測試其他案例。  
  


