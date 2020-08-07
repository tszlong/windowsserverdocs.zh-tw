---
title: bitsadmin monitor
description: Bitsadmin monitor 命令的參考文章，它會監視目前使用者所擁有之傳輸佇列中的作業。
ms.topic: article
ms.assetid: 2c424d27-e011-49c2-b579-a2c235467c39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3ba2cb315fb0696b8363506669aa41f1693bb758
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893661"
---
# <a name="bitsadmin-monitor"></a>bitsadmin monitor

監視目前使用者所擁有之傳輸佇列中的作業。

## <a name="syntax"></a>語法

```
bitsadmin /monitor [/allusers] [/refresh <seconds>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| /allusers | 選擇性。 監視所有使用者的作業。 您必須具有系統管理員許可權，才能使用此參數。 |
| /refresh | 選擇性。 以所指定的間隔重新整理資料 `<seconds>` 。 預設重新整理間隔為5秒。 若要停止重新整理，請按 CTRL + C。 |

## <a name="examples"></a>範例

針對目前使用者所擁有的作業監視傳送佇列，並每隔60秒重新整理一次資訊。

```
bitsadmin /monitor /refresh 60
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
