---
ms.assetid: 28ecaf5c-9131-406c-b211-a230162e462e
title: 決定排程
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: e0e285efb05dabc1e60f20622ac4dff982b6fb4a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87941263"
---
# <a name="determining-the-schedule"></a>決定排程

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以藉由設定站台連結的排程來控制站台連結的可用性。 當兩個網站之間的複寫流經多個站台連結時，所有相關連結上的複寫排程交集會決定這兩個網站之間的連接排程。

若要規劃設定站台連結排程，請在包含直接彼此複寫之網域控制站的站台連結之間，建立兩個重迭的排程。 除非您想要在尖峰時段封鎖複寫流量，否則請使用預設 (100-可用) 排程的連結。 藉由封鎖複寫，您可以優先考慮其他流量，但也會增加複寫延遲。

網域控制站會以國際標準時間 (UTC) 儲存時間。 站台連結物件排程中的時間設定符合設定排程的網站和電腦的本機時間。 當網域控制站連線到位於不同網站和時區的電腦時，網域控制站上的排程會根據電腦網站的當地時間顯示時間設定。



