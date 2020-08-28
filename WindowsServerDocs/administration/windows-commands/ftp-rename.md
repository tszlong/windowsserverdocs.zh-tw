---
title: ftp rename
description: Ftp 重新命名命令的參考文章，會重新命名遠端檔案。
ms.topic: reference
ms.assetid: 977b7c95-6428-4980-80ec-79c3ae7e8c4d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a722605e451fff3ea8d4a758434a7509deaf355c
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89035736"
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
