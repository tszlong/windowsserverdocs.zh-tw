---
title: ksetup:setrealm
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ab268c40-276b-46ef-ab16-d5ce7667fbed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aa6b2a21904ec4dae1e60def5bd36647291b1af6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877399"
---
# <a name="ksetupsetrealm"></a>ksetup:setrealm



設定 Kerberos 領域的名稱。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /setrealm <DNSDomainName>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<DNSDomainName>|DNS 網域名稱可以是完整的網域名稱或簡單的網域名稱的格式。|

## <a name="remarks"></a>備註

DNS 網域名稱的參數應以大寫字母輸入。 否則，請**ksetup**命令會要求驗證，以繼續。

不支援在網域控制站上設定 Kerberos 領域。 嘗試執行此作業會導致警告及命令失敗。

## <a name="BKMK_Examples"></a>範例

設定這部電腦在特定的網域名稱限制的非網域控制站的存取，只是為了 CONTOSO Kerberos 領域的領域：
```
ksetup /setrealm CONTOSO
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [Ksetup](ksetup.md)
-   [Ksetup:removerealm](ksetup-removerealm.md)