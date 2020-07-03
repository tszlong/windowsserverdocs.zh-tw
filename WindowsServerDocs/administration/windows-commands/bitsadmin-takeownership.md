---
title: bitsadmin takeownership
description: Bitsadmin takeownership 命令的參考文章，可讓具有系統管理許可權的使用者取得指定工作的擁有權。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ea0ce7cb-440a-498f-a3ef-8368fa43e399
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bc406d1d5f7c9553082f85f3315ab7622f6bc7db
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927469"
---
# <a name="bitsadmin-takeownership"></a>bitsadmin takeownership

讓具有系統管理許可權的使用者取得指定工作的擁有權。

## <a name="syntax"></a>語法

```
bitsadmin /takeownership <job>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ---------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為*myDownloadJob*之作業的擁有權：

```
bitsadmin /takeownership myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
