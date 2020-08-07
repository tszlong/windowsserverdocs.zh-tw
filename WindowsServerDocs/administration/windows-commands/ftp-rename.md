---
title: ftp rename
description: Ftp rename 命令的參考文章，會重新命名遠端檔案。
ms.topic: article
ms.assetid: 977b7c95-6428-4980-80ec-79c3ae7e8c4d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c85903987be9df566f4c07bc7fb5b96e76b0aa43
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87888980"
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
| `<filename>` | 指定您要重新命名的檔案。 |
| `<newfilename>` | 指定新的檔案名。 |

### <a name="examples"></a>範例

若要將遠端檔案*example.txt*重新命名為*example1.txt*，請輸入：

```
rename example.txt example1.txt
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [其他 FTP 指引](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
