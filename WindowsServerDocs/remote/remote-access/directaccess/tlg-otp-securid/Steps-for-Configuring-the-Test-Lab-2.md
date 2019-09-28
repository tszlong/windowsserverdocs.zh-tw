---
title: 設定測試實驗室的步驟
description: 本主題屬於測試實驗室指南-示範使用 OTP 驗證的 DirectAccess 和適用于 Windows Server 2016 的 RSA SecurID
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0a40183c-afd1-43ca-b306-05745640a37d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ef5ce37983b8565fab8287eeaae7423be0c269f6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404722"
---
# <a name="steps-for-configuring-the-test-lab"></a>設定測試實驗室的步驟

>適用於：Windows Server (半年度管道)、Windows Server 2016

下列步驟說明如何設定遠端存取基礎結構、設定遠端存取服務器和用戶端，以及從 Homenet 和網際網路子網測試 DirectAccess 連線能力。  
  
在此測試實驗室指南中，您將執行下列步驟，以建立使用 OTP 環境的遠端存取：  
  
-   [步驟 1：完成 DirectAccess 設定 @ no__t-0。 完成 @no__t 0Test 實驗室指南中的所有步驟：示範使用混合 IPv4 和 IPv6 @ no__t-0 的 DirectAccess 單一伺服器設定。  
  
-   [步驟 2：設定 APP1 @ no__t-0。 以 OTP 憑證範本設定 APP1，以供 EDGE1 使用。  
  
-   [步驟 3：設定 DC1 @ no__t-0。 確認 DC1 上定義的使用者主體名稱。  
  
-   [步驟 7：安裝和設定 RSA @ no__t-0。 安裝和設定 RSA、RADIUS 和 OTP 伺服器，並設定 OTP 的 EDGE1。  
  
-   [步驟 11：確認 EDGE1 @ no__t-0 的 OTP 健全狀況。 請確定遠端存取服務器上的 OTP 狀態為狀況良好。  
  
-   [步驟 8：從 Homenet 子網 @ no__t-0 測試 DirectAccess 連線能力。 從 NAT 裝置後方測試 DirectAccess OTP 功能。  
  
-   [步驟 10：測試來自網際網路 @ no__t-0 的 DirectAccess 連線能力。 測試來自網際網路的 DirectAccess 用戶端連線能力。  
  
-   [步驟 12：將設定快照集 @ no__t-0。 完成測試實驗室之後，請建立工作 DirectAccess 與 OTP 設定的快照，讓您稍後可以返回它來測試其他案例。  
  


