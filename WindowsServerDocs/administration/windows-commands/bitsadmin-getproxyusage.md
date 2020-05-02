---
title: bitsadmin getproxyusage
description: Bitsadmin getproxyusage 命令的參考主題，它會抓取所指定工作的 proxy 使用方式設定。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f940a70e-3b02-497e-a47f-b37b905c299e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 13a3f216b1ed3c77dbbefee37d73a657525daa36
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717642"
---
# <a name="bitsadmin-getproxyusage"></a>bitsadmin getproxyusage

抓取指定之作業的 proxy 使用方式設定。

## <a name="syntax"></a>語法

```
bitsadmin /getproxyusage <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

#### <a name="output"></a>輸出

傳回的 proxy 使用方式值可以是：

- **Lnk-what-are-preconfig-solutions** -使用擁有者的 Internet Explorer 預設值。

- **No_Proxy** -不要使用 Proxy 伺服器。

- 覆**寫**-使用明確的 proxy 清單。

- 自動**偵測-自動**偵測 proxy 設定。

## <a name="examples"></a>範例

若要取得名為*myDownloadJob*之作業的 proxy 使用方式：

```
bitsadmin /getproxyusage myDownloadJob
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
