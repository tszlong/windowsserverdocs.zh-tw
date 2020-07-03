---
title: revert
description: '* * * * 的參考文章'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 75ad40e4-502a-401e-b11e-8b31e00424b5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8d2d8bfbfa10df9776e849b1450c2fce47c097b7
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85933043"
---
# <a name="revert"></a>revert



將磁片區還原回指定的陰影複製。 只有 CLIENTACCESSIBLE 內容中的陰影複製才支援這項功能。 這些陰影複製是持續性的，而且只能由系統提供者進行。 如果使用時不含參數， **revert**會在命令提示字元中顯示說明。

## <a name="syntax"></a>語法

```
revert <ShadowCopyID>
```

### <a name="parameters"></a>參數

|參數|說明|
|---------|-----------|
|\<ShadowCopyID>|指定要復原磁碟區的陰影複製識別碼。|

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)