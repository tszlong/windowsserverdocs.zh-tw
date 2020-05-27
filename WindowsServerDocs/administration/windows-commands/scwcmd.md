---
title: Scwcmd
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 188ae881-c7d4-4a7a-b967-8fdc79f5f345
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9a8c002aa735b188fdd9ad75b0db7dbe06053516
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83821158"
---
# <a name="scwcmd"></a>Scwcmd

> 適用于： Windows Server 2012 R2、Windows Server 2012

安全性設定 Wizard （SCW）隨附的 Scwcmd 命令列工具可用來執行下列工作：
-   使用 SCW 產生的原則設定一或多部伺服器。
-   使用 SCW 產生的原則來分析一或多部伺服器。
-   以 HTML 格式觀看分析結果。
-   復原 SCW 原則。
-   將 SCW 產生的原則轉換成群組原則支援的原生檔案。
-   使用 SCW 註冊安全性設定資料庫延伸模組。

當您使用**scwcmd**在遠端伺服器上設定、分析或復原原則時，SCW 必須安裝在遠端伺服器上。

## <a name="syntax"></a>語法

```
scwcmd <command> [<subcommand>]
```

### <a name="parameters"></a>參數

|子命令|說明|
|----------|-----------|
|/analyze|判斷電腦是否符合原則。</br>如需語法和選項，請參閱[Scwcmd：分析](scwcmd-analyze.md)。|
|/configure|將 SCW 產生的安全性原則套用至電腦。</br>如需語法和選項，請參閱[Scwcmd： configure](scwcmd-configure.md) 。|
|/register|藉由註冊包含角色、工作、服務或埠定義的安全性設定資料庫檔案，延伸或自訂 SCW 安全性設定資料庫。</br>請參閱[Scwcmd：註冊](scwcmd-register.md)以取得語法和選項。|
|/rollback|套用最新可用的復原原則，然後刪除該復原原則。</br>如需語法和選項，請參閱[Scwcmd： rollback](scwcmd-rollback.md) 。|
|/transform|將使用 SCW 產生的安全性原則檔案轉換成 Active Directory Domain Services 中新的群組原則物件（GPO）。</br>請參閱[Scwcmd：轉換](scwcmd-transform.md)語法和選項。|
|/view|使用指定的 .xsl 轉換來呈現 .xml 檔案。</br>如需語法和選項，請參閱[Scwcmd： view](scwcmd-view.md) 。|
|/?|在命令提示字元顯示說明。|

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
