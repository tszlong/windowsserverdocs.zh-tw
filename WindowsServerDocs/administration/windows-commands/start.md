---
title: start
description: Start 命令的參考文章，它會啟動個別的命令提示字元視窗來執行指定的程式或命令。
ms.topic: reference
ms.assetid: 0173f9b3-5cd7-4edb-b01e-d02193b4fadc
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6c154cd0c3cab4520f64c77d2ecc920dfc8bdcc9
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718265"
---
# <a name="start"></a>start

啟動個別的命令提示字元視窗，以執行指定的程式或命令。

## <a name="syntax"></a>語法

```
start [<title>] [/d <path>] [/i] [{/min | /max}] [{/separate | /shared}] [{/low | /normal | /high | /realtime | /abovenormal | belownormal}] [/affinity <hexaffinity>] [/wait] [/elevate] [/b] [<command> [<parameter>... ] | <program> [<parameter>... ]]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<title>` | 指定要顯示在 [ **命令提示** 字元] 視窗標題列中的標題。 |
| /d `<path>` | 指定啟動目錄。 |
| /i | 將 Cmd.exe 啟動環境傳遞至新的 **命令提示** 字元視窗。 如果未指定 **/i** ，則會使用目前的環境。 |
| `{/min | /max}` | 指定將 (**/min**) ，或) 新的 [**命令提示**字元] 視窗中，將 (**/max**最大化。 |
| `{/separate | /shared}` | 在不同的記憶體空間中啟動16位程式 (**/separate**) 或共用記憶體空間 (**/shared**) 。 64位平臺上不支援這些選項。 |
| `{/low | /normal | /high | /realtime | /abovenormal | belownormal}` | 以指定的 priority 類別啟動應用程式。 |
| /affinity `<hexaffinity>` | 套用指定的處理器親和性遮罩 (以) 至新應用程式的十六進位數位表示。 |
| /wait | 啟動應用程式，並等候其結束。 |
| /elevate | 以系統管理員身分執行應用程式。 |
| /b | 啟動應用程式，而不開啟新的 **命令提示** 字元視窗。 除非應用程式啟用 CTRL + C 處理，否則會忽略 CTRL + C 處理。 使用 CTRL + BREAK 來中斷應用程式。 |
| `[<command> [<parameter>... ] | <program> [<parameter>... ]]` | 指定要啟動的命令或程式。 |
| `<parameter>` | 指定要傳遞至命令或程式的參數。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 您可以藉由將檔案的名稱輸入為命令，以透過檔案關聯執行無法執行檔。

- 如果您在沒有副檔名或路徑辨識符號的情況下，執行包含字串 CMD 作為第一個 token 的命令，則會將 CMD 取代為 COMSPEC 變數的值。 這可防止使用者從目前目錄中挑選 **cmd** 。

- 如果您 (GUI) 應用程式執行32位的圖形化使用者介面， **cmd** 不會等待應用程式在返回命令提示字元之前結束。 如果您從命令腳本執行應用程式，則不會發生這種行為。

- 如果您執行的命令使用不包含擴充功能的第一個權杖，Cmd.exe 會使用 PATHEXT 環境變數的值來決定要尋找的延伸模組和順序。 PATHEXT 變數的預設值為：

  ```
  .COM;.EXE;.BAT;.CMD;.VBS;.VBE;.JS;.JSE;.WSF;.WSH;.MSC
  ```

  請注意，語法與 PATH 變數相同，並以分號 (; ) 分隔每個副檔名。

- 搜尋可執行檔時，如果任何擴充功能都沒有相符專案，請 **開始** 檢查名稱是否與目錄名稱相符。 如果 **有，就會在** 該路徑上開啟 Explorer.exe。

## <a name="examples"></a>範例

若要在命令提示字元中啟動 *Myapp* 程式，並保留使用目前的 **命令提示** 字元視窗，請輸入：

```
start Myapp
```

若要在另一個最大化的**命令提示**字元視窗中，查看**啟動**命令列說明主題，請輸入：

```
start /max start /?
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
