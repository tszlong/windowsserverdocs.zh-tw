---
title: Scwcmd 復原
description: '* * * * 的參考文章'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4fd9f89b-0420-420a-ad20-4a328636b1e7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b329791b16e333e38669258eeeedfa8d65f334db
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85932649"
---
# <a name="scwcmd-rollback"></a>Scwcmd: rollback

> 適用于： Windows Server 2012 R2、Windows Server 2012

套用最新可用的復原原則，然後刪除該復原原則。

## <a name="syntax"></a>語法

```
scwcmd rollback /m:<ComputerName> [/u:<UserName>] [/pw:<Password>]
```

#### <a name="parameters"></a>參數

|參數|說明|
|---------|-----------|
|/m\<ComputerName>|指定應執行復原作業之電腦的 NetBIOS 名稱、DNS 名稱或 IP 位址。|
|u\<UserName>|指定執行遠端復原時要使用的替代使用者帳戶。 預設為登入的使用者。|
|/pw\<Password>|指定執行遠端復原時要使用的替代使用者認證。 預設為登入的使用者。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

Scwcmd.exe 只能在執行 Windows Server 2008 R2、Windows Server 2008 或 Windows Server 2003 的電腦上使用。

## <a name="examples"></a>範例

若要在 IP 位址為172.16.0.0 的電腦上復原安全性原則，請輸入：
```
scwcmd rollback /m:172.16.0.0
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)