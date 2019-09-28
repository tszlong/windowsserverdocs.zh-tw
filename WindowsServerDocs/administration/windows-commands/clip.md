---
title: clip
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 85322d85-3376-4806-845b-93ac77fe27bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 82186869782c47f41930d46b4c33a710e6addedf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379328"
---
# <a name="clip"></a>clip



從命令列將命令輸出重新導向至 Windows 剪貼簿。 然後，您可以將此文字輸出貼入其他程式中。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
<Command> | clip
clip < <FileName>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<Command >|指定要傳送至 Windows 剪貼簿之輸出的命令。|
|\<檔案名 >|指定您想要傳送至 Windows 剪貼簿之內容的檔案。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

您可以使用 [**剪輯**] 命令，將資料直接複製到任何可以從剪貼簿接收文字的應用程式。

## <a name="BKMK_examples"></a>典型

若要將目前的目錄清單複製到 Windows 剪貼簿，請輸入：
```
dir | clip
```
若要將名為 Generic 的程式輸出複製到 Windows 剪貼簿，請輸入：
```
awk -f generic.awk input.txt | clip
```
若要將稱為 readme.txt 的檔案內容複寫到 Windows 剪貼簿，請輸入：
```
clip < readme.txt
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)