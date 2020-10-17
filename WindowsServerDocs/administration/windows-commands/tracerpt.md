---
title: tracerpt
description: Tracerpt 命令的參考文章，此命令會剖析事件追蹤記錄檔、效能監視器所產生的記錄檔，以及即時事件追蹤提供者。
ms.topic: reference
ms.assetid: cb9eaf86-0ef6-4197-b6c8-9cca8a1d723c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 1e468dab3c99219560047668f9f1bd001e8e451e
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156672"
---
# <a name="tracerpt"></a>tracerpt

**Tracerpt**命令會剖析事件追蹤記錄檔、效能監視器所產生的記錄檔，以及即時事件追蹤提供者。 它也會產生傾印檔案、報表檔和報表架構。

## <a name="syntax"></a>語法

```
tracerpt <[-l] <value [value [...]]>|-rt <session_name [session_name [...]]>> [options]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| -config `<filename>` | 指定要載入的設定檔案，其中包含您的命令選項。 |
| -y | 指定針對所有問題回答 **yes** ，而不提示。 |
| -f `<XML | HTML>` | 指定報表檔案格式。 |
| -/ `<CSV | EVTX | XML>` | 指定傾印檔案格式。 預設值為 **XML*。 |
| -df `<filename>` | 指定要建立 Microsoft 特定的計數/報告架構檔案。 |
| -int `<filename>` | 指定將解讀的事件結構傾印至指定的檔案。 |
| -rts | 指定在事件追蹤標頭中加入報表的原始時間戳記。 只能搭配 **-o**使用。 不支援 **-report** 或 **-summary**。 |
| -tmf `<filename>` | 指定要使用的追蹤訊息格式定義檔。 |
| -tp `<value>` | 指定 TMF 檔案搜尋路徑。 您可以使用多個路徑，並以分號 (; ) 。 |
| -i `<value>` | 指定提供者映射路徑。 相符的 PDB 將位於符號伺服器中。 您可以使用多個路徑，並以分號 (; ) 。 |
| -pdb `<value>` | 指定符號伺服器路徑。 您可以使用多個路徑，並以分號 (; ) 。 |
| -gmt | 指定將 WPP 承載時間戳記轉換為格林尼治平均時間。 |
| -rl `<value>` | 指定從1到5的系統報告層級。 預設值為 *1*。 |
| -summary [filename] | 指定要建立摘要報表文字檔。 如果未指定，則會 *summary.txt*檔案名。 |
| -o [filename] | 指定要建立文字輸出檔。 如果未指定，則會 *dumpfile.xml*檔案名。 |
| -report [filename] | 指定要建立文字輸出報告檔。 如果未指定，則會 *workload.xml*檔案名。 |
| -lr | 指定較不嚴格。 這會針對不符合事件架構的事件使用最佳的做法。 |
| -export [filename] | 指定要建立事件架構匯出檔案。 如果未指定檔案名，則為 *架構. man*。 |
| [-l] `<value [value […]]>` | 指定要處理的事件追蹤記錄檔。 |
| -rt `<session_name [session_name […]]>` | 指定即時事件追蹤會話資料來源。 |
| -? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要根據兩個事件記錄檔建立報表*logfile1 .etl*和*logfile2*，並以*XML*格式建立傾印檔案*logdump.xml* ，請輸入：

```
tracerpt logfile1.etl logfile2.etl -o logdump.xml -of XML
```

若要根據事件記錄檔 *.etl*建立報告，以 XML 格式建立傾印檔案 *logdmp.xml* ，若要使用最佳的方式來識別不在架構中的事件，以及產生摘要報告檔案 *logdump.txt* 和報表檔 *logrpt.xml*，請輸入：

```
tracerpt logfile.etl -o logdmp.xml -of XML -lr -summary logdmp.txt -report logrpt.xml
```

若要使用這兩個事件記錄檔 *logfile1 .etl* 和 *logfile2* 來產生傾印檔案，並使用預設檔案名來報告檔案，請輸入：

```
tracerpt logfile1.etl logfile2.etl -o -report
```

若要使用事件記錄檔記錄檔 *.etl* 和 performance log *counterfile* 來產生報表檔 *logrpt.xml* 以及 Microsoft 特定的 XML 架構檔案 *schema.xml*，請輸入：

```
tracerpt logfile.etl counterfile.blg -report logrpt.xml -df schema.xml
```

若要讀取即時事件追蹤會話 NT 核心記錄器，並以*CSV*格式產生傾印檔案*logfile.csv* ，請輸入：

```
tracerpt -rt NT Kernel Logger -o logfile.csv -of CSV
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
