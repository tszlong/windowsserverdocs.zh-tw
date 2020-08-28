---
title: 查詢處理序
description: 查詢程式命令的參考檔，它會顯示在遠端桌面工作階段主機伺服器上執行之進程的相關資訊。
ms.topic: reference
ms.assetid: 36ce3ffc-0092-4eb1-a374-28e6616ca946
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c0cf1952be3e7885c4631c229061b4630ef4598c
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037226"
---
# <a name="query-process"></a>查詢處理序

適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示遠端桌面工作階段主機伺服器上執行之處理常式的相關資訊。 您可以使用此命令來找出特定使用者正在執行的程式，以及哪些使用者正在執行特定程式。 此命令會傳回下列資訊：

- 擁有流程的使用者

- 擁有處理常式的會話

- 會話的識別碼

- 處理序的名稱

- 進程的識別碼

> [!NOTE]
> 若要瞭解最新版本的新功能，請參閱 [Windows Server 遠端桌面服務中的新功能](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11))。

## <a name="syntax"></a>語法

```
query process [*|<processID>|<username>|<sessionname>|/id:<nn>|<programname>] [/server:<servername>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| * | 列出所有會話的處理常式。 |
| `<processID>` | 指定識別您要查詢之進程的數值識別碼。 |
| `<username>` | 指定您要列出其進程的使用者名稱。 |
| `<sessionname>` | 指定要列出其進程之作用中會話的名稱。 |
| /id`<nn>` | 指定您要列出其進程的會話識別碼。 |
| `<programname>` | 指定您要查詢其進程的程式名稱。 需要 .exe 副檔名。 |
| /server:`<servername>` | 指定要列出其進程的遠端桌面工作階段主機伺服器。 如果未指定，則會使用您目前登入的伺服器。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 系統管理員具有所有 **查詢處理** 函式的完整存取權。

- 如果您未指定 <使用者*名稱*>、<*sessionname*>、 */id： `<nn>` *、<*programname*> 或 *&#42;* 參數，此查詢只會顯示屬於目前使用者的處理常式。

- 當 **查詢** 程式傳回信息時，會在 `(>)` 屬於目前會話的每個進程之前顯示大於符號。

## <a name="examples"></a>範例

若要顯示所有會話所使用之進程的相關資訊，請輸入：

```
query process *
```

若要顯示 *會話識別碼 2*所使用之進程的相關資訊，請輸入：

```
query process /ID:2
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [查詢命令](query.md)

- [遠端桌面服務 (終端機服務) 命令參考資料](remote-desktop-services-terminal-services-command-reference.md)
