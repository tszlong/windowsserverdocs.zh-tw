---
title: bitsadmin getowner
description: Bitsadmin **getowner**的 Windows 命令主題，它會抓取指定之作業的擁有者。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5203f84c-a879-4f31-ae3e-7ea74bd63ca5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3e622c3759c9ec20867c693539c4481c70aa4f26
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850561"
---
# <a name="bitsadmin-getowner"></a>bitsadmin getowner

顯示指定作業之擁有者的顯示名稱或 GUID。

## <a name="syntax"></a>語法

```
bitsadmin /getowner <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 工作 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會顯示名為*myDownloadJob*之作業的擁有者。

```
C:\>bitsadmin /getowner myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)