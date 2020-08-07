---
title: ksetup removerealm
description: Ksetup removerealm 命令的參考文章，它會從登錄中刪除指定領域的所有資訊。
ms.topic: article
ms.assetid: 39f0c6f0-4c50-4781-941e-0893495405e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a755600bc0d1bdbc7a1b19bed041cb4a7c5dea90
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87887819"
---
# <a name="ksetup-removerealm"></a>ksetup removerealm

從登錄中刪除指定領域的所有資訊。

領域名稱會儲存在登錄中的 `HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001` 和之下 `\CurrentControlSet\Control\Lsa\Kerberos` 。 此專案預設不存在於登錄中。 您可以使用[ksetup addrealmflags](ksetup-addrealmflags.md)命令來填入登錄。

> [!IMPORTANT]
> 您無法從網域控制站移除預設的領域名稱，因為這樣會重設其 DNS 資訊，而將它移除可能會使網域控制站無法使用。

## <a name="syntax"></a>語法

```
ksetup /removerealm <realmname>
```
### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<realmname>` | 指定大寫 DNS 名稱，例如 CORP。CONTOSO.COM，且在執行**ksetup**時列為預設領域或**領域 =** 。 |

### <a name="examples"></a>範例

若要移除錯誤的領域名稱 (。（而不是 .COM）從本機電腦) ，請輸入：
```
ksetup /removerealm CORP.CONTOSO.CON
```

若要確認移除，您可以執行**ksetup**命令並檢查輸出。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [ksetup 命令](ksetup.md)

- [ksetup setrealm 命令](ksetup-setrealm.md)
