---
title: ksetup： delhosttorealmmap
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3faee482-a96c-4614-86fd-aaa446643ec4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2b6b14785f254a63f0e16fcd16f1cd464a2d69c8
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724697"
---
# <a name="ksetupdelhosttorealmmap"></a>ksetup： delhosttorealmmap



移除所指定主機與領域之間的服務主體名稱（SPN）對應。

## <a name="syntax"></a>語法

```
ksetup /delhosttorealmmap <HostName> <RealmName>
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<主機名稱>|主機名稱是電腦名稱稱，可以指定為電腦的完整功能變數名稱。|
|\<RealmName>|領域名稱會指定為大寫 DNS 名稱，例如 CORP。CONTOSO.COM。|

## <a name="remarks"></a>備註

當主機到領域（或多部主機到領域）對應存在時，此命令會移除該對應。

對應會記錄在登錄中**HKEY_LOCAL_MACHINE \system\currentcontolset\lsa\kerberos\hosttorealm**。 使用此命令之後，您應該確認登錄中的對應。

## <a name="examples"></a>範例

改變領域 CONTOSO 的設定，刪除主機電腦 IPops897 與領域的對應：
```
ksetup /delhosttorealmmap IPops897 CONTOSO
```
執行此命令之後，您可以在登錄中驗證對應是否如預期。

## <a name="additional-references"></a>其他參考

-   [Ksetup:addhosttorealmmap](ksetup-addhosttorealmmap.md)
-   [Ksetup](ksetup.md)
-   - [命令列語法關鍵](command-line-syntax-key.md)