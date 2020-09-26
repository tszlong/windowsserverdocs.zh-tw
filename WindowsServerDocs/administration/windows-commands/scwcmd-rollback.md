---
title: scwcmd rollback
description: Scwcmd rollback 命令的參考文章，此命令會套用最新的可用復原原則，然後刪除該回復原則。
ms.topic: reference
ms.assetid: 4fd9f89b-0420-420a-ad20-4a328636b1e7
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: e8f50995131ca01ea2bb58ee9371b12d9ec9a917
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388908"
---
# <a name="scwcmd-rollback"></a>scwcmd rollback

> 適用于： Windows Server 2012 R2 和 Windows Server 2012

套用可用的最新復原原則，然後刪除該復原原則。

## <a name="syntax"></a>語法

```
scwcmd rollback /m:<computername> [/u:<username>] [/pw:<password>]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| 一樣`<computername>` | 指定應執行復原操作之電腦的 NetBIOS 名稱、DNS 名稱或 IP 位址。 |
| u`<username>` | 指定執行遠端回復時要使用的其他使用者帳戶。 預設值是登入的使用者。 |
| /pw`<password>` | 指定執行遠端回復時要使用的替代使用者認證。 預設值是登入的使用者。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要在位於 IP 位址 *172.16.0.0*的電腦上復原安全性原則，請輸入：

```
scwcmd rollback /m:172.16.0.0
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [scwcmd 分析命令](scwcmd-analyze.md)

- [scwcmd configure 命令](scwcmd-configure.md)

- [scwcmd register 命令](scwcmd-register.md)

- [scwcmd 轉換命令](scwcmd-transform.md)

- [scwcmd view 命令](scwcmd-view.md)