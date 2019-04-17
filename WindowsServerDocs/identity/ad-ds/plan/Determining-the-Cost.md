---
ms.assetid: e3ea1f67-60d4-4566-b24c-37faa95c3b2a
title: "判斷成本"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b32d1930fcef944fbd719338797f0f29b70fa29d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="determining-the-cost"></a>判斷成本

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您偏好便宜連接經由網站連結來指派成本值。 某些應用程式和服務，例如網域控制站定位器 (DCLocator) 和散發檔案系統命名空間 (DFSN)，也使用付費的資訊以尋找最近的資源。 網站連結成本可用來判斷如果指定網域控制站不存在，該網站的網域控制站連絡用中某個網站。 Client 連絡人的網域控制站使用它成本最低的網站連結。  
  
我們建議的網站全各定義成本值。 而不只是上的連結總共上可用性、延遲和成本連結，通常根據成本。  
  
若要判斷成本放到網站連結、文件連接速度每個網站連結。 在「地理位置和通訊連結「(DSSTOPO_1.doc) 試算表參考[收集網路資訊](../../ad-ds/plan/Collecting-Network-Information.md)有關連接您找出的速度。  
  
下表列出的不同類型的網路速度。  
  
|網路類型|速度|  
|----------------|---------|  
|非常慢|56 以每秒 56 kbps|  
|變慢|64 kbps|  
|數位網路 (ISDN) 整合的服務|64 kbps 或 128 Kbps|  
|框架轉接|變數速率，通常之間 56 Kbps 秒 Mbps 1.5 mb|  
|T1|1.5 倍行高 Mbps|  
|T3|45 Mbps|  
|10BaseT|10 Mbps|  
|非同步傳輸模式 (ATM)|常之間 155 Mbps 622 Mbps 變數率|  
|100BaseT|100 Mbps|  
|Gigabit 乙太網路|1 gb (Gbps) 秒|  
  
使用下表計算每個網站連結的費用 (WAN) 連結速度根據寬區域網路速度。 在表格中未列出的速度 WAN 連結，您可以來登入的頻寬，以在 Kbps 測量分割 1024 計算相對成本倍。  
  
|可用的頻寬 56 kbps|成本|  
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
  
這些成本不會反映不同的網路連結中的可靠性。 設定較高的成本任何網路易於失敗的連結，讓您不需要依賴複寫這些連結。 較高的網站連結成本設定，您可以控制複寫錯誤移轉的網站連結失敗時。  
  


