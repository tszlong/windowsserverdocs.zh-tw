---
title: bitsadmin setsecurityflags
description: Bitsadmin setsecurityflags 命令的參考文章，此命令會設定 HTTP 的安全性旗標，以判斷 BITS 是否應該檢查憑證撤銷清單、忽略某些憑證錯誤，以及定義伺服器重新導向 HTTP 要求時要使用的原則。
ms.topic: reference
ms.assetid: 0da5cbf5-5f7f-4833-bbbe-c4e8379a78ab
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 771ff8ba34f82e942877158d351ed8ff760bda9d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630631"
---
# <a name="bitsadmin-setsecurityflags"></a>bitsadmin setsecurityflags

設定 HTTP 的安全性旗標，以判斷 BITS 是否應該檢查憑證撤銷清單、忽略某些憑證錯誤，以及定義伺服器重新導向 HTTP 要求時要使用的原則。 此值為不帶正負號的整數。

## <a name="syntax"></a>語法

```
bitsadmin /setsecurityflags <job> <value>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| value | 可以包含下列一或多個通知旗標，包括：<ul><li>設定最不重要的位，以啟用 CRL 檢查。</li><li>設定右邊的第二位，以忽略伺服器憑證中不正確的一般名稱。</li><li>將右邊的第3位設定為忽略伺服器憑證中不正確的日期。</li><li>設定右邊的第4位，以忽略伺服器憑證中不正確的憑證授權單位單位。</li><li>設定右邊的第5位，以忽略伺服器憑證的不正確用法。</li><li>將第9個到右邊的第11個位，以執行您指定的重新導向原則，包括：<ul><li>**0、0、0。** 系統會自動允許重新導向。</li><li>**0、0、1。** 如果發生重新導向，則會更新 **IBackgroundCopyFile** 介面中的遠端名稱。</li><li>**0、1、0。** 如果發生重新導向，則 BITS 會失敗。</li></ul></li><li>設定右邊的第12位，以允許從 HTTPS 重新導向至 HTTP。</li></ul> |

## <a name="examples"></a>範例

若要設定安全性旗標，以針對名為 *myDownloadJob*的作業啟用 CRL 檢查：

```
bitsadmin /setsecurityflags myDownloadJob 0x0001
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
