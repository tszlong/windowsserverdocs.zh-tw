---
title: bitsadmin removeclientcertificate
description: Bitsadmin removeclientcertificate 命令的參考文章，此命令會從作業中移除用戶端憑證。
ms.topic: reference
ms.assetid: b417c3e5-aadd-4fcc-968f-45d8b67ca516
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: f2c739c1e1b1b3ee31d592adfb17e363dde865bf
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631167"
---
# <a name="bitsadmin-removeclientcertificate"></a>bitsadmin removeclientcertificate

從作業中移除用戶端憑證。

## <a name="syntax"></a>語法

```
bitsadmin /removeclientcertificate <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要從名為 *myDownloadJob*的作業中移除用戶端憑證：

```
bitsadmin /removeclientcertificate myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
