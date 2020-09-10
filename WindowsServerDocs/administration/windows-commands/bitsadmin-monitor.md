---
title: bitsadmin monitor
description: Bitsadmin monitor 命令的參考文章，此命令會監視目前使用者所擁有之傳送佇列中的工作。
ms.topic: reference
ms.assetid: 2c424d27-e011-49c2-b579-a2c235467c39
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2245d9e4375130877755f96eed42a46a479742f3
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631478"
---
# <a name="bitsadmin-monitor"></a>bitsadmin monitor

監視目前使用者所擁有之傳送佇列中的工作。

## <a name="syntax"></a>語法

```
bitsadmin /monitor [/allusers] [/refresh <seconds>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| /allusers | 選擇性。 監視所有使用者的作業。 您必須具有系統管理員許可權，才能使用此參數。 |
| /refresh | 選擇性。 以指定的間隔重新整理資料 `<seconds>` 。 預設重新整理間隔為五秒。 若要停止重新整理，請按 CTRL + C。 |

## <a name="examples"></a>範例

若要監視目前使用者所擁有之作業的傳送佇列，並每隔60秒重新整理資訊一次。

```
bitsadmin /monitor /refresh 60
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
