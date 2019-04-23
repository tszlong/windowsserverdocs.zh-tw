---
title: attrib
description: 適用於 Windows 命令主題**attrib** -顯示、 集合或移除屬性指派給檔案或目錄。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5e763ca5-21a2-45d2-b26d-a9c44c99091a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0f640af2c7957e43dd3f31dfa732bfc887112651
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841089"
---
# <a name="attrib"></a>attrib



顯示、 設定，或移除指派給檔案或目錄的屬性。 如果未指定參數，使用**attrib**顯示目前目錄中的所有檔案的屬性。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
attrib [{+|-}r] [{+|-}a] [{+|-}s] [{+|-}h] [{+|-}i] [<Drive>:][<Path>][<FileName>] [/s [/d] [/l]]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|{+\|-}r|集合 (**+**) 或清除 (**-**) 的唯讀檔案屬性。|
|{+\|-}a|集合 (**+**) 或清除 (**-**) 封存檔案屬性。|
|{+\|-}s|集合 (**+**) 或清除 (**-**) 系統檔案屬性。|
|{+\|-}h|集合 (**+**) 或清除 (**-**) 隱藏的 [檔案] 屬性。|
|{+\|-}i|集合 (**+**) 或清除 (**-**) 未內容編製索引的檔案屬性。|
|[\<Drive>:][<Path>][<FileName>]|指定的目錄、 檔案或您想要顯示或變更屬性的檔案群組的名稱與位置。 您可以使用**嗎？** 並 **&#42;** 中的萬用字元*檔名*參數來顯示或變更的檔案群組中的屬性。|
|/s|適用於**attrib**和任何命令列的選項，以符合目前的目錄中的檔案及其所有子目錄。|
|/d|適用於**attrib**和目錄的任何命令列選項。|
|/l|適用於**attrib**和符號連結，而不是符號連結的目標的任何命令列選項。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   您可以使用萬用字元 (**嗎？** 並 **&#42;**) 與*檔名*參數來顯示或變更的檔案群組中的屬性。
-   如果檔案系統 (**s**) 或 隱藏 (**h**) 屬性集，您必須清除屬性，才能變更該檔案的任何其他屬性。
-   封存屬性 () 將標示已備份的最後一個時間以來變更的檔案。 請注意， **xcopy**命令會使用封存屬性。

## <a name="BKMK_examples"></a>範例

若要顯示的屬性，名為 News86 位於目前目錄中的檔案，請輸入：
```
attrib news86 
```
若要將唯讀屬性指派至名為 Report.txt 檔案，請輸入：
```
attrib +r report.txt 
```
若要移除公用目錄及其子目錄中的檔案在磁碟機 B 中的磁碟上的唯讀屬性，請輸入：
```
attrib -r b:\public\*.* /s 
```
如果您要設定 封存 屬性的所有檔案在磁碟機，然後清除 副檔名為.bak 檔案的封存屬性，請輸入：
```
attrib +a a:*.* & attrib -a a:*.bak 
```
