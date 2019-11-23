---
title: Cmd
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6ec588db-31a9-4a73-a970-65a2c6f4abbe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 032fbea2039faa09753ac0c2b51e4b62004d36ac
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379341"
---
# <a name="cmd"></a>Cmd

啟動新的命令直譯器（Cmd.exe）實例。 如果使用時不含參數， **cmd**會顯示作業系統的版本和著作權資訊。

## <a name="syntax"></a>語法

```
cmd [/c|/k] [/s] [/q] [/d] [/a|/u] [/t:{<B><F>|<F>}] [/e:{on|off}] [/f:{on|off}] [/v:{on|off}] [<String>]
```

## <a name="parameters"></a>Parameters

|參數|描述|
|---------|-----------|
|/c|執行*String*所指定的命令，然後停止。|
|/k|執行*字串*所指定的命令，並繼續進行。|
|/s|在 **/c**或 **/K**之後修改*字串*的處理方式。|
|/q|關閉回應。|
|/d|停用自動執行命令。|
|/a|將內部命令輸出格式化為管道或檔案，做為美國國家標準局（ANSI）。|
|u|將內部命令輸出格式化為管道或檔案，以做為 Unicode。|
|/t： {\<B\>\<F\>\|\<F\>}|設定背景（*B*）和前景（*F*）色彩。|
|/e：開啟|啟用命令延伸模組。|
|/e： off|停用命令延伸模組。|
|/f：開啟|啟用檔案和目錄名稱的完成。|
|/f： off|停用檔案和目錄名稱完成。|
|/v：開啟|啟用延遲的環境變數擴充。|
|/v： off|停用延遲環境變數擴充。|
|\<字串 >|指定您想要執行的命令。|
|/?|在命令提示字元顯示說明。|

下表列出可用來做為 \<B\> 和 \<光圈值的有效十六進位數位\>

|值|色彩|
|-----|-----|
|0|號|
|1|藍天|
|2|綠色|
|3|綠色|
|4|紅色|
|5|變成|
|6|黃色|
|7|份|
|8|灰色|
|9|淺藍色|
|a|淺綠色|
|b|淺淺綠色|
|c|淺紅色|
|d|淺紫色|
|版|淺黃色|
|f|明亮白色|

## <a name="remarks"></a>備註

-   使用多個命令

    若要針對 \<字串 > 使用多個命令，請 **&&** 命令分隔符號來分隔它們，並將它們括在引號中。 例如：

    ```
    "<Command>&&<Command>&&<Command>"
    ``` 
 
-   處理引號

    如果您指定 **/c**或 **/k**， **Cmd**會處理其餘的*字串，* 而且只有在符合下列所有條件時，才會保留引號：  
    -   您不會使用 **/s**。
    -   您只能使用一組引號。
    -   您不會在引號內使用任何特殊字元（例如： & < > （） @ ^ |）。
    -   在引號內使用一或多個空白字元。
    -   引號內的*字串*是可執行檔的名稱。

    如果不符合先前的條件，則會藉由檢查第一個字元來驗證*字串*是否為左引號來處理。 如果第一個字元是左引號，則會與右引號一起移除。 結尾引號後面的任何文字都會保留下來。
-   執行登錄子機碼

    如果您未在*字串*中指定 **/d** ，cmd.exe 會尋找下列登錄子機碼：

    **HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\AutoRun\ REG_SZ**

    **HKEY_CURRENT_USER \Software\Microsoft\Command Processor\AutoRun\ REG_EXPAND_SZ**

    如果有一或兩個登錄子機碼存在，則會在所有其他變數之前執行。

> [!CAUTION]
> 不正確地編輯登錄可能會對系統造成嚴重的損害。 變更登錄之前，您應該先備份電腦所有的重要資料。

-   啟用和停用命令延伸模組

    Windows XP 預設會啟用命令延伸模組。 您可以使用 **/e： off**，針對特定進程停用它們。 您可以藉由設定下列**REG_DWORD**值，為電腦或使用者會話上的所有**cmd**命令列選項啟用或停用延伸模組：

    **HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\EnableExtensions\ REG_DWORD**

    **HKEY_CURRENT_USER \Software\Microsoft\Command Processor\EnableExtensions\ REG_DWORD**

    使用 Regedit.exe，在登錄中將**REG_DWORD**值設定為**0 × 1** （已啟用）或**0 × 0** （已停用）。 使用者指定的設定會優先于電腦設定，而命令列選項的優先順序高於登錄設定。

