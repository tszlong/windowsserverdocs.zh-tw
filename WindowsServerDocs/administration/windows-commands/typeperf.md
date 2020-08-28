---
title: typeperf
description: Typeperf 的參考文章，會將效能資料寫入至命令視窗或記錄檔。
ms.topic: reference
ms.assetid: 0c7ca89a-03b3-4626-afcf-ef8565e90043
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 856279c96a8c1904dcf182dbf613447e02291330
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89023382"
---
# <a name="typeperf"></a>typeperf

**Typeperf**命令會將效能資料寫入至命令視窗或記錄檔。 若要停止 **typeperf**，請按 CTRL + C。

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

## <a name="options"></a>選項

|                   選項                   |                                                         描述                                                          |
|--------------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
|                     -?                     |                                               顯示即時線上說明。                                               |
| -f \<CSV&verbar;TSV&verbar;BIN&verbar;SQL> |                                    指定輸出檔案格式。 預設值為 CSV。                                     |
|              -cf \<filename>               |              指定一個檔案，其中包含要監視的效能計數器清單，每行一個計數器。               |
|             -si < [[hh：] mm：] ss>             |                                  指定取樣間隔。 預設值為一秒。                                   |
|               -o \<filename>               |     指定輸出檔案或 SQL 資料庫的路徑。 預設值為 STDOUT (寫入至命令視窗) 。      |
|                -q [object]                 | 顯示已安裝的計數器清單， (沒有任何實例) 。 若要列出一個物件的計數器，請包含物件名稱。 \*\*\*例子 |
|                -qx [object]                |        顯示已安裝的計數器清單與實例。 若要列出一個物件的計數器，請包含物件名稱。        |
|               -sc \<samples>               |             指定要收集的樣本數目。 預設值是在按下 CTRL + C 之前收集資料。              |
|            -config \<filename>             |                                    指定包含命令選項的設定檔。                                     |
|            -s \<computer_name>             |                   指定當計數器路徑中沒有指定電腦時，要監視的遠端電腦。                    |
|                     -y                     |                                        回答所有問題而不提示。                                        |

## <a name="examples"></a>範例

- 若要將本機電腦性能** \\ \\)  (_Total \% **計數器的值寫入至命令視窗，請以預設的取樣間隔1秒為間隔，直到按下 CTRL + C 為止。
  ```
  typeperf \Processor(_Total)\% Processor Time
  ```
- 若要將檔案 **counters.txt** 中的計數器清單值寫入至以 tab 分隔的檔案 **>domain2.contoso.com** ，請在5秒的取樣間隔內，直到收集50樣本為止。
  ```
  typeperf -cf counters.txt -si 5 -sc 50 -f TSV -o domain2.tsv
  ```
- 若要查詢具有計數器物件 **PhysicalDisk** 實例的已安裝計數器，並將產生的清單寫入 **counters.txt**的檔案。
  ```
  typeperf -qx PhysicalDisk -o counters.txt
  ```
