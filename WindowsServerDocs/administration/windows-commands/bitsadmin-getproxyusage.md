---
title: bitsadmin getproxyusage
description: 適用于**bitsadmin getproxyusage**的 Windows 命令主題，它會抓取所指定工作的 proxy 使用方式設定。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f940a70e-3b02-497e-a47f-b37b905c299e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 01c9bb9a1d413fa847482f652e18eed30ad76109
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850511"
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
| 工作 | 作業的顯示名稱或 GUID。 |

## <a name="remarks"></a>備註

Proxy 使用值包括：

- **Lnk-what-are-preconfig-solutions** -使用擁有者的 Internet Explorer 預設值。

- **No_Proxy** -不要使用 Proxy 伺服器。

- 覆**寫**-使用明確的 proxy 清單。

- 自動**偵測-自動**偵測 proxy 設定。

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會抓取名為*myDownloadJob*之作業的 proxy 使用方式。

```
C:\>bitsadmin /getproxyusage myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)