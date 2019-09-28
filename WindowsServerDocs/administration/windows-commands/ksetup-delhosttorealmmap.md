---
title: ksetup： delhosttorealmmap
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 70b54aaebc0b7b46c34c6f52e45f6583afd6c477
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375146"
---
# <a name="ksetupdelhosttorealmmap"></a>ksetup： delhosttorealmmap



移除所指定主機與領域之間的服務主體名稱（SPN）對應。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /delhosttorealmmap <HostName> <RealmName>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<HostName >|主機名稱是電腦名稱稱，可以指定為電腦的完整功能變數名稱。|
|\<RealmName >|領域名稱會指定為大寫 DNS 名稱，例如 CORP。CONTOSO.COM。|

## <a name="remarks"></a>備註

當主機到領域（或多部主機到領域）對應存在時，此命令會移除該對應。

對應會記錄在**HKEY_LOCAL_MACHINE\SYSTEM\CurrentContolSet\Lsa\Kerberos\HostToRealm**的登錄中。 使用此命令之後，您應該確認登錄中的對應。

## <a name="BKMK_Examples"></a>典型

改變領域 CONTOSO 的設定，刪除主機電腦 IPops897 與領域的對應：
```
ksetup /delhosttorealmmap IPops897 CONTOSO
```
執行此命令之後，您可以在登錄中驗證對應是否如預期。

#### <a name="additional-references"></a>其他參考資料

-   [Ksetup:addhosttorealmmap](ksetup-addhosttorealmmap.md)
-   [Ksetup](ksetup.md)
-   [命令列語法關鍵](command-line-syntax-key.md)