---
title: Scwcmd 暫存器
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe4d126a-9f27-4076-b7b1-fbefa45f378a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cc8b4a06af519b0da01dfcab8de0139b12cc68f2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834969"
---
# <a name="scwcmd-register"></a>Scwcmd: register

> 適用於：Windows Server 2012 R2, Windows Server 2012

擴充或自訂安全性設定精靈 (SCW) 的安全性設定資料庫註冊包含角色、 工作、 服務或連接埠定義的安全性設定資料庫檔案。

## <a name="syntax"></a>語法

```
scwcmd register /kbname:<MyApp> [/kbfile:<kb.xml>] [/kb:<path>] [/d]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/kbname:\<MyApp >|指定將在其下登錄的安全性設定資料庫延伸模組的名稱。 必須指定這個參數。|
|/kbfile:\<Kb.xml>|指定將用來擴充或自訂基底的安全性設定資料庫的安全性設定資料庫檔案的路徑和檔案名稱。 若要驗證的安全性設定資料庫檔案與 SCW 架構相容，請使用 %windir%\security\KBRegistrationInfo.xsd 結構描述定義檔。 必須提供這個選項，除非 **/d**指定參數。|
|/kb:\<路徑 >|指定包含要更新的 SCW 安全性設定資料庫檔案的目錄路徑。 如果未指定此選項，則會使用 %windir%\security\msscw\kbs。|
|/d|取消註冊從 「 安全性設定資料庫的安全性設定資料庫延伸模組。 若要取消註冊的延伸模組是 /kbname 參數所指定。 ( **/Kbfile**不應指定參數。)若要取消註冊的延伸模組，從 「 安全性設定資料庫由 **/kb**參數。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

只有在執行 Windows Server 2008 R2、 Windows Server 2008 或 Windows Server 2003 的電腦上使用 Scwcmd.exe。

## <a name="BKMK_Examples"></a>範例

若要註冊安全性設定資料庫檔案名稱中位置的 MyApp 下名為 SCWKBForMyApp.xml \\ \\kbserver\kb，型別：
```
scwcmd register /kbfile:d:\SCWKBForMyApp.xml /kbname:MyApp /kb:\\kbserver\kb
```
若要取消註冊位於安全性設定資料庫 MyApp \\ \\kbserver\kb，型別：
```
scwcmd register /d /kbname:MyApp /kb:\\kbserver\kb
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)