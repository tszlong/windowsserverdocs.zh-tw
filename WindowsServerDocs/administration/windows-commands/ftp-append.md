---
title: ftp append
description: Ftp append 命令的參考文章，它會使用目前的檔案類型設定，將本機檔案附加至遠端電腦上的檔案。
ms.topic: article
ms.assetid: 7c1a133c-31dc-41a4-9eb9-258efd79804d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0c98e9eb4ed5d2dc560deac0720158cf90d801e4
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87889689"
---
# <a name="ftp-append"></a>ftp append

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用目前的檔案類型設定，將本機檔案附加至遠端電腦上的檔案。

## <a name="syntax"></a>語法

```
append <localfile> [remotefile]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<localfile>` | 指定要加入的本機檔案。 |
| [remotefile] | 指定在新增的遠端電腦上的檔案 <localfile> 。 如果您未使用此參數，則 `<localfile>` 會使用此名稱來取代遠端檔案名。 |

### <a name="examples"></a>範例

若要將*file1.txt*附加至遠端電腦上的*file2.txt* ，請輸入：

```
append file1.txt file2.txt
```

將本機*file1.txt*附加至遠端電腦上名為*file1.txt*的檔案。

```
append file1.txt
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [其他 FTP 指引](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
