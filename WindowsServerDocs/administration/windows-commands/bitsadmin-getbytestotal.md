---
title: bitsadmin getbytestotal
description: '**Bitsadmin getbytestotal**的 Windows 命令主題，它會抓取指定工作的大小。'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 784e0bfa-7b09-4262-9104-adbc9beb479b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 84e3e0311ead0eb79f9247d4f06844ece5f20fa2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850781"
---
# <a name="bitsadmin-getbytestotal"></a>bitsadmin getbytestotal

抓取指定之作業的大小。

## <a name="syntax"></a>語法

```
bitsadmin /getbytestotal <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 工作 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會捕獲名為*myDownloadJob*的作業大小。

```
C:\>bitsadmin /getbytestotal myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)