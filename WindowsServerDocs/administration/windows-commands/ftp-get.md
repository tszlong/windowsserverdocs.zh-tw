---
title: ftp get
description: Ftp get 命令的參考文章，此命令會使用目前的檔案傳輸類型，將遠端檔案複製到本機電腦。
ms.topic: reference
ms.assetid: d70355c4-58ef-43e0-916b-c7ecf77e6ee4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 55e57167b7918aee7db9fc8f6f9304273dfcc4e0
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037736"
---
# <a name="ftp-get"></a>ftp get

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用目前的檔案傳輸類型，將遠端檔案複製到本機電腦。

> [!NOTE]
> 此命令與 [ftp 接收命令](ftp-recv.md)相同。

## <a name="syntax"></a>語法

```
get <remotefile> [<localfile>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<remotefile>` | 指定要複製的遠端檔案。 |
| `[<localfile>]` | 指定要在本機電腦上使用的檔案名。 如果未指定 *localfile* ，則會提供檔案 *remotefile*的名稱。 |

### <a name="examples"></a>範例

若要使用目前的檔案傳輸將 *test.txt* 複製到本機電腦，請輸入：

```
get test.txt
```

若要使用目前的檔案傳輸*test1.txt*將*test.txt*複製到本機電腦，請輸入：

```
get test.txt test1.txt
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [ftp 接收命令](ftp-recv.md)

- [ftp ascii 命令](ftp-ascii.md)

- [ftp 二進位命令](ftp-binary.md)

- [其他 FTP 指導方針](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
