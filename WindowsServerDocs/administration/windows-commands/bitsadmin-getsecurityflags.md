---
title: bitsadmin getsecurityflags
description: Bitsadmin getsecurityflags 命令的參考文章，此命令會在傳輸期間報告 URL 重新導向的 HTTP 安全性旗標和伺服器憑證上執行的檢查。
ms.topic: reference
ms.assetid: c2e73519-34f4-487b-af11-97d5d08ef9bb
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ce0637527b39ebf05546b62671ce11a87151b572
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631699"
---
# <a name="bitsadmin-getsecurityflags"></a>bitsadmin getsecurityflags

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

報告傳輸期間在伺服器憑證上執行的 URL 重新導向和檢查的 HTTP 安全性旗標。

## <a name="syntax"></a>語法

```
bitsadmin /getsecurityflags <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要從名為 *myDownloadJob*的作業取出安全性旗標：

```
bitsadmin /getsecurityflags myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
