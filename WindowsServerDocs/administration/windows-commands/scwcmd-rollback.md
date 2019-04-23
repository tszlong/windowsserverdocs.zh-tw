---
title: Scwcmd 復原
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4fd9f89b-0420-420a-ad20-4a328636b1e7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6d6cd79c7068d86915141a37b5a4510bddefc94c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852199"
---
# <a name="scwcmd-rollback"></a>Scwcmd: rollback

> 適用於：Windows Server 2012 R2, Windows Server 2012

適用於可用，最新的復原原則，然後刪除 復原原則。

## <a name="syntax"></a>語法

```
scwcmd rollback /m:<ComputerName> [/u:<UserName>] [/pw:<Password>]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/m:\<電腦名稱 >|請指定 NetBIOS 名稱、 DNS 名稱或 IP 位址的電腦，應該在其中執行復原作業。|
|您\<使用者名稱 >|指定執行遠端復原時要使用替代的使用者帳戶。 預設為已登入的使用者。|
|/pw:\<密碼 >|指定替代的使用者認證，以便在執行遠端回復時使用。 預設為已登入的使用者。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

只有在執行 Windows Server 2008 R2、 Windows Server 2008 或 Windows Server 2003 的電腦上使用 Scwcmd.exe。

## <a name="BKMK_Examples"></a>範例

若要復原的安全性原則上的電腦 IP 位址為 172.16.0.0，請輸入：
```
scwcmd rollback /m:172.16.0.0
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)