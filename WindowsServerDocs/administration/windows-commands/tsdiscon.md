---
title: tsdiscon
description: Tsdiscon 的參考文章，會中斷會話與遠端桌面工作階段主機伺服器之間的連線。
ms.topic: reference
ms.assetid: 13139674-7dee-4965-8cac-32f4928e8b9a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 8b7126e2e9d1f5185ea64566843c523bf0570891
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156387"
---
# <a name="tsdiscon"></a>tsdiscon

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

中斷會話與遠端桌面工作階段主機伺服器的連線。 如果您未指定會話識別碼或會話名稱，此命令會中斷目前會話的連線。

> [!IMPORTANT]
> 您必須擁有 [ **完全控制] 訪問** 許可權或 **[中斷連線] 特殊訪問** 許可權，才能中斷另一位使用者與會話的連線。

> [!NOTE]
> 若要瞭解最新版本的新功能，請參閱 [Windows Server 遠端桌面服務中的新功能](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11))。

## <a name="syntax"></a>語法

```
tsdiscon [<sessionID> | <sessionname>] [/server:<servername>] [/v]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<sessionID>` | 指定要中斷連接之會話的識別碼。 |
| `<sessionname>` | 指定要中斷連接之會話的名稱。 |
| /server:`<servername>` | 指定包含您要中斷連線之會話的終端機伺服器。 否則，就會使用目前的遠端桌面工作階段主機伺服器。 只有當您從遠端伺服器執行 **tsdiscon** 命令時，才需要此參數。 |
| /v | 顯示正在執行之動作的相關資訊。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 當您在中斷連線會話時執行的任何應用程式，都會在您重新連線到該會話時自動執行，而不會遺失任何資料。 您可以使用 [ [重設會話] 命令](reset-session.md) 來結束已中斷連線會話的執行中應用程式，但這可能會導致會話遺失資料。

- 主控台會話無法中斷連線。

## <a name="examples"></a>範例

若要中斷目前會話的連線，請輸入：

```
tsdiscon
```

若要中斷 *會話 10*的連線，請輸入：

```
tsdiscon 10
```

若要中斷名為 *TERM04*的會話連線，請輸入：

```
tsdiscon TERM04
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [遠端桌面服務 (終端機服務) 命令參考資料](remote-desktop-services-terminal-services-command-reference.md)

- [重設會話命令](reset-session.md)