> [!CAUTION]
> 不正確地編輯登錄可能會對系統造成嚴重的損害。 變更登錄之前，您應該先備份電腦所有的重要資料。

    When you enable command extensions, the following commands are affected:  
    -  **assoc**
    -  **call**
    -  **chdir (cd)**
    -  **color**
    -  **del (erase)**
    -  **endlocal**
    -  **for**
    -  **ftype**
    -  **goto**
    -  **if**
    -  **mkdir (md)**
    -  **popd**
    -  **prompt**
    -  **pushd**
    -  **set**
    -  **setlocal**
    -  **shift**
    -  **start** (also includes changes to external command processes)

-   啟用延遲的環境變數擴充

    如果您啟用延遲的環境變數擴充，您可以在執行時間使用驚嘆號字元來取代環境變數的值。
-   啟用檔案和目錄名稱完成

    檔案和目錄名稱的自動完成預設為不啟用。 您可以使用 **/f：** {**on**|**off**} 來啟用或停用**cmd**命令之特定進程的檔案名完成。 您可以藉由設定下列**REG_DWORD**值，針對電腦上**cmd**命令的所有進程或使用者登入會話，啟用或停用檔案和目錄名稱完成：

    **HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\CompletionChar\ REG_DWORD**

    **HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\PathCompletionChar\ REG_DWORD**

    **HKEY_CURRENT_USER \Software\Microsoft\Command Processor\CompletionChar\ REG_DWORD**

    **HKEY_CURRENT_USER \Software\Microsoft\Command Processor\PathCompletionChar\ REG_DWORD**

    若要設定**REG_DWORD**值，請執行 regedit.exe，並針對特定函式使用控制字元的十六進位值（例如， **0 × 9**是 TAB， **0 × 08**是倒退鍵）。 使用者指定的設定會優先于電腦設定，而命令列選項的優先順序高於登錄設定。

> [!CAUTION]
> 不正確地編輯登錄可能會對系統造成嚴重的損害。 變更登錄之前，您應該先備份電腦所有的重要資料。

如果您使用 **/f： on**啟用檔案和目錄名稱完成功能，請使用 Ctrl + D 進行目錄名稱自動完成，然後按 Ctrl + f 進行檔案名自動完成。 若要在登錄中停用特定的完成字元，請使用空白字元 [**0 × 20**] 的值，因為它不是有效的控制字元。

當您按下 CTRL + D 或 CTRL + F 時， **cmd**會處理檔案和目錄名稱的完成。 這些按鍵組合函數會將萬用字元附加至*字串*（如果沒有的話），建立符合的路徑清單，然後顯示第一個相符的路徑。 如果沒有相符的路徑，檔案和目錄名稱完成功能就會發出嗶聲，而且不會變更顯示。 若要在相符路徑的清單中移動，請重複按下 CTRL + D 或 CTRL + F。 若要向後移動清單，請同時按 SHIFT 鍵和 CTRL + D 或 CTRL + F。 若要捨棄已儲存的相符路徑清單，並產生新的清單，請編輯*字串*，然後按下 Ctrl + D 或 Ctrl + F。 如果您在 CTRL + D 和 CTRL + F 之間切換，則會捨棄相符路徑的已儲存清單，並產生新的清單。 CTRL + D 和 CTRL + F 按鍵組合的唯一差別在於，CTRL + D 只會符合目錄名稱，而 CTRL + F 則會同時符合檔案和目錄名稱。 如果您在任何內建目錄命令（也就是**CD**、 **MD**或**RD**）上使用檔案和目錄名稱完成，則會假設目錄已完成。

如果您在比對路徑前後加上引號，檔案和目錄名稱完成就會正確地處理包含空白字元或特殊字元的檔案名。

下列特殊字元需要加上引號： & < > [] {} ^ =;！ ' +，' ~ [空白字元]。

如果您提供的資訊包含空格，請使用引號括住文字（例如「電腦名稱稱」）。

如果您從*字串*內處理檔案和目錄名稱自動完成，則會捨棄游標右邊*路徑*的任何部分（在*字串*中處理完成的位置）。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
