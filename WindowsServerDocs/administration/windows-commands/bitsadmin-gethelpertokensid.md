---
title: bitsadmin gethelpertokensid
description: Bitsadmin gethelpertokensid 命令的參考文章，它會傳回 BITS 傳送作業的協助程式權杖 SID （如果有設定的話）。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: b616b9cc80b21c4c6a72fcca55dcdd893fac2730
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85928199"
---
# <a name="bitsadmin-gethelpertokensid"></a>bitsadmin gethelpertokensid

傳回 BITS 傳送作業的協助程式 [權杖](https://docs.microsoft.com/windows/win32/bits/helper-tokens-for-bits-transfer-jobs)SID （如果已設定的話）。

> [!NOTE]
> BITS 3.0 和更早版本不支援此命令。

## <a name="syntax"></a>語法

```
bitsadmin /gethelpertokensid <job>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取出名為*myDownloadJob*的 BITS 傳送工作的 SID：

```
bitsadmin /gethelpertokensid myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
