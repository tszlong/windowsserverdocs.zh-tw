---
title: reset session
description: 重設會話命令的參考文章，可讓您重設遠端桌面工作階段主機伺服器上的會話。
ms.topic: reference
ms.assetid: 4f029ecc-874e-415a-95a8-8b731bae35f9
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 07/11/2018
ms.openlocfilehash: 745a3ba51714ad3f5431dedbe9cebedf77e4ae72
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89626917"
---
# <a name="reset-session"></a>reset session

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

可讓您重設 (刪除) 遠端桌面工作階段主機伺服器上的會話。 只有在工作階段的功能有問題或似乎已停止回應時，才需要重設工作階段。

> [!NOTE]
> 若要瞭解最新版本的新功能，請參閱 [Windows Server 遠端桌面服務中的新功能](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11))。

## <a name="syntax"></a>語法

```
reset session {<sessionname> | <sessionID>} [/server:<servername>] [/v]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<sessionname>` | 指定您想要重設的會話名稱。 若要判斷會話的名稱，請使用 [查詢會話命令](query-session.md)。 |
| `<sessionID>` | 指定要重設之會話的識別碼。 |
| /server:`<servername>` | 指定包含您要重設之會話的終端機伺服器。 否則，它會使用目前的遠端桌面工作階段主機伺服器。 只有當您從遠端伺服器使用此命令時，才需要此參數。 |
| /v | 顯示正在執行之動作的相關資訊。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="remarks"></a>備註

- 您一律可以重設自己的會話，但您必須擁有 [ **完全控制** ] 存取權限，才能重設其他使用者的會話。 請注意，重設使用者的會話而不發出警告，使用者可能會導致會話遺失資料。

## <a name="examples"></a>範例

若要重設指定的 *rdp-tcp # 6*會話，請輸入：

```
reset session rdp-tcp#6
```

若要重設使用 *會話識別碼 3*的會話，請輸入：

```
reset session 3
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [遠端桌面服務命令參考](remote-desktop-services-terminal-services-command-reference.md)
