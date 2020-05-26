---
title: ftp 附加
description: Ftp append 命令的參考主題，會使用目前的檔案類型設定，將本機檔案附加至遠端電腦上的檔案。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7c1a133c-31dc-41a4-9eb9-258efd79804d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7d1b6ab4a6ae0c1654d4335d24f135b2893bdcb7
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83819138"
---
# <a name="ftp-append"></a>ftp 附加

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用目前的檔案類型設定，將本機檔案附加至遠端電腦上的檔案。

## <a name="syntax"></a>語法

```
append <localfile> [remotefile]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<localfile>` | 指定要加入的本機檔案。 |
| [remotefile] | 指定在新增的遠端電腦上的檔案 <localfile> 。 如果您未使用此參數，則 `<localfile>` 會使用此名稱來取代遠端檔案名。 |

### <a name="examples"></a>範例

若要將*file1*附加至遠端電腦上的*file2* ，請輸入：

```
append file1.txt file2.txt
```

將本機*file1*附加至遠端電腦上名為*file1 .txt*的檔案。

```
append file1.txt
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [其他 FTP 指引](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
