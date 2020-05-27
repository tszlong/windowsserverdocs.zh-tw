---
title: Scwcmd register
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fe4d126a-9f27-4076-b7b1-fbefa45f378a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c7540bb4ba83ebffa12f1f6d9b48f0f9ff787d68
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820968"
---
# <a name="scwcmd-register"></a>Scwcmd: register

> 適用于： Windows Server 2012 R2、Windows Server 2012

藉由註冊包含角色、工作、服務或埠定義的安全性設定資料庫檔案，延伸或自訂安全性設定 Wizard （SCW）安全性設定資料庫。

## <a name="syntax"></a>語法

```
scwcmd register /kbname:<MyApp> [/kbfile:<kb.xml>] [/kb:<path>] [/d]
```

#### <a name="parameters"></a>參數

|參數|說明|
|---------|-----------|
|/kbname： \< MyApp>|指定將用來註冊安全性設定資料庫延伸模組的名稱。 必須指定這個參數。|
|/kbfile： \< Kb .xml>|指定將用來延伸或自訂基本安全性設定資料庫的安全性設定資料庫檔案的路徑和檔案名。 若要驗證安全性設定資料庫檔案是否符合 SCW 架構，請使用%windir%\security\KBRegistrationInfo.xsd 架構定義檔。 除非指定了 **/d**參數，否則必須提供此選項。|
|/kb： \< Path>|指定包含要更新之 SCW 安全性設定資料庫檔案的目錄路徑。 如果未指定此選項，則會使用%windir%\security\msscw\kbs。|
|/d|從安全性設定資料庫中取消註冊安全性設定資料庫延伸模組。 要取消註冊的延伸模組是由/kbname 參數所指定。 （不應指定 **/kbfile**參數）。要從中取消註冊擴充功能的安全性設定資料庫，是由 **/kb**參數所指定。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

Scwcmd 只能在執行 Windows Server 2008 R2、Windows Server 2008 或 Windows Server 2003 的電腦上使用。

## <a name="examples"></a>範例

若要在位置 kbserver\kb 中的名稱 MyApp 底下註冊名為 SCWKBForMyApp 的安全性設定資料庫檔案 \\ \\ ，請輸入：
```
scwcmd register /kbfile:d:\SCWKBForMyApp.xml /kbname:MyApp /kb:\\kbserver\kb
```
若要取消註冊位於 kbserver\kb 的安全性設定資料庫 MyApp \\ \\ ，請輸入：
```
scwcmd register /d /kbname:MyApp /kb:\\kbserver\kb
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)