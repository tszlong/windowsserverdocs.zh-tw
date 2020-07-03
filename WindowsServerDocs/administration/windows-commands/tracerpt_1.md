---
title: tracerpt
description: Tracerpt 的參考文章，它會剖析事件追蹤記錄檔、由效能監視器產生的記錄檔，以及即時事件追蹤提供者。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cb9eaf86-0ef6-4197-b6c8-9cca8a1d723c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7060932f0b7eb996d0f0934e6945665c0c91e916
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85935578"
---
# <a name="tracerpt"></a>tracerpt

**Tracerpt**命令可以用來剖析事件追蹤記錄、由效能監視器產生的記錄檔，以及即時事件追蹤提供者。 它會產生傾印檔案、報表檔案和報表架構。

## <a name="syntax"></a>Syntax

```
tracerpt <[-l] <value [value [...]]>|-rt <session_name [session_name [...]]>> [options]
```

## <a name="options"></a>選項

|              選項旗標               |                                                                    Description                                                                    |
|----------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
|                   -?                   |                                                         顯示即時線上說明。                                                          |
|          -config\<filename>           |                                                 載入包含命令選項的設定檔案。                                                  |
|                   -y                   |                                                  對所有問題回答 [是] 而不提示。                                                   |
|            -f\<XML\|HTML>             |                                                                  報表格式。                                                                   |
|         -/\<CSV\|EVTX\|XML>          |                                                         傾印格式，預設值為 XML。                                                          |
|            -df\<filename>             |                                            建立 Microsoft 特定的計數/報告架構檔案。                                            |
|            -int\<filename>            |                                            將解讀的事件結構傾印到指定的檔案。                                            |
|                  -rts                  |                        報告事件追蹤標頭中的原始時間戳記。 只能與-o、非報表或-summary 搭配使用。                         |
|            -tmf\<filename>            |                                                  指定追蹤訊息格式定義檔。                                                  |
|              -tp\<value>              |                            指定 TMF 檔案搜尋路徑。 可以使用多個路徑，並以分號分隔（;)。                            |
|              -i\<value>               | 指定提供者映射路徑。 相符的 PDB 將位於符號伺服器中。 可以使用多個路徑，並以分號分隔（;)。 |
|             -pdb\<value>              |                             指定符號伺服器路徑。 可以使用多個路徑，並以分號分隔（;)。                             |
|                  -gmt                  |                                              將 WPP 裝載時間戳記轉換為格林威治標準時間。                                               |
|              -rl \<value>              |                                               定義從1到5的系統報表層級。 預設值為 1。                                               |
|          -summary [filename]           |                                  產生摘要報告文字檔。 如果未指定，則為檔案名 summary.txt。                                   |
|             -o [filename]              |                                      產生文字輸出檔案。 如果未指定，則為檔案名 dumpfile.xml。                                      |
|           -report [filename]           |                                  產生文字輸出報告檔案。 如果未指定，則為檔案名 workload.xml。                                   |
|                  -lr                   |                        指定較少的限制。 這會針對不符合事件架構的事件使用最佳做法。                         |
|           -export [filename]           |                                  產生事件架構匯出檔案。 如果未指定 Filename，則為 schema. man。                                   |
|       [-l]\<value [value […]]>        |                                                   指定要處理的事件追蹤記錄檔。                                                    |
| -rt\<session_name [session_name […]]> |                                                指定即時事件追蹤會話資料來源。                                                |

## <a name="examples"></a>範例

- 這個範例會根據**logfile1**和**logfile2**這兩個事件記錄檔建立報表，並以 XML 格式建立傾印檔案**logdump.xml** 。
  ```
  tracerpt logfile1.etl logfile2.etl -o logdump.xml -of XML
  ```
- 這個範例會建立以事件記錄檔 **.etl**為基礎的報表、以 XML 格式建立傾印檔案**logdmp.xml** 、使用最佳的方式來識別不在架構中的事件、產生摘要報告檔案**logdump.txt**，並**logrpt.xml**產生報告檔案。
  ```
  tracerpt logfile.etl -o logdmp.xml -of XML -lr -summary logdmp.txt -report logrpt.xml
  ```
- 這個範例會使用兩個事件記錄檔**logfile1**和**logfile2**來產生具有預設檔案名的傾印檔案和報表檔案。
  ```
  tracerpt logfile1.etl logfile2.etl -o -report
  ```
- 這個範例會使用事件記錄檔 **.etl**和效能記錄檔**counterfile** ，產生報表檔案**logrpt.xml**和 Microsoft 特定的 XML 架構檔案**schema.xml**。
  ```
  tracerpt logfile.etl counterfile.blg -report logrpt.xml -df schema.xml
  ```
- 此範例會讀取即時事件追蹤會話 NT 核心記錄器，並產生 CSV 格式的傾印檔案**logfile.csv** 。
  ```
  tracerpt -rt NT Kernel Logger -o logfile.csv -of CSV
  ```
