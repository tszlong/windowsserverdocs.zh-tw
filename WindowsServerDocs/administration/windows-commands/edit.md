---
title: edit
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4e0ff2e8-3518-47c1-8c69-5e93f895fa0e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5a51a81a0ed2d28a30e8ec221d5719968dce48ac
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377625"
---
# <a name="edit"></a>edit



啟動 MS-DOS 編輯器，它會建立及變更 ASCII 文字檔。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
edit [/b] [/h] [/r] [/s] [/<NNN>] [[<Drive>:][<Path>]<FileName> [<FileName2> [...]]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[\<Drive >：][<Path>] <FileName> [<FileName2> [...]]|指定一或多個 ASCII 文字檔的位置和名稱。 如果檔案不存在，MS-DOS 編輯器會建立檔案。 如果檔案已存在，MS-DOS 編輯器會開啟該檔案，並在螢幕上顯示其內容。 *FileName*可以包含萬用字元（ **&#42;** 和 **？** ）。 以空格分隔多個檔案名。|
|/b|強制執行單色模式，讓 MS-DOS 編輯器以黑色和白色顯示。|
|/h|顯示目前監視器可能的最大行數。|
|/r|以唯讀模式載入檔案。|
|/s|強制使用短檔案名。|
|\<NNN >|載入二進位檔案，將線條換行至*NNN*個字元寬。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   如需其他說明，請開啟 [MS-DOS 編輯器]，然後按下 F1 鍵。
-   有些監視器預設不支援顯示快速鍵。 如果您的監視器不會顯示快速鍵，請使用 **/b**。

## <a name="BKMK_examples"></a>典型

若要開啟 MS-DOS 編輯器，請輸入：
```
edit
```
若要建立和編輯目前目錄中名為 newtextfile 的檔案，請輸入：
```
edit newtextfile.txt
```