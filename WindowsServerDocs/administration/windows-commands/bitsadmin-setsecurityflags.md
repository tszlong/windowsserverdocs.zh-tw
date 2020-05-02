---
title: bitsadmin setsecurityflags
description: Bitsadmin setsecurityflags 命令的參考主題，其會設定 HTTP 的安全性旗標，以判斷 BITS 是否應檢查憑證撤銷清單、忽略特定憑證錯誤，以及定義伺服器重新導向 HTTP 要求時所要使用的原則。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0da5cbf5-5f7f-4833-bbbe-c4e8379a78ab
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 63430f9814037aeb948cc8b91e8590f13a4df00e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720470"
---
# <a name="bitsadmin-setsecurityflags"></a>bitsadmin setsecurityflags

設定 HTTP 的安全性旗標，以判斷 BITS 是否應檢查憑證撤銷清單、忽略某些憑證錯誤，以及定義伺服器重新導向 HTTP 要求時所要使用的原則。 值為不帶正負號的整數。

## <a name="syntax"></a>語法

```
bitsadmin /setsecurityflags <job> <value>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| value | 可以包含下列一個或多個通知旗標，包括：<ul><li>將最不重要的位設定為啟用 CRL 檢查。</li><li>設定右邊的第二個位，忽略伺服器憑證中不正確的一般名稱。</li><li>設定右邊的第三個位，忽略伺服器憑證中不正確的日期。</li><li>設定許可權的第4個位，忽略伺服器憑證中不正確的憑證授權單位單位。</li><li>設定許可權的第5位，以忽略伺服器憑證的不正確使用方式。</li><li>將第9個設定為右邊的第11個位，以執行您指定的重新導向原則，包括：<ul><li>**0、0、0。** 系統會自動允許重新導向。</li><li>**0、0、1。** 如果發生重新導向， **IBackgroundCopyFile**介面中的遠端名稱會更新。</li><li>**0、1、0。** 如果發生重新導向，BITS 會失敗作業。</li></ul></li><li>設定右邊的第12個位，允許從 HTTPS 重新導向至 HTTP。</li></ul> |

## <a name="examples"></a>範例

若要設定安全性旗標，以啟用名為*myDownloadJob*之作業的 CRL 檢查：

```
bitsadmin /setsecurityflags myDownloadJob 0x0001
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
