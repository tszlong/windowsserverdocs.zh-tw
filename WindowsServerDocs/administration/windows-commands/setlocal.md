---
title: setlocal
description: 適用于 setlocal 的參考文章，會在批次檔中啟動環境變數的當地語系化。
ms.topic: reference
ms.assetid: e4e4b6d3-3f1a-4851-a782-25ee2470e16e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 807bcb1d5694617f9632e88a4bc200a714048cc9
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639465"
---
# <a name="setlocal"></a>setlocal

開始對批次檔中的環境變數進行當地語系化。 當地語系化會繼續進行，直到遇到相符的 **endlocal** 命令或到達批次檔的結尾為止。



## <a name="syntax"></a>語法

```
setlocal [enableextensions | disableextensions] [enabledelayedexpansion | disabledelayedexpansion]
```

## <a name="arguments"></a>引數

|引數|描述|
|--------|-----------|
|enableextensions|在遇到相符的 **endlocal** 命令之前啟用命令延伸模組，無論在執行 **setlocal** 命令之前的設定為何。|
|disableextensions|除非您在執行**setlocal**命令之前的設定，否則會停用命令延伸模組，直到遇到相符的**endlocal**命令為止。|
|enabledelayedexpansion|啟用延遲的環境變數擴充，直到遇到 [比對 **endlocal** ] 命令，而不考慮執行 **setlocal** 命令之前的設定。|
|disabledelayedexpansion|停用延遲的環境變數擴充，直到遇到 [比對 **endlocal** ] 命令，而不考慮執行 **setlocal** 命令之前的設定。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   使用 **setlocal**

    當您在腳本或批次檔外面使用 **setlocal** 時，不會有任何作用。
-   變更環境變數

    當您執行批次檔時，請使用 **setlocal** 來變更環境變數。 執行 **setlocal** 之後所做的環境變更是批次檔的本機。 Cmd.exe 程式在遇到 **endlocal** 命令或到達批次檔的結尾時，會還原先前的設定。
-   嵌套命令

    Batch 程式中可以有一個以上的 **setlocal** 或 **endlocal** 命令 (也就是) 的嵌套命令。
-   測試批次檔中的命令延伸模組

    **Setlocal**命令會設定 ERRORLEVEL 變數。 如果您傳遞 {**enableextensions**  |  **disableextensions**} 或 {**enabledelayedexpansion**  |  **disabledelayedexpansion**}，ERRORLEVEL 變數會設為**0** (零) 。 否則，它會設定為 **1**。 您可以在批次腳本中使用這項資訊來判斷是否有可用的延伸模組，如下列範例所示：
    ```
    setlocal enableextensions
    verify other 2>nul
    if errorlevel 1 echo Unable to enable extensions
    ```
    因為 **cmd** 不會在命令延伸模組停用時設定 ERRORLEVEL 變數，所以當您使用不正確引數時， **verify** 命令會將 ERRORLEVEL 變數初始化為非零值。 此外，如果您使用具有**setlocal**引數 {**enableextensions**  |  **disableextensions**} 或 {**enabledelayedexpansion**  |  **disabledelayedexpansion**} 的 setlocal 命令，而且沒有將 ERRORLEVEL 變數設定為**1**，就無法使用命令延伸模組。

## <a name="examples"></a>範例

您可以將批次檔中的環境變數當地語系化，如下列範例腳本所示：
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

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)