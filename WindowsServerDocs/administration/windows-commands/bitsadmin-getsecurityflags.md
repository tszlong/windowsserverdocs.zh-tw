---
title: bitsadmin getsecurityflags
description: Bitsadmin getsecurityflags 命令的參考文章，它會報告 URL 重新導向的 HTTP 安全性旗標，以及在傳送期間在伺服器憑證上執行的檢查。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c2e73519-34f4-487b-af11-97d5d08ef9bb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8613f2bd293bdbf7680aa730ec6fc222ffcfe158
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926686"
---
# <a name="bitsadmin-getsecurityflags"></a>bitsadmin getsecurityflags

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

報告用於 URL 重新導向的 HTTP 安全性旗標，以及在傳送期間在伺服器憑證上執行的檢查。

## <a name="syntax"></a>語法

```
bitsadmin /getsecurityflags <job>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要從名為*myDownloadJob*的作業中取出安全性旗標：

```
bitsadmin /getsecurityflags myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
