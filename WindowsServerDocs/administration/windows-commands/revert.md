---
title: 還原
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 75ad40e4-502a-401e-b11e-8b31e00424b5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b967904c28661be82909ebcc0aa7cb42c73e418c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722329"
---
# <a name="revert"></a>還原



將磁片區還原回指定的陰影複製。 只有 CLIENTACCESSIBLE 內容中的陰影複製才支援這項功能。 這些陰影複製是持續性的，而且只能由系統提供者進行。 如果使用時不含參數， **revert**會在命令提示字元中顯示說明。

## <a name="syntax"></a>語法

```
revert <ShadowCopyID>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<ShadowCopyID>|指定要復原磁碟區的陰影複製識別碼。|

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)