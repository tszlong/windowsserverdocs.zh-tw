---
title: ftp recv
description: Ftp 接收命令的參考文章，此命令會使用目前的檔案傳輸類型，將遠端檔案複製到本機電腦。
ms.topic: reference
ms.assetid: f249ce61-247d-421b-9b93-48bce5108800
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 65acf6bae09d9b1cad1bf89028b4a51232116c60
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89624651"
---
# <a name="ftp-recv"></a>ftp recv

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用目前的檔案傳輸類型，將遠端檔案複製到本機電腦。

> [!NOTE]
> 此命令與 [ftp get 命令](ftp-get.md)相同。

## <a name="syntax"></a>語法

```
recv <remotefile> [<localfile>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<remotefile>` | 指定要複製的遠端檔案。 |
| `[<localfile>]` | 指定要在本機電腦上使用的檔案名。 如果未指定 *localfile* ，則會提供檔案 *remotefile*的名稱。 |

### <a name="examples"></a>範例

若要使用目前的檔案傳輸將 *test.txt* 複製到本機電腦，請輸入：

```
recv test.txt
```

若要使用目前的檔案傳輸*test1.txt*將*test.txt*複製到本機電腦，請輸入：

```
recv test.txt test1.txt
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [ftp get 命令](ftp-get.md)

- [ftp ascii 命令](ftp-ascii.md)

- [ftp 二進位命令](ftp-binary.md)

- [其他 FTP 指導方針](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
