---
ms.assetid: de054ac2-a386-43ec-a537-c0de21549741
title: 設定站台連結屬性
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: c9a9b25aa56948e3116ebfef67a6af73917b76c2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402481"
---
# <a name="setting-site-link-properties"></a>設定站台連結屬性

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

網站間複寫會根據連線物件的屬性進行。 當知識一致性檢查程式（KCC）建立連線物件時，它會從站台連結物件的屬性衍生複製排程。 每個站台連結物件都代表兩個或多個網站之間的廣域網路（WAN）連線。  
  
設定站台連結物件屬性包括下列步驟：  
  
-   判斷與該複寫路徑相關聯的成本。 KCC 會使用成本來判斷兩個複寫相同目錄分割之網站之間的複寫成本最低的路由。  
  
-   決定排程，以定義可進行網站間複寫的時間。  
  
-   決定複寫間隔時間，以定義在允許複寫時（如排程中所定義）進行複寫的頻率。  
  
## <a name="in-this-guide"></a>本指南內容  
  
-   [決定成本](../../ad-ds/plan/Determining-the-Cost.md)  
  
-   [決定排程](../../ad-ds/plan/Determining-the-Schedule.md)  
  
-   [決定間隔](../../ad-ds/plan/Determining-the-Interval.md)  
  


