---
title: ksetup： addhosttorealmmap
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 237742d5-fa68-466c-b97e-636f489248ea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 732dccc868ca85b108ba443d912788a14dd0e107
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724778"
---
# <a name="ksetupaddhosttorealmmap"></a>ksetup： addhosttorealmmap



在指定的主機和領域之間新增服務主體名稱（SPN）對應。

## <a name="syntax"></a>語法

```
ksetup /addhosttorealmmap <HostName> <RealmName>
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<主機名稱>|主機名稱是電腦名稱稱，可以指定為電腦的完整功能變數名稱。|
|\<RealmName>|領域名稱會指定為大寫 DNS 名稱，例如 CORP。CONTOSO.COM。|

## <a name="remarks"></a>備註

此命令可讓您將共用相同 DNS 尾碼的主機或多部主機對應到領域。

對應會記錄在登錄中**HKEY_LOCAL_MACHINE \system\currentcontolset\lsa\kerberos\hosttorealm**。

## <a name="examples"></a>範例

在設定領域 CONTOSO 的過程中，將主機電腦 IPops897 對應到領域：
```
ksetup /addhosttorealmmap IPops897 CONTOSO
```
請在登錄中驗證對應的預期。

## <a name="additional-references"></a>其他參考

-   [Ksetup:delhosttorealmmap](ksetup-delhosttorealmmap.md)
-   [Ksetup](ksetup.md)
-   - [命令列語法關鍵](command-line-syntax-key.md)