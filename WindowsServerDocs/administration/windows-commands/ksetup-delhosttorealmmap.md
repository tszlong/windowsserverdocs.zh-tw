---
title: ksetup:delhosttorealmmap
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3faee482-a96c-4614-86fd-aaa446643ec4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cf01edc4932fd5ec1cf98043de04286b3a100a34
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882339"
---
# <a name="ksetupdelhosttorealmmap"></a>ksetup:delhosttorealmmap



移除指定的主機和領域之間的服務主體名稱 (SPN) 對應。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /delhosttorealmmap <HostName> <RealmName>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<HostName>|主機名稱是電腦名稱，而它可以用來說明電腦的完整的網域名稱。|
|\<RealmName>|領域名稱會指定為大寫的 DNS 名稱，例如 CORP.CONTOSO.COM。|

## <a name="remarks"></a>備註

當主應用程式領域 （或多個主機領域） 對應存在時，此命令會移除該對應。

對應會記錄在登錄**HKEY_LOCAL_MACHINE\SYSTEM\CurrentContolSet\Lsa\Kerberos\HostToRealm**。 您應該確認登錄中的對應，即可使用此命令之後。

## <a name="BKMK_Examples"></a>範例

改變 CONTOSO 的領域設定，刪除對應的主機電腦 IPops897 加入領域：
```
ksetup /delhosttorealmmap IPops897 CONTOSO
```
執行此命令之後，您可以確認登錄中對應如預期般。

#### <a name="additional-references"></a>其他參考資料

-   [Ksetup:addhosttorealmmap](ksetup-addhosttorealmmap.md)
-   [Ksetup](ksetup.md)
-   [命令列語法關鍵](command-line-syntax-key.md)