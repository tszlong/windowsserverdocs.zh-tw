---
title: typeperf
description: Typeperf 的參考文章，它會將效能資料寫入至命令視窗或記錄檔。
ms.topic: article
ms.assetid: 0c7ca89a-03b3-4626-afcf-ef8565e90043
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 38a459fb1c52c627d05f3d19fb8f2e8055a89338
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896641"
---
# <a name="typeperf"></a>typeperf

**Typeperf**命令會將效能資料寫入至命令視窗或記錄檔。 若要停止**typeperf**，請按 CTRL + C。

## <a name="syntax"></a>語法

```
typeperf <counter [counter ...]> [options]
typeperf -cf <filename> [options]
typeperf -q [object] [options]
typeperf -qx [object] [options]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<counter [counter […]]>|指定要監視的效能計數器。|

> [!NOTE]
> **\<counter>** 這是* \\ \\ Computer\Object (實例) \counter*格式之效能計數器的完整名稱，例如** \\ \\ Server1\Processor (0) \% 使用者時間**。

## <a name="options"></a>選項。

|                   選項                   |                                                         描述                                                          |
|--------------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
|                     -?                     |                                               顯示即時線上說明。                                               |
| -f\<CSV&verbar;TSV&verbar;BIN&verbar;SQL> |                                    指定輸出檔案格式。 預設值為 CSV。                                     |
|              -cf\<filename>               |              指定包含要監視之效能計數器清單的檔案，每行一個計數器。               |
|             -si < [[hh：] mm：] ss>             |                                  指定取樣間隔。 預設值為一秒。                                   |
|               -o\<filename>               |     指定輸出檔或 SQL 資料庫的路徑。 預設值為 STDOUT (寫入至命令視窗) 。      |
|                -q [物件]                 | 顯示已安裝的計數器清單， (沒有任何實例) 。 若要列出一個物件的計數器，請包含物件名稱。 \*\*\*實例 |
|                -qx [物件]                |        顯示已安裝的計數器清單和實例。 若要列出一個物件的計數器，請包含物件名稱。        |
|               -sc\<samples>               |             指定要收集的樣本數。 預設為收集資料，直到按下 CTRL + C 為止。              |
|            -config\<filename>             |                                    指定包含命令選項的設定檔案。                                     |
|            -s\<computer_name>             |                   指定在計數器路徑中未指定電腦時要監視的遠端電腦。                    |
|                     -y                     |                                        對所有問題回答 [是] 而不提示。                                        |

## <a name="examples"></a>範例

- 若要將本機電腦之效能計數器處理器的值** \\ \\ (_Total) 的 \% 處理器時間**寫入命令視窗，其預設取樣間隔為1秒，直到按下 CTRL + C 為止。
  ```
  typeperf \Processor(_Total)\% Processor Time
  ```
- 若要將檔案中的計數器清單值寫入至**domain2> ...** 的 tab 鍵分隔檔案，請在5秒的取樣間隔**counters.txt**中，直到收集50樣本為止。
  ```
  typeperf -cf counters.txt -si 5 -sc 50 -f TSV -o domain2.tsv
  ```
- 查詢已安裝的計數器，其中包含計數器物件**PhysicalDisk**的實例，並將產生的清單寫入**counters.txt**的檔案。
  ```
  typeperf -qx PhysicalDisk -o counters.txt
  ```
