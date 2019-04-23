---
title: Cmd
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 966ac7f70984dff6d26265e07a26a6eebcde9fb6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874389"
---
# <a name="cmd"></a>Cmd



啟動命令直譯器，Cmd.exe 的新執行個體。 如果未指定參數，使用**cmd**顯示作業系統的版本和著作權資訊。

## <a name="syntax"></a>語法

```
cmd [/c|/k] [/s] [/q] [/d] [/a|/u] [/t:{<B><F>|<F>}] [/e:{on|off}] [/f:{on|off}] [/v:{on|off}] [<String>]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/c|所指定的命令會執行*字串*，然後停止。|
|/k|所指定的命令會執行*字串*並繼續進行。|
|/s|修改的處理方式*字串*之後 **/c**或是 **/k**。|
|/q|關閉回應。|
|/d|停用自動執行命令的執行。|
|/a|管道或檔案的內部命令輸出格式化為美國國家標準 Institute (ANSI)。|
|/u|管道或檔案的內部命令輸出格式化為 Unicode。|
|/t:{\<B\>\<F\>\|\<F\>}|設定背景 (*B*)] 和 [前景 (*F*) 色彩。|
|/e： 上|啟用命令擴充功能。|
|/e： 關閉|停用的命令擴充功能。|
|/f： 上|啟用檔案及目錄名稱完成。|
|/f:off|停用檔案及目錄名稱完成。|
|/v： 上|啟用延遲的環境變數擴充。|
|/v： 關閉|停用延遲環境變數擴充。|
|\<String>|指定您想要執行的命令。|
|/?|在命令提示字元顯示說明。|

下表列出有效的十六進位數字，您可以使用的值作為\<B\>和\<F\>

|值|色彩|
|-----|-----|
|0|黑色|
|1|藍色|
|2|綠色|
|3|青色|
|4|紅色|
|5|紫色|
|6|黃色|
|7|白皮書|
|8|灰色|
|9|淺藍色|
|a|淺綠色|
|b|淡藍|
|c|淡紅色|
|d|淺紫|
|E|淺黃色|
|f|亮白色|

## <a name="remarks"></a>備註

-   使用多個命令

    若要使用的多個命令\<字串 >，藉此命令分隔符號分隔**&&** 並將其括在引號內。 例如:   
    ```
    "<Command>&&<Command>&&<Command>"
    ```  
-   處理引號

    如果您指定 **/c**或 **/k**， **cmd**程序的其餘部分*字串*和引號會保留，只有當所有下列的條件成立：  
    -   您不使用 **/s**。
    -   您使用一組引號。
    -   您未使用引號內的任何特殊字元 (例如： & @ < > （) ^ |)。
    -   您使用的引號內的一或多個空格字元。
    -   *字串*引號內是可執行檔的名稱。

    如果不符合先前的條件，*字串*處理藉由檢查以確認它是否左引號的第一個字元。 如果第一個字元是左引號，它會移除以及右引號。 會保留結尾引號內的任何文字。
-   執行登錄子機碼

    如果您未指定 **/d**中*字串*，Cmd.exe 尋找下列登錄子機碼：

    **HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\AutoRun\REG_SZ**

    **HKEY_CURRENT_USER\Software\Microsoft\Command Processor\AutoRun\REG_EXPAND_SZ**

    如果一或兩個登錄子機碼存在，它們會執行所有其他變數之前。

> [!CAUTION]
> 不正確地編輯登錄可能會對系統造成嚴重的損害。 變更登錄之前，您應該先備份電腦所有的重要資料。
-   啟用和停用擴充命令

    在 Windows XP 中預設會啟用命令延伸模組。 您可以使用停用這些特定處理序 **/e： 關閉**。 您可以啟用或停用所有的延伸模組**cmd**電腦或使用者的工作階段，藉由設定下列的命令列選項**REG_DWORD**值：

    **HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\EnableExtensions\REG_DWORD**

    **兩 Processor\EnableExtensions\REG_DWORD**

    設定**REG_DWORD**設為值**0 × 1** （啟用） 或**0 × 0** （停用） 在登錄中使用 Regedit.exe。 使用者指定的設定值優先於電腦設定，以及命令列選項的優先順序高於登錄設定。

>     [!CAUTION]
>     Incorrectly editing the registry may severely damage your system. Before making changes to the registry, you should back up any valued data on the computer.

    When you enable command extensions, the following commands are affected:  
    -   **assoc**
    -   **call**
    -   **chdir (cd)**
    -   **color**
    -   **del (erase)**
    -   **endlocal**
    -   **for**
    -   **ftype**
    -   **goto**
    -   **if**
    -   **mkdir (md)**
    -   **popd**
    -   **prompt**
    -   **pushd**
    -   **set**
    -   **setlocal**
    -   **shift**
    -   **start** (also includes changes to external command processes)
-   啟用延遲的環境變數擴充

    如果您啟用延遲的環境變數擴充，您可以使用環境變數的值替換成在執行階段的驚嘆號字元。
-   啟用檔案及目錄名稱完成

    依預設未啟用檔案及目錄名稱完成。 您可以啟用或停用特定的程序的檔案名稱完成**cmd**命令搭配 **/f**{**上**|**關閉**}. 您可以啟用或停用的所有處理程序的檔案和目錄名稱完成**cmd**命令的電腦或使用者登入工作階段藉由設定下列**REG_DWORD**值：

    **HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\CompletionChar\REG_DWORD**

    **HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\PathCompletionChar\REG_DWORD**

    **HKEY_CURRENT_USER\Software\Microsoft\Command Processor\CompletionChar\REG_DWORD**

    **HKEY_CURRENT_USER\Software\Microsoft\Command Processor\PathCompletionChar\REG_DWORD**

    若要設定**REG_DWORD**值、 執行 Regedit.exe，並使用特定的函式的控制字元的十六進位值 (例如**0 × 9**  索引標籤和**0 × 08**是退格鍵）。 使用者指定的設定值優先於電腦設定，以及命令列選項的優先順序高於登錄設定。

> [!CAUTION]
> 不正確地編輯登錄可能會對系統造成嚴重的損害。 變更登錄之前，您應該先備份電腦所有的重要資料。

如果您使用啟用檔案及目錄名稱完成 **/f： 上**，檔案名稱完成使用 CTRL + D，目錄名稱完成和 CTRL + F。 若要停用特定完成字元在登錄中的，使用泛空白字元的值 [**0 × 20**] 因為它不是有效的控制字元。

當您按下 CTRL + D 或 CTRL + F **cmd**處理檔案及目錄名稱完成。 這些按鍵組合函式將萬用字元*字串*（如果尚未存在），建置一份相符，而且顯示的第一個符合路徑的路徑。 如果沒有任何路徑相符，檔案和目錄的名稱完成函式就會發出嗶聲，並不會變更顯示。 若要瀏覽的比對的路徑清單，請按 CTRL + D 或 CTRL + F 重複。 若要向後瀏覽清單，請按 SHIFT 鍵和 CTRL + D 或 CTRL + F 同時。 若要放棄比對路徑的已儲存的清單，並產生新的清單，請編輯*字串*按 CTRL + D 或 CTRL + F。 CTRL + D 和 CTRL + F 之間切換時，比對路徑的已儲存的清單會捨棄，並產生新的清單。 CTRL + D 只會比對目錄名稱，和 CTRL + F 符合檔案和目錄名稱的組合鍵 CTRL + D 和 CTRL + F 唯一的差別。 如果您使用檔案及目錄名稱完成任何內建目錄命令 (亦即**CD**， **MD**，或**RD**)，則採用目錄完成。

檔案及目錄名稱完成正確處理檔案名稱包含空格或特殊字元，如果您將引號括住的比對的路徑。

下列特殊字元需要引號： & < > [] {} ^ =; ！ '+、' ~ [空格]。

如果您提供的資訊包含空格，請使用引號括住的文字 （例如，「 電腦名稱 」）。

如果處理序內的檔案和目錄名稱完成*字串*，任何屬於*路徑*捨棄右邊的資料指標 (在*字串*位置完成已處理）。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
