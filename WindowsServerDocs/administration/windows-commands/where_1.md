---
title: 其中
description: 適用于的 Windows 命令主題，其中顯示符合指定搜尋模式的檔案位置。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0b3486a5-896b-4d92-84b8-e463a0b76487
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1b32424622e8a893023aad9365b6aec4a91764fa
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80829331"
---
# <a name="where"></a>其中



顯示符合指定搜尋模式之檔案的位置。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
where [/r <Dir>] [/q] [/f] [/t] [$<ENV>:|<Path>:]<Pattern>[ ...] 
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/r \<Dir >|表示遞迴搜尋，從指定的目錄開始。|
|/q|傳回結束代碼（**0**表示成功， **1**表示失敗），而不顯示相符檔案的清單。|
|/f|以引號顯示**where**命令的結果。|
|/t|顯示每個相符檔案的檔案大小和上次修改日期和時間。|
|[$\<ENV >：\|\<路徑 >：]\<模式 > [...]|指定要比對之檔案的搜尋模式。 至少需要一個模式，而且該模式可以包含萬用字元（ **&#42;** 和 **？** ）。 根據預設，會**在其中**搜尋目前目錄，以及在 PATH 環境變數中指定的路徑。 您可以指定不同的路徑來搜尋，方法是使用 $*ENV*：*pattern*格式（其中*ENV*是包含一或多個路徑的現有環境變數），或使用格式*path*：*pattern* （其中*path*是您想要搜尋的目錄路徑）。 這些選用格式不應與 **/r**命令列選項搭配使用。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   如果您未指定副檔名，PATHEXT 環境變數中所列的延伸模組預設會附加至模式。
-   **其中**可以執行遞迴搜尋、顯示日期或大小等檔案資訊，並接受環境變數來取代本機電腦上的路徑。

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要在目前電腦及其子目錄的磁片磁碟機 C 中尋找名為 Test 的所有檔案，請輸入：
```
where /r c:\ test 
```
若要列出公用目錄中的所有檔案，請輸入：
```
where $public:*.*
```
若要在遠端電腦的 C 磁片磁碟機、Computer1 及其子目錄中尋找名為 [記事本] 的所有檔案，請輸入：
```
where /r \\computer1\c notepad.*
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)