---
title: bitsadmin nowrap
description: 適用于**bitsadmin nowrap**的 Windows 命令主題，它會截斷延伸至命令視窗最右邊邊緣以外的任何一行輸出文字。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 85a47b90-783a-41e4-96f2-81f26ae8ca93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f9f1db370d8a8917aa03a414a27623a1024df192
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850181"
---
# <a name="bitsadmin-nowrap"></a>bitsadmin nowrap

截斷任何延伸到命令視窗最右邊邊緣外的輸出文字行。 根據預設，除了**監視器**參數之外，所有參數都會包裝輸出。 在其他參數前指定**nowrap**參數。

## <a name="syntax"></a>語法

```
bitsadmin /nowrap
```

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會抓取名為*myDownloadJob*之作業的狀態，而且不會包裝輸出。

```
C:\>bitsadmin /nowrap /getstate myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)