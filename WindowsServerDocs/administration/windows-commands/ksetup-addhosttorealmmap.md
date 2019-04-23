---
title: ksetup:addhosttorealmmap
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 237742d5-fa68-466c-b97e-636f489248ea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 25cf258309c94f0efde980018dd5dcf3c7df4d60
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837499"
---
# <a name="ksetupaddhosttorealmmap"></a>ksetup:addhosttorealmmap



加入指定的主機和領域之間的服務主要名稱 (SPN) 對應。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /addhosttorealmmap <HostName> <RealmName>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<HostName>|主機名稱是電腦名稱，而它可以用來說明電腦的完整的網域名稱。|
|\<RealmName>|領域名稱會指定為大寫的 DNS 名稱，例如 CORP.CONTOSO.COM。|

## <a name="remarks"></a>備註

此命令可讓您將對應的主機或多部主機共用相同 DNS 尾碼加入領域。

對應會記錄在登錄**HKEY_LOCAL_MACHINE\SYSTEM\CurrentContolSet\Lsa\Kerberos\HostToRealm**。

## <a name="BKMK_Examples"></a>範例

在設定 CONTOSO 的領域，將主機電腦 IPops897 對應加入領域：
```
ksetup /addhosttorealmmap IPops897 CONTOSO
```
請確認登錄中，對應會如預期般。

#### <a name="additional-references"></a>其他參考資料

-   [Ksetup:delhosttorealmmap](ksetup-delhosttorealmmap.md)
-   [Ksetup](ksetup.md)
-   [命令列語法關鍵](command-line-syntax-key.md)