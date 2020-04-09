---
title: verify
description: 適用于 verify 的 Windows 命令主題會告訴**cmd** ，是否要確認您的檔案已正確寫入至磁片。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: dfe8bc91-d948-4e47-84ad-a79a60506ffa
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 91a0777999a604a23e2de83eda6b89c926cb241c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830051"
---
# <a name="verify"></a>verify



告訴**cmd** ，是否要確認您的檔案已正確寫入至磁片。 如果使用時不含參數， **verify**會顯示目前的設定。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
verify [on | off]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[on \| off]|將 [**驗證**] 設定設為 [開啟] 或 [關閉]。|
|/?|在命令提示字元顯示說明。|

## <a name="examples"></a><a name=BKMK_examples></a>典型

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