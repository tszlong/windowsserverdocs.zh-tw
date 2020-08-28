---
title: bitsadmin complete
description: 完成工作的 bitsadmin complete 命令的參考文章。
ms.topic: reference
ms.assetid: a5e86677-8f7b-43b3-929e-97706c57e7f1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c21cd7919d49b2ac68c665dfd7043cbe55937e45
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89026686"
---
# <a name="bitsadmin-complete"></a>bitsadmin complete

完成工作。 在作業移至 [已傳送] 狀態之後，請使用此參數。 否則，只會提供已成功傳輸的檔案。

## <a name="syntax"></a>語法

```
bitsadmin /complete <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="example"></a>範例

若要完成 *myDownloadJob* 作業，在達到此 `TRANSFERRED` 狀態之後：

```
bitsadmin /complete myDownloadJob
```

如果有多個作業使用 *myDownloadJob* 做為其名稱，您必須使用作業的 GUID 來唯一識別它是否完成。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
