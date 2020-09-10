---
title: Scwcmd 註冊
description: '* * * * 的參考文章'
ms.topic: reference
ms.assetid: fe4d126a-9f27-4076-b7b1-fbefa45f378a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 644e4bff424b64b8e6a9a49b0b19320526b49a11
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89636992"
---
# <a name="scwcmd-register"></a>Scwcmd: register

> 適用于： Windows Server 2012 R2、Windows Server 2012

藉由註冊包含角色、工作、服務或埠定義的安全性設定資料庫檔案，擴充或自訂安全性設定 Wizard (SCW) 安全性設定資料庫。

## <a name="syntax"></a>語法

```
scwcmd register /kbname:<MyApp> [/kbfile:<kb.xml>] [/kb:<path>] [/d]
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/kbname\<MyApp>|指定將在其中註冊安全性設定資料庫延伸模組的名稱。 必須指定這個參數。|
|/kbfile:\<Kb.xml>|指定將用來擴充或自訂基本安全性設定資料庫之安全性設定資料庫檔案的路徑和檔案名。 若要驗證安全性設定資料庫檔案是否符合 SCW 架構，請使用%windir%\security\KBRegistrationInfo.xsd 架構定義檔。 除非指定了 **/d** 參數，否則必須提供這個選項。|
|/kb\<Path>|指定包含要更新之 SCW 安全性設定資料庫檔案之目錄的路徑。 如果未指定此選項，則會使用%windir%\security\msscw\kbs。|
|/d|從安全性設定資料庫取消註冊安全性設定資料庫延伸模組。 要取消註冊的副檔名是由/kbname 參數所指定。  (不應指定 **/kbfile** 參數。若要取消註冊擴充功能，請從 **/kb** 參數指定此安全性設定資料庫 ) 。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

Scwcmd.exe 只能在執行 Windows Server 2008 R2、Windows Server 2008 或 Windows Server 2003 的電腦上使用。

## <a name="examples"></a>範例

若要在位置 kbserver\kb 中的 MyApp 名稱下註冊名為 SCWKBForMyApp.xml 的安全性設定資料庫檔案 \\ \\ ，請輸入：
```
scwcmd register /kbfile:d:\SCWKBForMyApp.xml /kbname:MyApp /kb:\\kbserver\kb
```
若要取消註冊位於 kbserver\kb 的安全性設定資料庫 MyApp \\ \\ ，請輸入：
```
scwcmd register /d /kbname:MyApp /kb:\\kbserver\kb
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)