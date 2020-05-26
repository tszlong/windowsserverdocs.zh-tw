---
title: expand
description: Expand 命令的參考主題，它會展開一或多個壓縮檔案。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 66de0488-a0c4-40c2-9b03-e40c107ba343
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1204f3db338f835b47db03eab3d178544a6acc85
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83819118"
---
# <a name="expand"></a>expand

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

展開一或多個壓縮檔案。 您也可以使用此命令，從散發磁片取出壓縮檔案。

**Expand**命令也可以從 Windows 修復主控台使用不同的參數執行。 如需詳細資訊，請參閱[Windows 修復環境（WinRE）](https://docs.microsoft.com/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference)。

## <a name="syntax"></a>語法

```
expand [/r] <source> <destination>
expand /r <source> [<destination>]
expand /i <source> [<destination>]
expand /d <source>.cab [/f:<files>]
expand <source>.cab /f:<files> <destination>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| /r | 重新命名展開的檔案。 |
| 來源 | 指定要展開的檔案。 *來源*可以包含磁碟機號和冒號、目錄名稱、檔案名或這些的組合。 您可以使用萬用字元（**&#42;** 或 **？**）。 |
| 目的地 | 指定檔案的展開位置。<p>如果*來源*包含多個檔案，而您未指定 **/r**，則*目的地*必須是目錄。 *目的地*可以包含磁碟機號和冒號、目錄名稱、檔案名或這些的組合。 目的地 `file | path` 規格。 |
| /i | 重新命名已展開的檔案，但忽略目錄結構。 |
| /d | 顯示來源位置的檔案清單。 不會展開或解壓縮檔案。 |
| /f`<files>` | 指定您想要展開之封包檔（.cab）中的檔案。 您可以使用萬用字元（**&#42;** 或 **？**）。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
