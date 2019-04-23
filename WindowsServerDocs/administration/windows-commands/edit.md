---
title: edit
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 096632005b3e42dd941ccc7c72c08ead1d291b53
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873149"
---
# <a name="edit"></a>edit



啟動 MS-DOS 編輯器，以建立和變更 ASCII 文字檔。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
edit [/b] [/h] [/r] [/s] [/<NNN>] [[<Drive>:][<Path>]<FileName> [<FileName2> [...]]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[\<Drive>:][<Path>]<FileName> [<FileName2> [...]]|指定一或多個 ASCII 文字檔案的名稱與位置。 如果檔案不存在，MS-DOS 編輯器加以建立。 如果檔案存在，MS-DOS 編輯器開啟它，並在螢幕上顯示其內容。 *檔名*可以包含萬用字元 (**&#42;** 並 **？**)。 以空格分隔多個檔案名稱。|
|/b|強制單色模式，因此該 MS-DOS 編輯器會顯示以黑白。|
|/h|顯示目前監視器的最大可能的行數。|
|/r|載入以唯讀模式的檔案。|
|/s|強制使用短檔名。|
|\<NNN>|載入二進位檔案，包裝線條*NNN*個字元寬。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   其他說明，請開啟 MS-DOS 編輯器，然後按下 F1 鍵。
-   某些監視器預設不支援顯示的快速鍵。 如果您的監視器不會顯示快速鍵，使用**b**。

## <a name="BKMK_examples"></a>範例

若要開啟 MS-DOS 編輯器，請輸入：
```
edit
```
若要建立和編輯檔案，名為 newtextfile.txt 目前目錄中的，輸入：
```
edit newtextfile.txt
```