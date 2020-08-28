---
title: bitsadmin addfile
description: Bitsadmin addfile 命令的參考文章，此命令會將檔案新增至指定的作業。
ms.topic: reference
ms.assetid: 1b31aa93-0364-465b-af36-754968825989
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 63df6c33bae3022c91633d4507c1fe9709cdd65e
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89031376"
---
# <a name="bitsadmin-addfile"></a>bitsadmin addfile

將檔案加入至指定的作業。

## <a name="syntax"></a>語法

```
bitsadmin /addfile <job> <remoteURL> <localname>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| remoteURL | 伺服器上檔案的 URL。 |
| localname | 本機電腦上的檔案名。 *Localname* 必須包含檔案的絕對路徑。 |

## <a name="examples"></a>範例

若要將檔案新增至作業：

```
bitsadmin /addfile myDownloadJob http://downloadsrv/10mb.zip c:\10mb.zip
```

針對要新增的每個檔案重複此呼叫。 如果有多個作業使用 *myDownloadJob* 做為其名稱，您必須以作業的 GUID 取代 *myDownloadJob* ，以唯一識別工作。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
