---
title: bitsadmin getnotifyinterface
description: Bitsadmin getnotifyinterface 命令的參考文章，可判斷另一個程式是否已為指定的作業註冊 COM 回呼介面。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 40bf9dd8-b167-406a-80a6-a5a6f1b8cf7f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9c86b611eb5f46759c474171085884c5702e07d1
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926909"
---
# <a name="bitsadmin-getnotifyinterface"></a>bitsadmin getnotifyinterface

判斷另一個程式是否已為指定的作業註冊 COM 回呼介面（通知介面）。

## <a name="syntax"></a>語法

```
bitsadmin /getnotifyinterface <job>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

#### <a name="output"></a>輸出

此命令的輸出會顯示、**已註冊或已**取消**註冊**。

> [!NOTE]
> 您無法判斷註冊回呼介面的程式。

## <a name="examples"></a>範例

若要取得名為*myDownloadJob*之作業的通知介面：

```
bitsadmin /getnotifyinterface myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
