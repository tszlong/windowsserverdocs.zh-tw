---
title: bitsadmin getnoprogresstimeout
description: Bitsadmin getnoprogresstimeout 命令的參考文章，這個命令會抓取服務在發生暫時性錯誤之後嘗試傳輸檔案的時間長度（以秒為單位）。
ms.topic: reference
ms.assetid: 9cd9b19b-cbb4-4352-8419-978080f016b6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 51a8327669bdadbebec28329e0c6b11cc32e672b
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89033516"
---
# <a name="bitsadmin-getnoprogresstimeout"></a>bitsadmin getnoprogresstimeout

抓取在發生暫時性錯誤之後，服務將嘗試傳送檔案的時間長度（以秒為單位）。

## <a name="syntax"></a>語法

```
bitsadmin /getnoprogresstimeout <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為 *myDownloadJob*之作業的進度超時值：

```
bitsadmin /getnoprogresstimeout myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
