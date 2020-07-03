---
title: ksetup removerealm
description: Ksetup removerealm 命令的參考文章，它會從登錄中刪除指定領域的所有資訊。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 39f0c6f0-4c50-4781-941e-0893495405e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0330f7b5f9121da2fce99985fe116be46eb1c9d9
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85933659"
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

| 參數 | 說明 |
| --------- | ----------- |
| `<realmname>` | 指定大寫 DNS 名稱，例如 CORP。CONTOSO.COM，且在執行**ksetup**時列為預設領域或**領域 =** 。 |

### <a name="examples"></a>範例

移除錯誤的領域名稱（。（而不是 .COM）從本機電腦輸入：
```
ksetup /removerealm CORP.CONTOSO.CON
```

若要確認移除，您可以執行**ksetup**命令並檢查輸出。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [ksetup 命令](ksetup.md)

- [ksetup setrealm 命令](ksetup-setrealm.md)
