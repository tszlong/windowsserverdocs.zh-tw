---
ms.assetid: e3ea1f67-60d4-4566-b24c-37faa95c3b2a
title: 決定成本
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: f99c151840fcd32fd94567bba8a0bd9ce5196c69
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87943092"
---
# <a name="determining-the-cost"></a>決定成本

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以將成本值指派給站台連結，以優先于昂貴的連線來進行便宜的連接。 某些應用程式和服務（例如網域控制站定位器 (DCLocator) 和分散式檔案系統命名空間 (DFSN) ）也會使用成本資訊來找出最接近的資源。 如果指定網域的網域控制站不存在於該網站，則可以使用站台連結成本來判斷用戶端在一個網站中所聯繫的網域控制站。 用戶端會使用已指派最低成本的站台連結來聯絡網域控制站。

我們建議您以全網站為基礎來定義成本值。 成本通常是根據連結的總頻寬，以及連結的可用性、延遲和貨幣成本來計算。

若要決定要在站台連結上放置的成本，請記錄每個站台連結的連線速度。 如需您所識別之連線速度的相關資訊，請參閱「地理位置和通訊連結」 ( # A0) 工作表[收集網路資訊](../../ad-ds/plan/Collecting-Network-Information.md)。

下表列出不同網路類型的速度。

|網路類型|速度|
|----------------|---------|
|非常緩慢|56 千位元/秒 (Kbps)|
|緩慢|64 Kbps|
|整合服務數位網路 (ISDN)|64 kbps 或 128 Kbps|
|框架轉送|可變速率，通常介於 56 Kbps 和每秒 1.5 mb (Mbps) |
|T1|1.5 Mbps|
|T3|45 Mbps|
|10BaseT|10 Mbps|
| (ATM) 的非同步傳輸模式|可變速率，通常介於 155 Mbps 到 622 Mbps 之間|
|100BaseT|100 Mbps|
|Gigabit Ethernet|1 Gigabit/秒 (Gbps)|

使用下表來根據廣域網路速度 (WAN) 連結速度來計算每個站台連結的成本。 針對未列于資料表中的 WAN 連結速度，您可以藉由將1024除以可用頻寬的記錄來計算相對成本因素，以 Kbps 為單位。

|可用頻寬 (Kbps) |成本|
|--------------------------------|--------|
|9.6|1042|
|19.2|798|
|38.4|644|
|56|586|
|64|567|
|128|486|
|256|425|
|512|378|
|1,024|340|
|2,048|309|
|4,096|283|

這些成本不會反映網路連結之間的可靠性差異。 在任何容易發生錯誤的網路連結上設定較高的成本，讓您不必依賴這些連結進行複寫。 藉由設定較高的站台連結成本，您可以在站台連結失敗時控制複寫容錯移轉。



