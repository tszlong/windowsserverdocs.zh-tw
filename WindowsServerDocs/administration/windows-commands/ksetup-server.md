---
title: ksetup:server
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f370d4dede278e1facdda829503ea3793502b9e6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814569"
---
# <a name="ksetupserver"></a>ksetup:server



可讓您指定的名稱，讓變更藉由執行 Windows 作業系統的電腦**ksetup**會更新目標電腦。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /server <ServerName>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<ServerName>|在其設定生效，例如 IPops897.corp.contoso.com 完整電腦名稱。</br>如果未完成完整指定網域的電腦名稱，則命令會失敗。|

## <a name="remarks"></a>備註

沒有任何方法中移除目標的伺服器名稱;您可以只變更回本機電腦名稱，這是預設值。

目標伺服器名稱儲存在登錄中**HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\LSA\Kerberos**。 它不會報告利用**ksetup**。

## <a name="BKMK_Examples"></a>範例

讓您**ksetup**有效連線在 Contoso 網域 IPops897 電腦上的設定：
```
ksetup /server IPops897.corp.contoso.com
```

#### <a name="additional-references"></a>其他參考資料

-   [Ksetup](ksetup.md)
-   [命令列語法關鍵](command-line-syntax-key.md)