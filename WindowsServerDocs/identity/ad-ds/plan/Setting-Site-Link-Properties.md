---
ms.assetid: de054ac2-a386-43ec-a537-c0de21549741
title: 設定站台連結屬性
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4fa9a1fa8d2a463fe5f361a5a27ee2b9e3edc0f6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870359"
---
# <a name="setting-site-link-properties"></a>設定站台連結屬性

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

根據連接物件的屬性，就會發生站台間複寫。 當知識一致性檢查程式 (KCC) 建立連線物件時，它會衍生自站台連結物件的屬性的複寫排程。 每個站台連結物件代表兩個或多個站台間廣域網路 (wan) 連線。  
  
設定站台連結物件內容包含下列步驟：  
  
-   決定與該複寫路徑相關聯的成本。 修改 KCC 用來判斷最便宜的複寫相同的目錄磁碟分割的兩個站台間複寫路由的成本。  
  
-   決定排程，定義在哪些站台間複寫期間的時間可能會發生。  
  
-   判斷複寫應該發生的頻率時允許複寫的時間期間定義在排程中所定義的複寫間隔。  
  
## <a name="in-this-guide"></a>本指南內容  
  
-   [判斷成本](../../ad-ds/plan/Determining-the-Cost.md)  
  
-   [決定排程](../../ad-ds/plan/Determining-the-Schedule.md)  
  
-   [決定間隔](../../ad-ds/plan/Determining-the-Interval.md)  
  


