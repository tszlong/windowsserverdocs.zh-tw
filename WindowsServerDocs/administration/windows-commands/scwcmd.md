---
title: scwcmd
description: scwcmd.exe 的命令列工具的參考文章 (SCW) 的安全性設定向導隨附。
ms.topic: reference
ms.assetid: 188ae881-c7d4-4a7a-b967-8fdc79f5f345
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 87bd2c718bec18810026c5701b955d5ef655b53e
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388918"
---
# <a name="scwcmd"></a>scwcmd

> 適用于： Windows Server 2012 R2 和 Windows Server 2012

[安全性設定] Wizard)  (的 Scwcmd.exe 命令列工具，可用於執行下列工作：

- 使用 SCW 產生的原則來分析一或多部伺服器。

- 使用 SCW 產生的原則設定一或多部伺服器。

- 使用 SCW 註冊安全性設定資料庫延伸模組。

- 復原 SCW 原則。

- 將 SCW 產生的原則轉換成群組原則所支援的原生檔案。

- 以 HTML 格式查看分析結果。

> [!NOTE]
> 如果您使用 **scwcmd** 在遠端伺服器上設定、分析或復原原則，則必須在遠端伺服器上安裝 SCW。

## <a name="syntax"></a>語法

```
scwcmd analyze
scwcmd configure
scwcmd register
scwcmd rollback
scwcmd transform
scwcmd view
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| [scwcmd analyze](scwcmd-analyze.md) | 判斷電腦是否符合原則。 |
| [scwcmd configure](scwcmd-configure.md) | 將 SCW 產生的安全性原則套用至電腦。|
| [scwcmd register](scwcmd-register.md) | 註冊包含角色、工作、服務或埠定義的安全性設定資料庫檔案，以擴充或自訂 SCW 安全性設定資料庫。 |
| [scwcmd rollback](scwcmd-rollback.md) | 套用可用的最新復原原則，然後刪除該復原原則。 |
| [scwcmd transform](scwcmd-transform.md) | 將使用 SCW 產生的安全性原則檔轉換為 Active Directory Domain Services 中 (GPO) 的新群組原則物件。 |
| [scwcmd view](scwcmd-view.md) | 使用指定的 xsl 轉換轉譯 .xml 檔案。 |

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
