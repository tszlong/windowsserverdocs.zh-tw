---
title: System Insights 資料來源
description: 在系統深入解析中撰寫新功能時，您可以指定要在本機收集的現有或新資料來源，並進行分析。 本主題描述您在註冊新功能時可以選擇的資料來源。
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 7/31/2018
ms.openlocfilehash: 23150a741c9ec218077f63ca65e6948b1c48f8bf
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87972005"
---
# <a name="system-insights-data-sources"></a>System Insights 資料來源

>適用於：Windows Server 2019

System Insights 引進可擴充的資料收集功能。 撰寫新的功能時，您可以指定要在本機收集的現有或新資料來源，並進行分析。 本主題描述您在註冊新功能時可以選擇的資料來源。

## <a name="data-sources"></a>資料來源
撰寫新功能時，您必須識別要針對每項功能收集的特定資料來源。 您指定的資料來源將會直接收集並保存在您的電腦上，而且您可以從三種類型的資料來源中選擇：

- **效能計數器**：
    - 指定計數器路徑、名稱和實例，而 System Insights 會收集這些效能計數器所報告的相關資料。

- **系統事件**：
    - 指定通道名稱和事件識別碼，系統深入解析會記錄事件發生的次數。

- **知名系列**
    - System Insights 會針對一些定義完善的資源，收集您電腦上的一些基本資訊。 這些數列用於預設功能，但也可供任何自訂功能使用。 這些會收集下列資訊：

        - **磁片**：
            - *屬性*： Guid
            - *資料*：大小
        - **磁片**區：
            - *屬性*： UniqueId，磁碟機號，FileSystemLabel，Size
            - *Data*：已使用大小
        - **網路介面卡**：
            - *屬性*： InterfaceGuid、InterfaceDescription、速度
            - *資料*：接收的位元組數/秒，傳送的位元組數/秒，位元組總計/秒
        - **CPU**：
            - *屬性*：-
            - *資料*：處理器時間百分比

    - 指定已知的數列，系統深入解析會傳回該數列所收集的資料。


## <a name="retention-timelines-and-collection-intervals"></a>保留時間軸和收集間隔
上述資料來源有不同的保留時間軸和收集間隔。 下表顯示每個資料來源收集的時間和頻率：

| 資料來源 | 保留時間表 | 收集間隔 |
| --------------- | --------------- | ----------- |
| 效能計數器 | 3 個月 | 15 分鐘 |
| 系統事件 | 3 個月 | 15 分鐘 |
| 知名系列 | 1 年 | 1 小時 |


### <a name="aggregation-types"></a>彙總類型
由於每個數列只會記錄每個收集間隔的一個資料點，因此每個數列都有一個匯總類型相關聯。 下表描述資料來源和對應的匯總類型：

>[!NOTE]
>針對效能計數器，您可以選擇幾種不同的匯總類型。

| 資料來源 | 彙總類型 |
| --------------- | --------------- |
| 效能計數器 | 加總、平均、最大值、最小值 |
| 系統事件 | 計數 |
| 磁片知名系列 | 上次 (收集間隔中的最新值)  |
| 磁片區知名系列 | 上次 (收集間隔中的最新值)  |
| CPU 已知系列 | 平均 |
| 網路知名系列 | 平均 |

## <a name="data-footprint"></a>資料使用量

System Insights 會收集 C 磁片磁碟機本機上的所有資料， (C： ) 。 一般而言，系統深入解析資料使用量適中。 它會直接相依于每個功能所指定的資料來源類型和數目，下表詳細說明每種資料類型的儲存使用量：

| 資料來源 | 最大使用量 |
| --------------- | --------------- |
| 效能計數器 | 240 KB |
| 系統事件 | 200 KB |
| 磁片知名系列 | 每個磁片 200 KB |
| 磁片區知名系列 | 每個磁片區 300 KB |
| CPU 已知系列 | 100 KB |
| 網路知名系列 | 每張網路介面卡 300 KB |

>[!NOTE]
>**針對預設的預測功能，大部分的獨立機器的最大使用量應小於 10 MB。**

## <a name="additional-references"></a>其他參考資料
若要深入瞭解「系統深入解析」，請使用下列資源：

- [系統深入解析概觀](overview.md)
- [了解功能](understanding-capabilities.md)
- [管理功能](managing-capabilities.md)
- [新增與開發功能](adding-and-developing-capabilities.md)
- [System Insights 常見問題](faq.md)
