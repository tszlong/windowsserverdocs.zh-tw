---
title: ksetup server
description: '>ksetup server 命令的參考文章，此命令可讓您指定執行 Windows 作業系統之電腦的名稱，因此 >ksetup 命令所做的變更會更新目的電腦。'
ms.topic: reference
ms.assetid: e3407111-ac92-457f-aa1f-a04fe9109d59
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4be82315bc0d683b5399350c618aa87e615067ba
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639579"
---
# <a name="ksetup-server"></a>ksetup server

可讓您指定執行 Windows 作業系統之電腦的名稱，因此 **>ksetup** 命令所做的變更會更新目的電腦。

目標伺服器名稱會儲存在登錄下的登錄中 `HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\LSA\Kerberos` 。 當您執行 **>ksetup** 命令時，不會報告此專案。

> [!IMPORTANT]
> 沒有任何方法可以移除目標伺服器名稱。 相反地，您可以將它變更回本機電腦名稱稱（預設值）。

## <a name="syntax"></a>語法

```
ksetup /server <servername>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<servername>` | 指定設定將會生效的完整電腦名稱稱，例如 *IPops897.corp.contoso.com*。<p>如果指定了不完整的完整網域電腦名稱稱，命令將會失敗。 |

### <a name="examples"></a>範例

若要讓您的 **>ksetup** 設定在連線到 Contoso 網域的 *IPops897* 電腦上生效，請輸入：

```
ksetup /server IPops897.corp.contoso.com
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [>ksetup 命令](ksetup.md)
