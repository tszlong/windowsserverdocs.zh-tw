---
description: 深入瞭解：判斷排程
ms.assetid: 28ecaf5c-9131-406c-b211-a230162e462e
title: 決定排程
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: d342885208557f968fde1e6365dc3a4ac1eb61e6
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97046206"
---
# <a name="determining-the-schedule"></a>決定排程

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以藉由設定站台連結的排程來控制站台連結的可用性。 在兩個網站之間進行複寫時，如果在兩個網站之間進行複寫，則所有相關連結上的複寫排程交集都會決定兩個網站之間的連接排程。

若要規劃設定站台連結排程，請在包含直接彼此複寫之網域控制站的站台連結之間建立兩個重迭的排程。 除非您想要封鎖尖峰時間的複寫流量，否則請使用這些連結的預設 (100-% 可用) 排程。 藉由封鎖複寫，您可以為其他流量提供優先權，但也會增加複寫延遲。

網域控制站以國際標準時間 (UTC) 儲存時間。 站台連結物件排程中的時間設定符合設定排程之網站和電腦的本機時間。 當網域控制站將不同網站和時區的電腦連線時，網域控制站上的排程會根據電腦網站的當地時間顯示時間設定。



