---
title: bitsadmin removeclientcertificate
description: Bitsadmin removeclientcertificate 命令的參考文章，它會從作業中移除用戶端憑證。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b417c3e5-aadd-4fcc-968f-45d8b67ca516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3659f830b9462469762fcd4c7690295073400b1e
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927980"
---
# <a name="bitsadmin-removeclientcertificate"></a>bitsadmin removeclientcertificate

從作業中移除用戶端憑證。

## <a name="syntax"></a>語法

```
bitsadmin /removeclientcertificate <job>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要從名為*myDownloadJob*的作業中移除用戶端憑證：

```
bitsadmin /removeclientcertificate myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
