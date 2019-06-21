---
title: 設定測試實驗室的步驟
description: 本主題是一部分的 「 測試實驗室指南： 示範 DirectAccess 多站台部署的 Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc7205b4-a822-4038-ab67-ec0a870737f2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a3d01dd8002e28fb127ac6b1b4cea25c58953521
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281398"
---
# <a name="steps-for-configuring-the-test-lab"></a>設定測試實驗室的步驟

>適用於：Windows Server （半年通道），Windows Server 2016

下列步驟說明如何設定遠端存取基礎結構、 設定遠端存取伺服器和用戶端和測試來自網際網路及家用網路的子網路的 DirectAccess 連線。  
  
在這個測試實驗室指南中，您將建置多站台的 「 遠端存取 」 部署，執行下列步驟：  
  
-   [步驟 1：完成基本設定](assetId:///9eb4a9ba-9118-4ea3-8963-e643ec81c3ed)。 完成中的所有步驟[測試實驗室指南：示範 DirectAccess 單一伺服器安裝在混合的 IPv4 和 IPv6](https://go.microsoft.com/fwlink/p/?LinkId=237004)。  
  
-   [步驟 2：安裝和設定 「 路由器 1 」](assetId:///e4b1a298-d5b0-410e-970b-c5358a9378f9)。 「 路由器 1 」 會提供路由和轉送的公司網路和 2 Corpnet 子網路之間的功能。  
  
-   [步驟 3：安裝和設定 CLIENT2](assetId:///6cbee1b5-f6f6-443f-8fa9-31cc5c05a0ee)。 CLIENT2 是用來示範 Windows 7 用戶端電腦回溯相容性的 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 遠端存取部署。  
  
-   [步驟 4：設定 APP1](assetId:///a0ee655e-c01e-4bf3-a7b3-064e9614f810)。 將 APP1 設定與 「 路由器 1 」 為預設閘道和 2-DC1 做為替代的 DNS 伺服器。  
  
-   [步驟 5：設定 DC1](assetId:///205ca795-93ce-4e53-aa6b-b44c87f0e14a)。 設定 DC1 與其他 Active Directory 站台和 Windows 7 用戶端電腦的其他安全性群組。  
  
-   [步驟 6：安裝和設定 2-DC1](assetId:///16752f61-edbf-4ff4-9d7a-e2077b66a127)。 在多站台部署中，您會有兩個或多個網域及網站。 2-DC1 提供網域控制站和 corp2.corp.contoso.com 網域的 DNS 服務。  
  
-   [步驟 7：安裝和設定 2-APP1](assetId:///7d04b54e-590a-4d33-9766-415789859f29)。 2-APP1 是 2 公司網路中的 web 和檔案伺服器。  
  
-   [步驟 8：設定 INET1](assetId:///8ecc0b63-8626-4939-8d26-3d51d051d231)。 INET1 模擬網際網路在這個測試實驗室指南中。 您必須設定 2 EDGE1 的公用 IP 位址解析的 DNS 項目。  
  
-   [步驟 9：設定 EDGE1](assetId:///562744dc-30f6-42fa-bd5f-60a013b2179e)。 設定 2 公司網路的 DNS 伺服器和 EDGE1 上的路由。  
  
-   [步驟 10：安裝和設定 2 EDGE1](assetId:///1938c4f3-ca96-475d-9f2e-6bea3b7a4130)。 站台部署中需要兩部遠端存取伺服器。 2-EDGE1 提供第二個網域的遠端存取服務。  
  
-   [步驟 11：設定多站台部署](assetId:///537e4b68-043f-49c9-94d8-15ce8c4b18e2)。 設定兩部遠端存取伺服器之後, 您可以設定多站台部署。  
  
-   [步驟 12：測試 DirectAccess 連線](assetId:///aa293b5d-4b6f-4004-95f3-0ab54804b15c)。 測試從兩個來自用戶端電腦透過 EDGE1 和 2 EDGE1 網際網路子網路的 DirectAccess 連線。  
  
-   [步驟 13：測試從 NAT 裝置後方的 DirectAccess 連線](assetId:///41f8195b-00a1-4991-9db8-3703514dbe0c)。 測試從 NAT 裝置後方的 DirectAccess 連線。  
  
-   [步驟 14：建立設定的快照](assetId:///7b56d5c9-c334-463e-9e29-d652ca110d84)。 完成測試實驗室之後，快照集使用遠端存取多站台部署的因此您可以返回至該更新版本，以測試其他狀況。  
  


