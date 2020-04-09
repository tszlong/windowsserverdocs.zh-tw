---
title: bitsadmin getpriority
description: 適用于**bitsadmin getpriority**的 Windows 命令主題，它會抓取指定之作業的優先順序。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: b27829a0fb852abb88c88a65e61e8d7693ca2df2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850541"
---
# <a name="bitsadmin-getpriority"></a>bitsadmin getpriority

抓取指定之作業的優先順序。

## <a name="syntax"></a>語法

```
bitsadmin /getpriority <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 工作 | 作業的顯示名稱或 GUID。 |

## <a name="remarks"></a>備註

此命令的優先順序可以是：

- **提到**

- **更**

- **一般**

- **量**

- **未知**

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會抓取名為*myDownloadJob*之作業的優先順序。

```
C:\>bitsadmin /getpriority myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
