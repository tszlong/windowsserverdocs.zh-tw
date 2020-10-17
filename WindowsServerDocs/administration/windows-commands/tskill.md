---
title: tskill
description: Tskill 命令的參考文章，此命令會結束遠端桌面工作階段主機伺服器的會話中執行的處理常式。
ms.topic: reference
ms.assetid: 08986e6a-6900-4ece-85a1-8f73b14db1b3
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b281df4852bc5fbc0756e7b052d82ea2f9bab6d5
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156378"
---
# <a name="tskill"></a>tskill

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

結束在遠端桌面工作階段主機伺服器上的會話中執行的處理常式。

> [!NOTE]
> 除非您是系統管理員，否則您可以使用此命令只結束屬於您的進程。 系統管理員具有所有 **tskill** 功能的完整存取權，而且可以結束在其他使用者會話中執行的處理常式。
>
> 若要瞭解最新版本的新功能，請參閱 [Windows Server 遠端桌面服務中的新功能](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11))。

## <a name="syntax"></a>語法

```
tskill {<processID> | <processname>} [/server:<servername>] [/id:<sessionID> | /a] [/v]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<processID>` | 指定您要結束之進程的識別碼。 |
| `<processname>` | 指定您要結束之進程的名稱。 這個參數可以包含萬用字元。 |
| /server:`<servername>` | 指定包含您要結束之進程的終端機伺服器。 如果未指定 **/server** ，則會使用目前的遠端桌面工作階段主機伺服器。 |
| /id`<sessionID>` | 結束在指定的會話中執行的處理常式。 |
| /a | 結束正在所有會話中執行的進程。 |
| /v | 顯示正在執行之動作的相關資訊。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 當工作階段中執行的所有處理程序均結束時，工作階段也會結束。

- 如果您使用 `<processname>` 和 `/server:<servername>` 參數，則也必須指定 `/id:<sessionID>` 或 **/a** 參數。

## <a name="examples"></a>範例

若要結束進程6543，請輸入：

```
tskill 6543
```

若要結束在會話5上執行的 process explorer，請輸入：

```
tskill explorer /id:5
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [遠端桌面服務 (終端機服務) 命令參考資料](remote-desktop-services-terminal-services-command-reference.md)
