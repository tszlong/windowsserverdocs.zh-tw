---
title: ftp rename
description: Ftp 重新命名命令的參考文章，會重新命名遠端檔案。
ms.topic: reference
ms.assetid: 977b7c95-6428-4980-80ec-79c3ae7e8c4d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ea7862a759779a5f767b8e18cdd5a0b36db2a6a1
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89627637"
---
# <a name="ftp-rename"></a>ftp rename

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

重新命名遠端檔案。

## <a name="syntax"></a>語法

```
rename <filename> <newfilename>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<filename>` | 指定您想要重新命名的檔案。 |
| `<newfilename>` | 指定新的檔案名。 |

### <a name="examples"></a>範例

若要將遠端檔案 *example.txt* 重新命名為 *example1.txt*，請輸入：

```
rename example.txt example1.txt
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [其他 FTP 指導方針](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
