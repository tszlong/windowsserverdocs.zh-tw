---
title: bitsadmin getnoprogresstimeout
description: Bitsadmin getnoprogresstimeout 命令的參考文章，它會抓取服務在發生暫時性錯誤之後，嘗試傳輸檔案的時間長度（以秒為單位）。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9cd9b19b-cbb4-4352-8419-978080f016b6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 95884f5e6b0dc7ae01575ddf0cc12afea6d212c3
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927005"
---
# <a name="bitsadmin-getnoprogresstimeout"></a>bitsadmin getnoprogresstimeout

抓取在暫時性錯誤發生後，服務將嘗試傳輸檔案的時間長度（以秒為單位）。

## <a name="syntax"></a>語法

```
bitsadmin /getnoprogresstimeout <job>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為*myDownloadJob*之作業的進度超時值：

```
bitsadmin /getnoprogresstimeout myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
