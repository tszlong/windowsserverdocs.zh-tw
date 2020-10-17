---
title: typeperf
description: Typeperf 命令的參考文章，此命令會將效能資料寫入至命令視窗或記錄檔。
ms.topic: reference
ms.assetid: 0c7ca89a-03b3-4626-afcf-ef8565e90043
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6a06d422db5f681c3e1775facc4f8d0eee422244
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156353"
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

| 參數 | 描述 |
|--|--|
| `<counter [counter […]]>` | 指定要監視的效能計數器。 `<counter>`參數是 \\ Computer\Object (實例) \counter 格式之效能計數器的完整名稱，例如 `\\Server1\Processor(0)\% User Time` 。  |

#### <a name="options"></a>選項。

| 選項 | 描述 |
|--|--|
| -f `<CSV | TSV | BIN | SQL>` | 指定輸出檔案格式。 預設值為 CSV。 |
| -cf `<filename>` | 指定一個檔案，其中包含要監視的效能計數器清單，每行一個計數器。 |
| -si `<[[hh:]mm:]ss>` | 指定取樣間隔。 預設值為一秒。 |
| -o `<filename>` | 指定輸出檔案或 SQL 資料庫的路徑。 預設值為 STDOUT (寫入至命令視窗) 。 |
| -q `[object]` | 顯示已安裝的計數器清單， (沒有任何實例) 。 若要列出一個物件的計數器，請包含物件名稱。 例子 |
| -qx `[object]` | 顯示已安裝的計數器清單與實例。 若要列出一個物件的計數器，請包含物件名稱。 |
| -sc `<samples>` | 指定要收集的樣本數目。 預設值是在按下 CTRL + C 之前收集資料。 |
| -config `<filename>` | 指定包含命令選項的設定檔。 |
| -s `<computer_name>` | 指定當計數器路徑中沒有指定電腦時，要監視的遠端電腦。 |
| -y | 回答 *所有* 問題而不提示。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要將本機電腦的效能計數器值以 `\Processor(_Total)\% Processor Time` 1 秒的預設取樣間隔寫入至命令視窗，直到按下 CTRL + C 為止，請輸入：

```
typeperf \Processor(_Total)\% Processor Time
```

若要將檔案中的計數器清單值 *counters.txt* 到以 tab 分隔的檔案 *>domain2.contoso.com* ，在5秒的取樣間隔內，直到收集50樣本為止，請輸入：

```
typeperf -cf counters.txt -si 5 -sc 50 -f TSV -o domain2.tsv
```

若要使用 *PhysicalDisk* 計數器物件的實例來查詢已安裝的計數器，並將產生的清單寫入 *counters.txt*的檔案，請輸入：

```
typeperf -qx PhysicalDisk -o counters.txt
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
