---
title: ksetup： setrealm
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ab268c40-276b-46ef-ab16-d5ce7667fbed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 453977ac39dd3a52b4f5a3104995f944e4a48392
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724552"
---
# <a name="ksetupsetrealm"></a>ksetup： setrealm



設定 Kerberos 領域的名稱。

## <a name="syntax"></a>語法

```
ksetup /setrealm <DNSDomainName>
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<DNSDomainName>|DNS 功能變數名稱的格式可以是完整功能變數名稱或簡單功能變數名稱。|

## <a name="remarks"></a>備註

DNS 功能變數名稱參數應以大寫字母輸入。 否則， **ksetup**命令會要求驗證繼續進行。

不支援在網域控制站上設定 Kerberos 領域。 嘗試這麼做會導致警告和命令失敗。

## <a name="examples"></a>範例

將此電腦的領域設定為特定的功能變數名稱，以限制只有 CONTOSO Kerberos 領域的非網域控制站存取：
```
ksetup /setrealm CONTOSO
```

## <a name="additional-references"></a>其他參考

-   - [命令列語法關鍵](command-line-syntax-key.md)
-   [Ksetup](ksetup.md)
-   [Ksetup:removerealm](ksetup-removerealm.md)