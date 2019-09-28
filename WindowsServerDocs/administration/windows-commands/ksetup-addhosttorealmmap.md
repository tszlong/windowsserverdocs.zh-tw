---
title: ksetup： addhosttorealmmap
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: e6a28c6001707fac245de7136b5fb5bd38495027
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375646"
---
# <a name="ksetupaddhosttorealmmap"></a>ksetup： addhosttorealmmap



在指定的主機和領域之間新增服務主體名稱（SPN）對應。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /addhosttorealmmap <HostName> <RealmName>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<HostName >|主機名稱是電腦名稱稱，可以指定為電腦的完整功能變數名稱。|
|\<RealmName >|領域名稱會指定為大寫 DNS 名稱，例如 CORP。CONTOSO.COM。|

## <a name="remarks"></a>備註

此命令可讓您將共用相同 DNS 尾碼的主機或多部主機對應到領域。

對應會記錄在**HKEY_LOCAL_MACHINE\SYSTEM\CurrentContolSet\Lsa\Kerberos\HostToRealm**的登錄中。

## <a name="BKMK_Examples"></a>典型

在設定領域 CONTOSO 的過程中，將主機電腦 IPops897 對應到領域：
```
ksetup /addhosttorealmmap IPops897 CONTOSO
```
請在登錄中驗證對應的預期。

#### <a name="additional-references"></a>其他參考資料

-   [Ksetup:delhosttorealmmap](ksetup-delhosttorealmmap.md)
-   [Ksetup](ksetup.md)
-   [命令列語法關鍵](command-line-syntax-key.md)