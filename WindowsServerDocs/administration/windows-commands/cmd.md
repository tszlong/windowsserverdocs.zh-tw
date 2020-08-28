---
title: cmd
description: Cmd 命令的參考文章，此命令會啟動新的命令直譯器實例，Cmd.exe。
ms.topic: reference
ms.assetid: 6ec588db-31a9-4a73-a970-65a2c6f4abbe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b782a93d4c61f43bbe45497871fe66f29ef972a4
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89030976"
---
# <a name="cmd"></a>cmd

啟動命令直譯器的新實例，Cmd.exe。 如果使用時不含參數， **cmd** 會顯示作業系統的版本和著作權資訊。

## <a name="syntax"></a>語法

```
cmd [/c|/k] [/s] [/q] [/d] [/a|/u] [/t:{<b><f> | <f>}] [/e:{on | off}] [/f:{on | off}] [/v:{on | off}] [<string>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /C | 執行 *字串* 所指定的命令，然後停止。 |
| /k | 執行 *字串* 所指定的命令，並繼續進行。 |
| /s | 在 **/c**或 **/k**之後修改*字串*的處理方式。 |
| /q | 關閉回應。 |
| /d | 停用自動執行命令。 |
| /a | 將內部命令輸出格式化為管道或檔案，做為美國國家標準局 (ANSI) 。 |
| /U | 將內部命令輸出格式化為管線，或將檔案格式化為 Unicode。 |
| /t： {`<b><f>` | `<f>`} | 設定背景 (*b*) 和前景 (*f*) 色彩。 |
| /e：開啟 | 啟用命令延伸模組。 |
| /e： off | 停用命令延伸模組。 |
| /f：開啟 | 啟用檔案和目錄名稱的完成。 |
| /f： off | 停用檔案和目錄名稱自動完成。 |
| /v：開啟 | 啟用延遲的環境變數擴充。 |
| /v： off | 停用延遲的環境變數擴充。 |
| `<string>` | 指定您想要執行的命令。 |
| /? | 在命令提示字元顯示說明。 |

下表列出您可以用來做為和值的有效十六進位數位 `<b>` `<f>` ：

| 值 | 色彩 |
| ----- | ----- |
| 0 | 黑色 |
| 1 | 藍色 |
| 2 | 綠色 |
| 3 | Aqua |
| 4 | 紅色 |
| 5 | 紫色 |
| 6 | 黃色 |
| 7 | 白色 |
| 8 | 灰色 |
| 9 | 淺藍色 |
| a | 淺綠 |
| b | 淺綠色 |
| c | 淺紅色 |
| d | 淺紫色 |
| e | 淺黃色 |
| f | 亮白色 |

## <a name="remarks"></a>備註

- 若要使用多個命令 `<string>` ，請使用命令分隔符號分隔它們， **&&** 並將其括在引號中。 例如：

    ```
    "<command1>&&<command2>&&<command3>"
    ```

- 如果您指定 **/c** 或 **/k**，則只有在符合下列所有條件時，才會保留 **cmd** 進程、 *字串*的其餘部分和引號：

    - 您也不能使用 **/s**。

    - 您只能使用一組引號。

    - 您不會在引號內使用任何特殊字元 (例如：  & < >  ( ) @ ^ |) 。

    - 您可以使用引號內的一或多個空白字元。

    - 引號內的 *字串* 是可執行檔的名稱。

    如果不符合上述條件，則會藉由檢查第一個字元來處理 *字串* ，以確認它是否為左引號。 如果第一個字元是左雙引號，則會將它與右引號一起移除。 在右引號之後的任何文字都會保留下來。

- 如果您未在*字串*中指定 **/d** ，Cmd.exe 會尋找下列登錄子機碼：

    - **HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\AutoRun\ REG_SZ**

    - **HKEY_CURRENT_USER \Software\Microsoft\Command Processor\AutoRun\ REG_EXPAND_SZ**

    如果有一個或兩個登錄子機碼，則會在所有其他變數之前執行。

    > [!CAUTION]
    > 不正確地編輯登錄可能會對系統造成嚴重的損害。 變更登錄之前，您應該先備份電腦所有的重要資料。

- 您可以使用 **/e： off**來停用特定進程的命令延伸。 您可以設定下列**REG_DWORD**值，以啟用或停用電腦或使用者會話**上所有命令**行選項的延伸模組：

    - **HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\EnableExtensions\ REG_DWORD**

    - **HKEY_CURRENT_USER \Software\Microsoft\Command Processor\EnableExtensions\ REG_DWORD**

    使用) ，將 **REG_DWORD** 值設定為 **0 × 1** (啟用) 或 **0 × 0** (停用 Regedit.exe。 使用者指定的設定優先于電腦設定，而命令列選項優先于登錄設定。

    > [!CAUTION]
    > 不正確地編輯登錄可能會對系統造成嚴重的損害。 變更登錄之前，您應該先備份電腦所有的重要資料。

    當您啟用命令延伸模組時，會影響下列命令：
    - **assoc**

    - **call**

    - **chdir (cd) **

    - **color**

    - **del (清除) **

    - **endlocal**

    - **for**

    - **ftype**

    - **goto**

    - **if**

    - **mkdir (md) **

    - **popd**

    - **prompt**

    - **pushd**

    - **set**

    - **setlocal**

    - **shift**

    - **啟動** (也包含外部命令進程的變更) 

- 如果您啟用延遲的環境變數擴充，您可以在執行時間使用驚嘆號字元來取代環境變數的值。

- 檔案和目錄名稱完成預設不會啟用。 您可以使用 **/f：**{**on**off} 來啟用或停用**cmd**命令之特定進程的檔案名稱完成  |  ** **。 您可以藉由設定下列**REG_DWORD**值，啟用或停用電腦上**cmd**命令的所有進程或使用者登入會話的檔案和目錄名稱完成：

    - **HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\CompletionChar\ REG_DWORD**

    - **HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\PathCompletionChar\ REG_DWORD**

    - **HKEY_CURRENT_USER \Software\Microsoft\Command Processor\CompletionChar\ REG_DWORD**

    - **HKEY_CURRENT_USER \Software\Microsoft\Command Processor\PathCompletionChar\ REG_DWORD**

    若要設定 **REG_DWORD** 值，請執行 Regedit.exe，並針對特定函式使用控制字元的十六進位值 (例如， **0 × 9** 是 TAB，而 **0 × 08** 是倒退鍵) 。 使用者指定的設定優先于電腦設定，而命令列選項優先于登錄設定。

    > [!CAUTION]
    > 不正確地編輯登錄可能會對系統造成嚴重的損害。 變更登錄之前，您應該先備份電腦所有的重要資料。

- 如果您使用 **/f： on**來啟用檔案和目錄名稱完成，請使用 **ctrl + D** 進行目錄名稱自動完成，並使用 **ctrl + f** 來完成檔案名。 若要在登錄中停用特定的完成字元，請使用空白字元 [**0 × 20**] 的值，因為它不是有效的控制字元。

  - 按 **ctrl + D** 或 **CTRL + F**，會處理檔案和目錄名稱的完成。 這些按鍵組合函式會將萬用字元附加至 *字串* (如果沒有) ，則會建立符合的路徑清單，然後顯示第一個相符的路徑。<p>如果沒有相符的路徑，檔案和目錄名稱完成函式會發出嗶聲，且不會變更顯示。 若要在相符路徑的清單中移動，請重複按 **ctrl + D** 或 **ctrl + F** 。 若要向後移動清單，請同步選取 **SHIFT** 鍵和 **ctrl + D** 或 **ctrl + F** 。 若要捨棄符合路徑的已儲存清單，並產生新的清單，請編輯 *字串* ，然後按下 **ctrl + D** 或 **ctrl + F**。 如果您在 **ctrl + D** 和 **ctrl + F**之間切換，則會捨棄相符路徑的已儲存清單，並產生新的清單。 按鍵組合 **ctrl + d** 和 **ctrl + f** 之間的唯一差異在於， **ctrl + D** 只符合目錄名稱，而 **ctrl + F** 則符合檔案和目錄名稱。 如果您在任何內建目錄命令上使用檔案和目錄名稱完成 (也就是、 **CD**、 **MD**或 **RD**) ，則會假設為目錄自動完成。

  - 如果您在相符的路徑周圍加上引號，檔案和目錄名稱完成會正確地處理包含空白字元或特殊字元的檔案名。

  - 您必須使用引號括住下列特殊字元：  & < >  [] {} ^ =;！ ' +，' ~ [空白字元]。

  - 如果您提供的資訊包含空格，您必須使用引號括住文字 (例如「電腦名稱稱」 ) 。

  - 如果您處理 *字串*內的檔案和目錄名稱完成，資料指標右邊 *路徑* 的任何部分都會被捨棄 (在 *字串* 中) 處理完成的時間點。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
