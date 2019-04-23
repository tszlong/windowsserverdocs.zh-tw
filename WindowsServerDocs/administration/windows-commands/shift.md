---
title: shift
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b56574e8-570a-4cc9-bbac-1b94fbf6a47a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 52b3012a39409f9d48ae8aa7608e7bd0af5787d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858819"
---
# <a name="shift"></a>shift



變更批次參數的批次檔的位置。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
shift [/n <N>]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/n \<N>|指定開始轉移*N*個引數，其中*N*是介於 0 到 8 的任何值。 需要命令延伸模組，依預設會啟用。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   **Shift**命令會將變更批次參數的值 **%0**透過 **%9**複製到前一個的每個參數 — 值 **%1**複製到 **%0**的值 **%2**複製到 **%1**，依此類推。 這是適用於撰寫批次檔執行於任何數量的參數相同的作業。
-   如果已啟用命令延伸模組， **shift**命令支援 **/n**命令列選項。 **/N**選項會指定開始轉移第 n 個引數，其中**N**是介於 0 到 8 的任何值。 比方說， **SHIFT/2**都會移位 **%3**來 **%2**， **%4**到 **%3**，依此類推，並將 **%0**並 **%1**不會受到影響。 預設會啟用命令延伸模組。
-   您可以使用**shift**命令來建立可接受超過 10 個批次參數的批次檔。 如果您在命令列上指定 10 個以上的參數，這些會顯示之後的第十個 (**%9**) 將會變動的入一次 **%9**。
-   **Shift**命令並不會影響**% \*** 批次參數。
-   沒有不回溯**shift**命令。 實作後**shift**命令時，您無法復原的批次參數 (**%0**) 的排班之前就已存在。

## <a name="BKMK_examples"></a>範例

下列幾行範例批次檔呼叫 Mycopy.bat 示範如何使用**shift**與任意數目的批次參數。 在此範例中，Mycopy.bat 會將一份檔案複製到特定的目錄。 批次參數是由目錄和檔案名稱引數表示。
```
@echo off 
rem MYCOPY.BAT copies any number of files
rem to a directory.
rem The command uses the following syntax:
rem mycopy dir file1 file2 ... 
set todir=%1
:getfile
shift
if "%1"=="" goto end
copy %1 %todir%
goto getfile
:end
set todir=
echo All done
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)