---
title: bitsadmin getproxyusage
description: Bitsadmin getproxyusage 命令的參考文章，它會抓取所指定工作的 proxy 使用方式設定。
ms.topic: article
ms.assetid: f940a70e-3b02-497e-a47f-b37b905c299e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ff2320ba66cdc11781ac56900e5a4aaa9fb1dc0f
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893939"
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

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
