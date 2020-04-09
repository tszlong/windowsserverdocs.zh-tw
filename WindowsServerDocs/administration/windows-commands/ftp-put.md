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
ms.openlocfilehash: ecd579a313fe1cad1b8a5b4a622aaaec2d6a6d63
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843131"
---
# <a name="ftp-put"></a>ftp： put

> 適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

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
  ## <a name="examples"></a><a name="BKMK_Examples"></a>典型
  複製本機檔案**test.txt** ，並在遠端電腦上將它命名為**test1. .txt** 。
  ```
  put test.txt test1.txt
  ```
  將本機檔案**program**複製到遠端電腦。
  ```
  put program.exe
  ```
  ## <a name="additional-references"></a>其他參考資料
- [ftp： ascii](ftp-ascii.md)
- [ftp： binary](ftp-binary.md)
- - [命令列語法關鍵](command-line-syntax-key.md)
