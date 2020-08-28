---
title: bitsadmin getproxyusage
description: Bitsadmin getproxyusage 命令的參考文章，此命令會抓取指定作業的 proxy 使用方式設定。
ms.topic: reference
ms.assetid: f940a70e-3b02-497e-a47f-b37b905c299e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 423168d72a88ecad90fd8de43eab3cb4486a750b
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024402"
---
# <a name="bitsadmin-getproxyusage"></a>bitsadmin getproxyusage

抓取指定作業的 proxy 使用方式設定。

## <a name="syntax"></a>語法

```
bitsadmin /getproxyusage <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

#### <a name="output"></a>輸出

傳回的 proxy 使用值可以是：

- **Preconfig** -使用擁有者的 Internet Explorer 預設值。

- **No_Proxy** -不使用 Proxy 伺服器。

- 覆**寫**-使用明確的 proxy 清單。

- 自動**檢測-自動**偵測 proxy 設定。

## <a name="examples"></a>範例

若要取得名為 *myDownloadJob*之作業的 proxy 使用方式：

```
bitsadmin /getproxyusage myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
