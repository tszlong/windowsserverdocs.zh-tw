---
title: bitsadmin setnotifyflags
description: Bitsadmin setnotifyflags 命令的參考文章，可為指定的作業設定事件通知旗標。
ms.topic: article
ms.assetid: d5763d95-94a6-45ca-9e03-891c20047e06
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e47326fc93648fa2319f34c556596f04ccd6c240
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87892995"
---
# <a name="bitsadmin-setnotifyflags"></a>bitsadmin setnotifyflags

設定指定之作業的事件通知旗標。

## <a name="syntax"></a>語法

```
bitsadmin /setnotifyflags <job> <notifyflags>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| notifyflags | 可以包含下列一個或多個通知旗標，包括：<ul><li>**1.** 當作業中的所有檔案都已傳送時，就會產生事件。</li><li>**2.** 在發生錯誤時產生事件。</li><li>**3.** 當所有檔案都已完成傳送或發生錯誤時，就會產生事件。</li><li>**4.** 停用通知。</li></ul> |

## <a name="examples"></a>範例

若要設定通知旗標，以在發生錯誤時產生事件，請針對名為*myDownloadJob*的作業：

```
bitsadmin /setnotifyflags myDownloadJob 2
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
