---
title: tscon
description: Tscon 的參考文章，可連接到遠端桌面工作階段主機伺服器上的另一個會話。
ms.topic: reference
ms.assetid: 315a9793-cd10-4987-bb68-89a9d13f7fce
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 17b18aea265ff7c703c2ef6c9c3d0021a9d9ea00
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156391"
---
# <a name="tscon"></a>tscon

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

連接到遠端桌面工作階段主機伺服器上的另一個會話。

> [!IMPORTANT]
> 您必須擁有 [ **完全控制] 訪問** 許可權或 **[連接特殊訪問** 許可權]，才能連接到另一個會話。

> [!NOTE]
> 若要瞭解最新版本的新功能，請參閱 [Windows Server 遠端桌面服務中的新功能](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11))。

## <a name="syntax"></a>語法

```
tscon {<sessionID> | <sessionname>} [/dest:<sessionname>] [/password:<pw> | /password:*] [/v]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<sessionID>` | 指定您要連接之會話的識別碼。 如果您使用選擇性 `/dest:<sessionname>` 參數，您也可以指定目前會話的名稱。 |
| `<sessionname>` | 指定您要連接之會話的名稱。 |
| /dest`<sessionname>` | 指定目前會話的名稱。 當您連接到新的會話時，此會話將會中斷連線。 您也可以使用這個參數，將另一個使用者的會話連接至不同的會話。 |
| /password`<pw>` | 指定擁有您要連接之會話的使用者密碼。 當連接的使用者未擁有會話時，需要此密碼。 |
| /password`*` | 提示您輸入擁有您要連接之會話的使用者密碼。 |
| /v | 顯示正在執行之動作的相關資訊。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 如果您未在 **/password** 參數中指定密碼，且目標會話所屬的使用者不是目前的使用者，則此命令會失敗。

- 您無法連接到主控台會話。

## <a name="examples"></a>範例

若要連接到目前遠端桌面服務工作階段主機伺服器上的 *會話 12* ，並中斷目前會話的連線，請輸入：

```
tscon 12
```

若要使用密碼*mypass*連接到目前遠端桌面服務工作階段主機伺服器上的*會話 23* ，並中斷目前會話的連線，請輸入：

```
tscon 23 /password:mypass
```

若要將名為 *TERM03* 的會話連線到名為 *TERM05*的會話，然後將會話 *TERM05*中斷連線，請輸入：

```
tscon TERM03 /v /dest:TERM05
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [遠端桌面服務 (終端機服務) 命令參考資料](remote-desktop-services-terminal-services-command-reference.md)
