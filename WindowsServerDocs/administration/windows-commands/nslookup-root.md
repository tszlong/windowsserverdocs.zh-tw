---
title: nslookup root
description: Nslookup 根命令的參考主題，它會將伺服器的預設伺服器變更為網域名稱系統（DNS）功能變數名稱空間的根目錄。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9c29edc3-ec49-43f2-bc49-86bf0612d816
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0f1f2bbe3b71660d079a0b7c87f5be487e0ff437
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721651"
---
# <a name="nslookup-root"></a>nslookup root

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將伺服器的預設伺服器變更為網域名稱系統（DNS）功能變數名稱空間的根目錄。 目前會使用 ns.nic.ddn.mil 名稱伺服器。 您可以使用[nslookup set root](nslookup-set-root.md)命令來變更根功能變數名稱伺服器的名稱。

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

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [nslookup set root](nslookup-set-root.md)
