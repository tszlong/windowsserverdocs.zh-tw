---
title: bitsadmin setnotifyflags
description: Bitsadmin setnotifyflags 命令的參考文章，此命令會為指定的工作設定事件通知旗標。
ms.topic: reference
ms.assetid: d5763d95-94a6-45ca-9e03-891c20047e06
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a3b00d057659b2664098093c0a7e3b13b28cd806
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89026216"
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
| notifyflags | 可以包含下列一或多個通知旗標，包括：<ul><li>**1.** 當作業中的所有檔案都已傳送時，會產生事件。</li><li>**2.** 發生錯誤時產生事件。</li><li>**3.** 當所有檔案都已完成傳輸或發生錯誤時，會產生事件。</li><li>**4.** 停用通知。</li></ul> |

## <a name="examples"></a>範例

若要在發生錯誤時設定通知旗標以產生事件，請針對名為 *myDownloadJob*的作業：

```
bitsadmin /setnotifyflags myDownloadJob 2
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
