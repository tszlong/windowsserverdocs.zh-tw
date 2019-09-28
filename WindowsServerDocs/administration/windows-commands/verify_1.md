---
title: verify
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 840fd3609ed3aded1c9cfebd4e395ddcc6d5588b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363097"
---
# <a name="verify"></a>verify



告訴**cmd** ，是否要確認您的檔案已正確寫入至磁片。 如果使用時不含參數， **verify**會顯示目前的設定。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
verify [on | off]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[on \| off]|將 [**驗證**] 設定設為 [開啟] 或 [關閉]。|
|/?|在命令提示字元顯示說明。|

## <a name="BKMK_examples"></a>典型

若要顯示 [目前的**驗證**] 設定，請輸入：
```
verify
```
若要開啟 [**驗證**] 設定，請輸入：
```
Verify on
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)