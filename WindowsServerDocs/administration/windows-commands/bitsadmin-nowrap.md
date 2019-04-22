---
title: bitsadmin nowrap
description: 適用於 Windows 命令主題**bitsadmin nowrap** -截斷任何一行輸出擴充超過最右邊的 [命令] 視窗邊緣的文字。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 85a47b90-783a-41e4-96f2-81f26ae8ca93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4130f606a6b1874e1ea31952160de44d6e09c6b6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822919"
---
# <a name="bitsadmin-nowrap"></a>bitsadmin nowrap

會截斷任何一行輸出擴充超過最右邊的 [命令] 視窗邊緣的文字。

## <a name="syntax"></a>語法

```
bitsadmin /NoWrap
```

## <a name="remarks"></a>備註

根據預設，所有參數，除了**監視器**參數，輸出包裝。 指定**NoWrap**切換其他參數。

## <a name="BKMK_examples"></a>範例

下列範例會擷取名為作業的狀態*myDownloadJob*和不換行輸出
```
C:\>bitsadmin /NoWrap /GetState myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)