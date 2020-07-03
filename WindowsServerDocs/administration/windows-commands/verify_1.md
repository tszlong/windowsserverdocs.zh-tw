---
title: 驗證
description: 驗證的參考文章，它會告訴**cmd**是否要確認您的檔案已正確寫入磁片。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: dfe8bc91-d948-4e47-84ad-a79a60506ffa
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1455705d409e0273e85135a183279835e7238d7a
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931314"
---
# <a name="verify"></a>驗證



告訴**cmd** ，是否要確認您的檔案已正確寫入至磁片。 如果使用時不含參數， **verify**會顯示目前的設定。



## <a name="syntax"></a>語法

```
verify [on | off]
```

### <a name="parameters"></a>參數

|參數|說明|
|---------|-----------|
|[ \| 關閉]|將 [**驗證**] 設定設為 [開啟] 或 [關閉]。|
|/?|在命令提示字元顯示說明。|

## <a name="examples"></a>範例

若要顯示 [目前的**驗證**] 設定，請輸入：
```
verify
```
若要開啟 [**驗證**] 設定，請輸入：
```
Verify on
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)