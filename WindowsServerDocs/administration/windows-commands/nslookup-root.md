---
title: nslookup root
description: Nslookup 根命令的參考文章，此命令會將網域名稱系統的根目錄的預設伺服器變更為伺服器 (DNS) 的功能變數名稱空間。
ms.topic: reference
ms.assetid: 9c29edc3-ec49-43f2-bc49-86bf0612d816
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ef505427e92b6f0e14adf92d9eea463feb582314
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038789"
---
# <a name="nslookup-root"></a>nslookup root

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將網域名稱系統根目錄的預設伺服器變更為伺服器 (DNS) 功能變數名稱空間。 目前使用的是 ns.nic.ddn.mil 名稱伺服器。 您可以使用 [nslookup set root](nslookup-set-root.md) 命令變更根功能變數名稱伺服器的名稱。

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
