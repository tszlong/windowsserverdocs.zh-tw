---
title: ksetup server
description: Ksetup 伺服器命令的參考文章，可讓您指定執行 Windows 作業系統之電腦的名稱，因此 ksetup 命令所做的變更會更新目的電腦。
ms.topic: article
ms.assetid: e3407111-ac92-457f-aa1f-a04fe9109d59
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2bfa894468ee05b983d8c0122a1738982ef4d909
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87887783"
---
# <a name="ksetup-server"></a>ksetup server

可讓您指定執行 Windows 作業系統之電腦的名稱，因此**ksetup**命令所做的變更會更新目的電腦。

目標伺服器名稱會儲存在登錄中的底下 `HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\LSA\Kerberos` 。 當您執行**ksetup**命令時，不會回報此專案。

> [!IMPORTANT]
> 沒有任何方法可以移除目標伺服器名稱。 相反地，您可以將它變更回本機電腦名稱稱，這是預設值。

## <a name="syntax"></a>語法

```
ksetup /server <servername>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<servername>` | 指定設定會生效的完整電腦名稱稱，例如*IPops897.corp.contoso.com*。<p>如果指定了不完整的完整網域電腦名稱稱，此命令將會失敗。 |

### <a name="examples"></a>範例

若要讓您的**ksetup**設定在*IPops897*電腦上生效，其已連線到 Contoso 網域，請輸入：

```
ksetup /server IPops897.corp.contoso.com
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [ksetup 命令](ksetup.md)
