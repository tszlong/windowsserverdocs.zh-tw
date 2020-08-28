---
title: ksetup removerealm
description: '>ksetup removerealm 命令的參考文章，此命令會從登錄中刪除指定領域的所有資訊。'
ms.topic: reference
ms.assetid: 39f0c6f0-4c50-4781-941e-0893495405e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 801ef79449cabed4718e417cac9aba9173dd07fb
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89025452"
---
# <a name="ksetup-removerealm"></a>ksetup removerealm

從登錄中刪除指定領域的所有資訊。

領域名稱會儲存在和下的登錄 `HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001` 中 `\CurrentControlSet\Control\Lsa\Kerberos` 。 依預設，此專案不存在於登錄中。 您可以使用 [>ksetup addrealmflags](ksetup-addrealmflags.md) 命令來填入登錄。

> [!IMPORTANT]
> 您無法從網域控制站移除預設的領域名稱，因為這會重設其 DNS 資訊，並將其移除可能會讓網域控制站無法使用。

## <a name="syntax"></a>語法

```
ksetup /removerealm <realmname>
```
### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<realmname>` | 指定大寫 DNS 名稱，例如 CORP。CONTOSO.COM，並在 **>ksetup**執行時列為預設領域或**領域 =** 。 |

### <a name="examples"></a>範例

移除錯誤的領域名稱 (。從本機電腦執行，而非 .COM) ，請輸入：
```
ksetup /removerealm CORP.CONTOSO.CON
```

若要確認移除，您可以執行 **>ksetup** 命令並檢查輸出。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [>ksetup 命令](ksetup.md)

- [>ksetup setrealm 命令](ksetup-setrealm.md)
