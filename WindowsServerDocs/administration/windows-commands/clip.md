---
title: Clip - 裁剪
description: 適用于剪輯的 Windows 命令主題，它會從命令列將命令輸出重新導向至 Windows 剪貼簿。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 85322d85-3376-4806-845b-93ac77fe27bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0d997154a382cf39aa2b877d7a2b84f4ff34157d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847641"
---
# <a name="clip"></a>Clip - 裁剪

從命令列將命令輸出重新導向至 Windows 剪貼簿。 然後，您可以將此文字輸出貼入其他程式中。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
<Command> | clip
clip < <FileName>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<命令 >|指定要傳送至 Windows 剪貼簿之輸出的命令。|
|\<FileName >|指定您想要傳送至 Windows 剪貼簿之內容的檔案。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

您可以使用 [**剪輯**] 命令，將資料直接複製到任何可以從剪貼簿接收文字的應用程式。

## <a name="examples"></a><a name=BKMK_examples></a>典型

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

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)