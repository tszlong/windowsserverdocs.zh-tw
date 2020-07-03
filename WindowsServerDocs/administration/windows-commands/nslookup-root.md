---
title: nslookup root
description: Nslookup 根命令的參考文章，它會將伺服器的預設伺服器變更為網域名稱系統（DNS）功能變數名稱空間的根目錄。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9c29edc3-ec49-43f2-bc49-86bf0612d816
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 07dbfcf401314145cb0e7553d71480072da4b574
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85936282"
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

| 參數 | 說明 |
| --------- | ----------- |
| /? | 在命令提示字元顯示說明。 |
| /help | 在命令提示字元顯示說明。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [nslookup set root](nslookup-set-root.md)
