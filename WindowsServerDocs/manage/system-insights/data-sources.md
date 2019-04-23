---
title: 系統深入解析資料來源
description: 當系統 Insights 中撰寫的新功能，您可以指定現有或新資料來源，以在本機收集和分析。 本主題描述註冊新的功能時，您可以選擇資料來源。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: system-insights
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ''
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 7/31/2018
ms.openlocfilehash: 9b46db90787d24a173ffa472ec1ecb8eaffe054b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845409"
---
# <a name="system-insights-data-sources"></a>系統深入解析資料來源

>適用於：Windows Server 2019

系統 Insights 引進了可擴充的資料收集功能。 在撰寫新的功能，您可以指定現有或新資料來源，以在本機收集和分析。 本主題描述註冊新的功能時，您可以選擇資料來源。

## <a name="data-sources"></a>資料來源
在撰寫新的功能，您必須識別特定資料來源，用以收集每一項功能。 將要收集並保存直接在您的電腦上您指定的資料來源，您可以選擇三種類型的資料來源：

- **效能計數器**: 
    - 指定計數器路徑、 名稱和執行個體，以及系統 Insights 會收集這些效能計數器所報告的相關資料。 

- **系統事件**:
    - 指定通道名稱和事件識別碼和系統的深入解析會記錄多少次發生該事件。

- **已知的系列**
    - 系統 Insights 會收集您的電腦數且定義完善的資源的一些基本資訊。 這些數列會用於預設功能，但也可以使用任何自訂的功能。 這些收集下列資訊：

        - **磁碟**: 
            - *屬性*:GUID
            - *資料*:大小
        - **磁碟區**:
            - *屬性*:UniqueId，磁碟機代號，FileSystemLabel 大小
            - *資料*:使用的大小
        - **網路介面卡**:
            - *屬性*:InterfaceGuid，InterfaceDescription，速度
            - *資料*:Bytes Received/sec，位元組傳送/sec、 Bytes Total/sec
        - **CPU**: 
            - *屬性*:-
            - *資料*: %處理器時間

    - 指定已知的系列中，且系統深入解析會傳回該數列所收集的資料。 


## <a name="retention-timelines-and-collection-intervals"></a>保留的時間軸和集合的時間間隔
上述的資料來源有不同的保留時間軸和集合的時間間隔。 下表顯示每個資料來源收集的方式長時間及時間：

| 資料來源 | 保留時間軸 | 收集間隔 |
| --------------- | --------------- | ----------- |
| 效能計數器 | 3 個月 | 15 分鐘 |
| 系統事件 | 3 個月 | 15 分鐘 |
| 已知的系列 | 1 年 | 1 小時 |


### <a name="aggregation-types"></a>彙總類型
因為每個序列的每個收集間隔記錄只有一個資料點，每個數列都彙總類型相關聯它。 下表描述的資料來源和對應的彙總類型：

>[!NOTE]
>效能計數器，您可以選擇從幾個不同的彙總類型。

| 資料來源 | 彙總類型 |
| --------------- | --------------- |
| 效能計數器 | Sum、 average、 max、 min |
| 系統事件 | 計數 |
| 磁碟已知系列 | 最後一個 （最新值中的收集間隔） |
| 磁碟區已知系列 | 最後一個 （最新值中的收集間隔） |
| CPU 已知系列 | 平均 |
| 網路已知系列 | 平均 |

## <a name="data-footprint"></a>資料使用量

系統 Insights 會收集所有資料在本機 C 磁碟機 （c:）。 一般情況下，系統的深入解析資料量不是太大。 它直接相依於類型和數量的資料來源指定每一項功能，以及下表詳細說明每種資料類型的儲存體使用量：

| 資料來源 | 最大的使用量 |
| --------------- | --------------- |
| 效能計數器 | 240 KB |
| 系統事件 | 200 KB |
| 磁碟已知系列 | 每個磁碟的 200 KB |
| 磁碟區已知系列 | 每個磁碟區的 300 KB |
| CPU 已知系列 | 100 KB |
| 網路已知系列 | 每個網路介面卡的 300 KB |

>[!NOTE]
>**預測功能的預設值，最大的使用量應小於 10 MB 的大部分獨立機器。** 

## <a name="see-also"></a>另請參閱
若要深入了解系統的深入解析，使用下列資源：

- [系統 Insights 概觀](overview.md)
- [了解功能](understanding-capabilities.md)
- [管理功能](managing-capabilities.md)
- [新增和開發功能](adding-and-developing-capabilities.md)
- [系統 Insights 常見問題集](faq.md)
