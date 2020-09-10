---
title: expand
description: Expand 命令的參考文章，可展開一或多個壓縮檔案。
ms.topic: reference
ms.assetid: 66de0488-a0c4-40c2-9b03-e40c107ba343
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b64303d2a7cd38c86d8f5c99d319e46f837f2970
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635891"
---
# <a name="expand"></a>expand

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

展開一或多個壓縮檔案。 您也可以使用此命令從散發磁片取出壓縮檔案。

**展開**命令也可以使用不同的參數，從 Windows 修復主控台執行。 如需詳細資訊，請參閱 [Windows 修復環境 (WinRE) ](/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference)。

## <a name="syntax"></a>語法

```
expand [/r] <source> <destination>
expand /r <source> [<destination>]
expand /i <source> [<destination>]
expand /d <source>.cab [/f:<files>]
expand <source>.cab /f:<files> <destination>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /r | 重新命名展開的檔案。 |
| source | 指定要展開的檔案。 *來源* 可以包含磁碟機號與冒號、目錄名稱、檔案名或這些的組合。 您可以使用 (**&#42;** 或 **？**) 的萬用字元。 |
| 目的地 | 指定檔案的展開位置。<p>如果 *來源* 包含多個檔案，而且您未指定 **/r**，則 *目的地* 必須是目錄。 *目的地* 可以是由磁碟機號和冒號、目錄名稱、檔案名或這些的組合所組成。 目的地 `file | path` 規格。 |
| /i | 重新命名展開的檔案，但忽略目錄結構。 |
| /d | 顯示來源位置的檔案清單。 不會展開或解壓縮檔案。 |
| /f`<files>` | 指定您想要擴充的封包 ( .cab) 檔案中的檔案。 您可以使用 (**&#42;** 或 **？**) 的萬用字元。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
