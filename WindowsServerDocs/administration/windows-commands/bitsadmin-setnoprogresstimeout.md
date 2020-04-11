---
title: bitsadmin setnoprogresstimeout
description: 適用于**bitsadmin setnoprogresstimeout**的 Windows 命令主題，會設定服務在發生暫時性錯誤之後，嘗試傳輸檔案的時間長度（以秒為單位）。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7fac50d9-cc6b-46a4-a96f-fab751ee1756
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8adff95b0dbae68634db2e248d4493549c5ac85d
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122870"
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
| 工作 | 作業的顯示名稱或 GUID。 |
| timeoutvalue | 在第一個錯誤之後，BITS 等待傳輸檔案的時間長度（以秒為單位）。 |

## <a name="remarks"></a>備註

- 當作業遇到第一個暫時性錯誤時，就會開始「沒有進度」逾時間隔。

- 當成功傳輸位元組的資料時，會停止或重設逾時間隔。

- 如果「沒有進度」逾時間隔超過*timeoutvalue*，則會將工作置於嚴重的錯誤狀態。

## <a name="examples"></a>範例

下列範例會針對名為*myDownloadJob*的作業，將 "no 進度" 超時值設定為20秒。

```
C:\>bitsadmin /setnoprogresstimeout myDownloadJob 20
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)