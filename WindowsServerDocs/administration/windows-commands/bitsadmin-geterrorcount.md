---
title: bitsadmin geterrorcount
description: Bitsadmin geterrorcount 命令的參考主題，它會抓取指定之作業產生暫時性錯誤的次數計數。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8840ae78-52b0-4c7e-b592-0547359a237e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 516bd02ed296a2eba75e174c6f084926bde63e90
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718002"
---
# <a name="bitsadmin-geterrorcount"></a>bitsadmin geterrorcount

抓取指定的作業產生暫時性錯誤的次數計數。

## <a name="syntax"></a>語法

```
bitsadmin /geterrorcount <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為*myDownloadJob*之作業的錯誤計數資訊：

```
bitsadmin /geterrorcount myDownloadJob
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
