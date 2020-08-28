---
title: bitsadmin setnoprogresstimeout
description: Bitsadmin setnoprogresstimeout 命令的參考文章，此命令會設定服務在發生暫時性錯誤之後嘗試傳輸檔案的時間長度（以秒為單位）。
ms.topic: reference
ms.assetid: 7fac50d9-cc6b-46a4-a96f-fab751ee1756
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dc2559a963900234fd3111edb1a32e3f13b27e40
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89027796"
---
# <a name="bitsadmin-setnoprogresstimeout"></a>bitsadmin setnoprogresstimeout

設定在發生第一個暫時性錯誤之後，BITS 嘗試傳輸檔案的時間長度（以秒為單位）。

## <a name="syntax"></a>語法

```
bitsadmin /setnoprogresstimeout <job> <timeoutvalue>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| timeoutvalue | BITS 在第一個錯誤之後等候傳送檔案的時間長度（以秒為單位）。 |

### <a name="remarks"></a>備註

- 當作業遇到第一個暫時性錯誤時，就會開始「沒有進度」逾時間隔。

- 當資料位元組成功傳輸時，逾時間隔就會停止或重設。

- 如果「沒有進度」逾時間隔超過 *timeoutvalue*，則會將工作置於嚴重的錯誤狀態。

## <a name="examples"></a>範例

若要針對名為 *myDownloadJob*的作業將「沒有進度」超時值設定為20秒：

```
bitsadmin /setnoprogresstimeout myDownloadJob 20
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
