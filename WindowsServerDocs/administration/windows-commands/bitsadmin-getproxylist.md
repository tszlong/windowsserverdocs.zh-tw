---
title: bitsadmin getproxylist-抓取指定之作業的 proxy 清單。
description: 適用于**bitsadmin getproxylist**的 Windows 命令主題，它會抓取指定之作業的 proxy 清單。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: eebfa727-d8f1-4ae3-9382-6d8ffe8c3df3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0c0d26fb074bd1b792caa7fe2ce8fd31b64365e2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850521"
---
# <a name="bitsadmin-getproxylist"></a>bitsadmin getproxylist

抓取要用於指定作業的 proxy 伺服器清單（以逗號分隔）。

## <a name="syntax"></a>語法

```
bitsadmin /getproxylist <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 工作 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會抓取名為*myDownloadJob*之作業的 proxy 清單。

```
C:\>bitsadmin /getproxylist myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)