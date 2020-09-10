---
title: bitsadmin setpeercachingflags
description: Bitsadmin setpeercachingflags 命令的參考文章，此命令會設定旗標，以判斷是否可以快取和提供作業的檔案給對等，以及作業是否可以從對等下載內容。
ms.topic: reference
ms.assetid: 3f54a127-fb68-49a5-b843-664ec833df67
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 59784ef9220abf1954b611524ba48b006550c1cd
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630735"
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
| value | 不帶正負號的整數，包括：<ul><li>**1.** 作業可以從對等下載內容。</li><li>**2.** 可將作業的檔案快取並提供給對等。</li></ul> |

## <a name="examples"></a>範例

若要允許名為 *myDownloadJob* 的作業從對等下載內容：

```
bitsadmin /setpeercachingflags myDownloadJob 1
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
