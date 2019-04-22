---
title: move
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fde290a8-d385-450f-8987-ee837fed667d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d651e586c31ff64664079bdd10ffde3701ec317d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824989"
---
# <a name="move"></a>move



會將一或多個檔案從一個目錄移到另一個目錄。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
move [{/y | /-y}] [<Source>] [<Target>]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/y|不要提示您確認您想要覆寫現有目的地檔案。|
|/-y|提示您確認您想要覆寫現有目的地檔案的原因。|
|\<來源 >|指定的路徑和檔案或要移動的檔案名稱。 如果您想要移動或重新命名目錄，而*來源*應該是目前的目錄路徑和名稱。|
|\<Target>|指定的路徑和名稱，以將檔案移至。 如果您想要移動或重新命名目錄，而*目標*應該想要的目錄路徑和名稱。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   **/Y**命令列選項可能預設的 COPYCMD 環境變數中。 您可以覆寫這 **/-y**命令列上。 預設值是要覆寫檔案，除非之前提示**複製**命令從批次指令碼內執行。
-   將加密的檔案移到不支援加密檔案系統 (EFS) 結果中發生錯誤的磁碟區。 第一次解密檔案的檔案或移動到支援 EFS 的磁碟區。

## <a name="BKMK_examples"></a>範例

若要從 \Data 目錄的所有檔案，副檔名為.xls 都移往 \Second_Q\Reports 的目錄中，輸入：
```
move \data\*.xls \second_q\reports\ 
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)