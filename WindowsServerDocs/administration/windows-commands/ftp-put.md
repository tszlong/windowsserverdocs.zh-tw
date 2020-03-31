---
title: ftp put
description: FTP put 的 Windows 命令主題
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95cc1e3f-523d-4374-98b8-16e6c276b2ca vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/30/2020
ms.openlocfilehash: 019a81364dbedb443a3a23d5c5a6f8db1496d83d
ms.sourcegitcommit: 479ad84a0d6c7c7b8308122b8bac8308cb36fe9b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/30/2020
ms.locfileid: "80391719"
---
# <a name="ftp-put"></a>ftp： put

> 適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用目前的檔案傳輸類型，將本機檔案複製到遠端電腦。
## <a name="syntax"></a>語法
```
put <LocalFile> [<remoteFile>]
```
### <a name="parameters"></a>參數

|    參數     |                    描述                    |
|------------------|---------------------------------------------------|
|   `<LocalFile>`  |         指定要複製的本機檔案。         |
| `[<remoteFile>]` | 指定要在遠端電腦上使用的名稱。 |

## <a name="remarks"></a>備註
- **Put**命令等同于**send**命令。
- 如果未指定*remoteFile* ，則會為檔案提供*LocalFile*名稱。
  ## <a name="examples"></a><a name="BKMK_Examples"></a>典型
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
- [命令列語法關鍵](command-line-syntax-key.md)
