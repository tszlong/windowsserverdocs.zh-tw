---
ms.assetid: 28ecaf5c-9131-406c-b211-a230162e462e
title: "判斷排程"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0ec953b34475c50e62553a9ba95e4d45d9904bf3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="determining-the-schedule"></a>判斷排程

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以控制網站連結可用性設定排程的網站連結。 當複寫兩台穿過多個網站連結時，交叉之複寫排程的所有相關連結的判斷連接排程之間的兩個網站。  
  
計劃設定的網站連結排程，建立兩個重疊的排程之間包含直接彼此的網域控制站的網站連結。 除非您想要封鎖複寫資料傳輸期間尖峰使用預設（100%可用) 排程這些連結。 封鎖複寫，優先順序給其他流量，但您也會增加複寫延遲。  
  
網域控制站儲存時間以國際標準時間 (UTC)。 本地時間的網站和電腦設定排程符合網站連結物件排程時間設定。 網域控制站連絡人的不同的網站和時區中的電腦，網域控制站排程會顯示時間設定依據本機電腦的網站的時間。  
  


