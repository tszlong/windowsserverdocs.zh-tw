---
title: bitsadmin complete
description: 完成作業的 bitsadmin complete 命令參考主題。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a5e86677-8f7b-43b3-929e-97706c57e7f1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6b61f3475afdb0e29e5777940e6426a04fe33e78
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718222"
---
# <a name="bitsadmin-complete"></a>bitsadmin complete

完成作業。 在作業移至已轉移狀態之後，使用此參數。 否則，只有已成功傳輸的檔案才可供使用。

## <a name="syntax"></a>語法

```
bitsadmin /complete <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="example"></a>範例

若要完成*myDownloadJob*作業，請在達到`TRANSFERRED`狀態之後：

```
bitsadmin /complete myDownloadJob
```

如果有多個作業使用*myDownloadJob*作為其名稱，您必須使用作業的 GUID 來唯一識別它以完成。

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
