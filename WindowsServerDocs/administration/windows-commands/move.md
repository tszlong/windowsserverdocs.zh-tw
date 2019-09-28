---
title: move
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 4302a3403f73892500c3f9deb608e9489c7e91ae
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373533"
---
# <a name="move"></a>move



將一個或多個檔案從一個目錄移至另一個目錄。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
move [{/y | /-y}] [<Source>] [<Target>]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/y|隱藏提示確認您想要覆寫現有的目的檔案。|
|/-y|導致提示確認您想要覆寫現有的目的檔案。|
|\<Source >|指定要移動之檔案的路徑和名稱。 如果您想要移動或重新命名目錄，*來源*應該是目前的目錄路徑和名稱。|
|\<Target >|指定要用來移動檔案的路徑和名稱。 如果您想要移動或重新命名目錄，*目標*應該是所需的目錄路徑和名稱。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   **/Y**命令列選項可能在 COPYCMD 環境變數中預設為預設值。 您可以使用命令列上的 **/-y**來覆寫此參數。 預設值是在覆寫檔案之前先提示，除非從批次腳本內執行**copy**命令。
-   將加密的檔案移至不支援加密檔案系統（EFS）的磁片區會導致錯誤。 先將檔案解密，或將檔案移至支援 EFS 的磁片區。

## <a name="BKMK_examples"></a>典型

若要將 \Data 目錄中副檔名為 .xls 的所有檔案移到 \Second_Q\Reports 目錄，請輸入：
```
move \data\*.xls \second_q\reports\ 
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)