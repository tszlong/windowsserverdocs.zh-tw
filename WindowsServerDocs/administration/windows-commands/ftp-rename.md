---
title: ftp 重新命名
description: Ftp rename 命令的參考主題，會重新命名遠端檔案。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 977b7c95-6428-4980-80ec-79c3ae7e8c4d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a8d3ea25e48266db6a4a282f2ea395bd8b8d5fd9
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820308"
---
# <a name="ftp-rename"></a>ftp 重新命名

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

重新命名遠端檔案。

## <a name="syntax"></a>語法

```
rename <filename> <newfilename>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<filename>` | 指定您要重新命名的檔案。 |
| `<newfilename>` | 指定新的檔案名。 |

### <a name="examples"></a>範例

若要將遠端檔案*範例 .txt*重新命名為*example1 .txt*，請輸入：

```
rename example.txt example1.txt
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [其他 FTP 指引](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
