---
title: start
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0173f9b3-5cd7-4edb-b01e-d02193b4fadc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1fb0875c972f8259b47f48ef84ed486fc678d8b0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370893"
---
# <a name="start"></a>start



啟動個別的 [命令提示字元] 視窗，以執行指定的程式或命令。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
start ["<Title>"] [/d <Path>] [/i] [{/min | /max}] [{/separate | /shared}] [{/low | /normal | /high | /realtime | /abovenormal | belownormal}] [/affinity <HexAffinity>] [/wait] [/b {<Command> | <Program>} [<Parameters>]]
```

## <a name="parameters"></a>Parameters

|參數|描述|
|---------|-----------|
|「\<標題 >」|指定要在 [命令提示字元] 視窗標題列中顯示的標題。|
|/d \<路徑 >|指定啟動目錄。|
|/i|將 Cmd.exe 啟動環境傳遞至新的命令提示字元視窗。 如果未指定 **/i** ，則會使用目前的環境。|
|/min \|/max|指定以最小化（ **/min**）或最大化（ **/max**）新的命令提示字元視窗。|
|/separate \|/shared|在個別的記憶體空間（ **/separate**）或共用記憶體空間（ **/shared**）中啟動16位程式。 64位平臺上不支援這些選項。|
|/low \|/normal \|/high ... program.exe \|/realtime \|/abovenormal \|/belownormal|以指定的優先權類別啟動應用程式。 有效的優先權類別值為 **/low**、 **/normal**、 **/high ... program.exe**、 **/realtime**、 **/abovenormal**和 **/belownormal**。|
|/affinity \<HexAffinity >|將指定的處理器親和性遮罩（以十六進位數表示）套用至新的應用程式。|
|/wait|啟動應用程式，並等候它結束。|
|/b|啟動應用程式，而不開啟新的命令提示字元視窗。 除非應用程式啟用 CTRL + C 處理，否則會忽略 CTRL + C 處理。 使用 CTRL + BREAK 來中斷應用程式。|
|/b \<命令 > \| \<程式 >|指定要啟動的命令或程式。|
|\<參數 >|指定要傳遞給命令或程式的參數。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

- 您可以藉由將檔案的名稱輸入為命令，以透過檔案關聯來執行非檔的檔案。
- 當您執行的命令包含字串 "CMD" 做為第一個不含副檔名或路徑辨識符號的權杖時，"CMD" 會取代為 COMSPEC 變數的值。 這可防止使用者從目前目錄中挑選**cmd** 。
- 當您執行32點陣圖形使用者介面（GUI）應用程式時， **cmd**不會等待應用程式結束，然後再返回命令提示字元。 如果您從命令腳本執行應用程式，就不會發生這種行為。
- 當您執行使用第一個不含擴充功能之權杖的命令時，Cmd.exe 會使用 PATHEXT 環境變數的值來決定要尋找的延伸模組和順序。 PATHEXT 變數的預設值為：  
  ```
  .COM;.EXE;.BAT;.CMD 
  ```  
  請注意，語法與 PATH 變數相同，以分號分隔每個副檔名。
- 當它搜尋可執行檔時，如果有任何延伸模組沒有相符專案，就會**開始**檢查名稱是否符合目錄名稱。 如果**有，就會開啟**該路徑上的 Explorer。

## <a name="BKMK_examples"></a>典型

若要在命令提示字元中啟動 Myapp 程式，並繼續使用目前的命令提示字元視窗，請輸入：
```
start myapp 
```
若要在另一個最大化的命令提示字元視窗中，查看**啟動**命令列說明主題，請輸入：
```
start /max start /?
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
