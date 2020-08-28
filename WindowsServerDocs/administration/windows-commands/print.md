---
title: print
description: 列印命令的參考文章，可將文字檔傳送至印表機。
ms.topic: reference
ms.assetid: aa2325d5-a993-4ed3-b996-255165452db8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ecd679a3891a073bd73c0526c395dc67c2cf0933
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89035216"
---
# <a name="print"></a>print

將文字檔傳送至印表機。 如果您將檔案傳送到連接到本機電腦上串列或平行埠的印表機，檔案就可以在背景中列印。

> [!NOTE]
> 您可以使用 [Mode 命令](mode.md)從命令提示字元執行許多設定工作，包括設定連接到平行或序列埠的印表機、顯示印表機狀態，或準備印表機進行字碼頁切換。

## <a name="syntax"></a>語法

```
print [/d:<printername>] [<drive>:][<path>]<filename>[ ...]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| /d`<printername>` | 指定您要列印工作的印表機。 若要列印到本機連線的印表機，請在您的電腦上指定印表機連線的埠。 平行埠的有效值為 **LPT1**、 **LPT2**和 **LPT3**。 有效的序列埠值為 **COM1**、 **COM2**、 **COM3**和 **COM4**。 您也可以使用其佇列名稱 () 來指定網路印表機 `\\server_name\printer_name` 。 如果您未指定印表機，則預設會將列印工作傳送至 **LPT1** 。 |
| `<drive>`: | 指定您要列印的檔案所在的邏輯或實體磁片磁碟機。 如果您想要列印的檔案位於目前磁片磁碟機上，則不需要此參數。 |
| `<path>` | 指定您要列印之檔案的位置。 如果您想要列印的檔案位於目前的目錄中，則不需要這個參數。 |
| `<filename>[ ...]` | 必要。 指定您要列印的檔案。 您可以在一個命令中包含多個檔案。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

若要將位於目前目錄中的 **report.txt** 檔案傳送至本機電腦上的 **lpt2** 連接到的印表機，請輸入：

```
print /d:lpt2 report.txt
```

若要將位於**c:\accounting**目錄中的**report.txt**檔案傳送到 **/d： \\ copyroom**伺服器上的**printer1**列印佇列，請輸入：

```
print /d:\\copyroom\printer1 c:\accounting\report.txt
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [列印命令參考資料](print-command-reference.md)

- [Mode 命令](mode.md)