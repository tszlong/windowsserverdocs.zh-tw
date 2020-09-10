---
title: Scwcmd
description: '* * * * 的參考文章'
ms.topic: reference
ms.assetid: 188ae881-c7d4-4a7a-b967-8fdc79f5f345
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4095d022a9bd84033eebf63b63420dc400f2c594
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89636872"
---
# <a name="scwcmd"></a>Scwcmd

> 適用于： Windows Server 2012 R2、Windows Server 2012

[安全性設定] Wizard)  (的 Scwcmd.exe 命令列工具，可用於執行下列工作：
-   使用 SCW 產生的原則設定一或多部伺服器。
-   使用 SCW 產生的原則來分析一或多部伺服器。
-   以 HTML 格式查看分析結果。
-   復原 SCW 原則。
-   將 SCW 產生的原則轉換成群組原則所支援的原生檔案。
-   使用 SCW 註冊安全性設定資料庫延伸模組。

當您使用 **scwcmd** 在遠端伺服器上設定、分析或復原原則時，必須在遠端伺服器上安裝 SCW。

## <a name="syntax"></a>語法

```
scwcmd <command> [<subcommand>]
```

### <a name="parameters"></a>參數

|子命令|描述|
|----------|-----------|
|/analyze|判斷電腦是否符合原則。</br>請參閱 [Scwcmd：分析](scwcmd-analyze.md) 語法和選項。|
|/configure|將 SCW 產生的安全性原則套用至電腦。</br>請參閱 [Scwcmd：設定](scwcmd-configure.md) 語法和選項。|
|/register|註冊包含角色、工作、服務或埠定義的安全性設定資料庫檔案，以擴充或自訂 SCW 安全性設定資料庫。</br>請參閱 [Scwcmd：註冊](scwcmd-register.md) 以取得語法和選項。|
|/rollback|套用可用的最新復原原則，然後刪除該復原原則。</br>請參閱 [Scwcmd： rollback](scwcmd-rollback.md) 以取得語法和選項。|
|/transform|將使用 SCW 產生的安全性原則檔轉換為 Active Directory Domain Services 中 (GPO) 的新群組原則物件。</br>請參閱 [Scwcmd：轉換](scwcmd-transform.md) 語法和選項。|
|/view|使用指定的 xsl 轉換轉譯 .xml 檔案。</br>請參閱 [Scwcmd：查看](scwcmd-view.md) 語法和選項。|
|/?|在命令提示字元顯示說明。|

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
