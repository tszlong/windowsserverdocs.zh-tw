---
title: bitsadmin cache 和 setexpirationtime
description: 適用于**bitsadmin cache 和 setexpirationtime**的 Windows 命令主題，可設定快取到期時間。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 00ea6e4e-b707-4b31-88dd-b61a78565c8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bf283a0a8b94fd55c591609e3dcd1d127a2be81a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850881"
---
>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

# <a name="bitsadmin-cache-and-setexpirationtime"></a>bitsadmin cache 和 setexpirationtime

設定快取到期時間。

## <a name="syntax"></a>語法

```
bitsadmin /cache /setexpirationtime secs
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| secs | 快取到期之前的秒數。 |

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會在60秒內過期快取。

```
C:\>bitsadmin /cache / setexpirationtime 60
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
