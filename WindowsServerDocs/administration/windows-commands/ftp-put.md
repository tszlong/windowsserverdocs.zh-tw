---
title: ftp put
description: FTP put 的 Windows 命令主題
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 95cc1e3f-523d-4374-98b8-16e6c276b2ca vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/30/2020
ms.openlocfilehash: d86ba8418f4c43c26f3745a9f70e676ca790c640
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725171"
---
# <a name="ftp-put"></a>ftp： put

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用目前的檔案傳輸類型，將本機檔案複製到遠端電腦。
## <a name="syntax"></a>語法
```
put <LocalFile> [<remoteFile>]
```
#### <a name="parameters"></a>參數

|    參數     |                    描述                    |
|------------------|---------------------------------------------------|
|   `<LocalFile>`  |         指定要複製的本機檔案。         |
| `[<remoteFile>]` | 指定要在遠端電腦上使用的名稱。 |

## <a name="remarks"></a>備註
- **Put**命令等同于**send**命令。
- 如果未指定*remoteFile* ，則會為檔案提供*LocalFile*名稱。
  ## <a name="examples"></a>範例
  複製本機檔案**test.txt** ，並在遠端電腦上將它命名為**test1. .txt** 。
  ```
  put test.txt test1.txt
  ```
  將本機檔案**program**複製到遠端電腦。
  ```
  put program.exe
  ```
  ## <a name="additional-references"></a>其他參考
- [ftp： ascii](ftp-ascii.md)
- [ftp： binary](ftp-binary.md)
- - [命令列語法關鍵](command-line-syntax-key.md)
