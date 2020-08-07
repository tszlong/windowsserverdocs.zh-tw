---
title: bitsadmin addfile
description: Bitsadmin addfile 命令的參考文章，它會將檔案新增至指定的作業。
ms.topic: article
ms.assetid: 1b31aa93-0364-465b-af36-754968825989
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c9bc00f1b63c559d048c9ae590df29f7421e42ec
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894933"
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
| localname | 本機電腦上的檔案名。 *Localname*必須包含檔案的絕對路徑。 |

## <a name="examples"></a>範例

若要將檔案新增至作業：

```
bitsadmin /addfile myDownloadJob http://downloadsrv/10mb.zip c:\10mb.zip
```

針對要新增的每個檔案重複此呼叫。 如果多個作業使用*myDownloadJob*作為其名稱，您必須將*myDownloadJob*取代為作業的 GUID，以唯一識別作業。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
