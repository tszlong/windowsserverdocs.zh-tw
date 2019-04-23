---
title: setlocal
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e4e4b6d3-3f1a-4851-a782-25ee2470e16e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 70e58e3c3a7c3de594c620f7530816b57727d4c3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868859"
---
# <a name="setlocal"></a>setlocal



啟動批次檔中的當地語系化環境變數。 當地語系化會繼續直到相符**endlocal**遇到命令或批次檔的結尾為止。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
setlocal [enableextensions | disableextensions] [enabledelayedexpansion | disabledelayedexpansion]
```

## <a name="arguments"></a>引數

|引數|描述|
|--------|-----------|
|enableextensions|可讓命令擴充功能，直到比對**endlocal**遇到命令時，無論之前設定為何**setlocal**執行命令。|
|disableextensions|停用的命令擴充功能，直到比對**endlocal**遇到命令時，無論之前設定為何**setlocal**執行命令。|
|enabledelayedexpansion|可讓延遲的環境變數擴充，直到比對**endlocal**遇到命令時，無論之前設定為何**setlocal**執行命令。|
|disabledelayedexpansion|停用延遲的環境變數擴充，直到比對**endlocal**遇到命令時，無論之前設定為何**setlocal**執行命令。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   使用**setlocal**

    當您使用**setlocal**外部指令碼或批次檔，會有任何作用。
-   變更環境變數

    使用**setlocal**變更環境變數，當您執行的批次檔。 執行之後所做的環境變更**setlocal**本機批次檔。 Cmd.exe 程式還原先前的設定，當它遇到**endlocal**命令，或到達批次檔的結尾。
-   巢狀命令

    您可以有一個以上**setlocal**或是**endlocal**命令批次程式 （也就是巢狀命令）。
-   測試批次檔中的命令擴充功能

    **Setlocal**命令會設定 ERRORLEVEL 變數。 如果您傳遞 {**enableextensions** | **disableextensions**} 或 {**enabledelayedexpansion**  |  **disabledelayedexpansion**}，ERRORLEVEL 變數設定為**0** （零）。 否則，它會設定為**1**。 您可以使用批次指令碼中的這項資訊來判斷延伸模組是否可用，如下列範例所示：  
    ```
    setlocal enableextensions
    verify other 2>nul
    if errorlevel 1 echo Unable to enable extensions
    ```  
    因為**cmd**命令擴充功能會停用時，不會設定 ERRORLEVEL 變數**確認**當使用無效時，命令會初始化為非零值的 ERRORLEVEL 變數引數。 此外，如果您使用**setlocal**命令的引數搭配 {**enableextensions** | **disableextensions**} 或 {**enabledelayedexpansion**  |  **disabledelayedexpansion**} 並不會設定 ERRORLEVEL 變數**1**，命令延伸模組未提供。

## <a name="BKMK_examples"></a>範例

您可以當地語系化批次檔中的環境變數，如下列範例指令碼所示：
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

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)