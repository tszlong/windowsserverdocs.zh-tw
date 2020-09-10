---
title: bitsadmin getproxyusage
description: Bitsadmin getproxyusage 命令的參考文章，此命令會抓取指定作業的 proxy 使用方式設定。
ms.topic: reference
ms.assetid: f940a70e-3b02-497e-a47f-b37b905c299e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: bdfcfa92a5886857920d56a0028a450ee9a3e521
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631732"
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
