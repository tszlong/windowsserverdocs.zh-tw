---
title: setlocal
description: Setlocal 命令的參考文章，此命令會啟動批次檔中環境變數的當地語系化。
ms.topic: reference
ms.assetid: e4e4b6d3-3f1a-4851-a782-25ee2470e16e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2ad4e4c6bd023a5722d823a3bf67c5503329bda5
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388998"
---
# <a name="setlocal"></a>setlocal

開始對批次檔中的環境變數進行當地語系化。 當地語系化會繼續進行，直到遇到相符的 **endlocal** 命令或到達批次檔的結尾為止。

## <a name="syntax"></a>語法

```
setlocal [enableextensions | disableextensions] [enabledelayedexpansion | disabledelayedexpansion]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| enableextensions | 在遇到相符的 **endlocal** 命令之前啟用命令延伸模組，無論在執行 **setlocal** 命令之前的設定為何。 |
| disableextensions | 除非您在執行**setlocal**命令之前的設定，否則會停用命令延伸模組，直到遇到相符的**endlocal**命令為止。 |
| enabledelayedexpansion | 啟用延遲的環境變數擴充，直到遇到 [比對 **endlocal** ] 命令，而不考慮執行 **setlocal** 命令之前的設定。 |
| disabledelayedexpansion | 停用延遲的環境變數擴充，直到遇到 [比對 **endlocal** ] 命令，而不考慮執行 **setlocal** 命令之前的設定。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 如果您在腳本或批次檔外面使用 **setlocal** ，則不會有任何作用。

- 當您執行批次檔時，請使用 **setlocal** 來變更環境變數。 執行 **setlocal** 之後所做的環境變更是批次檔的本機。 Cmd.exe 程式在遇到 **endlocal** 命令或到達批次檔的結尾時，會還原先前的設定。

- Batch 程式中可以有一個以上的 **setlocal** 或 **endlocal** 命令 (也就是) 的嵌套命令。

- **Setlocal**命令會設定 ERRORLEVEL 變數。 如果您傳遞 {**enableextensions**  |  **disableextensions**} 或 {**enabledelayedexpansion**  |  **disabledelayedexpansion**}，ERRORLEVEL 變數會設為**0** (零) 。 否則，它會設定為 **1**。 您可以在批次腳本中使用這項資訊來判斷是否有可用的延伸模組，如下列範例所示：

    ```
    setlocal enableextensions
    verify other 2>nul
    if errorlevel 1 echo Unable to enable extensions
    ```

    因為 **cmd** 不會在命令延伸模組停用時設定 ERRORLEVEL 變數，所以當您使用不正確引數時， **verify** 命令會將 ERRORLEVEL 變數初始化為非零值。 此外，如果您使用具有**setlocal**引數 {**enableextensions**  |  **disableextensions**} 或 {**enabledelayedexpansion**  |  **disabledelayedexpansion**} 的 setlocal 命令，而且沒有將 ERRORLEVEL 變數設定為**1**，就無法使用命令延伸模組。

## <a name="examples"></a>範例

若要將批次檔中的環境變數當地語系化，請遵循此範例腳本：

```
rem *******Begin Comment**************
rem This program starts the superapp batch program on the network,
rem directs the output to a file, and displays the file
rem in Notepad.
rem *******End Comment**************
@echo off
setlocal
path=g:\programs\superapp;%path%
call superapp>c:\superapp.out
endlocal
start notepad c:\superapp.out
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
