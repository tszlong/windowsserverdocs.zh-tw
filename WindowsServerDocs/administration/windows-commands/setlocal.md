---
title: setlocal
description: Setlocal 的參考文章，它會啟動批次檔中環境變數的當地語系化。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e4e4b6d3-3f1a-4851-a782-25ee2470e16e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4e990cf931e72bd8f6972db448d24db08c2e5208
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85934608"
---
# <a name="setlocal"></a>setlocal

啟動批次檔中環境變數的當地語系化。 當地語系化會繼續進行，直到遇到相符的**endlocal**命令或到達批次檔的結尾為止。



## <a name="syntax"></a>語法

```
setlocal [enableextensions | disableextensions] [enabledelayedexpansion | disabledelayedexpansion]
```

## <a name="arguments"></a>引數

|引數|說明|
|--------|-----------|
|enableextensions|會啟用命令延伸模組，直到遇到相符的**endlocal**命令為止，不論在**setlocal**命令執行之前的設定為何。|
|disableextensions|除非執行**setlocal**命令之前的設定，否則會停用命令延伸模組，直到遇到相符的**endlocal**命令為止。|
|enabledelayedexpansion|啟用延遲的環境變數擴充，直到遇到符合的**endlocal**命令為止，不論在**setlocal**命令執行之前的設定為何。|
|disabledelayedexpansion|會停用延遲的環境變數擴充，直到遇到符合的**endlocal**命令為止，不論在**setlocal**命令執行之前的設定為何。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   使用**setlocal**

    當您在腳本或批次檔外部使用**setlocal**時，不會有任何作用。
-   變更環境變數

    當您執行批次檔時，請使用**setlocal**來變更環境變數。 執行**setlocal**之後所做的環境變更是批次檔的本機。 Cmd.exe 程式會在遇到**endlocal**命令或到達批次檔尾時，還原先前的設定。
-   嵌套命令

    Batch 程式中可以有一個以上的**setlocal**或**endlocal**命令（也就是，也就是嵌套的命令）。
-   在批次檔中測試命令延伸模組

    **Setlocal**命令會設定 ERRORLEVEL 變數。 如果您傳遞 {**enableextensions**  |  **disableextensions**} 或 {**enabledelayedexpansion**  |  **disabledelayedexpansion**}，則 ERRORLEVEL 變數會設定為**0** （零）。 否則，它會設定為**1**。 您可以在批次腳本中使用這項資訊來判斷延伸模組是否可用，如下列範例所示：
    ```
    setlocal enableextensions
    verify other 2>nul
    if errorlevel 1 echo Unable to enable extensions
    ```
    由於在停用命令延伸模組時， **cmd**不會設定 ERRORLEVEL 變數，因此當您使用不正確引數時， **verify**命令會將 ERRORLEVEL 變數初始化為非零值。 此外，如果您使用**setlocal**命令搭配引數 {**enableextensions**  |  **disableextensions**} 或 {**enabledelayedexpansion**  |  **disabledelayedexpansion**}，且未將 ERRORLEVEL 變數設定為**1**，則無法使用命令延伸模組。

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