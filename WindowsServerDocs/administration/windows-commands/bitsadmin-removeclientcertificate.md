---
title: bitsadmin removeclientcertificate
description: Bitsadmin removeclientcertificate 命令的參考文章，此命令會從作業中移除用戶端憑證。
ms.topic: reference
ms.assetid: b417c3e5-aadd-4fcc-968f-45d8b67ca516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a447354a309d16ed75c89c586c8edabd2eadb528
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89026426"
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
