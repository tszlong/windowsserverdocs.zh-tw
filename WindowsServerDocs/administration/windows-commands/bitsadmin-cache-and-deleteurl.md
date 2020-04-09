---
title: bitsadmin cache 和 deleteurl
description: 適用于**bitsadmin cache 和 deleteurl**的 Windows 命令主題，會刪除指定 URL 的所有快取專案。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e108b76b-fae9-4c16-bf4c-d74c9f025953
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 70099e795d0f05d0fcf75fbf6b82f5466d1c0c55
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850931"
---
# <a name="bitsadmin-cache-and-deleteurl"></a>bitsadmin cache 和 deleteurl

刪除指定 URL 的所有快取專案。

## <a name="syntax"></a>語法

```
bitsadmin /deleteURL url
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| URL | 識別遠端檔案的統一資源定位器。 |

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會刪除 `https://www.contoso.com/en/us/default.aspx` 的所有快取專案

```
C:\>bitsadmin /deleteURL https://www.contoso.com/en/us/default.aspx 
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)