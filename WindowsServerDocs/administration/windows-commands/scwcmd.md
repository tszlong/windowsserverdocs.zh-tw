---
title: Scwcmd
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 188ae881-c7d4-4a7a-b967-8fdc79f5f345
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 19d631f97c194a78819491f32955e391d3be5a70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883879"
---
# <a name="scwcmd"></a>Scwcmd

> 適用於：Windows Server 2012 R2, Windows Server 2012

包含安全性設定精靈 (SCW) 使用 Scwcmd.exe 命令列工具可用來執行下列工作：
-   使用 SCW 產生的原則中設定一個或多部伺服器。
-   分析與 SCW 產生原則的一或多個伺服器。
-   以 HTML 格式檢視分析結果。
-   回復 SCW 原則。
-   產生的 SCW 原則轉換為原生群組原則所支援的檔案。
-   向 SCW 安全性設定資料庫延伸模組。

當您使用**scwcmd**必須在遠端伺服器上安裝在遠端伺服器上，SCW 原則設定、 分析，或回復。

## <a name="syntax"></a>語法

```
scwcmd <command> [<subcommand>]
```

## <a name="parameters"></a>參數

|子命令|描述|
|----------|-----------|
|/ 分析|判斷電腦是否符合原則。</br>請參閱[Scwcmd： 分析](scwcmd-analyze.md)取得語法和選項。|
|/configure|SCW 產生的安全性原則套用至電腦。</br>請參閱[Scwcmd： 設定](scwcmd-configure.md)取得語法和選項。|
|/register|擴充或自訂 SCW 安全性設定資料庫註冊包含角色、 工作、 服務或連接埠定義的安全性設定資料庫檔案。</br>請參閱[Scwcmd： 註冊](scwcmd-register.md)取得語法和選項。|
|/rollback|適用於可用，最新的復原原則，然後刪除 復原原則。</br>請參閱[Scwcmd: rollback](scwcmd-rollback.md)取得語法和選項。|
|/transform|轉換到新群組原則物件 (GPO) 在 Active Directory 網域服務中使用 SCW 來產生安全性原則檔案。</br>請參閱[Scwcmd： 轉換](scwcmd-transform.md)語法和選項。|
|/view|使用指定的.xsl 轉換，以呈現的.xml 檔案。</br>請參閱[Scwcmd： 檢視](scwcmd-view.md)取得語法和選項。|
|/?|在命令提示字元顯示說明。|

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
