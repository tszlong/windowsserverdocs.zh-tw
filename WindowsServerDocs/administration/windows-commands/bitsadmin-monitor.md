---
title: bitsadmin monitor
description: Bitsadmin monitor 命令的參考文章，它會監視目前使用者所擁有之傳輸佇列中的作業。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2c424d27-e011-49c2-b579-a2c235467c39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7ce08eccf46fc17086d216bc6797bec451ace7eb
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926493"
---
# <a name="bitsadmin-monitor"></a>bitsadmin monitor

監視目前使用者所擁有之傳輸佇列中的作業。

## <a name="syntax"></a>語法

```
bitsadmin /monitor [/allusers] [/refresh <seconds>]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
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
