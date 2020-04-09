---
title: ksetup： setrealm
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ab268c40-276b-46ef-ab16-d5ce7667fbed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: acdbfaabe341c8efb19c6e9d183022375f679de7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841311"
---
# <a name="ksetupsetrealm"></a>ksetup： setrealm



設定 Kerberos 領域的名稱。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /setrealm <DNSDomainName>
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<DNSDomainName >|DNS 功能變數名稱的格式可以是完整功能變數名稱或簡單功能變數名稱。|

## <a name="remarks"></a>備註

DNS 功能變數名稱參數應以大寫字母輸入。 否則， **ksetup**命令會要求驗證繼續進行。

不支援在網域控制站上設定 Kerberos 領域。 嘗試這麼做會導致警告和命令失敗。

## <a name="examples"></a><a name=BKMK_Examples></a>典型

將此電腦的領域設定為特定的功能變數名稱，以限制只有 CONTOSO Kerberos 領域的非網域控制站存取：
```
ksetup /setrealm CONTOSO
```

## <a name="additional-references"></a>其他參考資料

-   - [命令列語法關鍵](command-line-syntax-key.md)
-   [Ksetup](ksetup.md)
-   [Ksetup:removerealm](ksetup-removerealm.md)