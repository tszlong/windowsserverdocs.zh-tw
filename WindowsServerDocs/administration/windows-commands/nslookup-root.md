---
title: nslookup root
description: Nslookup 根命令的參考文章，它會將預設伺服器變更為網域名稱系統根目錄的伺服器， (DNS) 功能變數名稱空間。
ms.topic: article
ms.assetid: 9c29edc3-ec49-43f2-bc49-86bf0612d816
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 708c30f8915b422dd12c38a25fd07d62c27ea268
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87885818"
---
# <a name="nslookup-root"></a>nslookup root

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

針對網域名稱系統的根目錄，將預設伺服器變更為伺服器 (DNS) 功能變數名稱空間。 目前會使用 ns.nic.ddn.mil 名稱伺服器。 您可以使用[nslookup set root](nslookup-set-root.md)命令來變更根功能變數名稱伺服器的名稱。

> [!NOTE]
> 此命令與相同 `lserver ns.nic.ddn.mil` 。

## <a name="syntax"></a>語法

```
root
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /? | 在命令提示字元顯示說明。 |
| /help | 在命令提示字元顯示說明。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [nslookup set root](nslookup-set-root.md)
