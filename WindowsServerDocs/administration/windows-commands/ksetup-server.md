---
title: ksetup：伺服器
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e3407111-ac92-457f-aa1f-a04fe9109d59
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dd05fd294640c63e633b7b866307197ae6770476
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374964"
---
# <a name="ksetupserver"></a>ksetup：伺服器



可讓您指定執行 Windows 作業系統之電腦的名稱，如此一來，使用**ksetup**所做的變更就會更新目的電腦。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /server <ServerName>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<ServerName >|設定將會生效的完整電腦名稱稱，例如 IPops897.corp.contoso.com。</br>如果指定了不完整的完整網域電腦名稱稱，此命令將會失敗。|

## <a name="remarks"></a>備註

沒有任何方法可以移除目標伺服器名稱;您只能將它變更回本機電腦名稱稱，這是預設值。

目標伺服器名稱會儲存在**HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\LSA\Kerberos**的登錄中。 不會使用**ksetup**來報告。

## <a name="BKMK_Examples"></a>典型

在連接到 Contoso 網域的 IPops897 電腦上，讓您的**ksetup**設定生效：
```
ksetup /server IPops897.corp.contoso.com
```

#### <a name="additional-references"></a>其他參考資料

-   [Ksetup](ksetup.md)
-   [命令列語法關鍵](command-line-syntax-key.md)