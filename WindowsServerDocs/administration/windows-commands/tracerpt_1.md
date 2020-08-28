---
title: tracerpt
description: Tracerpt 的參考文章，可剖析事件追蹤記錄檔、效能監視器所產生的記錄檔，以及即時事件追蹤提供者。
ms.topic: reference
ms.assetid: cb9eaf86-0ef6-4197-b6c8-9cca8a1d723c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 250c938963342ca46308f601b00d44773638a4ad
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89026946"
---
# <a name="tracerpt"></a>tracerpt

**Tracerpt**命令可以用來剖析事件追蹤記錄檔、效能監視器所產生的記錄檔，以及即時事件追蹤提供者。 它會產生傾印檔案、報表檔和報表架構。

## <a name="syntax"></a>語法

```
tracerpt <[-l] <value [value [...]]>|-rt <session_name [session_name [...]]>> [options]
```

## <a name="options"></a>選項

|              選項旗標               |                                                                    描述                                                                    |
|----------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
|                   -?                   |                                                         顯示即時線上說明。                                                          |
|          -config \<filename>           |                                                 載入包含命令選項的設定檔。                                                  |
|                   -y                   |                                                  回答所有問題而不提示。                                                   |
|            -f \<XML\|HTML>             |                                                                  報表格式。                                                                   |
|         -/ \<CSV\|EVTX\|XML>          |                                                         傾印格式，預設值為 XML。                                                          |
|            -df \<filename>             |                                            建立 Microsoft 特定的計數/報告架構檔案。                                            |
|            -int \<filename>            |                                            將解讀的事件結構傾印到指定的檔案。                                            |
|                  -rts                  |                        報告事件追蹤標頭中的原始時間戳記。 只能搭配-o、not-report 或-summary 一起使用。                         |
|            -tmf \<filename>            |                                                  指定追蹤訊息格式定義檔。                                                  |
|              -tp \<value>              |                            指定 TMF 檔案搜尋路徑。 您可以使用多個路徑，並以分號 (; ) 。                            |
|              -i \<value>               | 指定提供者映射路徑。 相符的 PDB 將位於符號伺服器中。 您可以使用多個路徑，並以分號 (; ) 。 |
|             -pdb \<value>              |                             指定符號伺服器路徑。 您可以使用多個路徑，並以分號 (; ) 。                             |
|                  -gmt                  |                                              將 WPP 承載時間戳記轉換為格林尼治平均時間。                                               |
|              -rl \<value>              |                                               定義從1到5的系統報告層級。 預設值為 1。                                               |
|          -summary [filename]           |                                  產生摘要報表文字檔。 如果未指定，則為 summary.txt 的檔案名。                                   |
|             -o [filename]              |                                      產生文字輸出檔。 如果未指定，則為 dumpfile.xml 的檔案名。                                      |
|           -report [filename]           |                                  產生文字輸出報告檔。 如果未指定，則為 workload.xml 的檔案名。                                   |
|                  -lr                   |                        指定較不嚴格。 這會針對不符合事件架構的事件使用最佳的工作。                         |
|           -export [filename]           |                                  產生事件架構匯出檔案。 如果未指定，則為檔案名，也就是架構。                                   |
|       [-l] \<value [value […]]>        |                                                   指定要處理的事件追蹤記錄檔。                                                    |
| -rt \<session_name [session_name […]]> |                                                指定即時事件追蹤會話資料來源。                                                |

## <a name="examples"></a>範例

- 此範例會根據兩個事件記錄檔 **logfile1** 來建立報表 **，並以** XML 格式建立傾印檔案 **logdump.xml** 。
  ```
  tracerpt logfile1.etl logfile2.etl -o logdump.xml -of XML
  ```
- 此範例會根據事件記錄檔記錄檔建立 **報告，以**XML 格式建立傾印 ** 檔案logdmp.xml** 、使用最佳的工作來識別不在架構中的事件、產生摘要報告檔案 **logdump.txt**，以及產生報表檔案 **logrpt.xml**。
  ```
  tracerpt logfile.etl -o logdmp.xml -of XML -lr -summary logdmp.txt -report logrpt.xml
  ```
- 此範例會使用兩個事件記錄檔 **logfile1 .etl** 和 **logfile2** 來產生含有預設檔案名的傾印檔案和報表檔案。
  ```
  tracerpt logfile1.etl logfile2.etl -o -report
  ```
- 此範例使用事件記錄檔 **logfile** 和 performance log **counterfile** 來產生報表檔 **logrpt.xml** 以及 Microsoft 特定的 XML 架構檔案 **schema.xml**。
  ```
  tracerpt logfile.etl counterfile.blg -report logrpt.xml -df schema.xml
  ```
- 此範例會讀取即時事件追蹤會話 NT 核心記錄器，並以 CSV 格式產生傾印檔案 **logfile.csv** 。
  ```
  tracerpt -rt NT Kernel Logger -o logfile.csv -of CSV
  ```
