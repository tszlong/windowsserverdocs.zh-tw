---
title: start
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ab7388633681120442544adf4ee0e337d8599854
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441221"
---
# <a name="start"></a>start



啟動另一個命令提示字元視窗，執行指定的程式或命令。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
start ["<Title>"] [/d <Path>] [/i] [{/min | /max}] [{/separate | /shared}] [{/low | /normal | /high | /realtime | /abovenormal | belownormal}] [/affinity <HexAffinity>] [/wait] [/b {<Command> | <Program>} [<Parameters>]]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|「\<標題 > 」|指定要在命令提示字元 視窗標題列中顯示的標題。|
|/d \<Path>|指定 [啟動] 目錄。|
|/i|給新的 [命令提示字元] 視窗中的 Cmd.exe 啟動環境。 如果 **/i**未指定，會使用目前的環境。|
|/min  \| /最大|指定最小化 ( **/min**) 或最大化 ( **/最大**) 新的 [命令提示字元] 視窗。|
|/separate \| /shared|開始在不同的記憶體空間中的 16 位元程式 ( **/separate**) 或共用記憶體空間 ( **/ 共用**)。 在 64 位元平台上不支援這些選項。|
|/ 低\|/正常\|/高\|/realtime \| /abovenormal \| /belownormal|啟動指定的優先順序類別中的應用程式。 有效的優先權類別的值 **/低**， **/一般**， **/高**， **/realtime**， **/abovenormal**，並 **/belownormal**。|
|/affinity \<HexAffinity >|適用於新的應用程式指定的處理器親和性遮罩 （以十六進位數字）。|
|/wait|啟動應用程式，並等候它結束。|
|/b|啟動應用程式，而不需要開啟新的 [命令提示字元] 視窗。 除非應用程式，可讓 「 CTRL + C 鍵處理，則會忽略 CTRL + C 鍵處理。 若要中斷應用程式中使用 CTRL + BREAK。|
|/ b\<命令 > \| \<程式 >|指定要啟動的命令或程式。|
|\<參數 >|指定要傳遞至程式的命令參數。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

- 您可以輸入檔案的名稱做為命令透過檔案關聯的檔案，以執行非執行檔的檔案。
- 當您為第一個語彙基元，不含副檔名或路徑的限定詞執行命令，以包含字串"CMD"時，"CMD"會取代 COMSPEC 變數的值。 這可防止使用者挑選**cmd**從目前的目錄。
- 當您執行的 32 位元的圖形化使用者介面 (GUI) 應用程式中， **cmd**不會等到應用程式結束然後再回到命令提示字元。 如果您從命令指令碼執行的應用程式，則不會發生這種行為。
- 當您執行的命令，會使用不包含延伸模組的第一個語彙基元時，Cmd.exe 會使用 PATHEXT 環境變數的值來決定哪些擴充功能，尋找並以何種順序。 附檔名的預設值是：  
  ```
  .COM;.EXE;.BAT;.CMD 
  ```  
  請注意，語法是 PATH 變數，以分號區隔每個擴充功能相同。
- 搜尋可執行檔，如果沒有任何延伸模組，在沒有相符項目時**啟動**檢查名稱是否符合目錄名稱。 若是如此，請**啟動**該路徑上開啟 Explorer.exe。

## <a name="BKMK_examples"></a>範例

若要開始在命令提示字元，Myapp 程式，並保留使用目前的 [命令提示字元] 視窗，請輸入：
```
start myapp 
```
若要檢視**啟動**在個別的命令列說明主題最大化的命令提示字元視窗中，輸入：
```
start /max start /?
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
