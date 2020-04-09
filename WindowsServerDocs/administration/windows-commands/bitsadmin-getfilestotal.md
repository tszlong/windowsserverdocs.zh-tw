---
title: bitsadmin getfilestotal
description: '**Bitsadmin getfilestotal**的 Windows 命令主題，它會抓取指定之作業中的檔案數目。'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c5de113e-f29c-4cd3-9392-0e300018d516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bad42a8bef57ca4c4a1411a12f20979e4a95d178
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850681"
---
# <a name="bitsadmin-getfilestotal"></a>bitsadmin getfilestotal

抓取指定之作業中的檔案數目。

## <a name="syntax"></a>語法

```
bitsadmin /getfilestotal <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 工作 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會抓取名為*myDownloadJob*之作業中包含的檔案數目。

```
C:\>bitsadmin /getfilestotal myDownloadJob
```

## <a name="see-also"></a>另請參閱

- [命令列語法關鍵](command-line-syntax-key.md)
