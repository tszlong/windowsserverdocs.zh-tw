---
ms.assetid: e3ea1f67-60d4-4566-b24c-37faa95c3b2a
title: 決定成本
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0b34f1672311768d644c467fda10dc2fc643282d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847079"
---
# <a name="determining-the-cost"></a>決定成本

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

您可將成本值指派給站台連結，讓低成本的連線優先於昂貴的連線中。 某些應用程式和服務，例如網域控制站定位程式 (DCLocator) 和分散式檔案系統命名空間 (DFSN)，也使用成本資訊來尋找最接近的資源。 若要判斷哪一個網域控制站會連絡位於一個站台的用戶端上，如果不存在指定的網域的網域控制站，這是在該站台可用站台連結成本。 用戶端會使用指派給它的成本最低的站台連結，以連絡網域控制站。  
  
我們建議的成本值，定義全站台為基礎。 成本時，通常依連結總頻寬不僅在可用性、 延遲及成本的連結。  
  
若要判斷將站台連結成本，文件的每個站台連結的連線速度。 「 地理位置和通訊連結 」 (DSSTOPO_1.doc) 工作表中是指[收集網路資訊](../../ad-ds/plan/Collecting-Network-Information.md)如您所識別的連線速度的相關資訊。  
  
下表列出不同類型的網路速度。  
  
|網路類型|速度|  
|----------------|---------|  
|速度很慢|56 kb / 秒 (Kbps)|  
|慢速|64 Kbps|  
|整合式的服務數位網路 (ISDN)|64 kbps 或 128 Kbps|  
|框架轉接|變數的速度，通常介於 56 Kbps 到 1.5 mb / 秒 (Mbps)|  
|T1|1.5 Mbps|  
|T3|45 Mbps|  
|10BaseT|10 Mbps|  
|非同步傳輸模式 (ATM)|變數的速率，通常介於 155 Mbps 到 622 Mbps|  
|100BaseT|100 Mbps|  
|Gigabit 乙太網路|1 gigabit / 秒 (Gbps)|  
  
使用下表來計算每個站台連結成本根據廣域網路網路速度 (WAN) 連結速度。 未列在資料表的 WAN 連結速度，您可以計算的相對成本因數 1,024 除以可用的頻寬，以 kbps 為單位測量的記錄檔。  
  
|可用的頻寬 (Kbps)|成本|  
|--------------------------------|--------|  
|9.6|1,042|  
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
  
這些成本不會反映在可靠性的網路連結之間的差異。 設定任何可能會失敗的網路連結更高的成本，如此一來，不用依賴這些連結進行複寫。 設定較高站台連結成本，您可以控制複寫容錯移轉的站台連結失敗時。  
  


