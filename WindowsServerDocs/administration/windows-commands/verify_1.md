---
title: verify
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dfe8bc91-d948-4e47-84ad-a79a60506ffa
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e5e99237c2bac93625dedec0254c274e4f8dbc9f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827249"
---
# <a name="verify"></a>verify



會告訴**cmd**是否要驗證您的檔案正確地寫入至磁碟。 如果未指定參數，使用**確認**顯示目前的設定。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
verify [on | off]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[在\|關閉]|交換器**確認**設定開啟或關閉。|
|/?|在命令提示字元顯示說明。|

## <a name="BKMK_examples"></a>範例

若要顯示目前**確認**設定中，輸入：
```
verify
```
若要開啟**確認**設定中，輸入：
```
Verify on
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)