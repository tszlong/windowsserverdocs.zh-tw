---
ms.assetid: de054ac2-a386-43ec-a537-c0de21549741
title: 設定站台連結屬性
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: c35b4b59fdebc186357d57e6bf566f66b0f34a74
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93070160"
---
# <a name="setting-site-link-properties"></a>設定站台連結屬性

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

網站間複寫會根據連線物件的屬性進行。 當知識一致性檢查程式 (KCC) 建立連線物件時，它會從站台連結物件的屬性衍生複寫排程。 每個站台連結物件都代表在兩個或多個網站之間 (WAN) 連線的廣域網路。

設定站台連結物件屬性包含下列步驟：

-   判斷與該複寫路徑相關聯的成本。 KCC 會使用成本來判斷兩個複寫相同目錄分割的網站之間的複寫成本最低的路由。

-   判斷排程，以定義可進行網站間複寫的時間。

-   判斷複寫間隔，此間隔定義在允許複寫時（如排程中所定義）進行複寫的頻率。

## <a name="in-this-guide"></a>本指南內容

-   [決定成本](../../ad-ds/plan/Determining-the-Cost.md)

-   [決定排程](../../ad-ds/plan/Determining-the-Schedule.md)

-   [決定間隔](../../ad-ds/plan/Determining-the-Interval.md)



