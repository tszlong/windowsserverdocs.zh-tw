---
title: bitsadmin setpeercachingflags
description: Bitsadmin setpeercachingflags 命令的參考文章，它會設定旗標來判斷作業的檔案是否可以快取並提供給對等，以及作業是否可以從對等下載內容。
ms.topic: article
ms.assetid: 3f54a127-fb68-49a5-b843-664ec833df67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 73135ed6f08962cbe73aad4765dc4d3b4f7ff958
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87892971"
---
# <a name="bitsadmin-setpeercachingflags"></a>bitsadmin setpeercachingflags

設定旗標，以判斷是否可以快取作業的檔案，並將其提供給對等，以及作業是否可以從對等下載內容。

## <a name="syntax"></a>語法

```
bitsadmin /setpeercachingflags <job> <value>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| value | 不帶正負號的整數，包括：<ul><li>**1.** 作業可以從對等下載內容。</li><li>**2.** 作業的檔案可以快取並提供給對等。</li></ul> |

## <a name="examples"></a>範例

若要允許名為*myDownloadJob*的作業從對等下載內容：

```
bitsadmin /setpeercachingflags myDownloadJob 1
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
