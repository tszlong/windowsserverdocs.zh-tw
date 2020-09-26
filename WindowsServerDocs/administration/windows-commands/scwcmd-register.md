---
title: scwcmd register
description: Scwcmd register 命令的參考文章，此命令會藉由註冊包含角色、工作、服務或埠定義的安全性設定資料庫檔案，來擴充或自訂安全性設定 Wizard (SCW) 安全性設定資料庫。
ms.topic: reference
ms.assetid: fe4d126a-9f27-4076-b7b1-fbefa45f378a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b1e981ef2a428174406fd793d5c959de57a8067c
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388574"
---
# <a name="scwcmd-register"></a>scwcmd register

> 適用于： Windows Server 2012 R2 和 Windows Server 2012

藉由註冊包含角色、工作、服務或埠定義的安全性設定資料庫檔案，擴充或自訂安全性設定 Wizard (SCW) 安全性設定資料庫。

## <a name="syntax"></a>語法

```
scwcmd register /kbname:<MyApp> [/kbfile:<kb.xml>] [/kb:<path>] [/d]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| /kbname`<MyApp>` | 指定將在其中註冊安全性設定資料庫延伸模組的名稱。 必須指定這個參數。 |
| /kbfile:`<kb.xml>` | 指定用於擴充或自訂基本安全性設定資料庫之安全性設定資料庫檔案的路徑和檔案名。 若要驗證安全性設定資料庫檔案是否符合 SCW 架構，請使用 `%windir%\security\KBRegistrationInfo.xsd` 架構定義檔。 除非指定了 **/d** 參數，否則必須提供這個選項。 |
| /kb`<path>` | 指定包含要更新之 SCW 安全性設定資料庫檔案之目錄的路徑。 如果未指定此選項， `%windir%\security\msscw\kbs` 則會使用。 |
| /d | 從安全性設定資料庫取消註冊安全性設定資料庫延伸模組。 要取消註冊的副檔名是由 **/kbname** 參數所指定。  (不應指定 **/kbfile** 參數。若要取消註冊擴充功能，請從 **/kb** 參數指定此設定資料庫 ) 。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要在位置的名稱*MyApp*下註冊名為*SCWKBForMyApp.xml*的安全性設定資料庫檔案 `\\kbserver\kb` ，請輸入：

```
scwcmd register /kbfile:d:\SCWKBForMyApp.xml /kbname:MyApp /kb:\\kbserver\kb
```

若要取消註冊安全性設定資料庫 *MyApp*（位於 `\\kbserver\kb` ），請輸入：

```
scwcmd register /d /kbname:MyApp /kb:\\kbserver\kb
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [scwcmd 分析命令](scwcmd-analyze.md)

- [scwcmd configure 命令](scwcmd-configure.md)

- [scwcmd rollback 命令](scwcmd-rollback.md)

- [scwcmd 轉換命令](scwcmd-transform.md)

- [scwcmd view 命令](scwcmd-view.md)