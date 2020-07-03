---
title: bitsadmin nowrap
description: Bitsadmin nowrap 命令的參考文章，它會截斷延伸至命令視窗最右邊邊緣以外的任何輸出文字行。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 85a47b90-783a-41e4-96f2-81f26ae8ca93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8f55faeea79e5f2cf02fde0732ed82ae13e902f
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923020"
---
# <a name="bitsadmin-nowrap"></a>bitsadmin nowrap

截斷任何延伸到命令視窗最右邊邊緣外的輸出文字行。 根據預設，除了**監視器**參數之外，所有參數都會包裝輸出。 在其他參數前指定**nowrap**參數。

## <a name="syntax"></a>語法

```
bitsadmin /nowrap
```

## <a name="examples"></a>範例

若要在未包裝輸出的情況下，取得名為*myDownloadJob*之作業的狀態：

```
bitsadmin /nowrap /getstate myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
