---
title: bitsadmin getproxyusage
description: Bitsadmin getproxyusage 命令的參考文章，它會抓取所指定工作的 proxy 使用方式設定。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f940a70e-3b02-497e-a47f-b37b905c299e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ad1ffe5786202d6fecc0d65a719c9d6be0f5609e
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926786"
---
# <a name="bitsadmin-getproxyusage"></a>bitsadmin getproxyusage

抓取指定之作業的 proxy 使用方式設定。

## <a name="syntax"></a>語法

```
bitsadmin /getproxyusage <job>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
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
