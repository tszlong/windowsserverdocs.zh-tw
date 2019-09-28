---
title: ksetup： setrealm
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 1bbe5c000b7e84066c19511639fe3d92d7e4b558
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374896"
---
# <a name="ksetupsetrealm"></a>ksetup： setrealm



設定 Kerberos 領域的名稱。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /setrealm <DNSDomainName>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<DNSDomainName >|DNS 功能變數名稱的格式可以是完整功能變數名稱或簡單功能變數名稱。|

## <a name="remarks"></a>備註

DNS 功能變數名稱參數應以大寫字母輸入。 否則， **ksetup**命令會要求驗證繼續進行。

不支援在網域控制站上設定 Kerberos 領域。 嘗試這麼做會導致警告和命令失敗。

## <a name="BKMK_Examples"></a>典型

將此電腦的領域設定為特定的功能變數名稱，以限制只有 CONTOSO Kerberos 領域的非網域控制站存取：
```
ksetup /setrealm CONTOSO
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [Ksetup](ksetup.md)
-   [Ksetup:removerealm](ksetup-removerealm.md)