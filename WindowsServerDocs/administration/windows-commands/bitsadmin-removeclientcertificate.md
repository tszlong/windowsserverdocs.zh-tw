---
title: bitsadmin removeclientcertificate
description: 適用于**bitsadmin removeclientcertificate**的 Windows 命令主題，它會從作業中移除用戶端憑證。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b417c3e5-aadd-4fcc-968f-45d8b67ca516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 312226b73b91385436e15c4afbb49df161258768
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123101"
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
| 工作 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

下列範例會從名為*myDownloadJob*的作業中移除用戶端憑證。

```
C:\>bitsadmin /removeclientcertificate myDownloadJob 
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)