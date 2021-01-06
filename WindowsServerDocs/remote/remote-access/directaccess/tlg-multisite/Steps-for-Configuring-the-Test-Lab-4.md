---
title: 設定測試實驗室的步驟
description: 本主題是測試實驗室指南的一部分-示範適用于 Windows Server 2016 的 DirectAccess 多網站部署
manager: brianlic
ms.topic: article
ms.assetid: dc7205b4-a822-4038-ab67-ec0a870737f2
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 54241bc7cb593fda1f4bfd1f60a20eb0b03694ec
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947944"
---
# <a name="steps-for-configuring-the-test-lab"></a>設定測試實驗室的步驟

>適用於：Windows Server (半年度管道)、Windows Server 2016

下列步驟說明如何設定遠端存取基礎結構、設定遠端存取服務器和用戶端，以及從網際網路和 Homenet 子網測試 DirectAccess 連線能力。

在此測試實驗室指南中，您將會執行下列步驟來建立多個遠端存取部署：

-   [步驟1：完成基本](assetId:///9eb4a9ba-9118-4ea3-8963-e643ec81c3ed)設定。 完成 [測試實驗室指南：使用混合的 IPv4 與 IPv6 示範 DirectAccess 單一伺服器安裝程式](https://go.microsoft.com/fwlink/p/?LinkId=237004)中的所有步驟。

-   [步驟2：安裝和設定 ROUTER1](assetId:///e4b1a298-d5b0-410e-970b-c5358a9378f9)。 ROUTER1 會在公司網路和2公司網路的子網之間提供路由和轉送功能。

-   [步驟3：安裝和設定 CLIENT2](assetId:///6cbee1b5-f6f6-443f-8fa9-31cc5c05a0ee)。 CLIENT2 是 Windows 7 用戶端電腦，用來示範 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 遠端存取部署的回溯相容性。

-   [步驟4：設定 APP1](assetId:///a0ee655e-c01e-4bf3-a7b3-064e9614f810)。 使用 ROUTER1 做為預設閘道來設定 APP1，並以 2-DC1 作為替代 DNS 伺服器。

-   [步驟5：設定 DC1](assetId:///205ca795-93ce-4e53-aa6b-b44c87f0e14a)。 使用額外的 Active Directory 網站設定 DC1，並為 Windows 7 用戶端電腦設定額外的安全性群組。

-   [步驟6：安裝和設定 2-DC1](assetId:///16752f61-edbf-4ff4-9d7a-e2077b66a127)。 在多網站部署中，您有兩個或多個網域和網站。 2-DC1 提供 corp2.corp.contoso.com 網域的網域控制站和 DNS 服務。

-   [步驟7：安裝和設定 2-APP1](assetId:///7d04b54e-590a-4d33-9766-415789859f29)。 2-APP1 是網路和檔案伺服器，位於2公司網路。

-   [步驟8：設定 INET1](assetId:///8ecc0b63-8626-4939-8d26-3d51d051d231)。 INET1 會在此測試實驗室指南中模擬網際網路。 您必須設定可解析為 EDGE1 之公用 IP 位址的 DNS 專案。

-   [步驟9：設定 EDGE1](assetId:///562744dc-30f6-42fa-bd5f-60a013b2179e)。 在 EDGE1 上設定2公司網路的 DNS 伺服器和路由。

-   [步驟10：安裝和設定 2-EDGE1](assetId:///1938c4f3-ca96-475d-9f2e-6bea3b7a4130)。 多個遠端存取服務器在多網站部署中是必要的。 2-EDGE1 提供第二個網域的遠端存取服務。

-   [步驟11：設定多網站部署](assetId:///537e4b68-043f-49c9-94d8-15ce8c4b18e2)。 設定兩部遠端存取服務器之後，您可以設定您的多網站部署。

-   [步驟12：測試 DirectAccess 連線能力](assetId:///aa293b5d-4b6f-4004-95f3-0ab54804b15c)。 透過 EDGE1 和 2 EDGE1，從網際網路子網的兩部用戶端電腦測試 DirectAccess 連線能力。

-   [步驟13：從 NAT 裝置後方測試 DirectAccess 連線能力](assetId:///41f8195b-00a1-4991-9db8-3703514dbe0c)。 從 NAT 裝置後方測試 DirectAccess 連線能力。

-   [步驟14：設定設定的快照](assetId:///7b56d5c9-c334-463e-9e29-d652ca110d84)。 完成測試實驗室之後，請建立工作遠端存取多網站部署的快照，讓您稍後可以返回以測試其他案例。



