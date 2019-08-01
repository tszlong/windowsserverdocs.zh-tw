---
title: typeperf
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0c7ca89a-03b3-4626-afcf-ef8565e90043
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cfcbac82b88c0c8d8bcc706ebfd807f96e359de7
ms.sourcegitcommit: af80963a1d16c0b836da31efd9c5caaaf6708133
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/31/2019
ms.locfileid: "66440784"
---
# <a name="typeperf"></a>typeperf



**Typeperf**命令會將效能資料寫入至命令視窗或記錄檔。 若要停止**typeperf**, 請按 CTRL + C。

如需如何使用**typeperf**的範例, 請參閱[範例](#BKMK_EXAMPLES)。

## <a name="syntax"></a>語法

```
typeperf <counter [counter ...]> [options]
typeperf -cf <filename> [options]
typeperf -q [object] [options]
typeperf -qx [object] [options]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<counter [counter [...]]>|指定要監視的效能計數器。|

> [!NOTE]
> counter > 是效能計數器 *\\的完整名稱 ( Computer\Object(Instance)\Counter格式),例如Server1\Processor(0)\\* **\<**  **\\ \\ \%使用者時間**。

## <a name="options"></a>選項。

|                   選項                   |                                                         描述                                                          |
|--------------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
|                     -?                     |                                               顯示即時線上說明。                                               |
| -f \<CSV&verbar;TSV&verbar; BIN&verbar;SQL > |                                    指定輸出檔案格式。 預設值為 CSV。                                     |
|              -cf \<filename >               |              指定包含要監視之效能計數器清單的檔案, 每行一個計數器。               |
|             -si < [[hh:] mm:] ss >             |                                  指定取樣間隔。 預設值為一秒。                                   |
|               -o \<filename >               |     指定輸出檔或 SQL 資料庫的路徑。 預設值為 STDOUT (寫入至命令視窗)。      |
|                -q [物件]                 | 顯示已安裝的計數器清單 (沒有實例)。 若要列出一個物件的計數器, 請包含物件名稱。 \*\*\*實例 |
|                -qx [物件]                |        顯示已安裝的計數器清單和實例。 若要列出一個物件的計數器, 請包含物件名稱。        |
|               -sc \<範例 >               |             指定要收集的樣本數。 預設為收集資料, 直到按下 CTRL + C 為止。              |
|            -config \<檔案名 >             |                                    指定包含命令選項的設定檔案。                                     |
|            -s \<電腦 2 >             |                   指定在計數器路徑中未指定電腦時要監視的遠端電腦。                    |
|                     -y                     |                                        對所有問題回答 [是] 而不提示。                                        |

## <a name="BKMK_EXAMPLES"></a>典型

- 下列範例會將本機電腦的效能計數器 **\\ \\processor (_total\% ) 處理器時間**值寫入至命令視窗, 預設取樣間隔為1秒, 直到按下 CTRL + C 為止.  
  ```
  typeperf "\Processor(_Total)\% Processor Time"
  ```  
- 下列範例會以5秒的取樣間隔, 將檔案**計數器**中的計數器清單值寫入至 tab 鍵分隔的檔案**domain2> ...** , 直到收集50樣本為止。  
  ```
  typeperf -cf counters.txt -si 5 -sc 50 -f TSV -o domain2.tsv
  ```  
- 下列範例會查詢已安裝的計數器, 其中包含 counter 物件**PhysicalDisk**的實例, 並將產生的清單寫入檔案**計數器 .txt**。  
  ```
  typeperf -qx PhysicalDisk -o counters.txt
  ```
