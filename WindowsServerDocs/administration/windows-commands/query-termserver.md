---
title: query termserver
description: '[查詢 termserver] 命令的參考文章，此命令會顯示網路上所有遠端桌面工作階段主機伺服器的清單。'
ms.topic: reference
ms.assetid: 3b89d3b4-236f-4376-90b6-939a0ec4b288
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 103e1c888f20e368e014eba762b739e099d9e491
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038396"
---
# <a name="query-termserver"></a>query termserver

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示網路上所有遠端桌面工作階段主機伺服器的清單。 此命令會在網路中搜尋所有附加遠端桌面工作階段主機伺服器，並傳回下列資訊：

- 伺服器的名稱

- 如果使用/address 選項，則會使用網路 (和節點位址) 

> [!NOTE]
> 若要瞭解最新版本的新功能，請參閱 [Windows Server 遠端桌面服務中的新功能](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11))。

## <a name="syntax"></a>語法

```
query termserver [<servername>] [/domain:<domain>] [/address] [/continue]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<servername>` | 指定識別遠端桌面工作階段主機伺服器的名稱。 |
| /domain`<domain>` | 指定要為終端機伺服器查詢的網域。 如果您要查詢目前正在使用的網域，就不需要指定網域。 |
| /address | 顯示每部伺服器的網路和節點位址。 |
| /continue | 防止在顯示每個資訊畫面之後暫停。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

若要顯示網路上所有遠端桌面工作階段主機伺服器的相關資訊，請輸入：

```
query termserver
```

若要顯示名為 *Server3*的遠端桌面工作階段主機伺服器的相關資訊，請輸入：

```
query termserver Server3
```

若要顯示 domain *CONTOSO*中所有遠端桌面工作階段主機伺服器的相關資訊，請輸入：

```
query termserver /domain:CONTOSO
```

若要顯示名為 *Server3*的遠端桌面工作階段主機伺服器的網路和節點位址，請輸入：

```
query termserver Server3 /address
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [查詢命令](query.md)

- [遠端桌面服務 (終端機服務) 命令參考資料](remote-desktop-services-terminal-services-command-reference.md)
