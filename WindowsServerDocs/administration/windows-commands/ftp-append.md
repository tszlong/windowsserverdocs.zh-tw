---
title: ftp append
description: Ftp append 命令的參考文章，此命令會使用目前的檔案類型設定，將本機檔案附加至遠端電腦上的檔案。
ms.topic: reference
ms.assetid: 7c1a133c-31dc-41a4-9eb9-258efd79804d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 42270b8f3633158e12d472a234fcf1904cee86de
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89638579"
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
| [remotefile] | 指定遠端電腦上新增的檔案 <localfile> 。 如果您未使用此參數，則 `<localfile>` 會使用該名稱來取代遠端檔案名。 |

### <a name="examples"></a>範例

若要將 *file1.txt* 附加至遠端電腦上的 *file2.txt* ，請輸入：

```
append file1.txt file2.txt
```

將本機 *file1.txt* 附加至遠端電腦上名為 *file1.txt* 的檔案。

```
append file1.txt
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [其他 FTP 指導方針](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
