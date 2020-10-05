---
title: telnet send
description: Telnet 傳送命令的參考文章，此命令會將 telnet 命令傳送到 telnet 伺服器。
ms.topic: reference
ms.assetid: 7c217abc-1182-466e-914c-1ff16755021b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ab6c94f619ab28be7b850479a7e3d476e1394e09
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91717985"
---
# <a name="telnet-send"></a>telnet：傳送

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將 telnet 命令傳送到 telnet 伺服器。

## <a name="syntax"></a>語法

```
sen {ao | ayt | brk | esc | ip | synch | <string>} [?]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| ao | 傳送 telnet 命令 **中止輸出**。 |
| ayt | 傳送 telnet 命令給 **您？** |
| brk | 傳送 telnet 命令 **brk**。 |
| esc | 傳送目前的 telnet escape 字元。 |
| ip | 傳送 telnet 命令 **中斷進程**。 |
| synch | 傳送 telnet 命令同步處理。 |
| `<string>` | 將您輸入的任何字串傳送到 telnet 伺服器。 |
| ? | 顯示與此命令相關聯的說明。 |

## <a name="example"></a>範例

若要在 telnet 伺服器上傳送給 **您？** 命令，請輸入：

```
sen ayt
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
