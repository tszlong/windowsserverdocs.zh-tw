---
title: ksetup：伺服器
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e3407111-ac92-457f-aa1f-a04fe9109d59
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 91549eb78f825264016ec0e03b7035f79132f260
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724598"
---
# <a name="ksetupserver"></a>ksetup：伺服器



可讓您指定執行 Windows 作業系統之電腦的名稱，如此一來，使用**ksetup**所做的變更就會更新目的電腦。

## <a name="syntax"></a>語法

```
ksetup /server <ServerName>
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<ServerName>|設定將會生效的完整電腦名稱稱，例如 IPops897.corp.contoso.com。</br>如果指定了不完整的完整網域電腦名稱稱，此命令將會失敗。|

## <a name="remarks"></a>備註

沒有任何方法可以移除目標伺服器名稱;您只能將它變更回本機電腦名稱稱，這是預設值。

目標伺服器名稱會儲存在登錄中的**HKEY_LOCAL_MACHINE \system\controlset001\control\lsa\kerberos**。 不會使用**ksetup**來報告。

## <a name="examples"></a>範例

在連接到 Contoso 網域的 IPops897 電腦上，讓您的**ksetup**設定生效：
```
ksetup /server IPops897.corp.contoso.com
```

## <a name="additional-references"></a>其他參考

-   [Ksetup](ksetup.md)
-   - [命令列語法關鍵](command-line-syntax-key.md)