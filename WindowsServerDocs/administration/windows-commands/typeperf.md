---
title: typeperf
description: '適用於 Windows 命令主題 * * *- '
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
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440784"
---
# <a name="typeperf"></a>typeperf



**Typeperf**命令在命令視窗或記錄檔寫入的效能資料。 若要停止**typeperf**，按 CTRL + C。

如需如何使用的範例**typeperf**，請參閱[範例](#BKMK_EXAMPLES)。

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
|\<計數器 [計數器 [...]]>|指定要監視的效能計數器。|

> [!NOTE]
> **\<計數器 >** 中的效能計數器的完整名稱 *\\ \\Computer\Object （執行個體） \Counter*格式，例如 **\\ \\Server1\Processor(0)\%使用者時間**。

## <a name="options"></a>選項。

|                   選項                   |                                                         描述                                                          |
|--------------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
|                     -?                     |                                               顯示即時線上說明。                                               |
| -f \<CSV&verbar;TSV&verbar;BIN&verbar;SQL> |                                    指定輸出檔案格式。 預設值為 CSV。                                     |
|              -cf \<filename>               |              指定檔案，內含監視，與每行一個計數器的效能計數器的清單。               |
|             si < [[hh:] mm:] ss >             |                                  指定取樣間隔時間。 預設值為一秒。                                   |
|               -o \<filename>               |     指定輸出檔或 SQL 資料庫的路徑。 預設為 STDOUT （寫入命令視窗中）。      |
|                -q [object]                 | 顯示一份已安裝的計數器 （沒有執行個體）。 若要列出一個物件的計數器，包括物件名稱。 \*\*\*範例 |
|                -qx [object]                |        顯示已安裝執行個體的計數器清單。 若要列出一個物件的計數器，包括物件名稱。        |
|               -sc\<範例 >               |             指定要收集樣本的數目。 預設值是收集資料，直到按下 CTRL + C。              |
|            -config \<filename>             |                                    指定包含命令選項的設定檔。                                     |
|            -s \<computer_name>             |                   指定要監視如果未指定電腦的計數器路徑中的遠端電腦。                    |
|                     -y                     |                                        所有的問題是回應，而不提示。                                        |

## <a name="BKMK_EXAMPLES"></a>範例

- 下列範例會將值寫入本機電腦的效能計數器 **\\ \\processor （_total)\% Processor Time**命令視窗預設範例間隔為 1 秒直到按下 CTRL + C。  
  ```
  typeperf "\Processor(_Total)\% Processor Time"
  ```  
- 下列範例會寫入檔案中的計數器清單的值**counters.txt** tab 鍵分隔檔案**domain2.tsv** 5 秒，直到已收集 50 範例的範例間隔。  
  ```
  typeperf -cf counters.txt -si 5 -sc 50 -f TSV -o domain2.tsv
  ```  
- 下列範例會查詢安裝計數器物件的執行個體的計數器**PhysicalDisk**並將產生的清單寫入檔案**counters.txt**。  
  ```
  typeperf -qx PhysicalDisk -o counters.txt
  ```
