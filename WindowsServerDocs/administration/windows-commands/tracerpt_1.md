---
title: tracerpt
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cb9eaf86-0ef6-4197-b6c8-9cca8a1d723c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7c105fe714e30866297e4f6c3c83a670ff7966a6
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440968"
---
# <a name="tracerpt"></a>tracerpt



**Tracerpt**命令可以用來剖析事件追蹤記錄檔、 效能監視和即時的事件追蹤提供者所產生的記錄檔。 它會產生傾印檔案、 報表檔案和報表結構描述。

如需如何使用的範例**tracerpt**，請參閱[範例](#BKMK_EXAMPLES)。

## <a name="syntax"></a>語法

```
tracerpt <[-l] <value [value [...]]>|-rt <session_name [session_name [...]]>> [options]
```

## <a name="options"></a>選項。

|              選項旗標               |                                                                    描述                                                                    |
|----------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
|                   -?                   |                                                         顯示即時線上說明。                                                          |
|          -config \<filename>           |                                                 載入包含命令選項的設定檔。                                                  |
|                   -y                   |                                                  所有的問題是回應，而不提示。                                                   |
|                -f \<XML                |                                                                       HTML>                                                                       |
|               -的\<CSV                |                                                                       EVTX                                                                        |
|            -df \<filename>             |                                            建立 Microsoft 專有計數/報告結構描述檔案。                                            |
|            -int\<檔名 >            |                                            傾印解譯的事件結構至指定的檔案。                                            |
|                  -rts                  |                        報告的事件追蹤標頭中的原始時間戳記。 僅能與-o、-報表或-彙總。                         |
|            -tmf \<filename>            |                                                  指定的追蹤訊息格式定義檔案。                                                  |
|              -tp \<value>              |                            指定 TMF 檔案搜尋路徑。 可能會使用多個路徑，並以分號 （;）。                            |
|              -i \<value>               | 指定的提供者的影像路徑。 比對的 PDB 將位於符號伺服器。 多個路徑可用，並以分號 （;）。 |
|             -pdb \<value>              |                             指定符號伺服器的路徑。 多個路徑可用，並以分號 （;）。                             |
|                  -gmt                  |                                              WPP 裝載時間戳記轉換為格林威治標準時間。                                               |
|              -rl \<value>              |                                               定義系統報表層級從 1 到 5。 預設值為 1。                                               |
|          -摘要 [filename]           |                                  產生摘要的報表文字檔案。 如果未指定的檔名是 summary.txt。                                   |
|             -o [filename]              |                                      產生文字輸出檔案。 如果未指定的檔名是 dumpfile.xml。                                      |
|           -report [filename]           |                                  產生文字輸出報告檔。 如果未指定的檔名是 workload.xml。                                   |
|                  -lr                   |                        指定 「 較不嚴格。 」 這會竭盡所能使用不相符的事件結構描述的事件。                         |
|           -export [filename]           |                                  產生的事件結構描述匯出檔案。 如果未指定的檔名是 schema.man。                                   |
|       [-l]。\<值 [值 [...]]>        |                                                   指定要處理的事件追蹤記錄檔。                                                    |
| -rt \<session_name [session_name […]]> |                                                指定即時事件追蹤工作階段資料來源。                                                |

## <a name="BKMK_EXAMPLES"></a>範例

- 這個範例會建立兩個事件記錄檔為基礎的報表**logfile1.etl**並**logfile2.etl** ，並建立傾印檔**logdump.xml**以 XML 格式。  
  ```
  tracerpt logfile1.etl logfile2.etl -o logdump.xml -of XML
  ```  
- 這個範例會建立事件記錄檔為基礎的報表**logfile.etl**，會建立傾印檔案**logdmp.xml**以 XML 格式，使用 最大努力來識別事件不會在結構描述中，會產生摘要的報表檔案**logdump.txt**，並產生報告檔**logrpt.xml**。  
  ```
  tracerpt logfile.etl -o logdmp.xml -of XML -lr -summary logdmp.txt -report logrpt.xml
  ```  
- 這個範例會使用兩個事件記錄檔**logfile1.etl**並**logfile2.etl**產生傾印檔案和報告檔案的預設檔名。  
  ```
  tracerpt logfile1.etl logfile2.etl -o -report
  ```  
- 這個範例會使用事件記錄檔**logfile.etl**和 效能記錄檔**counterfile.blg**來產生報告檔**logrpt.xml**和 Microsoft 特定 XML 結構描述檔案**schema.xml**。  
  ```
  tracerpt logfile.etl counterfile.blg -report logrpt.xml -df schema.xml
  ```  
- 這個範例會讀取即時的事件追蹤工作階段"NT Kernel Logger"，並產生傾印檔案**logfile.csv** CSV 格式。  
  ```
  tracerpt -rt "NT Kernel Logger" -o logfile.csv -of CSV
  ```