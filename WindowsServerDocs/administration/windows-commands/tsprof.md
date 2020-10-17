---
title: tsprof
description: Tsprof 命令的參考文章，此命令會將遠端桌面服務使用者設定資訊從一個使用者複製到另一個使用者。
ms.topic: reference
ms.assetid: 27047868-b706-4208-b7e0-1437a2325dd3
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: e18f736a31af3cc649d4921197710f713e7df6af
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156347"
---
# <a name="tsprof"></a>tsprof

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將遠端桌面服務的使用者設定資訊從一個使用者複製到另一個使用者。 遠端桌面服務的使用者設定資訊會出現在 [本機使用者和群組] 和 [active directory 使用者和電腦] 的遠端桌面服務擴充功能中。

> [!NOTE]
> 您也可以使用 [tsprof 命令](tsprof.md) 來設定使用者的設定檔路徑。
>
> 若要瞭解最新版本的新功能，請參閱 [Windows Server 遠端桌面服務中的新功能](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11))。

## <a name="syntax"></a>語法

```
tsprof /update {/domain:<Domainname> | /local} /profile:<path> <username>
tsprof /copy {/domain:<Domainname> | /local} [/profile:<path>] <src_user> <dest_user>
tsprof /q {/domain:<Domainname> | /local} <username>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| /update | 將網域中的設定檔路徑資訊更新為 `<username>` `<domainname>` `<profilepath>` 。 |
| /domain`<Domainname>` | 指定要在其中套用作業之網域的名稱。 |
| /local | 只將作業套用至本機使用者帳戶。 |
| /profile`<path>` | 指定 [本機使用者和群組] 和 [active directory 使用者和電腦] 中遠端桌面服務擴充功能所顯示的設定檔路徑。 |
| `<username>` | 指定您要更新或查詢伺服器設定檔路徑之使用者的名稱。 |
| /copy | 從複製使用者設定資訊 `<src_user>` `<dest_user>` ，並將的設定檔路徑資訊更新為 `<dest_user>` `<profilepath>` 。 `<src_user>`和都 `<dest_user>` 必須是本機或必須在網域中 `<domainname>` 。 |
| `<src_user>` | 指定您要從中複製使用者設定資訊之使用者的名稱。 也稱為來源使用者。 |
| `<dest_user>` | 指定您要複製使用者設定資訊之使用者的名稱。 也稱為「目的地使用者」。 |
| /q | 顯示您要查詢伺服器設定檔路徑的使用者目前的設定檔路徑。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要將使用者設定資訊從 *LocalUser1* 複製到 *LocalUser2*，請輸入：

```
tsprof /copy /local LocalUser1 LocalUser2
```

若要將 *LocalUser1* 的遠端桌面服務設定檔路徑設定為名為 *c:\profiles*的目錄，請輸入：

```
tsprof /update /local /profile:c:\profiles LocalUser1
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [遠端桌面服務 (終端機服務) 命令參考資料](remote-desktop-services-terminal-services-command-reference.md)
