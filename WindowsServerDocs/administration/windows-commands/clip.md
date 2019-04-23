---
title: clip
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: b10876e115c1f0dcac3448948003852449012087
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862389"
---
# <a name="clip"></a>clip



將重新導向到 Windows 剪貼簿，從命令列命令輸出。 然後，您就可以將此文字輸出至其他程式。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
<Command> | clip
clip < <FileName>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<命令 >|指定您想要傳送到 Windows 剪貼簿其輸出的命令。|
|\<FileName>|指定您想要傳送到 Windows 剪貼簿內容的檔案。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

您可以使用**剪輯**命令，以將資料複製到任何應用程式都可以接收文字從剪貼簿直接。

## <a name="BKMK_examples"></a>範例

若要複製到 Windows 剪貼簿 列出目前的目錄，請輸入：
```
dir | clip
```
若要複製到 Windows 剪貼簿 Generic.awk 程式的輸出，請輸入：
```
awk -f generic.awk input.txt | clip
```
若要複製呼叫到 Windows 剪貼簿的 Readme.txt 檔案的內容，請輸入：
```
clip < readme.txt
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)